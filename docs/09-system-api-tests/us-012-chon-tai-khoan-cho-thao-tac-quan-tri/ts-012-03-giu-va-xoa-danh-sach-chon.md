# `TS-012-03` — Giữ và xoá danh sách chọn

**Liên quan:** [danh sách scenario của `US-012`](README.md), [user story `US-012`](../../06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** danh sách chọn được giữ khi chuyển trang, và bị xoá toàn bộ khi điều kiện tra cứu thay đổi hoặc khi người thao tác bỏ chọn.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-012-03-01` | Giữ danh sách chọn khi chuyển trang | Positive | High | | `US012-SC-07` |
| `TC-012-03-02` | Xoá danh sách chọn khi điều kiện tra cứu thay đổi | Positive | High | | `US012-SC-11` |
| `TC-012-03-03` | Bỏ chọn toàn bộ bằng nút "Bỏ chọn" | Positive | Medium | | `US012-SC-13` |

## `TC-012-03-01` — Giữ danh sách chọn khi chuyển trang

**Precondition:** Dữ liệu nền và 15 tài khoản trong Test data. ACC-ADMIN đã đăng nhập. Số kết quả mỗi trang là 10.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "acc4", chọn trạng thái "Đang hoạt động".
2. Trên trang 1, chọn ACC-401 và ACC-402.
3. Chuyển sang trang 2, quan sát thanh hành động.
4. Quay lại trang 1, quan sát các dòng đã chọn.

**Test data:** ACC-401 đến ACC-415, email `acc401@example.test` đến `acc415@example.test`, Đang hoạt động.

**Expected result:**

- Sau bước 3: thanh hành động vẫn hiển thị "Đã chọn 2 tài khoản".
- Sau bước 4: danh sách chọn vẫn gồm đúng ACC-401 và ACC-402.

**Actual result:**

**Bug:**

## `TC-012-03-02` — Xoá danh sách chọn khi điều kiện tra cứu thay đổi

**Precondition:** Dữ liệu nền và hai tài khoản trong Test data. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "acc80", chọn trạng thái "Đang hoạt động".
2. Chọn ACC-801 và ACC-802. Xác nhận thanh hành động hiển thị "Đã chọn 2 tài khoản".
3. Xoá danh sách request trong tab Network.
4. Thay đổi điều kiện tra cứu theo dòng dữ liệu.
5. Quan sát danh sách chọn, thanh hành động và tab Network.

**Test data:** ACC-801, ACC-802, email `acc801@example.test`, `acc802@example.test`, Đang hoạt động.

| Dòng | Thay đổi |
|---|---|
| a | Đổi từ khoá thành "acc8" |
| b | Chọn vai trò "Người dùng" |
| c | Đổi trạng thái thành "Đã vô hiệu hoá" |
| d | Bấm tiêu đề "Ngày tham gia" để đổi chiều sắp xếp |
| e | Đổi số kết quả mỗi trang thành 20 |

**Expected result:**

- Danh sách chọn rỗng; thanh hành động không hiển thị.
- Tab Network không có request `POST /users/selection-previews`.

**Actual result:**

**Bug:**

## `TC-012-03-03` — Bỏ chọn toàn bộ bằng nút "Bỏ chọn"

**Precondition:** Dữ liệu nền và ba tài khoản trong Test data. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "acc95", chọn trạng thái "Đang hoạt động".
2. Chọn ACC-951, ACC-952 và ACC-953. Xác nhận thanh hành động hiển thị "Đã chọn 3 tài khoản".
3. Xoá danh sách request trong tab Network.
4. Bấm "Bỏ chọn".

**Test data:** ACC-951 đến ACC-953, email `acc951@example.test` đến `acc953@example.test`, Đang hoạt động.

**Expected result:**

- Danh sách chọn rỗng; ô chọn tất cả ở trạng thái "Chưa chọn".
- Thanh hành động biến mất ngay.
- Tab Network không có request `POST /users/selection-previews`.

**Actual result:**

**Bug:**
