# `TS-013-03` — Xác nhận và đổi vai trò thành công

**Liên quan:** [danh sách scenario của `US-013`](README.md), [user story `US-013`](../../06-user-stories/us-013-doi-vai-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** hộp xác nhận hiển thị đúng quyết định; lệnh hợp lệ đổi vai trò trọn vẹn, ghi nhật ký và gửi thông báo; quyền mới có hiệu lực đúng thời điểm và dữ liệu sở hữu không bị ảnh hưởng.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-013-03-01` | Hộp xác nhận hiển thị đúng tài khoản, vai trò và lý do đã chuẩn hoá | Positive | High | | `US013-SC-07` |
| `TC-013-03-02` | Đổi vai trò thành công và cập nhật mọi nơi lưu vai trò | Positive | High | | `US013-SC-01` |
| `TC-013-03-03` | Máy chủ đổi vai trò thành công và trả kết quả CHANGED | Positive | High | | `US013-SC-01` |
| `TC-013-03-04` | Báo "Không có thay đổi" khi chọn đúng vai trò hiện tại | Positive | Medium | | `US013-SC-11` |
| `TC-013-03-05` | Máy chủ trả NO_CHANGE khi vai trò mới trùng vai trò hiện tại | Positive | Medium | | `US013-SC-11` |
| `TC-013-03-06` | Hạ vai trò Nhà tổ chức không xoá dữ liệu sở hữu | Positive | High | | `US013-SC-15` |
| `TC-013-03-07` | Phiên đang mở giữ quyền cũ cho tới khi đăng nhập lại | Positive | High | | `US013-SC-16` |
| `TC-013-03-08` | Nhật ký thao tác không chứa thông tin định danh nhạy cảm | Positive | High | | `NFR-P-06` |

## `TC-013-03-01` — Hộp xác nhận hiển thị đúng tài khoản, vai trò và lý do đã chuẩn hoá

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET, chọn "Quản trị viên".
2. Nhập lý do trong Test data, bấm "Tiếp tục".
3. Quan sát hộp thoại xác nhận.
4. Kiểm tra vai trò của ACC-TARGET trong bảng `users`.

**Test data:** Lý do: hai khoảng trắng, "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", hai khoảng trắng.

**Expected result:**

- Hộp thoại có tiêu đề "Xác nhận thay đổi vai trò" và dòng phụ "Kiểm tra lại thông tin trước khi xác nhận".
- Hộp thoại hiển thị "Lan Nguyễn" và "lan@example.test".
- Hộp thoại hiển thị đủ vai trò cũ "Người dùng" (nhãn xám) và vai trò mới "Quản trị viên" (nhãn xanh).
- Lý do hiển thị đầy đủ là "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", không có khoảng trắng ở đầu và cuối.
- Ghi chú "Vai trò mới có hiệu lực từ lần đăng nhập tiếp theo." hiển thị; hộp thoại không có ô nhập nào; có nút "Quay lại" và "Xác nhận".
- Vai trò của ACC-TARGET không đổi.

**Actual result:**

**Bug:**

## `TC-013-03-02` — Đổi vai trò thành công và cập nhật mọi nơi lưu vai trò

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập. Ghi lại `currentRoleRevision` của ACC-TARGET.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "lan@example.test".
2. Mở hộp thoại đổi vai trò cho ACC-TARGET, chọn vai trò theo dòng dữ liệu, nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", bấm "Tiếp tục".
3. Bấm "Xác nhận", quan sát màn hình.
4. Gửi `GET {{base_url}}/users?q=lan@example.test` với `{{token_admin}}`.
5. Kiểm tra `roleId`, `currentRoleRevision` của ACC-TARGET trong bảng `users` và bản ghi mới nhất trong `user_action_logs` có `entityId` là `{{id_target}}`.
6. Đăng nhập ACC-TARGET trên trình duyệt khác, mở danh sách thông báo.

**Test data:**

| Dòng | Vai trò mới | Mã vai trò mới |
|---|---|---|
| a | Quản trị viên | `{{role_admin}}` |
| b | Kiểm duyệt viên | `{{role_mod}}` |

**Expected result:**

- Sau bước 3: hai hộp thoại đóng; thông báo "Đã thay đổi vai trò cho Lan Nguyễn" kèm "Vai trò mới: <vai trò mới>" hiển thị; danh sách tải lại với từ khoá "lan@example.test"; dòng ACC-TARGET hiển thị vai trò mới.
- Phản hồi ở bước 4 và bảng `users` cùng có `roleId` là mã vai trò mới; `currentRoleRevision` tăng đúng 1.
- Bản ghi nhật ký có `actorId: {{id_admin}}`, `action: UPDATE`, `entity: users`, `entityId: {{id_target}}`, `result: success`, `reason` là lý do đã nhập; `changes` chỉ gồm `roleId` với giá trị trước là `{{role_user}}` và giá trị sau là mã vai trò mới.
- ACC-TARGET có thông báo trong ứng dụng nêu vai trò mới và lý do đã nhập.

**Actual result:**

**Bug:**

## `TC-013-03-03` — Máy chủ đổi vai trò thành công và trả kết quả CHANGED

**Precondition:** Dữ liệu nền. Đọc `{{rev_target}}`.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_admin}}` và body đổi vai trò chuẩn.
2. Gửi `GET {{base_url}}/users?q=lan@example.test` với `{{token_admin}}`.

**Test data:** Body đổi vai trò chuẩn, `roleId: {{role_admin}}`.

**Expected result:**

- Bước 1: phản hồi `200`, `data.outcome: CHANGED`, `data.affectedCount: 1`, `data.effectiveAt` có giá trị thời điểm.
- Bước 2: `roleId` của ACC-TARGET là `{{role_admin}}`; `currentRoleRevision` bằng `{{rev_target}}` cộng 1.

**Actual result:**

**Bug:**

## `TC-013-03-04` — Báo "Không có thay đổi" khi chọn đúng vai trò hiện tại

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập. Ghi lại `currentRoleRevision` của ACC-TARGET và số bản ghi trong `user_action_logs`.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET, chọn "Người dùng", nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.", bấm "Tiếp tục".
2. Bấm "Xác nhận", quan sát màn hình.
3. Kiểm tra bảng `users` và bản ghi mới nhất trong `user_action_logs` có `entityId` là `{{id_target}}`.
4. Đăng nhập ACC-TARGET trên trình duyệt khác, mở danh sách thông báo.

**Test data:** Vai trò mới "Người dùng", trùng vai trò hiện tại của ACC-TARGET.

**Expected result:**

- Hai hộp thoại đóng; danh sách tải lại theo điều kiện tra cứu hiện tại và hiển thị thông báo "Không có thay đổi".
- Vai trò không đổi; `currentRoleRevision` không tăng.
- Có một bản ghi nhật ký mới với `result: no_op`, không có `changes`.
- Không gửi thông báo.

**Actual result:**

**Bug:**

## `TC-013-03-05` — Máy chủ trả NO_CHANGE khi vai trò mới trùng vai trò hiện tại

**Precondition:** Dữ liệu nền. Đọc `{{rev_target}}`.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_admin}}` và body đổi vai trò chuẩn, `roleId: {{role_user}}`.
2. Kiểm tra vai trò của ACC-TARGET.

**Test data:** `roleId: {{role_user}}`, trùng vai trò hiện tại.

**Expected result:**

- Phản hồi `200`, `data.outcome: NO_CHANGE`, `data.affectedCount: 0`.
- Vai trò không đổi; `currentRoleRevision` không tăng.

**Actual result:**

**Bug:**

## `TC-013-03-06` — Hạ vai trò Nhà tổ chức không xoá dữ liệu sở hữu

**Precondition:** Dữ liệu nền. ACC-ORG sở hữu thử thách CHALLENGE-101 có ít nhất 3 người tham gia và ít nhất 1 minh chứng. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Ghi lại dữ liệu của CHALLENGE-101 trong cơ sở dữ liệu: chủ sở hữu, nội dung, số người tham gia, số minh chứng.
2. Đổi vai trò của ACC-ORG sang "Người dùng" với lý do "Tạm ngừng tổ chức thử thách theo đề nghị của nhà tổ chức.".
3. So sánh dữ liệu của CHALLENGE-101 với số liệu ở bước 1.
4. Đổi vai trò của ACC-ORG sang "Nhà tổ chức" với lý do "Cấp lại vai trò Nhà tổ chức theo đề nghị hoạt động trở lại.".
5. Đăng nhập ACC-ORG, mở mục quản lý thử thách của Nhà tổ chức.

**Test data:** ACC-ORG, CHALLENGE-101.

**Expected result:**

- Sau bước 2: ACC-ORG có vai trò "Người dùng".
- Sau bước 3: CHALLENGE-101 vẫn thuộc sở hữu của ACC-ORG; nội dung, số người tham gia và số minh chứng không đổi.
- Sau bước 5: ACC-ORG thấy CHALLENGE-101 trong mục quản lý thử thách với dữ liệu khớp bước 1.

**Actual result:**

**Bug:**

## `TC-013-03-07` — Phiên đang mở giữ quyền cũ cho tới khi đăng nhập lại

**Precondition:** Dữ liệu nền. ACC-TARGET đang đăng nhập trên trình duyệt thứ hai; mã đăng nhập của phiên này được ghi vào biến `{{token_target}}`. ACC-ADMIN đã đăng nhập trên trình duyệt thứ nhất.

**Steps:**

1. ACC-ADMIN đổi vai trò của ACC-TARGET sang "Quản trị viên".
2. ACC-ADMIN quan sát dòng ACC-TARGET trên danh sách.
3. Trên phiên đang mở, ACC-TARGET tải lại trang và mở trực tiếp đường dẫn màn hình Tài khoản.
4. Gửi `GET {{base_url}}/users` với `{{token_target}}`.
5. ACC-TARGET đăng xuất, đăng nhập lại và mở màn hình Tài khoản.
6. Gửi `GET {{base_url}}/users` với mã đăng nhập mới của ACC-TARGET.

**Test data:** Vai trò mới "Quản trị viên".

**Expected result:**

- Bước 2: dòng ACC-TARGET hiển thị vai trò "Quản trị viên".
- Bước 3: thanh điều hướng không có mục "Tài khoản"; màn hình hiển thị "Bạn không có quyền quản lý tài khoản.".
- Bước 4: phản hồi `403`, `errorCode: PERMISSION_DENIED`.
- Bước 5: màn hình Tài khoản hiển thị danh sách tài khoản.
- Bước 6: phản hồi `200`.

**Actual result:**

**Bug:**

## `TC-013-03-08` — Nhật ký thao tác không chứa thông tin định danh nhạy cảm

**Precondition:** Dữ liệu nền và [`DS-05`](../00-moi-truong-va-du-lieu.md#ds-05--hồ-sơ-đầy-đủ-của-acc-target). Ghi lại thời điểm bắt đầu.

**Steps:**

1. Gửi ba lệnh `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_admin}}` theo dòng dữ liệu; đọc lại `{{rev_target}}` trước mỗi lệnh.
2. Truy vấn các bản ghi trong `user_action_logs` có `entityId` là `{{id_target}}` được tạo sau thời điểm bắt đầu.
3. Kiểm tra mọi trường của các bản ghi.

**Test data:**

| Dòng | Body | Kết quả lệnh |
|---|---|---|
| a | Body đổi vai trò chuẩn, `roleId: {{role_admin}}` | `CHANGED` |
| b | Body đổi vai trò chuẩn, `roleId: {{role_admin}}` | `NO_CHANGE` |
| c | Body đổi vai trò chuẩn, `reason` gồm 19 ký tự "a" | Bị từ chối |

**Expected result:**

- Có ba bản ghi với `result` lần lượt là `success`, `no_op`, `rejected`.
- `actorId` và `entityId` chỉ chứa mã tài khoản.
- Không bản ghi nào chứa email, tên, số điện thoại, địa chỉ hoặc ngày sinh của ACC-TARGET hay ACC-ADMIN, ở bất kỳ trường nào.
- Chỉ bản ghi `success` có `changes`, và `changes` chỉ gồm `roleId`.
- `path` chỉ chứa mã tài khoản, không chứa email hay tên.

**Actual result:**

**Bug:**
