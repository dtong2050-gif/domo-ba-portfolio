# `TS-013-06` — Lỗi hệ thống và mất phản hồi

**Liên quan:** [danh sách scenario của `US-013`](README.md), [user story `US-013`](../../06-user-stories/us-013-doi-vai-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** khi máy chủ lỗi trong lúc ghi, không có thay đổi một phần; khi không xác định được kết quả, giao diện không tự gửi lại và không báo nhầm là thất bại.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-013-06-01` | Thất bại an toàn khi máy chủ lỗi trong lúc ghi | Negative | High | | `US013-SC-13` |
| `TC-013-06-02` | Không tự gửi lại lệnh khi không xác định được kết quả | Negative | High | | `US013-SC-14` |

## `TC-013-06-01` — Thất bại an toàn khi máy chủ lỗi trong lúc ghi

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập. Cơ chế gây lỗi khi ghi vai trò đang bật. Ghi lại `currentRoleRevision` của ACC-TARGET.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET, chọn "Quản trị viên", nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", bấm "Tiếp tục".
2. Bấm "Xác nhận", quan sát hộp thoại và phản hồi trong tab Network.
3. Kiểm tra vai trò của ACC-TARGET trên danh sách, trong phản hồi `GET /users` và trong bảng `users`; kiểm tra bản ghi mới nhất trong `user_action_logs` có `entityId` là `{{id_target}}`.
4. Tắt cơ chế gây lỗi, bấm "Thử lại".
5. Kiểm tra request vừa gửi trong tab Network và vai trò của ACC-TARGET.

**Test data:** Vai trò mới Quản trị viên.

**Expected result:**

- Bước 2: phản hồi `500`, `errorCode: M03_COMMAND_FAILED_SAFE`, `details.commitStatus: NOT_COMMITTED`, `details.retryable: true`, có `traceId`. Hộp thoại có tiêu đề "Thay đổi vai trò thất bại"; khung đỏ hiển thị "Không thể thay đổi vai trò" và "Hệ thống chưa ghi nhận thay đổi nào. Vui lòng thử lại."; vẫn hiển thị tài khoản đích, vai trò cũ và vai trò mới; lý do được ẩn; có nút "Thử lại" và "Quay lại".
- Bước 3: vai trò không đổi ở cả ba nơi; bản ghi nhật ký có `result: failed`. Không gửi thông báo.
- Bước 5: request gửi lại đúng `roleId`, `currentRoleRevision` và `reason` của quyết định cũ; ACC-TARGET mang vai trò Quản trị viên.

**Actual result:**

**Bug:**

## `TC-013-06-02` — Không tự gửi lại lệnh khi không xác định được kết quả

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập. Trình duyệt đi qua proxy kiểm thử; proxy chuyển request `PATCH /users/{id}/role` tới máy chủ nhưng huỷ phản hồi trả về.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET, chọn "Quản trị viên", nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", bấm "Tiếp tục".
2. Xoá danh sách request trong tab Network.
3. Bấm "Xác nhận", chờ tới khi hộp thoại đổi trạng thái.
4. Nhấn phím Esc và bấm ra ngoài hộp thoại.
5. Kiểm tra tab Network.
6. Bấm "Đóng và tải lại", so sánh vai trò trên danh sách với bảng `users`.

**Test data:** Vai trò mới Quản trị viên.

**Expected result:**

- Sau bước 3: hộp thoại có tiêu đề "Không xác định được kết quả"; khung xám hiển thị "Không nhận được phản hồi từ hệ thống" và "Thay đổi có thể đã được áp dụng hoặc chưa. Hãy tải lại danh sách trước khi thử lại."; vẫn hiển thị tài khoản đích, vai trò cũ và vai trò mới; lý do được ẩn; chỉ có nút "Đóng và tải lại"; không có nút đóng.
- Hộp thoại không hiển thị trạng thái thất bại.
- Bước 4: phím Esc và bấm ra ngoài không có tác dụng.
- Bước 5: chỉ có một request `PATCH`; giao diện không tự gửi lại.
- Bước 6: hai hộp thoại đóng; vai trò của ACC-TARGET trên danh sách khớp với bảng `users`.

**Actual result:**

**Bug:**
