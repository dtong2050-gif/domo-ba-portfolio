# Đặc tả API

Đặc tả API là hợp đồng giữa giao diện và máy chủ cho từng endpoint: đầu vào, đầu ra, mẫu yêu cầu, mẫu phản hồi và mã lỗi.
Mỗi nguyên nhân từ chối có mẫu phản hồi riêng, nên giao diện biết chính xác phải hiển thị gì trong từng trường hợp.
Nhờ đó dev front end, back end và tester cùng làm việc trên một bản mô tả, không phải đoán hành vi của nhau.

## 1. Quy ước chung

**Phạm vi:** 4 endpoint phục vụ luồng tra cứu tài khoản và đổi vai trò. Ba endpoint đầu không thay đổi tài khoản nào; chỉ endpoint cuối ghi dữ liệu.

| Quy ước | Nội dung |
|---|---|
| Xác thực | Mọi endpoint yêu cầu đăng nhập và có quyền quản trị tài khoản. Thiếu quyền thì bị từ chối **trước khi** truy vấn dữ liệu, và phản hồi không kèm dữ liệu nào |
| Định dạng phản hồi | Thành công: `success: true`, `data`, `message`; endpoint trả danh sách có thêm `meta` phân trang<br>Lỗi: `success: false`, `data: null`, `message`, `errorCode`; có thể kèm `details` (chi tiết lỗi) và `traceId` (chỉ với lỗi `5xx`, dùng để tra log) |
| Lý do thao tác | Mọi lệnh ghi bắt buộc kèm lý do dài 20–500 ký tự sau khi bỏ khoảng trắng đầu cuối. Giao diện và máy chủ dùng cùng một ràng buộc |
| Phân trang | Áp dụng trên **toàn bộ dữ liệu sau khi lọc**, không cắt ở 100 bản ghi đầu |
| Kiểm tra lại điều kiện | Máy chủ kiểm tra lại toàn bộ điều kiện bảo vệ **tại thời điểm ghi**, kể cả khi giao diện đã chặn trước |
| Phân biệt mã lỗi | Mỗi nguyên nhân từ chối có một mã riêng. Giao diện phân nhánh theo mã, không đọc nội dung câu thông báo |

## 2. Endpoint

### `GET /users` — Lấy danh sách tài khoản

**Mục đích:** cung cấp dữ liệu cho bảng tài khoản, đã lọc và sắp xếp sẵn theo điều kiện quản trị viên chọn.

**Đầu vào:**

| Tham số | Kiểu | Mặc định | Ràng buộc |
|---|---|---|---|
| `q` | chuỗi | — | Tự bỏ khoảng trắng đầu cuối. Khớp theo tên **hoặc** email, không phân biệt chữ hoa chữ thường |
| `roleId` | mã vai trò | — | Nhận cả vai trò đã công bố lẫn vai trò nháp. Không nhận vai trò đã ngừng dùng |
| `status` | chọn một | `all` | `all`, `active`, `disabled` |
| `sortOrder` | chọn một | `desc` | `asc` hoặc `desc`. Sắp xếp theo ngày tham gia. Khi hai bản ghi trùng ngày thì dùng mã tài khoản làm thứ tự phụ, để không đảo dòng giữa các trang |
| `page` | số nguyên | `1` | Từ 1 trở lên |
| `pageSize` | số nguyên | `10` | Chỉ nhận 10, 20, 50 hoặc 100. Giá trị khác bị **từ chối**, không tự đổi về mặc định |

**Request mẫu:** lọc tài khoản đang hoạt động có tên hoặc email chứa "huy"

```http
GET /users?q=huy&status=active&sortOrder=desc&page=1&pageSize=10 HTTP/1.1
Host: <địa chỉ máy chủ>
Authorization: Bearer <mã đăng nhập>
Accept: application/json
```

Body: không có

**Đầu ra — mỗi dòng trong danh sách:**

| Trường | Ý nghĩa |
|---|---|
| `id` | Mã tài khoản, dùng làm khoá cho mọi lệnh sau đó |
| `name`, `email` | Hai thông tin nhận diện duy nhất được phép hiển thị |
| `roleId` | Mã vai trò hiện tại |
| `deletedAt` | Thời điểm vô hiệu hoá. Rỗng là đang hoạt động, có giá trị là đã vô hiệu hoá |
| `createdAt` | Ngày tham gia, tức thời điểm tạo tài khoản |
| `isSuperAdmin` | Có phải tài khoản superAdmin không |
| `allowedActions` | Danh sách lệnh được phép trên dòng này, gồm một phần của `CHANGE_ROLE`, `DISABLE`, `RESTORE`, `SELECT`. Máy chủ tính theo trạng thái tài khoản, cờ superAdmin và tài khoản của chính người xem. Giao diện dựng menu từ danh sách này, không tự suy ra |
| `currentRoleRevision` | Mốc phiên bản vai trò, số nguyên tăng 1 mỗi lần `roleId` đổi, dùng để phát hiện vai trò của tài khoản đã bị người khác đổi sau lúc tải |

Khi đang lọc theo một trạng thái cụ thể, phản hồi trả thêm `selectionEligibleCount`: **số tài khoản đủ điều kiện chọn** trên toàn bộ danh sách đã lọc, đã loại người đang thao tác và tài khoản superAdmin. Giao diện dùng số này để hiển thị "Đã chọn n tài khoản".

**Response mẫu — thành công (`200`):**

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "success": true,
  "message": "OK",
  "data": [
    {
      "id": "7f3c2a10-5b1e-4c8d-9a61-2e4f8b0c1d23",
      "name": "Huy Tran",
      "email": "huy@example.test",
      "roleId": "a1b2c3d4-0000-4000-8000-000000000001",
      "deletedAt": null,
      "createdAt": "2026-08-04T01:00:00.000Z",
      "isSuperAdmin": false,
      "allowedActions": ["CHANGE_ROLE", "DISABLE", "SELECT"],
      "currentRoleRevision": 7
    }
  ],
  "meta": {
    "type": "offset",
    "page": 1,
    "pageSize": 10,
    "total": 1,
    "totalPages": 1
  },
  "selectionEligibleCount": 1
}
```

**Mã lỗi:**

| Mã lỗi | HTTP | Khi nào |
|---|---|---|
| `VALIDATION_FAILED` | `400` | Tham số sai kiểu hoặc ngoài khoảng cho phép |
| `AUTH_TOKEN_MISSING` | `401` | Yêu cầu không kèm mã đăng nhập (token) |
| `AUTH_TOKEN_INVALID` | `401` | Mã đăng nhập đã hết hạn hoặc không hợp lệ |
| `PERMISSION_DENIED` | `403` | Thiếu quyền quản trị tài khoản |

**`VALIDATION_FAILED` (`400`):** request `GET /users?pageSize=25`

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Dữ liệu gửi lên không hợp lệ",
  "errorCode": "VALIDATION_FAILED",
  "details": [
    {
      "code": "custom",
      "path": ["pageSize"],
      "message": "pageSize phải là 10, 20, 50 hoặc 100"
    }
  ]
}
```

**`AUTH_TOKEN_MISSING` (`401`):** request `GET /users` không kèm header `Authorization`

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Chưa đăng nhập",
  "errorCode": "AUTH_TOKEN_MISSING"
}
```

**`AUTH_TOKEN_INVALID` (`401`):** request `GET /users` kèm mã đăng nhập đã hết hạn hoặc bị sửa

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Phiên đăng nhập không hợp lệ hoặc đã hết hạn",
  "errorCode": "AUTH_TOKEN_INVALID"
}
```

**`PERMISSION_DENIED` (`403`):** request `GET /users` kèm mã đăng nhập của tài khoản không có quyền quản trị tài khoản

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Bạn không có quyền thực hiện thao tác này",
  "errorCode": "PERMISSION_DENIED"
}
```

**Phục vụ màn hình:** `SCR-002`

### `POST /users/selection-previews` — Chốt danh sách tài khoản đã chọn

**Mục đích:** khi quản trị viên chọn nhiều tài khoản trải qua nhiều trang, endpoint này lưu **đúng** danh sách đó thành một bản cố định, để hộp thoại xác nhận và lệnh ghi sau đó làm việc trên cùng một danh sách.

**Đầu vào:**

| Trường | Quy tắc |
|---|---|
| `action` | `DISABLE` — vô hiệu hoá<br>`RESTORE` — khôi phục |
| `selection.mode` | `EXPLICIT` — quản trị viên chọn từng dòng<br>`FILTER` — quản trị viên bấm "chọn tất cả" theo điều kiện lọc |
| `selection.userIds` | Bắt buộc với `EXPLICIT`. Danh sách mã tài khoản, không rỗng, không trùng |
| `selection.query` | Bắt buộc với `FILTER`. Dùng đúng điều kiện lọc của `GET /users`, nhưng không nhận tham số phân trang |
| `selection.excludedUserIds` | Tuỳ chọn với `FILTER`. Những dòng quản trị viên bỏ chọn sau khi đã "chọn tất cả" |
| `pageSize` | Từ 1 đến 100. Chỉ điều khiển việc xem trước danh sách, **không** làm đổi danh sách đã chốt |

Hai ràng buộc theo cặp: chọn theo bộ lọc để vô hiệu hoá thì chỉ nhận tài khoản đang hoạt động; để khôi phục thì chỉ nhận tài khoản đã vô hiệu hoá.

**Request mẫu:** chọn tất cả tài khoản đang hoạt động có tên hoặc email chứa "an" để vô hiệu hoá, bỏ chọn một dòng

```http
POST /users/selection-previews HTTP/1.1
Host: <địa chỉ máy chủ>
Authorization: Bearer <mã đăng nhập>
Content-Type: application/json

{
  "action": "DISABLE",
  "selection": {
    "mode": "FILTER",
    "query": {
      "q": "an",
      "roleId": null,
      "status": "active",
      "sortOrder": "desc"
    },
    "excludedUserIds": ["5d1e8c44-2a7b-4f0e-b3c9-8e6a1f2d4b70"]
  },
  "pageSize": 100
}
```

**Đầu ra:**

| Trường | Ý nghĩa |
|---|---|
| `selectionToken` | Mã đại diện cho danh sách đã chốt. Có hạn **10 phút** và không chứa thông tin cá nhân đọc được |
| `targetCount` | Tổng số tài khoản trong danh sách, tính trên toàn bộ dữ liệu |
| `excludedCount` | Số tài khoản bị loại tự động |
| `targets` | Trang đầu của danh sách, chỉ gồm mã, tên và email |

Xử lý người đang thao tác và tài khoản superAdmin theo chế độ chọn:

| | Chọn theo bộ lọc | Chọn từng dòng |
|---|---|---|
| Người đang thao tác | **Luôn bị loại** | **Luôn bị chặn**, ở cả lệnh vô hiệu hoá lẫn khôi phục |
| Tài khoản superAdmin | **Luôn bị loại** | **Luôn bị chặn** |

**Response mẫu — thành công (`201`):**

```http
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8

{
  "success": true,
  "message": "OK",
  "data": {
    "selectionToken": "opaque-signed-token",
    "targetCount": 237,
    "excludedCount": 2,
    "targets": [
      {
        "id": "0191a42a-70f4-7c38-8af8-4c659b4761e1",
        "name": "An Nguyen",
        "email": "an@example.test"
      }
    ]
  }
}
```

**Mã lỗi:**

| Mã lỗi | HTTP | Khi nào |
|---|---|---|
| `VALIDATION_FAILED` | `400` | Thiếu trường bắt buộc, hoặc cặp chế độ và trạng thái không hợp lệ |
| `AUTH_TOKEN_MISSING` | `401` | Yêu cầu không kèm mã đăng nhập |
| `AUTH_TOKEN_INVALID` | `401` | Mã đăng nhập đã hết hạn hoặc không hợp lệ |
| `PERMISSION_DENIED` | `403` | Thiếu quyền quản trị tài khoản |
| `USER_ACCOUNT_PROTECTED` | `403` | Chọn trúng tài khoản superAdmin |
| `USER_SELF_MANAGEMENT_FORBIDDEN` | `403` | Chọn trúng chính mình |

**`VALIDATION_FAILED` (`400`):** request `POST /users/selection-previews`, body có `"mode": "EXPLICIT"` nhưng không có `userIds`

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Dữ liệu gửi lên không hợp lệ",
  "errorCode": "VALIDATION_FAILED",
  "details": [
    {
      "code": "invalid_type",
      "path": ["selection", "userIds"],
      "message": "Required"
    }
  ]
}
```

**`AUTH_TOKEN_MISSING` (`401`):** request `POST /users/selection-previews` không kèm header `Authorization`

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Chưa đăng nhập",
  "errorCode": "AUTH_TOKEN_MISSING"
}
```

**`AUTH_TOKEN_INVALID` (`401`):** request `POST /users/selection-previews` kèm mã đăng nhập đã hết hạn hoặc bị sửa

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Phiên đăng nhập không hợp lệ hoặc đã hết hạn",
  "errorCode": "AUTH_TOKEN_INVALID"
}
```

**`PERMISSION_DENIED` (`403`):** request `POST /users/selection-previews` kèm mã đăng nhập của tài khoản không có quyền quản trị tài khoản

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Bạn không có quyền thực hiện thao tác này",
  "errorCode": "PERMISSION_DENIED"
}
```

**`USER_ACCOUNT_PROTECTED` (`403`):** request `POST /users/selection-previews`, body có `"mode": "EXPLICIT"` và `userIds` chứa mã của tài khoản superAdmin

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Không thể thay đổi tài khoản super admin",
  "errorCode": "USER_ACCOUNT_PROTECTED"
}
```

**`USER_SELF_MANAGEMENT_FORBIDDEN` (`403`):** request `POST /users/selection-previews`, body có `"action": "DISABLE"`, `"mode": "EXPLICIT"` và `userIds` chứa mã tài khoản của chính người thao tác

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Không thể thay đổi quyền hoặc vô hiệu hoá chính tài khoản của bạn",
  "errorCode": "USER_SELF_MANAGEMENT_FORBIDDEN"
}
```

**Phục vụ màn hình:** `SCR-002` và hai hộp thoại xác nhận hàng loạt

### `GET /roles/assignable` — Lấy danh sách vai trò được phép gán

**Mục đích:** cung cấp đúng những vai trò mà quản trị viên được chọn, để giao diện không phải tự lọc.

**Đầu vào:**

| Tham số | Ràng buộc |
|---|---|
| `purpose` | `USER_ROLE_CHANGE` khi đổi vai trò cho tài khoản<br>`ARCHIVE_REPLACEMENT` khi chọn vai trò thay thế lúc ngừng dùng một vai trò |
| `excludeRoleId` | Tuỳ chọn. Với mục đích chọn vai trò thay thế, **bắt buộc** loại chính vai trò sắp ngừng dùng |

**Request mẫu:** lấy danh sách vai trò để đổi vai trò cho một tài khoản

```http
GET /roles/assignable?purpose=USER_ROLE_CHANGE HTTP/1.1
Host: <địa chỉ máy chủ>
Authorization: Bearer <mã đăng nhập>
Accept: application/json
```

Body: không có

**Đầu ra:** chỉ gồm vai trò đã công bố và chưa ngừng dùng. Danh sách **không bao giờ** có lựa chọn cấp quyền superAdmin.

| Trường | Ý nghĩa |
|---|---|
| `id` | Mã vai trò, gửi lên làm `roleId` khi đổi vai trò |
| `name` | Tên vai trò, hiển thị trong danh sách chọn |

Danh sách rỗng là lỗi cấu hình, không phải kết quả bình thường; giao diện xử lý như lỗi tải danh mục.

**Response mẫu — thành công (`200`):**

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "success": true,
  "message": "OK",
  "data": [
    { "id": "a1b2c3d4-0000-4000-8000-000000000001", "name": "Người dùng" },
    { "id": "a1b2c3d4-0000-4000-8000-000000000002", "name": "Nhà tổ chức" },
    { "id": "a1b2c3d4-0000-4000-8000-000000000003", "name": "Quản trị viên" },
    { "id": "a1b2c3d4-0000-4000-8000-000000000010", "name": "Kiểm duyệt viên" }
  ]
}
```

**Mã lỗi:**

| Mã lỗi | HTTP | Khi nào |
|---|---|---|
| `VALIDATION_FAILED` | `400` | Thiếu hoặc sai mục đích |
| `AUTH_TOKEN_MISSING` | `401` | Yêu cầu không kèm mã đăng nhập |
| `AUTH_TOKEN_INVALID` | `401` | Mã đăng nhập đã hết hạn hoặc không hợp lệ |
| `PERMISSION_DENIED` | `403` | Thiếu quyền quản trị tài khoản |

**`VALIDATION_FAILED` (`400`):** request `GET /roles/assignable`, thiếu `?purpose=…`

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Dữ liệu gửi lên không hợp lệ",
  "errorCode": "VALIDATION_FAILED",
  "details": [
    {
      "code": "invalid_type",
      "path": ["purpose"],
      "message": "Required"
    }
  ]
}
```

**`AUTH_TOKEN_MISSING` (`401`):** request `GET /roles/assignable?purpose=USER_ROLE_CHANGE` không kèm header `Authorization`

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Chưa đăng nhập",
  "errorCode": "AUTH_TOKEN_MISSING"
}
```

**`AUTH_TOKEN_INVALID` (`401`):** request `GET /roles/assignable?purpose=USER_ROLE_CHANGE` kèm mã đăng nhập đã hết hạn hoặc bị sửa

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Phiên đăng nhập không hợp lệ hoặc đã hết hạn",
  "errorCode": "AUTH_TOKEN_INVALID"
}
```

**`PERMISSION_DENIED` (`403`):** request `GET /roles/assignable?purpose=USER_ROLE_CHANGE` kèm mã đăng nhập của tài khoản không có quyền quản trị tài khoản

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Bạn không có quyền thực hiện thao tác này",
  "errorCode": "PERMISSION_DENIED"
}
```

**Phục vụ màn hình:** `SCR-003` và hộp thoại ngừng dùng vai trò

### `PATCH /users/{id}/role` — Đổi vai trò cho một tài khoản

**Mục đích:** ghi vai trò mới cho đúng một tài khoản.

**Đầu vào:**

| Trường | Bắt buộc | Ràng buộc |
|---|---|---|
| `roleId` | Có | Vai trò mới. Phải đã công bố, đang dùng, và không phải lựa chọn cấp quyền superAdmin |
| `currentRoleRevision` | Có | Mốc phiên bản vai trò **đã nhận** lúc mở hộp thoại |
| `reason` | Có | 20–500 ký tự sau khi bỏ khoảng trắng đầu cuối |

**Điều kiện với tài khoản đích:** phải tồn tại, đang hoạt động, không phải chính người đang thao tác, và không phải tài khoản superAdmin.

**Thứ tự kiểm tra:** sau khi qua kiểm tra đăng nhập, quyền và dữ liệu gửi lên, máy chủ kiểm tra lần lượt: tài khoản đích tồn tại → không phải chính người đang thao tác → không phải tài khoản superAdmin → đang hoạt động → vai mới gán được → mốc phiên bản vai trò còn khớp. Điều kiện đầu tiên không đạt quyết định mã lỗi trả về. Qua hết các điều kiện mới xét kết quả là `CHANGED` hay `NO_CHANGE`.

**Request mẫu:** đổi tài khoản đích từ Người dùng sang Quản trị viên

```http
PATCH /users/7f3c2a10-5b1e-4c8d-9a61-2e4f8b0c1d23/role HTTP/1.1
Host: <địa chỉ máy chủ>
Authorization: Bearer <mã đăng nhập>
Content-Type: application/json

{
  "roleId": "a1b2c3d4-0000-4000-8000-000000000003",
  "currentRoleRevision": 7,
  "reason": "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới."
}
```

**Đầu ra:**

| Trường | Ý nghĩa |
|---|---|
| `outcome` | `CHANGED` — đã đổi<br>`NO_CHANGE` — vai trò mới trùng vai trò đang có |
| `affectedCount` | Số tài khoản bị tác động. Với `NO_CHANGE` thì bằng 0 |
| `effectiveAt` | Thời điểm ghi nhận |

Khi trả `NO_CHANGE`, hệ thống **không** ghi như một thay đổi thật và **không** gửi thông báo cho ai.

Việc ghi vai trò mới và đồng bộ mọi nơi lưu vai trò diễn ra trong **một giao dịch**. Lỗi ở bất kỳ bước nào thì hoàn tác toàn bộ.

**Response mẫu — thành công (`200`), đã đổi:**

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "success": true,
  "message": "OK",
  "data": {
    "outcome": "CHANGED",
    "affectedCount": 1,
    "effectiveAt": "2026-09-01T04:00:00.000Z"
  }
}
```

**Response mẫu — thành công (`200`), vai trò mới trùng vai trò đang có:**

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "success": true,
  "message": "OK",
  "data": {
    "outcome": "NO_CHANGE",
    "affectedCount": 0,
    "effectiveAt": "2026-09-01T04:00:00.000Z"
  }
}
```

**Mã lỗi:**

| Mã lỗi | HTTP | Khi nào |
|---|---|---|
| `VALIDATION_FAILED` | `400` | Thiếu trường, lý do ngoài khoảng 20–500, hoặc gửi kèm trường không có trong đặc tả, ví dụ trường cấp quyền superAdmin |
| `AUTH_TOKEN_MISSING` | `401` | Yêu cầu không kèm mã đăng nhập |
| `AUTH_TOKEN_INVALID` | `401` | Mã đăng nhập đã hết hạn hoặc không hợp lệ |
| `PERMISSION_DENIED` | `403` | Người thao tác không có quyền quản trị tài khoản |
| `USER_SELF_MANAGEMENT_FORBIDDEN` | `403` | Tài khoản đích là chính người đang thao tác |
| `USER_ACCOUNT_PROTECTED` | `403` | Tài khoản đích là tài khoản superAdmin |
| `NOT_FOUND` | `404` | Tài khoản đích không tồn tại, hoặc mã vai trò mới không tồn tại |
| `USER_TARGET_DISABLED` | `409` | Tài khoản đích đã bị vô hiệu hoá. Phải khôi phục trước khi đổi vai trò |
| `USER_ROLE_UNAVAILABLE` | `409` | Vai trò mới đã chuyển thành nháp hoặc đã ngừng dùng |
| `USER_ROLE_STALE` | `409` | Vai trò của tài khoản đã bị người khác đổi sau lúc mở hộp thoại |
| `M03_COMMAND_FAILED_SAFE` | `500` | Lỗi trong lúc ghi, nhưng máy chủ **xác nhận đã hoàn tác toàn bộ** |

**`VALIDATION_FAILED` (`400`):** request `PATCH /users/{id}/role`, body có `reason` dài 19 ký tự

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Dữ liệu gửi lên không hợp lệ",
  "errorCode": "VALIDATION_FAILED",
  "details": [
    {
      "code": "too_small",
      "path": ["reason"],
      "message": "Lý do phải từ 20 đến 500 ký tự"
    }
  ]
}
```

**`AUTH_TOKEN_MISSING` (`401`):** request `PATCH /users/{id}/role` không kèm header `Authorization`

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Chưa đăng nhập",
  "errorCode": "AUTH_TOKEN_MISSING"
}
```

**`AUTH_TOKEN_INVALID` (`401`):** request `PATCH /users/{id}/role` kèm mã đăng nhập đã hết hạn hoặc bị sửa

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Phiên đăng nhập không hợp lệ hoặc đã hết hạn",
  "errorCode": "AUTH_TOKEN_INVALID"
}
```

**`PERMISSION_DENIED` (`403`):** request `PATCH /users/{id}/role` kèm mã đăng nhập của tài khoản không có quyền quản trị tài khoản

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Bạn không có quyền thực hiện thao tác này",
  "errorCode": "PERMISSION_DENIED"
}
```

**`USER_SELF_MANAGEMENT_FORBIDDEN` (`403`):** request `PATCH /users/{id}/role`, `{id}` là mã tài khoản của chính người thao tác

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Không thể thay đổi quyền hoặc vô hiệu hoá chính tài khoản của bạn",
  "errorCode": "USER_SELF_MANAGEMENT_FORBIDDEN"
}
```

**`USER_ACCOUNT_PROTECTED` (`403`):** request `PATCH /users/{id}/role`, `{id}` là mã của tài khoản superAdmin

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Không thể thay đổi tài khoản super admin",
  "errorCode": "USER_ACCOUNT_PROTECTED"
}
```

**`NOT_FOUND` (`404`):** request `PATCH /users/{id}/role`, `{id}` đúng định dạng nhưng không có tài khoản nào mang mã này

```http
HTTP/1.1 404 Not Found
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Không tìm thấy tài khoản",
  "errorCode": "NOT_FOUND"
}
```

**`USER_TARGET_DISABLED` (`409`):** request `PATCH /users/{id}/role`, `{id}` là mã của tài khoản đã bị vô hiệu hoá

```http
HTTP/1.1 409 Conflict
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Tài khoản đã bị vô hiệu hoá — khôi phục trước khi đổi vai",
  "errorCode": "USER_TARGET_DISABLED"
}
```

**`USER_ROLE_UNAVAILABLE` (`409`):** request `PATCH /users/{id}/role`, body có `roleId` là vai trò vừa chuyển về nháp

```http
HTTP/1.1 409 Conflict
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Vai trò đã chọn không còn khả dụng",
  "errorCode": "USER_ROLE_UNAVAILABLE",
  "details": {
    "roleId": "a1b2c3d4-0000-4000-8000-000000000010",
    "unavailableReason": "draft"
  }
}
```

**`USER_ROLE_STALE` (`409`):** request `PATCH /users/{id}/role`, body có `currentRoleRevision` cũ vì người khác đã đổi vai trò của tài khoản đích sau lúc mở hộp thoại

```http
HTTP/1.1 409 Conflict
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Vai trò của tài khoản vừa được thay đổi",
  "errorCode": "USER_ROLE_STALE",
  "details": {
    "actualCurrentRoleId": "a1b2c3d4-0000-4000-8000-000000000010",
    "currentRoleRevision": 8
  }
}
```

**`M03_COMMAND_FAILED_SAFE` (`500`):** request `PATCH /users/{id}/role` hợp lệ, máy chủ lỗi khi ghi và đã hoàn tác toàn bộ

```http
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8

{
  "success": false,
  "data": null,
  "message": "Không thể thay đổi vai trò",
  "errorCode": "M03_COMMAND_FAILED_SAFE",
  "details": {
    "commitStatus": "NOT_COMMITTED",
    "retryable": true
  },
  "traceId": "K7Q2M9XA"
}
```

**Phục vụ màn hình:** `SCR-004`

## 3. Danh mục mã lỗi

| Mã lỗi | HTTP | Nghĩa |
|---|---|---|
| `VALIDATION_FAILED` | `400` | Dữ liệu gửi lên sai kiểu, sai định dạng hoặc thiếu trường bắt buộc |
| `AUTH_TOKEN_MISSING`, `AUTH_TOKEN_INVALID` | `401` | Chưa đăng nhập, hoặc mã đăng nhập không còn hợp lệ |
| `PERMISSION_DENIED` | `403` | Đã đăng nhập nhưng **không có quyền** quản trị tài khoản |
| `USER_SELF_MANAGEMENT_FORBIDDEN` | `403` | Có quyền, nhưng **tài khoản đích là chính mình** |
| `USER_ACCOUNT_PROTECTED` | `403` | Có quyền, nhưng **tài khoản đích là tài khoản superAdmin** |
| `NOT_FOUND` | `404` | Tài khoản đích hoặc vai trò không tồn tại |
| `USER_TARGET_DISABLED` | `409` | Tài khoản đích đã bị vô hiệu hoá |
| `USER_ROLE_UNAVAILABLE` | `409` | Vai trò mới không còn gán được |
| `USER_ROLE_STALE` | `409` | Thông tin vai trò mà quản trị viên đã xem nay đã cũ |
| `M03_COMMAND_FAILED_SAFE` | `500` | Ghi thất bại nhưng đã hoàn tác toàn bộ, có thể thử lại |
| `INTERNAL_ERROR` | `500` | Lỗi ngoài dự kiến |
