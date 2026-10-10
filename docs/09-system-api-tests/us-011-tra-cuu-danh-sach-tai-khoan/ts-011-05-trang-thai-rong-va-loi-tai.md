# `TS-011-05` — Trạng thái rỗng và lỗi tải

**Liên quan:** [danh sách scenario của `US-011`](README.md), [user story `US-011`](../../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** màn hình thông báo rõ khi không có kết quả hoặc không lấy được danh sách, và không hiển thị dữ liệu cũ như kết quả mới.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-011-05-01` | Hiển thị trạng thái rỗng và xoá điều kiện tra cứu | Positive | Medium | | `US011-SC-13` |
| `TC-011-05-02` | Hiển thị trạng thái lỗi khi không lấy được danh sách | Negative | High | | `US011-SC-14` |

## `TC-011-05-01` — Hiển thị trạng thái rỗng và xoá điều kiện tra cứu

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản. Bấm tiêu đề "Ngày tham gia" để chuyển sang cũ đến mới; chọn số kết quả mỗi trang là 50.
2. Bấm "Bộ lọc", chọn vai trò "Người dùng", trạng thái "Đang hoạt động", nhập "khong-ton-tai".
3. Quan sát màn hình.
4. Bấm "Xoá tìm kiếm và bộ lọc".

**Test data:** Từ khoá "khong-ton-tai".

**Expected result:**

- Sau bước 3: màn hình hiển thị "Không tìm thấy kết quả", "Không có bản ghi nào phù hợp với từ khoá hoặc bộ lọc hiện tại." và nút "Xoá tìm kiếm và bộ lọc"; chân bảng ghi "0 trong 0 tài khoản"; không hiển thị dữ liệu của lần tra cứu trước và không có cột ô chọn.
- Sau bước 4: ô tìm kiếm trống; bộ lọc vai trò là "Tất cả vai trò", bộ lọc trạng thái là "Tất cả trạng thái"; màn hình ở trang 1; chiều cũ đến mới và số kết quả mỗi trang 50 được giữ nguyên.

**Actual result:**

**Bug:**

## `TC-011-05-02` — Hiển thị trạng thái lỗi khi không lấy được danh sách

**Precondition:** Dữ liệu nền và tài khoản U-101 (Huy Tran, `huy@example.test`). ACC-ADMIN đã đăng nhập. Trình duyệt đi qua proxy kiểm thử.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "huy". Xác nhận U-101 đang hiển thị.
2. Cấu hình proxy xử lý request `GET /users` theo dòng dữ liệu.
3. Bấm nút làm mới, quan sát màn hình.
4. Gỡ cấu hình proxy, bấm "Thử lại".

**Test data:**

| Dòng | Cấu hình proxy |
|---|---|
| a | Trả phản hồi `500` |
| b | Huỷ phản hồi, không trả kết quả |

**Expected result:**

- Sau bước 3: thân bảng hiển thị "Không thể tải danh sách tài khoản.", "Vui lòng thử lại." và nút "Thử lại"; chân bảng ghi "— trong — tài khoản"; bốn nút chuyển trang bị tắt.
- Từ khoá "huy" vẫn trong ô tìm kiếm; bảng không hiển thị U-101.
- Màn hình không hiển thị thông báo không đủ quyền hay yêu cầu đăng nhập lại.
- Sau bước 4: danh sách hiển thị U-101.
- Không dữ liệu tài khoản nào bị thay đổi.

**Actual result:**

**Bug:**
