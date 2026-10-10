# `TS-013-05` — Thao tác đồng thời và gửi lặp

**Liên quan:** [danh sách scenario của `US-013`](README.md), [user story `US-013`](../../06-user-stories/us-013-doi-vai-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** hệ thống không ghi đè quyết định mới hơn bằng thông tin cũ, và mỗi quyết định chỉ được gửi một lần.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-013-05-01` | Từ chối quyết định dựa trên vai trò đã bị người khác thay đổi | Negative | High | | `US013-SC-12` |
| `TC-013-05-02` | Máy chủ từ chối mốc phiên bản vai trò đã cũ | Negative | High | | `US013-SC-12` |
| `TC-013-05-03` | Chỉ gửi một lệnh khi bấm "Xác nhận" nhiều lần | Negative | Medium | | `US013-SC-17` |

## `TC-013-05-01` — Từ chối quyết định dựa trên vai trò đã bị người khác thay đổi

**Precondition:** Dữ liệu nền. ACC-ADMIN đăng nhập trên trình duyệt thứ nhất, ACC-ADMIN-2 đăng nhập trên trình duyệt thứ hai.

**Steps:**

1. ACC-ADMIN mở hộp thoại đổi vai trò cho ACC-TARGET, chọn "Quản trị viên", nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", bấm "Tiếp tục".
2. ACC-ADMIN-2 đổi vai trò của ACC-TARGET sang "Kiểm duyệt viên" thành công.
3. ACC-ADMIN bấm "Xác nhận", quan sát hộp thoại.
4. ACC-ADMIN bấm "Đóng và tải lại".
5. Kiểm tra bản ghi mới nhất trong `user_action_logs` có `entityId` là `{{id_target}}` và `actorId` là `{{id_admin}}`.

**Test data:** Vai trò do ACC-ADMIN-2 gán: Kiểm duyệt viên. Vai trò trong quyết định của ACC-ADMIN: Quản trị viên.

**Expected result:**

- Sau bước 3: hộp thoại có tiêu đề "Không thể thay đổi vai trò"; khung đỏ có tiêu đề "Vai trò của tài khoản vừa được thay đổi" và nội dung "Vai trò của tài khoản vừa được thay đổi. Thay đổi chưa được áp dụng. Vui lòng tải lại danh sách."; chỉ có nút "Đóng và tải lại"; không có nút "Xác nhận" hay "Thử lại".
- ACC-TARGET vẫn mang vai trò "Kiểm duyệt viên".
- Sau bước 4: hai hộp thoại đóng; dòng ACC-TARGET hiển thị "Kiểm duyệt viên".
- Bản ghi nhật ký của lệnh từ ACC-ADMIN có `result: rejected`.

**Actual result:**

**Bug:**

## `TC-013-05-02` — Máy chủ từ chối mốc phiên bản vai trò đã cũ

**Precondition:** Dữ liệu nền. Đọc `{{rev_target}}`, gọi giá trị này là R.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_admin2}}`, body đổi vai trò chuẩn với `roleId: {{role_mod}}` và `currentRoleRevision: R`.
2. Gửi `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_admin}}`, body đổi vai trò chuẩn với `roleId: {{role_admin}}` và `currentRoleRevision: R`.
3. Kiểm tra vai trò của ACC-TARGET.

**Test data:** R là `currentRoleRevision` của ACC-TARGET trước bước 1.

**Expected result:**

- Bước 1: phản hồi `200`, `data.outcome: CHANGED`.
- Bước 2: phản hồi `409`, `errorCode: USER_ROLE_STALE`, `data: null`; `details.actualCurrentRoleId` bằng `{{role_mod}}`, `details.currentRoleRevision` bằng R cộng 1.
- ACC-TARGET mang vai trò Kiểm duyệt viên; `currentRoleRevision` bằng R cộng 1.

**Actual result:**

**Bug:**

## `TC-013-05-03` — Chỉ gửi một lệnh khi bấm "Xác nhận" nhiều lần

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập. DevTools bật giả lập mạng "Slow 3G". Ghi lại `currentRoleRevision` của ACC-TARGET và số bản ghi trong `user_action_logs`.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET, chọn "Quản trị viên", nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", bấm "Tiếp tục".
2. Xoá danh sách request trong tab Network.
3. Bấm "Xác nhận" hai lần liên tiếp thật nhanh.
4. Trong lúc chờ, nhấn phím Esc và bấm ra ngoài hộp thoại.
5. Chờ kết quả, kiểm tra tab Network, bảng `users` và `user_action_logs`.

**Test data:** Vai trò mới Quản trị viên.

**Expected result:**

- Trong lúc chờ: tiêu đề "Đang thay đổi vai trò"; nút chính hiển thị "Đang xử lý..." và bị khoá; nút "Quay lại" bị khoá; không có nút đóng; ghi chú "Đang gửi yêu cầu — vui lòng không đóng cửa sổ này."; phím Esc và bấm ra ngoài không có tác dụng.
- Tab Network có đúng một request `PATCH /users/{{id_target}}/role`.
- `currentRoleRevision` tăng đúng 1; có đúng một bản ghi nhật ký mới với `result: success`.

**Actual result:**

**Bug:**
