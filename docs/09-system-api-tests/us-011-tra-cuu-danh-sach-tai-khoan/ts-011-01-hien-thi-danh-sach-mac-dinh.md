# `TS-011-01` — Hiển thị danh sách mặc định

**Liên quan:** [danh sách scenario của `US-011`](README.md), [user story `US-011`](../../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** khi mở màn hình Tài khoản, danh sách hiển thị đúng cột, thứ tự và số dòng mặc định, và phản hồi của máy chủ không chứa thông tin cá nhân nhạy cảm.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-011-01-01` | Hiển thị danh sách với cột, thứ tự và số dòng mặc định | Positive | High | | `US011-SC-01` |
| `TC-011-01-02` | Phản hồi danh sách không chứa trường thông tin nhạy cảm | Positive | High | | `US011-SC-01` |

## `TC-011-01-01` — Hiển thị danh sách với cột, thứ tự và số dòng mặc định

**Precondition:** Dữ liệu tài khoản chỉ gồm bốn tài khoản trong Test data. U-001 đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản.
2. Quan sát bảng, chân bảng và nút thao tác trên từng dòng.

**Test data:**

| Mã | Tên | Email | Vai trò | Trạng thái | Ngày tham gia | superAdmin |
|---|---|---|---|---|---|---|
| U-001 | Admin An | `admin@example.test` | Quản trị viên | Đang hoạt động | 01/08/2026 08:00 | Không |
| U-002 | Huy Tran | `huy@example.test` | Người dùng | Đang hoạt động | 04/08/2026 08:00 | Không |
| U-003 | Lan Le | `lan.le@example.test` | Quản trị viên | Đã vô hiệu hoá | 03/08/2026 08:00 | Không |
| U-004 | Root Domo | `root@example.test` | Quản trị viên | Đang hoạt động | 02/08/2026 08:00 | Có |

**Expected result:**

- Bảng hiển thị bốn dòng theo thứ tự U-002, U-003, U-004, U-001.
- Bảng có các cột Tài khoản (tên và email), Vai trò, Trạng thái, Ngày tham gia và Thao tác. Ngày tham gia hiển thị dạng dd/mm/yyyy.
- Danh sách sắp xếp theo ngày tham gia từ mới đến cũ. Số kết quả mỗi trang là 10. Chân bảng ghi "1 – 4 trong 4 tài khoản".
- Dòng U-002 có nút thao tác. Dòng U-001 có thẻ "Bạn" và không có nút thao tác. Dòng U-004 có thẻ "superAdmin", nền xám nhạt và không có nút thao tác.
- Bảng không hiển thị ngày sinh, giới tính, địa chỉ, số điện thoại hoặc giới thiệu bản thân.
- Vai trò và trạng thái của cả bốn tài khoản không đổi.

**Actual result:**

**Bug:**

## `TC-011-01-02` — Phản hồi danh sách không chứa trường thông tin nhạy cảm

**Precondition:** Dữ liệu nền và [`DS-05`](../00-moi-truong-va-du-lieu.md#ds-05--hồ-sơ-đầy-đủ-của-acc-target).

**Steps:**

1. Gửi `GET {{base_url}}/users?q=lan@example.test` với `{{token_admin}}`.
2. Kiểm tra các trường của từng phần tử trong `data`.

**Test data:** [`DS-05`](../00-moi-truong-va-du-lieu.md#ds-05--hồ-sơ-đầy-đủ-của-acc-target).

**Expected result:**

- Phản hồi `200`, `success: true`.
- Mỗi phần tử trong `data` chỉ gồm các trường `id`, `name`, `email`, `roleId`, `deletedAt`, `createdAt`, `isSuperAdmin`, `allowedActions`, `currentRoleRevision`.
- Phản hồi không chứa các trường `dateOfBirth`, `gender`, `address`, `phone`, `bio` và không chứa giá trị nào trong [`DS-05`](../00-moi-truong-va-du-lieu.md#ds-05--hồ-sơ-đầy-đủ-của-acc-target).

**Actual result:**

**Bug:**
