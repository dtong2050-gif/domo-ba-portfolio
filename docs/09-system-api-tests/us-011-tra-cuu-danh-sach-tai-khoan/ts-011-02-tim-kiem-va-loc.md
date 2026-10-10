# `TS-011-02` — Tìm kiếm và lọc

**Liên quan:** [danh sách scenario của `US-011`](README.md), [user story `US-011`](../../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** từ khoá, bộ lọc vai trò và bộ lọc trạng thái trả đúng tập kết quả, được áp dụng trên toàn bộ dữ liệu trước khi chia trang.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-011-02-01` | Tìm theo tên không phân biệt chữ hoa chữ thường | Positive | High | | `US011-SC-02` |
| `TC-011-02-02` | Tìm theo email không phân biệt chữ hoa chữ thường | Positive | High | | `US011-SC-02` |
| `TC-011-02-03` | Bỏ khoảng trắng đầu cuối của từ khoá | Positive | High | | `US011-SC-02` |
| `TC-011-02-04` | Kết hợp đồng thời từ khoá, vai trò và trạng thái | Positive | High | | `US011-SC-03` |
| `TC-011-02-05` | Tìm theo từ khoá khi kết quả vượt quá 100 tài khoản | Positive | High | | `US011-SC-05` |
| `TC-011-02-06` | Lọc theo vai trò và trạng thái khi kết quả vượt quá 100 tài khoản | Positive | High | | `US011-SC-05` |

## `TC-011-02-01` — Tìm theo tên không phân biệt chữ hoa chữ thường

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và [`DS-01`](../00-moi-truong-va-du-lieu.md#ds-01--tài-khoản-tìm-kiếm-theo-tên-và-email). ACC-ADMIN đã đăng nhập và đang ở trang 1 của màn hình Tài khoản.

**Steps:**

1. Bấm "Bộ lọc".
2. Nhập "hUy" vào ô tìm kiếm, không bấm Enter.
3. Quan sát danh sách sau khi ngừng gõ.

**Test data:** Từ khoá "hUy". Tài khoản: [`DS-01`](../00-moi-truong-va-du-lieu.md#ds-01--tài-khoản-tìm-kiếm-theo-tên-và-email).

**Expected result:**

- Danh sách tự cập nhật sau khi ngừng gõ, không cần bấm Enter.
- Danh sách chỉ gồm U-101 và U-102; không có U-103.
- Chân bảng ghi "1 – 2 trong 2 tài khoản".

**Actual result:**

**Bug:**

## `TC-011-02-02` — Tìm theo email không phân biệt chữ hoa chữ thường

**Precondition:** Như `TC-011-02-01`.

**Steps:**

1. Bấm "Bộ lọc".
2. Nhập "HUY@EXAMPLE.TEST" vào ô tìm kiếm.
3. Quan sát danh sách.

**Test data:** Từ khoá "HUY@EXAMPLE.TEST". Tài khoản: [`DS-01`](../00-moi-truong-va-du-lieu.md#ds-01--tài-khoản-tìm-kiếm-theo-tên-và-email).

**Expected result:**

- Danh sách chỉ gồm U-101; không có U-102 và U-103.
- Chân bảng ghi "1 – 1 trong 1 tài khoản".

**Actual result:**

**Bug:**

## `TC-011-02-03` — Bỏ khoảng trắng đầu cuối của từ khoá

**Precondition:** Như `TC-011-02-01`.

**Steps:**

1. Bấm "Bộ lọc".
2. Nhập từ khoá trong Test data vào ô tìm kiếm.
3. Quan sát danh sách.

**Test data:** Từ khoá "huy" có hai khoảng trắng ở đầu và hai khoảng trắng ở cuối. Tài khoản: [`DS-01`](../00-moi-truong-va-du-lieu.md#ds-01--tài-khoản-tìm-kiếm-theo-tên-và-email).

**Expected result:**

- Danh sách chỉ gồm U-101 và U-102, giống kết quả khi tìm "huy".
- Chân bảng ghi "1 – 2 trong 2 tài khoản".

**Actual result:**

**Bug:**

## `TC-011-02-04` — Kết hợp đồng thời từ khoá, vai trò và trạng thái

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và bốn tài khoản trong Test data. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc".
2. Nhập từ khoá, chọn vai trò và trạng thái theo dòng dữ liệu.
3. Quan sát danh sách và các thẻ bộ lọc đang áp dụng.

**Test data:**

| Mã | Tên | Email | Vai trò | Trạng thái |
|---|---|---|---|---|
| U-201 | Huy Tran | `huy@example.test` | Người dùng | Đang hoạt động |
| U-202 | Huy Admin | `huy.admin@example.test` | Quản trị viên | Đã vô hiệu hoá |
| U-203 | Lan User | `lan.user@example.test` | Người dùng | Đã vô hiệu hoá |
| U-204 | Hanh Tran | `hanh@example.test` | Người dùng | Đang hoạt động |

| Dòng | Từ khoá | Vai trò | Trạng thái | Kết quả |
|---|---|---|---|---|
| a | huy | Người dùng | Đang hoạt động | U-201 |
| b | (để trống) | Người dùng | Đã vô hiệu hoá | U-203 |
| c | huy | Quản trị viên | Đã vô hiệu hoá | U-202 |

**Expected result:**

- Với mỗi dòng, danh sách chỉ gồm tài khoản ở cột Kết quả; các tài khoản còn lại trong U-201 đến U-204 không xuất hiện.
- Mỗi bộ lọc khác "Tất cả" có một thẻ hiển thị giá trị đang áp dụng.

**Actual result:**

**Bug:**

## `TC-011-02-05` — Tìm theo từ khoá khi kết quả vượt quá 100 tài khoản

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và [`DS-02`](../00-moi-truong-va-du-lieu.md#ds-02--tài-khoản-cho-tra-cứu-vượt-quá-100-bản-ghi). ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, chọn số kết quả mỗi trang là 100.
2. Bấm "Bộ lọc", nhập "an" vào ô tìm kiếm.
3. Lần lượt mở trang 1, 2 và 3, ghi lại các tài khoản trên từng trang.

**Test data:** Từ khoá "an". Tài khoản: [`DS-02`](../00-moi-truong-va-du-lieu.md#ds-02--tài-khoản-cho-tra-cứu-vượt-quá-100-bản-ghi).

**Expected result:**

- Chân bảng ghi "1 – 100 trong 205 tài khoản"; danh sách có 3 trang.
- Trang 1 gồm AN-001 đến AN-100, trang 2 gồm AN-101 đến AN-200, trang 3 gồm AN-201 đến AN-205.
- Không có tài khoản BINH nào trong kết quả.
- Không tài khoản nào bị lặp hoặc bị thiếu giữa ba trang.

**Actual result:**

**Bug:**

## `TC-011-02-06` — Lọc theo vai trò và trạng thái khi kết quả vượt quá 100 tài khoản

**Precondition:** Như `TC-011-02-05`.

**Steps:**

1. Mở màn hình Tài khoản, chọn số kết quả mỗi trang là 100.
2. Bấm "Bộ lọc", chọn vai trò "Người dùng" và trạng thái "Đang hoạt động".
3. Lần lượt mở trang 1, 2 và 3, ghi lại các tài khoản trên từng trang.

**Test data:** [`DS-02`](../00-moi-truong-va-du-lieu.md#ds-02--tài-khoản-cho-tra-cứu-vượt-quá-100-bản-ghi).

**Expected result:** Như `TC-011-02-05`.

**Actual result:**

**Bug:**
