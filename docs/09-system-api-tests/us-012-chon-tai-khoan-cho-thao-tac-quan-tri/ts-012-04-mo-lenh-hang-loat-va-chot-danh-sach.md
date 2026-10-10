# `TS-012-04` — Mở lệnh hàng loạt và chốt danh sách tài khoản đích

**Liên quan:** [danh sách scenario của `US-012`](README.md), [user story `US-012`](../../06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** thanh hành động mở đúng hộp thoại xác nhận với đúng danh sách đã chọn, và máy chủ chốt danh sách theo đúng quy tắc loại trừ, kiểm tra dữ liệu và xác thực.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-012-04-01` | Mở hộp thoại xác nhận vô hiệu hoá với đúng danh sách đã chọn | Positive | High | | `US012-SC-06` |
| `TC-012-04-02` | Mở hộp thoại xác nhận khôi phục với đúng danh sách đã chọn | Positive | High | | `US012-SC-06` |
| `TC-012-04-03` | Chốt danh sách theo bộ lọc tự loại người thao tác và superAdmin | Positive | High | | `US012-SC-05`, `US012-SC-06` |
| `TC-012-04-04` | Từ chối chốt danh sách chọn từng dòng có tài khoản được bảo vệ | Negative | High | | `US012-SC-03`, `US012-SC-12` |
| `TC-012-04-05` | Từ chối yêu cầu chốt danh sách có dữ liệu không hợp lệ | Negative | Medium | | Đặc tả API |
| `TC-012-04-06` | Kiểm tra giá trị biên của số dòng xem trước | Boundary | Low | | Đặc tả API |
| `TC-012-04-07` | Từ chối yêu cầu chốt danh sách khi thiếu quyền hoặc mã đăng nhập không hợp lệ | Negative | High | | Đặc tả API |

## `TC-012-04-01` — Mở hộp thoại xác nhận vô hiệu hoá với đúng danh sách đã chọn

**Precondition:** Dữ liệu nền và hai tài khoản trong Test data. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "acc30", chọn trạng thái "Đang hoạt động".
2. Chọn ACC-301 và ACC-302 bằng ô chọn trên từng dòng.
3. Xoá danh sách request trong tab Network.
4. Bấm "Vô hiệu hoá" trên thanh hành động.
5. Quan sát hộp thoại, tab Network và trạng thái hai tài khoản trong bảng `users`.

**Test data:** ACC-301, ACC-302, email `acc301@example.test`, `acc302@example.test`, Đang hoạt động.

**Expected result:**

- Tab Network có đúng một request `POST /users/selection-previews` với `action: DISABLE`, `selection.mode: EXPLICIT`, `selection.userIds` gồm đúng mã của ACC-301 và ACC-302; phản hồi `201` với `targetCount: 2`.
- Hộp thoại xác nhận vô hiệu hoá mở ra với danh sách gồm đúng ACC-301 và ACC-302.
- Không có request vô hiệu hoá nào được gửi; `users.deletedAt` của hai tài khoản vẫn rỗng.

**Actual result:**

**Bug:**

## `TC-012-04-02` — Mở hộp thoại xác nhận khôi phục với đúng danh sách đã chọn

**Precondition:** Dữ liệu nền và hai tài khoản trong Test data. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "acc31", chọn trạng thái "Đã vô hiệu hoá".
2. Chọn ACC-311 và ACC-312 bằng ô chọn trên từng dòng.
3. Xoá danh sách request trong tab Network.
4. Bấm "Khôi phục" trên thanh hành động.
5. Quan sát hộp thoại, tab Network và trạng thái hai tài khoản trong bảng `users`.

**Test data:** ACC-311, ACC-312, email `acc311@example.test`, `acc312@example.test`, Đã vô hiệu hoá.

**Expected result:**

- Tab Network có đúng một request `POST /users/selection-previews` với `action: RESTORE`, `selection.mode: EXPLICIT`, `selection.userIds` gồm đúng mã của ACC-311 và ACC-312; phản hồi `201` với `targetCount: 2`.
- Hộp thoại xác nhận khôi phục mở ra với danh sách gồm đúng ACC-311 và ACC-312.
- Không có request khôi phục nào được gửi; `users.deletedAt` của hai tài khoản giữ nguyên giá trị.

**Actual result:**

**Bug:**

## `TC-012-04-03` — Chốt danh sách theo bộ lọc tự loại người thao tác và superAdmin

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và [`DS-04`](../00-moi-truong-va-du-lieu.md#ds-04--tài-khoản-cho-chọn-nhiều-dòng); cả 23 tài khoản đang hoạt động.

**Steps:**

1. Gửi `POST {{base_url}}/users/selection-previews` với `{{token_admin}}` và body theo dòng dữ liệu.
2. Kiểm tra phản hồi.
3. Kiểm tra trạng thái của 23 tài khoản trong bảng `users`.

**Test data:** Body dùng chung:

```json
{
  "action": "DISABLE",
  "selection": {
    "mode": "FILTER",
    "query": { "status": "active", "sortOrder": "desc" },
    "excludedUserIds": []
  },
  "pageSize": 100
}
```

| Dòng | `excludedUserIds` | `targetCount` | `excludedCount` |
|---|---|---|---|
| a | Mảng rỗng | 21 | 2 |
| b | Mã của ACC-003 | 20 | 2 |

**Expected result:**

- Phản hồi `201`, `success: true`; `targetCount` và `excludedCount` khớp với dòng dữ liệu.
- `targets` không chứa mã của ACC-ADMIN và ACC-ROOT; dòng b không chứa mã của ACC-003.
- Mỗi phần tử trong `targets` chỉ gồm `id`, `name`, `email`.
- `selectionToken` có giá trị và không chứa email hoặc tên của tài khoản nào.
- Trạng thái của 23 tài khoản không đổi.

**Actual result:**

**Bug:**

## `TC-012-04-04` — Từ chối chốt danh sách chọn từng dòng có tài khoản được bảo vệ

**Precondition:** Dữ liệu nền. Với dòng b, `users.deletedAt` của ACC-ADMIN được cập nhật thành thời điểm hiện tại; `{{token_admin}}` vẫn còn hạn.

**Steps:**

1. Gửi `POST {{base_url}}/users/selection-previews` với `{{token_admin}}`, `selection.mode: EXPLICIT`, `pageSize: 100`, `action` và `selection.userIds` theo dòng dữ liệu.
2. Kiểm tra phản hồi và trạng thái các tài khoản trong `userIds`.

**Test data:**

| Dòng | `action` | `selection.userIds` | Mã lỗi mong đợi |
|---|---|---|---|
| a | `DISABLE` | `{{id_admin}}` | `403 USER_SELF_MANAGEMENT_FORBIDDEN` |
| b | `RESTORE` | `{{id_admin}}` | `403 USER_SELF_MANAGEMENT_FORBIDDEN` |
| c | `DISABLE` | `{{id_root}}` | `403 USER_ACCOUNT_PROTECTED` |
| d | `DISABLE` | `{{id_target}}`, `{{id_root}}` | `403 USER_ACCOUNT_PROTECTED` |

**Expected result:**

- Mỗi dòng trả đúng mã lỗi mong đợi, `data: null`, không có `selectionToken`.
- Dòng d bị từ chối toàn bộ; không có danh sách nào được chốt cho ACC-TARGET.
- Trạng thái của mọi tài khoản trong `userIds` không đổi.

**Actual result:**

**Bug:**

## `TC-012-04-05` — Từ chối yêu cầu chốt danh sách có dữ liệu không hợp lệ

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `POST {{base_url}}/users/selection-previews` với `{{token_admin}}` và body theo dòng dữ liệu; các trường không nêu trong dòng dữ liệu giữ như body hợp lệ `action: DISABLE`, `selection.mode: EXPLICIT`, `selection.userIds: [{{id_target}}]`, `pageSize: 100`.

**Test data:**

| Dòng | Thay đổi so với body hợp lệ |
|---|---|
| a | Bỏ trường `selection.userIds` |
| b | `selection.userIds` là mảng rỗng |
| c | `selection.userIds` là `[{{id_target}}, {{id_target}}]` |
| d | `selection.mode: FILTER`, không có `selection.query` |
| e | `selection.mode: FILTER`, `action: DISABLE`, `selection.query.status: disabled` |
| f | `selection.mode: FILTER`, `action: RESTORE`, `selection.query.status: active` |
| g | `action: DELETE` |

**Expected result:**

- Mỗi dòng trả `400`, `errorCode: VALIDATION_FAILED`, `data: null`, không có `selectionToken`.
- Dòng a: `details` có phần tử với `path` là `["selection", "userIds"]`.

**Actual result:**

**Bug:**

## `TC-012-04-06` — Kiểm tra giá trị biên của số dòng xem trước

**Precondition:** Dữ liệu nền. Có bốn tài khoản đang hoạt động không phải ACC-ADMIN và không phải ACC-ROOT.

**Steps:**

1. Gửi `POST {{base_url}}/users/selection-previews` với `{{token_admin}}`, `action: DISABLE`, `selection.mode: FILTER`, `selection.query.status: active` và `pageSize` theo dòng dữ liệu.

**Test data:**

| Dòng | `pageSize` | Kết quả |
|---|---|---|
| a | 0 | Từ chối |
| b | 1 | Chấp nhận |
| c | 100 | Chấp nhận |
| d | 101 | Từ chối |

**Expected result:**

- Dòng a và d: phản hồi `400`, `errorCode: VALIDATION_FAILED`, `data: null`.
- Dòng b: phản hồi `201`, `targetCount: 4`, `targets` có 1 phần tử.
- Dòng c: phản hồi `201`, `targetCount: 4`, `targets` có 4 phần tử.

**Actual result:**

**Bug:**

## `TC-012-04-07` — Từ chối yêu cầu chốt danh sách khi thiếu quyền hoặc mã đăng nhập không hợp lệ

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `POST {{base_url}}/users/selection-previews` với body hợp lệ `action: DISABLE`, `selection.mode: EXPLICIT`, `selection.userIds: [{{id_target}}]`, `pageSize: 100` và header `Authorization` theo dòng dữ liệu.

**Test data:**

| Dòng | Header `Authorization` | Mã lỗi mong đợi |
|---|---|---|
| a | Không gửi header | `401 AUTH_TOKEN_MISSING` |
| b | `{{token_expired}}` | `401 AUTH_TOKEN_INVALID` |
| c | `{{token_tampered}}` | `401 AUTH_TOKEN_INVALID` |
| d | `{{token_noperm}}` | `403 PERMISSION_DENIED` |

**Expected result:**

- Mỗi dòng trả đúng mã lỗi mong đợi, `data: null`, không có `selectionToken`.
- Trạng thái của ACC-TARGET không đổi.

**Actual result:**

**Bug:**
