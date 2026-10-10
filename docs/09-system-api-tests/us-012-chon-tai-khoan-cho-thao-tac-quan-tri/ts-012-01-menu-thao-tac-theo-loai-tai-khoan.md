# `TS-012-01` — Menu thao tác theo loại tài khoản

**Liên quan:** [danh sách scenario của `US-012`](README.md), [user story `US-012`](../../06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** menu thao tác trên mỗi dòng chỉ chứa các lệnh được phép theo trạng thái tài khoản, theo tài khoản của chính người thao tác và theo tài khoản superAdmin.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-012-01-01` | Menu của tài khoản đang hoạt động có "Đổi vai trò" và "Vô hiệu hoá" | Positive | High | | `US012-SC-01` |
| `TC-012-01-02` | Menu của tài khoản đã vô hiệu hoá chỉ có "Khôi phục" | Positive | High | | `US012-SC-02` |
| `TC-012-01-03` | Dòng của chính người thao tác không có nút thao tác | Negative | High | | `US012-SC-03` |
| `TC-012-01-04` | Dòng tài khoản superAdmin không có nút thao tác ở mọi trạng thái | Negative | High | | `US012-SC-03` |
| `TC-012-01-05` | Tài khoản của chính người thao tác đã bị vô hiệu hoá chỉ có lệnh "Khôi phục" | Positive | High | | `US012-SC-12` |
| `TC-012-01-06` | Máy chủ trả danh sách lệnh được phép đúng theo loại tài khoản | Positive | High | | `US012-SC-01`, `US012-SC-02`, `US012-SC-03` |

## `TC-012-01-01` — Menu của tài khoản đang hoạt động có "Đổi vai trò" và "Vô hiệu hoá"

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "lan@example.test".
2. Bấm nút thao tác trên dòng ACC-TARGET.
3. Quan sát menu, sau đó đóng menu mà không chọn lệnh nào.

**Test data:** ACC-TARGET.

**Expected result:**

- Menu có đúng hai lựa chọn theo thứ tự "Đổi vai trò", "Vô hiệu hoá"; "Vô hiệu hoá" có chữ màu đỏ.
- Menu không có lựa chọn "Khôi phục".
- Không hộp thoại nào được mở. Vai trò và trạng thái của ACC-TARGET không đổi.

**Actual result:**

**Bug:**

## `TC-012-01-02` — Menu của tài khoản đã vô hiệu hoá chỉ có "Khôi phục"

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "disabled@example.test".
2. Bấm nút thao tác trên dòng ACC-DISABLED.
3. Quan sát menu, sau đó đóng menu mà không chọn lệnh nào.

**Test data:** ACC-DISABLED.

**Expected result:**

- Menu chỉ có lựa chọn "Khôi phục".
- Menu không có lựa chọn "Đổi vai trò" hoặc "Vô hiệu hoá".
- Trạng thái của ACC-DISABLED vẫn là "Đã vô hiệu hoá".

**Actual result:**

**Bug:**

## `TC-012-01-03` — Dòng của chính người thao tác không có nút thao tác

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "qa.admin@example.test".
2. Quan sát dòng ACC-ADMIN.

**Test data:** ACC-ADMIN.

**Expected result:**

- Dòng ACC-ADMIN có thẻ "Bạn" cạnh tên.
- Dòng ACC-ADMIN không có nút thao tác; không có cách nào mở hộp thoại đổi vai trò hoặc vô hiệu hoá cho dòng này.
- Vai trò và trạng thái của ACC-ADMIN không đổi.

**Actual result:**

**Bug:**

## `TC-012-01-04` — Dòng tài khoản superAdmin không có nút thao tác ở mọi trạng thái

**Precondition:** Dữ liệu nền. ACC-ROOT ở trạng thái theo dòng dữ liệu; trạng thái đã vô hiệu hoá được thiết lập bằng cách cập nhật `users.deletedAt` của ACC-ROOT. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "root@example.test".
2. Quan sát dòng ACC-ROOT.

**Test data:**

| Dòng | Trạng thái của ACC-ROOT |
|---|---|
| a | Đang hoạt động |
| b | Đã vô hiệu hoá |

**Expected result:**

- Dòng ACC-ROOT vẫn hiển thị trong danh sách, có thẻ "superAdmin" và nền xám nhạt.
- Dòng ACC-ROOT không có nút thao tác.
- Vai trò, trạng thái và cờ superAdmin của ACC-ROOT không đổi.

**Actual result:**

**Bug:**

## `TC-012-01-05` — Tài khoản của chính người thao tác đã bị vô hiệu hoá chỉ có lệnh "Khôi phục"

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập; sau đó `users.deletedAt` của ACC-ADMIN được cập nhật thành thời điểm hiện tại, phiên đăng nhập của ACC-ADMIN vẫn còn hạn.

**Steps:**

1. Trên màn hình Tài khoản, bấm nút làm mới.
2. Bấm "Bộ lọc", nhập "qa.admin@example.test".
3. Bấm nút thao tác trên dòng ACC-ADMIN.

**Test data:** ACC-ADMIN ở trạng thái đã vô hiệu hoá.

**Expected result:**

- Dòng ACC-ADMIN có thẻ "Bạn" và có nút thao tác.
- Menu chỉ có lựa chọn "Khôi phục"; không có "Đổi vai trò" hoặc "Vô hiệu hoá".

**Actual result:**

**Bug:**

## `TC-012-01-06` — Máy chủ trả danh sách lệnh được phép đúng theo loại tài khoản

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `GET {{base_url}}/users?status=active&pageSize=100` với `{{token_admin}}`; đọc `allowedActions` của ACC-TARGET, ACC-ADMIN và ACC-ROOT.
2. Gửi `GET {{base_url}}/users?status=disabled&pageSize=100` với `{{token_admin}}`; đọc `allowedActions` của ACC-DISABLED.

**Test data:**

| Tài khoản | Lệnh phải có | Lệnh không được có |
|---|---|---|
| ACC-TARGET | `CHANGE_ROLE`, `DISABLE`, `SELECT` | `RESTORE` |
| ACC-ADMIN | Không có lệnh nào | `CHANGE_ROLE`, `DISABLE`, `RESTORE`, `SELECT` |
| ACC-ROOT | Không có lệnh nào | `CHANGE_ROLE`, `DISABLE`, `RESTORE`, `SELECT` |
| ACC-DISABLED | `RESTORE`, `SELECT` | `CHANGE_ROLE`, `DISABLE` |

**Expected result:**

- Cả hai phản hồi `200`.
- `allowedActions` của từng tài khoản khớp với Test data.

**Actual result:**

**Bug:**
