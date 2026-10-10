# `TS-013-07` — Phiên đăng nhập và xác thực

**Liên quan:** [danh sách scenario của `US-013`](README.md), [user story `US-013`](../../06-user-stories/us-013-doi-vai-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** khi phiên đăng nhập không hợp lệ hoặc tài khoản thiếu quyền, máy chủ từ chối trước khi đọc hoặc ghi dữ liệu, và giao diện chuyển người dùng về trang đăng nhập khi phiên hết hạn.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-013-07-01` | Chuyển về trang đăng nhập khi phiên hết hạn lúc xác nhận đổi vai trò | Negative | High | | `US013-SC-18` |
| `TC-013-07-02` | Chuyển về trang đăng nhập khi phiên hết hạn lúc tải danh mục vai trò | Negative | High | | `US013-SC-19` |
| `TC-013-07-03` | Máy chủ từ chối lấy danh mục vai trò và đổi vai trò khi thiếu hoặc sai mã đăng nhập | Negative | High | | `US013-SC-18`, `US013-SC-19` |
| `TC-013-07-04` | Máy chủ từ chối lấy danh mục vai trò và đổi vai trò khi thiếu quyền quản trị tài khoản | Negative | High | | Đặc tả API |

## `TC-013-07-01` — Chuyển về trang đăng nhập khi phiên hết hạn lúc xác nhận đổi vai trò

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập. Ghi lại số bản ghi trong `user_action_logs`.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET, chọn "Quản trị viên", nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", bấm "Tiếp tục".
2. Thay mã đăng nhập trình duyệt đang lưu bằng `{{token_expired}}`.
3. Bấm "Xác nhận", quan sát màn hình và phản hồi trong tab Network.
4. Kiểm tra vai trò của ACC-TARGET và số bản ghi trong `user_action_logs`.

**Test data:** `{{token_expired}}`.

**Expected result:**

- Phản hồi `401`, `errorCode: AUTH_TOKEN_INVALID`.
- Hai hộp thoại đóng; hệ thống chuyển thẳng về trang đăng nhập; trang đăng nhập hiển thị "Phiên đăng nhập đã hết hạn" và "Vui lòng đăng nhập lại.".
- Không hiển thị trạng thái không đủ quyền, thất bại hay không xác định được kết quả.
- Vai trò của ACC-TARGET không đổi. Không có bản ghi nhật ký mới.

**Actual result:**

**Bug:**

## `TC-013-07-02` — Chuyển về trang đăng nhập khi phiên hết hạn lúc tải danh mục vai trò

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET, chờ danh mục vai trò tải xong, sau đó bấm "Huỷ".
2. Thay mã đăng nhập trình duyệt đang lưu bằng `{{token_expired}}`.
3. Mở lại hộp thoại đổi vai trò cho ACC-TARGET.
4. Quan sát màn hình và phản hồi trong tab Network.

**Test data:** `{{token_expired}}`.

**Expected result:**

- Phản hồi của `GET /roles/assignable` là `401`, `errorCode: AUTH_TOKEN_INVALID`.
- Hộp thoại không hiển thị danh sách vai trò và không dùng danh mục đã tải ở bước 1.
- Hộp thoại đóng; hệ thống chuyển thẳng về trang đăng nhập; trang đăng nhập hiển thị "Phiên đăng nhập đã hết hạn" và "Vui lòng đăng nhập lại.".

**Actual result:**

**Bug:**

## `TC-013-07-03` — Máy chủ từ chối lấy danh mục vai trò và đổi vai trò khi thiếu hoặc sai mã đăng nhập

**Precondition:** Dữ liệu nền. Đọc `{{rev_target}}`. Ghi lại số bản ghi trong `user_action_logs`.

**Steps:**

1. Với mỗi dòng dữ liệu, gửi `GET {{base_url}}/roles/assignable?purpose=USER_ROLE_CHANGE` với header `Authorization` của dòng.
2. Với mỗi dòng dữ liệu, gửi `PATCH {{base_url}}/users/{{id_target}}/role` với body đổi vai trò chuẩn và header `Authorization` của dòng.
3. Kiểm tra vai trò của ACC-TARGET và số bản ghi trong `user_action_logs`.

**Test data:**

| Dòng | Header `Authorization` | Mã lỗi mong đợi |
|---|---|---|
| a | Không gửi header | `401 AUTH_TOKEN_MISSING` |
| b | `{{token_expired}}` | `401 AUTH_TOKEN_INVALID` |
| c | `{{token_tampered}}` | `401 AUTH_TOKEN_INVALID` |
| d | `{{token_noperm_expired}}` | `401 AUTH_TOKEN_INVALID` |

**Expected result:**

- Mỗi request trả đúng mã lỗi mong đợi, `data: null`; không request nào trả `403`.
- Vai trò của ACC-TARGET không đổi. Không có bản ghi nhật ký mới.

**Actual result:**

**Bug:**

## `TC-013-07-04` — Máy chủ từ chối lấy danh mục vai trò và đổi vai trò khi thiếu quyền quản trị tài khoản

**Precondition:** Dữ liệu nền. Đọc `{{rev_target}}`.

**Steps:**

1. Gửi `GET {{base_url}}/roles/assignable?purpose=USER_ROLE_CHANGE` với `{{token_noperm}}`.
2. Gửi `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_noperm}}` và body đổi vai trò chuẩn.
3. Kiểm tra vai trò của ACC-TARGET.

**Test data:** `{{token_noperm}}`.

**Expected result:**

- Cả hai request trả `403`, `errorCode: PERMISSION_DENIED`, `data: null`.
- Vai trò của ACC-TARGET không đổi.

**Actual result:**

**Bug:**
