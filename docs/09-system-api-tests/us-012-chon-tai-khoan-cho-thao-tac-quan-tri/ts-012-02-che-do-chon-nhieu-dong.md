# `TS-012-02` — Chế độ chọn nhiều dòng

**Liên quan:** [danh sách scenario của `US-012`](README.md), [user story `US-012`](../../06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** chế độ chọn nhiều dòng chỉ bật khi lọc theo một trạng thái cụ thể; ô chọn tất cả và ô chọn từng dòng cập nhật đúng danh sách chọn trên mọi trang và không bao gồm tài khoản được bảo vệ.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-012-02-01` | Không hiển thị ô chọn khi chưa lọc theo trạng thái cụ thể | Negative | Medium | | `US012-SC-04` |
| `TC-012-02-02` | Chọn tất cả tài khoản đang hoạt động trên mọi trang | Positive | High | | `US012-SC-05` |
| `TC-012-02-03` | Chọn tất cả tài khoản đã vô hiệu hoá trên mọi trang | Positive | High | | `US012-SC-05` |
| `TC-012-02-04` | Ô chọn tất cả hiển thị đúng phạm vi đang chọn | Positive | Medium | | `US012-SC-08` |
| `TC-012-02-05` | Bấm ô chọn tất cả cập nhật toàn bộ danh sách chọn | Positive | High | | `US012-SC-09` |
| `TC-012-02-06` | Chọn hoặc bỏ chọn một dòng không làm đổi lựa chọn khác | Positive | Medium | | `US012-SC-10` |
| `TC-012-02-07` | Không đưa tài khoản của chính người thao tác vào danh sách khôi phục hàng loạt | Negative | High | | `US012-SC-12` |

## `TC-012-02-01` — Không hiển thị ô chọn khi chưa lọc theo trạng thái cụ thể

**Precondition:** Dữ liệu nền và sáu tài khoản trong Test data. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "mix".
2. Thực hiện thao tác theo dòng dữ liệu.
3. Quan sát tiêu đề bảng, các dòng và thanh hành động.

**Test data:**

| Mã | Email | Trạng thái |
|---|---|---|
| MIX-A01, MIX-A02, MIX-A03 | `mixa01@example.test` đến `mixa03@example.test` | Đang hoạt động |
| MIX-D01, MIX-D02, MIX-D03 | `mixd01@example.test` đến `mixd03@example.test` | Đã vô hiệu hoá |

| Dòng | Thao tác |
|---|---|
| a | Giữ bộ lọc trạng thái "Tất cả trạng thái" |
| b | Chọn trạng thái "Đang hoạt động", sau đó bấm nút đóng trên thẻ "Trạng thái: Đang hoạt động" |

**Expected result:**

- Danh sách gồm đúng sáu tài khoản trong Test data trên một trang.
- Không có ô chọn tất cả và không có ô chọn trên từng dòng.
- Không tài khoản nào được chọn; thanh hành động không hiển thị.

**Actual result:**

**Bug:**

## `TC-012-02-02` — Chọn tất cả tài khoản đang hoạt động trên mọi trang

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và [`DS-04`](../00-moi-truong-va-du-lieu.md#ds-04--tài-khoản-cho-chọn-nhiều-dòng); cả 23 tài khoản đang hoạt động. ACC-ADMIN đã đăng nhập. Số kết quả mỗi trang là 10.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", chọn trạng thái "Đang hoạt động".
2. Quan sát chân bảng và tiêu đề bảng.
3. Bấm ô chọn tất cả.
4. Lần lượt mở trang 1, 2 và 3, quan sát ô chọn trên từng dòng.

**Test data:** [`DS-04`](../00-moi-truong-va-du-lieu.md#ds-04--tài-khoản-cho-chọn-nhiều-dòng), 21 tài khoản ACC-003 đến ACC-023 ở trạng thái Đang hoạt động.

**Expected result:**

- Sau bước 2: chân bảng ghi "1 – 10 trong 23 tài khoản"; danh sách có 3 trang; ô chọn tất cả hiển thị.
- Sau bước 3: danh sách chọn gồm đúng 21 tài khoản ACC-003 đến ACC-023 trên cả 3 trang.
- Ô chọn trên dòng ACC-ADMIN và ACC-ROOT bị khoá, hiển thị màu xám và không được chọn.
- Thanh hành động hiển thị ngay dưới khu vực bộ lọc với "Đã chọn 21 tài khoản", chỉ gồm nút "Vô hiệu hoá" và nút "Bỏ chọn".

**Actual result:**

**Bug:**

## `TC-012-02-03` — Chọn tất cả tài khoản đã vô hiệu hoá trên mọi trang

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và [`DS-04`](../00-moi-truong-va-du-lieu.md#ds-04--tài-khoản-cho-chọn-nhiều-dòng); 21 tài khoản ACC-003 đến ACC-023 ở trạng thái đã vô hiệu hoá. ACC-ADMIN đã đăng nhập; sau đó `users.deletedAt` của ACC-ADMIN và ACC-ROOT được cập nhật thành thời điểm hiện tại, phiên đăng nhập của ACC-ADMIN vẫn còn hạn. Số kết quả mỗi trang là 10.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", chọn trạng thái "Đã vô hiệu hoá".
2. Quan sát chân bảng và tiêu đề bảng.
3. Bấm ô chọn tất cả.
4. Lần lượt mở trang 1, 2 và 3, quan sát ô chọn trên từng dòng.

**Test data:** [`DS-04`](../00-moi-truong-va-du-lieu.md#ds-04--tài-khoản-cho-chọn-nhiều-dòng), 21 tài khoản ACC-003 đến ACC-023 ở trạng thái Đã vô hiệu hoá.

**Expected result:**

- Sau bước 2: chân bảng ghi "1 – 10 trong 23 tài khoản"; danh sách có 3 trang; ô chọn tất cả hiển thị.
- Sau bước 3: danh sách chọn gồm đúng 21 tài khoản ACC-003 đến ACC-023 trên cả 3 trang.
- Ô chọn trên dòng ACC-ADMIN và ACC-ROOT bị khoá, hiển thị màu xám và không được chọn.
- Thanh hành động hiển thị "Đã chọn 21 tài khoản", chỉ gồm nút "Khôi phục" và nút "Bỏ chọn".

**Actual result:**

**Bug:**

## `TC-012-02-04` — Ô chọn tất cả hiển thị đúng phạm vi đang chọn

**Precondition:** Dữ liệu nền và năm tài khoản trong Test data. ACC-ADMIN đã đăng nhập, đang tra cứu từ khoá "acc50" với trạng thái "Đang hoạt động".

**Steps:**

1. Chọn các dòng theo cột "Danh sách chọn" của dòng dữ liệu bằng ô chọn trên từng dòng.
2. Quan sát ô chọn tất cả trên tiêu đề bảng.

**Test data:** ACC-501 đến ACC-505, email `acc501@example.test` đến `acc505@example.test`, Đang hoạt động.

| Dòng | Danh sách chọn | Trạng thái ô chọn tất cả |
|---|---|---|
| a | Không chọn dòng nào | Chưa chọn |
| b | ACC-501, ACC-502 | Chọn một phần |
| c | ACC-501 đến ACC-505 | Chọn tất cả |

**Expected result:**

- Ô chọn tất cả ở đúng trạng thái của dòng dữ liệu.
- Danh sách chọn không thay đổi khi quan sát.

**Actual result:**

**Bug:**

## `TC-012-02-05` — Bấm ô chọn tất cả cập nhật toàn bộ danh sách chọn

**Precondition:** Dữ liệu nền và năm tài khoản trong Test data. ACC-ADMIN đã đăng nhập, đang tra cứu từ khoá "acc60" với trạng thái "Đang hoạt động".

**Steps:**

1. Thiết lập danh sách chọn ban đầu theo dòng dữ liệu.
2. Bấm ô chọn tất cả.
3. Quan sát danh sách chọn, ô chọn tất cả và thanh hành động.

**Test data:** ACC-601 đến ACC-605, email `acc601@example.test` đến `acc605@example.test`, Đang hoạt động.

| Dòng | Chọn ban đầu | Chọn sau khi bấm | Ô chọn tất cả sau khi bấm |
|---|---|---|---|
| a | Không có | ACC-601 đến ACC-605 | Chọn tất cả |
| b | ACC-601, ACC-602 | ACC-601 đến ACC-605 | Chọn tất cả |
| c | ACC-601 đến ACC-605 | Không có | Chưa chọn |

**Expected result:**

- Danh sách chọn và ô chọn tất cả khớp với dòng dữ liệu.
- Dòng a và b: thanh hành động hiển thị "Đã chọn 5 tài khoản". Dòng c: thanh hành động không hiển thị.

**Actual result:**

**Bug:**

## `TC-012-02-06` — Chọn hoặc bỏ chọn một dòng không làm đổi lựa chọn khác

**Precondition:** Dữ liệu nền và năm tài khoản trong Test data. ACC-ADMIN đã đăng nhập, đang tra cứu từ khoá "acc70" với trạng thái "Đang hoạt động".

**Steps:**

1. Thiết lập danh sách chọn ban đầu theo dòng dữ liệu.
2. Bấm ô chọn trên dòng ACC-703.
3. Quan sát danh sách chọn, ô chọn tất cả và thanh hành động.

**Test data:** ACC-701 đến ACC-705, email `acc701@example.test` đến `acc705@example.test`, Đang hoạt động.

| Dòng | Chọn ban đầu | Chọn sau khi bấm | Số tài khoản đã chọn |
|---|---|---|---|
| a | ACC-701, ACC-702 | ACC-701, ACC-702, ACC-703 | 3 |
| b | ACC-701 đến ACC-705 | ACC-701, ACC-702, ACC-704, ACC-705 | 4 |

**Expected result:**

- Danh sách chọn khớp với cột "Chọn sau khi bấm".
- Ô chọn tất cả ở trạng thái "Chọn một phần".
- Thanh hành động hiển thị "Đã chọn N tài khoản", với N theo dòng dữ liệu.

**Actual result:**

**Bug:**

## `TC-012-02-07` — Không đưa tài khoản của chính người thao tác vào danh sách khôi phục hàng loạt

**Precondition:** Dữ liệu tài khoản chỉ gồm ACC-ADMIN, ACC-ROOT và ACC-901. ACC-ADMIN đã đăng nhập; sau đó `users.deletedAt` của ACC-ADMIN được cập nhật thành thời điểm hiện tại, phiên đăng nhập vẫn còn hạn.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", chọn trạng thái "Đã vô hiệu hoá".
2. Bấm ô chọn tất cả.
3. Quan sát danh sách chọn và dòng ACC-ADMIN.
4. Bấm nút thao tác trên dòng ACC-ADMIN.

**Test data:** ACC-901, email `acc901@example.test`, vai trò Người dùng, Đã vô hiệu hoá.

**Expected result:**

- Danh sách chọn chỉ gồm ACC-901; thanh hành động hiển thị "Đã chọn 1 tài khoản".
- Ô chọn trên dòng ACC-ADMIN bị khoá; ACC-ADMIN không nằm trong danh sách chọn.
- Menu thao tác trên dòng ACC-ADMIN có lựa chọn "Khôi phục".
- Chưa hộp thoại xác nhận khôi phục nào được mở; trạng thái của ACC-ADMIN và ACC-901 không đổi.

**Actual result:**

**Bug:**
