# `TS-013-04` — Từ chối theo quy tắc bảo vệ

**Liên quan:** [danh sách scenario của `US-013`](README.md), [user story `US-013`](../../06-user-stories/us-013-doi-vai-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** máy chủ kiểm tra lại toàn bộ điều kiện bảo vệ tại thời điểm ghi, từ chối đúng nguyên nhân, và không thay đổi dữ liệu khi tài khoản đích hoặc vai trò mới không hợp lệ.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-013-04-01` | Từ chối đổi vai trò cho chính người thao tác | Negative | High | | `US013-SC-08` |
| `TC-013-04-02` | Từ chối đổi vai trò cho tài khoản superAdmin | Negative | High | | `US013-SC-08` |
| `TC-013-04-03` | Báo đúng nguyên nhân khi tài khoản đích bị vô hiệu hoá sau khi mở hộp thoại | Negative | High | | `US013-SC-09` |
| `TC-013-04-04` | Máy chủ phân biệt tài khoản đích đã vô hiệu hoá với tài khoản không tồn tại | Negative | High | | `US013-SC-09` |
| `TC-013-04-05` | Từ chối vai trò mới đã chuyển về nháp hoặc ngừng dùng sau khi mở hộp xác nhận | Negative | High | | `US013-SC-10` |
| `TC-013-04-06` | Máy chủ từ chối vai trò mới không còn khả dụng hoặc không tồn tại | Negative | High | | `US013-SC-10` |
| `TC-013-04-07` | Từ chối yêu cầu kèm trường không có trong đặc tả | Negative | High | | `US013-SC-20` |
| `TC-013-04-08` | Mã lỗi theo đúng thứ tự kiểm tra khi nhiều điều kiện cùng không đạt | Negative | High | | `US013-SC-08`, `US013-SC-09` |

## `TC-013-04-01` — Từ chối đổi vai trò cho chính người thao tác

**Precondition:** Dữ liệu nền. Đọc `{{rev_admin}}`.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/{{id_admin}}/role` với `{{token_admin}}`, body đổi vai trò chuẩn với `roleId: {{role_user}}` và `currentRoleRevision: {{rev_admin}}`.
2. Kiểm tra vai trò của ACC-ADMIN và bản ghi mới nhất trong `user_action_logs` có `entityId` là `{{id_admin}}`.

**Test data:** Tài khoản đích ACC-ADMIN, vai trò mới Người dùng.

**Expected result:**

- Phản hồi `403`, `errorCode: USER_SELF_MANAGEMENT_FORBIDDEN`, `message: "Không thể thay đổi quyền hoặc vô hiệu hoá chính tài khoản của bạn"`, `data: null`.
- Vai trò của ACC-ADMIN không đổi.
- Bản ghi nhật ký của lệnh có `result: rejected`. Không gửi thông báo.

**Actual result:**

**Bug:**

## `TC-013-04-02` — Từ chối đổi vai trò cho tài khoản superAdmin

**Precondition:** Dữ liệu nền. Đọc `{{rev_root}}`.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/{{id_root}}/role` với `{{token_admin}}`, body đổi vai trò chuẩn với `roleId: {{role_user}}` và `currentRoleRevision: {{rev_root}}`.
2. Kiểm tra vai trò và cờ superAdmin của ACC-ROOT.

**Test data:** Tài khoản đích ACC-ROOT, vai trò mới Người dùng.

**Expected result:**

- Phản hồi `403`, `errorCode: USER_ACCOUNT_PROTECTED`, `message: "Không thể thay đổi tài khoản super admin"`, `data: null`.
- Vai trò của ACC-ROOT không đổi; `isSuperAdmin` vẫn là `true`.
- Không gửi thông báo.

**Actual result:**

**Bug:**

## `TC-013-04-03` — Báo đúng nguyên nhân khi tài khoản đích bị vô hiệu hoá sau khi mở hộp thoại

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET, chọn "Quản trị viên", nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", bấm "Tiếp tục".
2. Cập nhật `users.deletedAt` của ACC-TARGET thành thời điểm hiện tại.
3. Bấm "Xác nhận", quan sát hộp thoại.
4. Bấm "Đóng và tải lại".

**Test data:** ACC-TARGET chuyển sang trạng thái đã vô hiệu hoá sau khi hộp xác nhận được mở.

**Expected result:**

- Sau bước 3: hộp thoại có tiêu đề "Không thể thay đổi vai trò"; khung đỏ có tiêu đề "Tài khoản hiện đang bị vô hiệu hóa" và nội dung "Tài khoản này đã bị vô hiệu hoá. Vui lòng tải lại hoặc khôi phục tài khoản trước khi đổi vai trò."; chỉ có nút "Đóng và tải lại"; lý do được ẩn.
- Hộp thoại không hiển thị "Không tìm thấy tài khoản".
- Vai trò của ACC-TARGET không đổi. Không gửi thông báo.
- Sau bước 4: hai hộp thoại đóng; danh sách tải lại; dòng ACC-TARGET hiển thị trạng thái "Đã vô hiệu hoá".

**Actual result:**

**Bug:**

## `TC-013-04-04` — Máy chủ phân biệt tài khoản đích đã vô hiệu hoá với tài khoản không tồn tại

**Precondition:** Dữ liệu nền. Đọc `{{rev_disabled}}`.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/<tài khoản đích>/role` với `{{token_admin}}`, body đổi vai trò chuẩn với `currentRoleRevision: {{rev_disabled}}`, theo dòng dữ liệu.
2. Kiểm tra trạng thái và vai trò của ACC-DISABLED.

**Test data:**

| Dòng | Tài khoản đích | Mã lỗi mong đợi |
|---|---|---|
| a | `{{id_disabled}}` | `409 USER_TARGET_DISABLED` |
| b | `{{id_missing}}` | `404 NOT_FOUND` |

**Expected result:**

- Mỗi dòng trả đúng mã lỗi mong đợi, `data: null`.
- Dòng a không trả `NOT_FOUND`; dòng b không trả `USER_TARGET_DISABLED`.
- ACC-DISABLED vẫn ở trạng thái đã vô hiệu hoá; vai trò không đổi.

**Actual result:**

**Bug:**

## `TC-013-04-05` — Từ chối vai trò mới đã chuyển về nháp hoặc ngừng dùng sau khi mở hộp xác nhận

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET, chọn "Kiểm duyệt viên", nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", bấm "Tiếp tục".
2. Cập nhật ROLE-MOD theo dòng dữ liệu.
3. Bấm "Xác nhận", quan sát hộp thoại.
4. Bấm "Quay lại", quan sát hộp thoại đổi vai trò.

**Test data:**

| Dòng | Cập nhật ROLE-MOD |
|---|---|
| a | `roles.status` chuyển thành `draft` |
| b | `roles.deletedAt` cập nhật thành thời điểm hiện tại |

**Expected result:**

- Sau bước 3: hộp thoại có tiêu đề "Không thể thay đổi vai trò"; khung đỏ có tiêu đề "Vai trò đã chọn không còn khả dụng" và nội dung "Hãy quay lại và chọn một vai trò khác."; chỉ có nút "Quay lại".
- Vai trò của ACC-TARGET vẫn là "Người dùng". Không gửi thông báo.
- Sau bước 4: hộp thoại đổi vai trò hiển thị lại, lý do đã nhập được giữ; danh mục vai trò được tải lại; "Kiểm duyệt viên" không còn trong danh sách và không được chọn; nút "Tiếp tục" bị tắt.

**Actual result:**

**Bug:**

## `TC-013-04-06` — Máy chủ từ chối vai trò mới không còn khả dụng hoặc không tồn tại

**Precondition:** Dữ liệu nền. Đọc `{{rev_target}}`.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_admin}}` và body đổi vai trò chuẩn, thay `roleId` theo dòng dữ liệu.
2. Kiểm tra vai trò của ACC-TARGET.

**Test data:**

| Dòng | `roleId` | Mã lỗi mong đợi |
|---|---|---|
| a | `{{role_draft}}` | `409 USER_ROLE_UNAVAILABLE`, `details.unavailableReason: draft` |
| b | `{{role_archived}}` | `409 USER_ROLE_UNAVAILABLE` |
| c | `00000000-0000-4000-8000-000000000001`, không thuộc vai trò nào | `404 NOT_FOUND` |

**Expected result:**

- Mỗi dòng trả đúng mã lỗi mong đợi, `data: null`.
- Dòng a và b: `details.roleId` bằng `roleId` đã gửi.
- Vai trò của ACC-TARGET không đổi.

**Actual result:**

**Bug:**

## `TC-013-04-07` — Từ chối yêu cầu kèm trường không có trong đặc tả

**Precondition:** Dữ liệu nền. Đọc `{{rev_target}}`.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_admin}}` và body đổi vai trò chuẩn có thêm trường theo dòng dữ liệu.
2. Gửi `GET {{base_url}}/users?q=lan@example.test` với `{{token_admin}}`.
3. Kiểm tra `users.isSuperAdmin` của ACC-TARGET.

**Test data:**

| Dòng | Trường thêm vào body |
|---|---|
| a | `"isSuperAdmin": true` |
| b | `"note": "test"` |

**Expected result:**

- Mỗi dòng trả `400`, `errorCode: VALIDATION_FAILED`, `data: null`.
- `isSuperAdmin` của ACC-TARGET là `false` trong phản hồi bước 2 và trong bảng `users`.
- Vai trò của ACC-TARGET không đổi.

**Actual result:**

**Bug:**

## `TC-013-04-08` — Mã lỗi theo đúng thứ tự kiểm tra khi nhiều điều kiện cùng không đạt

**Precondition:** Dữ liệu nền. Với dòng d, `users.deletedAt` của ACC-ROOT được cập nhật thành thời điểm hiện tại. Đọc `currentRoleRevision` của tài khoản đích trước mỗi request; riêng dòng f dùng giá trị đã giảm 1.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/<tài khoản đích>/role` với body đổi vai trò chuẩn, thay các trường theo dòng dữ liệu.
2. Kiểm tra mã lỗi trả về.

**Test data:**

| Dòng | Mã đăng nhập | Tài khoản đích | Thay đổi trong body | Mã lỗi mong đợi |
|---|---|---|---|---|
| a | Không gửi header | `{{id_target}}` | `reason` gồm 19 ký tự "a" | `401 AUTH_TOKEN_MISSING` |
| b | `{{token_noperm}}` | `{{id_root}}` | Không | `403 PERMISSION_DENIED` |
| c | `{{token_admin}}` | `{{id_admin}}` | `reason` gồm 19 ký tự "a" | `400 VALIDATION_FAILED` |
| d | `{{token_admin}}` | `{{id_root}}`, đang bị vô hiệu hoá | Không | `403 USER_ACCOUNT_PROTECTED` |
| e | `{{token_admin}}` | `{{id_disabled}}` | `roleId: {{role_draft}}` | `409 USER_TARGET_DISABLED` |
| f | `{{token_admin}}` | `{{id_target}}` | `roleId: {{role_draft}}`, `currentRoleRevision` cũ | `409 USER_ROLE_UNAVAILABLE` |

**Expected result:**

- Mỗi dòng trả đúng mã lỗi mong đợi theo thứ tự kiểm tra của đặc tả API: phiên đăng nhập, quyền, dữ liệu gửi lên, tài khoản đích tồn tại, không phải chính người thao tác, không phải superAdmin, đang hoạt động, vai trò mới gán được, mốc phiên bản vai trò.
- Vai trò của mọi tài khoản đích không đổi.

**Actual result:**

**Bug:**
