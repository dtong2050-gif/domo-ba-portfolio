# `TS-011-04` — Bộ lọc, cột hiển thị và làm mới

**Liên quan:** [danh sách scenario của `US-011`](README.md), [user story `US-011`](../../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** thao tác ẩn hiện khu vực bộ lọc và cột hiển thị không làm mất điều kiện tra cứu hay tải lại dữ liệu; nút làm mới tra cứu lại đúng điều kiện đang áp dụng.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-011-04-01` | Ẩn và hiện khu vực bộ lọc không làm mất điều kiện đang áp dụng | Positive | Low | | `US011-SC-10` |
| `TC-011-04-02` | Ẩn và hiện cột tuỳ chọn không tra cứu lại danh sách | Positive | Low | | `US011-SC-11` |
| `TC-011-04-03` | Làm mới tra cứu lại đúng điều kiện hiện tại | Positive | Medium | | `US011-SC-12` |

## `TC-011-04-01` — Ẩn và hiện khu vực bộ lọc không làm mất điều kiện đang áp dụng

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và [`DS-01`](../00-moi-truong-va-du-lieu.md#ds-01--tài-khoản-tìm-kiếm-theo-tên-và-email). ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "huy" và chờ kết quả.
2. Bấm "Bộ lọc" để ẩn khu vực tìm kiếm và bộ lọc.
3. Xoá danh sách request trong tab Network.
4. Bấm "Bộ lọc" để hiện lại khu vực.
5. Bấm "Bộ lọc" lần nữa để ẩn khu vực.

**Test data:** Từ khoá "huy". Tài khoản: [`DS-01`](../00-moi-truong-va-du-lieu.md#ds-01--tài-khoản-tìm-kiếm-theo-tên-và-email).

**Expected result:**

- Sau bước 2, khu vực bị ẩn; danh sách vẫn chỉ gồm U-101 và U-102.
- Sau bước 4, khu vực hiển thị với từ khoá "huy" trong ô tìm kiếm.
- Sau bước 5, khu vực bị ẩn; danh sách vẫn chỉ gồm U-101 và U-102.
- Từ bước 3 đến bước 5, tab Network không có request `GET /users` mới và trang không tải lại.

**Actual result:**

**Bug:**

## `TC-011-04-02` — Ẩn và hiện cột tuỳ chọn không tra cứu lại danh sách

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập, màn hình Tài khoản hiển thị đủ năm cột.

**Steps:**

1. Xoá danh sách request trong tab Network.
2. Bấm "Cột hiển thị", thực hiện thao tác theo dòng dữ liệu.
3. Quan sát bảng, menu "Cột hiển thị" và tab Network.

**Test data:**

| Dòng | Thao tác | Kết quả |
|---|---|---|
| a | Bỏ chọn "Vai trò" | Cột Vai trò bị ẩn |
| b | Bỏ chọn "Trạng thái" | Cột Trạng thái bị ẩn |
| c | Bỏ chọn "Ngày tham gia" | Cột Ngày tham gia bị ẩn |
| d | Chọn lại "Vai trò" | Cột Vai trò hiển thị lại |

**Expected result:**

- Cột thay đổi đúng cột Kết quả của dòng dữ liệu.
- Cột Tài khoản và Thao tác luôn hiển thị và không có trong menu "Cột hiển thị".
- Tab Network không có request `GET /users` mới.

**Actual result:**

**Bug:**

## `TC-011-04-03` — Làm mới tra cứu lại đúng điều kiện hiện tại

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và [`DS-02`](../00-moi-truong-va-du-lieu.md#ds-02--tài-khoản-cho-tra-cứu-vượt-quá-100-bản-ghi). ACC-ADMIN đã đăng nhập. DevTools bật giả lập mạng "Slow 3G".

**Steps:**

1. Mở màn hình Tài khoản. Bấm "Bộ lọc", nhập "an", chọn vai trò "Người dùng", trạng thái "Đang hoạt động"; giữ chiều mới đến cũ và số kết quả mỗi trang là 10.
2. Chuyển sang trang 2.
3. Xoá danh sách request trong tab Network.
4. Bấm nút làm mới.
5. Quan sát màn hình trong lúc chờ và sau khi tải xong.

**Test data:** [`DS-02`](../00-moi-truong-va-du-lieu.md#ds-02--tài-khoản-cho-tra-cứu-vượt-quá-100-bản-ghi).

**Expected result:**

- Tab Network có đúng một request `GET /users` với `q=an`, `roleId={{role_user}}`, `status=active`, `sortOrder=desc`, `page=2`, `pageSize=10`; không có request tải lại toàn trang.
- Trong lúc chờ: thân bảng hiển thị trạng thái đang tải, chân bảng ghi "Đang tải danh sách..."; không hiển thị dữ liệu cũ; thanh công cụ và bộ lọc vẫn dùng được; các nút chuyển trang bị tắt.
- Sau khi tải xong: màn hình ở trang 2, hiển thị AN-011 đến AN-020.

**Actual result:**

**Bug:**
