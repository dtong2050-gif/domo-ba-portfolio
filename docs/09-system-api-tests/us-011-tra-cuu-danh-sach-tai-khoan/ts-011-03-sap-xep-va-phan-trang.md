# `TS-011-03` — Sắp xếp và phân trang

**Liên quan:** [danh sách scenario của `US-011`](README.md), [user story `US-011`](../../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** thứ tự sắp xếp ổn định giữa các lần tra cứu, số dòng mỗi trang chỉ nhận giá trị hợp lệ, và danh sách quay về trang 1 khi điều kiện tra cứu thay đổi.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-011-03-01` | Sắp xếp từ cũ đến mới, trùng ngày tham gia xếp theo mã tài khoản | Positive | High | | `US011-SC-04` |
| `TC-011-03-02` | Sắp xếp từ mới đến cũ, trùng ngày tham gia xếp theo mã tài khoản | Positive | High | | `US011-SC-04` |
| `TC-011-03-03` | Đổi số dòng mỗi trang qua các mức 10, 20, 50, 100 | Positive | Medium | | `US011-SC-06` |
| `TC-011-03-04` | Ô số kết quả mỗi trang chỉ có các giá trị hợp lệ | Positive | High | | `US011-SC-07` |
| `TC-011-03-05` | Quay về trang 1 khi điều kiện tra cứu thay đổi | Positive | Medium | | `US011-SC-09` |

## `TC-011-03-01` — Sắp xếp từ cũ đến mới, trùng ngày tham gia xếp theo mã tài khoản

**Precondition:** Dữ liệu nền và [`DS-03`](../00-moi-truong-va-du-lieu.md#ds-03--tài-khoản-trùng-ngày-tham-gia). ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "sort".
2. Bấm tiêu đề cột "Ngày tham gia" để chuyển sang chiều cũ đến mới.
3. Ghi lại thứ tự các dòng.
4. Bấm nút làm mới, ghi lại thứ tự các dòng.

**Test data:** Từ khoá "sort". Tài khoản: [`DS-03`](../00-moi-truong-va-du-lieu.md#ds-03--tài-khoản-trùng-ngày-tham-gia).

**Expected result:**

- Thứ tự ở bước 3 là U-301, U-302, U-303. Mũi tên cạnh tiêu đề "Ngày tham gia" chỉ chiều cũ đến mới.
- Thứ tự ở bước 4 giống bước 3.

**Actual result:**

**Bug:**

## `TC-011-03-02` — Sắp xếp từ mới đến cũ, trùng ngày tham gia xếp theo mã tài khoản

**Precondition:** Như `TC-011-03-01`.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "sort".
2. Ghi lại thứ tự các dòng ở chiều sắp xếp mặc định.
3. Bấm nút làm mới, ghi lại thứ tự các dòng.

**Test data:** Từ khoá "sort". Tài khoản: [`DS-03`](../00-moi-truong-va-du-lieu.md#ds-03--tài-khoản-trùng-ngày-tham-gia).

**Expected result:**

- Thứ tự ở bước 2 là U-303, U-302, U-301. Mũi tên cạnh tiêu đề "Ngày tham gia" chỉ chiều mới đến cũ.
- Thứ tự ở bước 3 giống bước 2.

**Actual result:**

**Bug:**

## `TC-011-03-03` — Đổi số dòng mỗi trang qua các mức 10, 20, 50, 100

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và [`DS-02`](../00-moi-truong-va-du-lieu.md#ds-02--tài-khoản-cho-tra-cứu-vượt-quá-100-bản-ghi). ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "an".
2. Chuyển sang trang 2.
3. Chọn số kết quả mỗi trang theo dòng dữ liệu.
4. Quan sát trang hiện tại, số dòng trên trang và chân bảng.

**Test data:**

| Dòng | Số kết quả mỗi trang |
|---|---|
| a | 10 |
| b | 20 |
| c | 50 |
| d | 100 |

**Expected result:**

- Màn hình quay về trang 1.
- Trang 1 hiển thị đúng số dòng bằng giá trị đã chọn; ô số kết quả mỗi trang hiển thị giá trị đó.
- Chân bảng ghi "1 – N trong 205 tài khoản", với N là giá trị đã chọn.

**Actual result:**

**Bug:**

## `TC-011-03-04` — Ô số kết quả mỗi trang chỉ có các giá trị hợp lệ

**Precondition:** Dữ liệu nền. ACC-ADMIN đăng nhập và mở màn hình Tài khoản lần đầu, chưa từng chọn số kết quả mỗi trang.

**Steps:**

1. Quan sát giá trị đang chọn của ô số kết quả mỗi trang.
2. Mở danh sách lựa chọn của ô.
3. Thử gõ "25" vào ô.

**Test data:** Giá trị gõ thử: 25.

**Expected result:**

- Giá trị mặc định là 10.
- Danh sách lựa chọn chỉ gồm 10, 20, 50 và 100.
- Ô không nhận giá trị gõ tay; giá trị đang chọn giữ nguyên.

**Actual result:**

**Bug:**

## `TC-011-03-05` — Quay về trang 1 khi điều kiện tra cứu thay đổi

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và [`DS-02`](../00-moi-truong-va-du-lieu.md#ds-02--tài-khoản-cho-tra-cứu-vượt-quá-100-bản-ghi). ACC-ADMIN đã đăng nhập. DevTools bật giả lập mạng "Slow 3G".

**Steps:**

1. Mở màn hình Tài khoản, chuyển tới trang 3. Xác nhận AN-025 đang hiển thị.
2. Thay đổi điều kiện tra cứu theo dòng dữ liệu.
3. Quan sát thân bảng trong lúc chờ kết quả và request `GET /users` vừa gửi trong tab Network.

**Test data:**

| Dòng | Thay đổi | Tham số mong đợi trong request |
|---|---|---|
| a | Nhập từ khoá "huy" | `q=huy` |
| b | Chọn vai trò "Người dùng" | `roleId={{role_user}}` |
| c | Chọn trạng thái "Đang hoạt động" | `status=active` |
| d | Bấm tiêu đề "Ngày tham gia" để chuyển sang cũ đến mới | `sortOrder=asc` |
| e | Chọn số kết quả mỗi trang là 50 | `pageSize=50` |

**Expected result:**

- Request mới có `page=1` và tham số mong đợi của dòng dữ liệu.
- Trong lúc chờ, thân bảng hiển thị trạng thái đang tải và không hiển thị AN-025.
- Sau khi tải xong, màn hình ở trang 1.

**Actual result:**

**Bug:**
