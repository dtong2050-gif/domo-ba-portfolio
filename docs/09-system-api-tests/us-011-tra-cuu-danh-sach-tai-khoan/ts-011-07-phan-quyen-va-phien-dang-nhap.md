# `TS-011-07` — Phân quyền và phiên đăng nhập

**Liên quan:** [danh sách scenario của `US-011`](README.md), [user story `US-011`](../../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** chỉ tài khoản có quyền quản trị tài khoản và phiên đăng nhập hợp lệ mới xem được danh sách; màn hình phân biệt rõ thiếu quyền với phiên hết hạn.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-011-07-01` | Tài khoản không có quyền quản trị không truy cập được màn hình Tài khoản | Negative | High | | `US011-SC-15` |
| `TC-011-07-02` | Máy chủ từ chối yêu cầu danh sách của tài khoản không có quyền | Negative | High | | `US011-SC-15` |
| `TC-011-07-03` | Máy chủ từ chối yêu cầu danh sách khi thiếu hoặc sai mã đăng nhập | Negative | High | | `US011-SC-16` |
| `TC-011-07-04` | Chuyển về trang đăng nhập khi phiên hết hạn lúc làm mới danh sách | Negative | High | | `US011-SC-17` |

## `TC-011-07-01` — Tài khoản không có quyền quản trị không truy cập được màn hình Tài khoản

**Precondition:** Dữ liệu nền. ACC-NOPERM đã đăng nhập.

**Steps:**

1. Quan sát thanh điều hướng bên trái.
2. Mở trực tiếp đường dẫn của màn hình Tài khoản.

**Test data:** Đường dẫn màn hình Tài khoản của môi trường kiểm thử.

**Expected result:**

- Thanh điều hướng không có mục "Tài khoản".
- Màn hình hiển thị "Bạn không có quyền quản lý tài khoản." và "Bạn không thể thực hiện thao tác này."; chỉ còn tiêu đề màn hình.
- Không hiển thị thanh công cụ, bảng danh sách hay nút "Thử lại"; không có dữ liệu tài khoản nào được hiển thị.

**Actual result:**

**Bug:**

## `TC-011-07-02` — Máy chủ từ chối yêu cầu danh sách của tài khoản không có quyền

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `GET {{base_url}}/users` với `{{token_noperm}}`.

**Test data:** `{{token_noperm}}`.

**Expected result:**

- Phản hồi `403`, `errorCode: PERMISSION_DENIED`, `data: null`.
- Phản hồi không chứa dữ liệu tài khoản nào.

**Actual result:**

**Bug:**

## `TC-011-07-03` — Máy chủ từ chối yêu cầu danh sách khi thiếu hoặc sai mã đăng nhập

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `GET {{base_url}}/users` với header `Authorization` theo dòng dữ liệu.

**Test data:**

| Dòng | Header `Authorization` | Mã lỗi mong đợi |
|---|---|---|
| a | Không gửi header | `401 AUTH_TOKEN_MISSING` |
| b | `{{token_expired}}` | `401 AUTH_TOKEN_INVALID` |
| c | `{{token_tampered}}` | `401 AUTH_TOKEN_INVALID` |
| d | `{{token_noperm_expired}}` | `401 AUTH_TOKEN_INVALID` |

**Expected result:**

- Mỗi dòng trả đúng mã lỗi mong đợi, `data: null`.
- Không dòng nào trả `403`, kể cả dòng d dùng mã đăng nhập của tài khoản không có quyền.

**Actual result:**

**Bug:**

## `TC-011-07-04` — Chuyển về trang đăng nhập khi phiên hết hạn lúc làm mới danh sách

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập và đang xem màn hình Tài khoản.

**Steps:**

1. Thay mã đăng nhập trình duyệt đang lưu bằng `{{token_expired}}`.
2. Bấm nút làm mới.
3. Quan sát màn hình.

**Test data:** `{{token_expired}}`.

**Expected result:**

- Hệ thống chuyển thẳng về trang đăng nhập; trang đăng nhập hiển thị "Phiên đăng nhập đã hết hạn" và "Vui lòng đăng nhập lại.".
- Không hiển thị thông báo không đủ quyền.
- Không hiển thị dữ liệu tài khoản nào.

**Actual result:**

**Bug:**
