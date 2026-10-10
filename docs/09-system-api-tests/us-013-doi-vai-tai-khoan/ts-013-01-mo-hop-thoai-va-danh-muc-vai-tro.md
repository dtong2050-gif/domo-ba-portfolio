# `TS-013-01` — Mở hộp thoại và danh mục vai trò

**Liên quan:** [danh sách scenario của `US-013`](README.md), [user story `US-013`](../../06-user-stories/us-013-doi-vai-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** hộp thoại đổi vai trò mở cho đúng tài khoản đích, danh mục vai trò chỉ gồm vai trò được phép gán, và hộp thoại không cho tiếp tục khi danh mục tải lỗi hoặc rỗng.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-013-01-01` | Mở hộp thoại đổi vai trò cho đúng tài khoản đích | Positive | High | | `US013-SC-02` |
| `TC-013-01-02` | Danh sách "Vai trò mới" chỉ gồm vai trò được phép gán | Positive | High | | `US013-SC-03` |
| `TC-013-01-03` | Máy chủ trả danh mục vai trò được phép gán đúng điều kiện | Positive | High | | `US013-SC-03` |
| `TC-013-01-04` | Báo lỗi và không cho tiếp tục khi danh mục vai trò tải lỗi | Negative | High | | `US013-SC-04` |
| `TC-013-01-05` | Xử lý danh mục vai trò rỗng như lỗi tải | Negative | High | | `US013-SC-04` |
| `TC-013-01-06` | Từ chối yêu cầu danh mục vai trò thiếu hoặc sai mục đích | Negative | Low | | Đặc tả API |

## `TC-013-01-01` — Mở hộp thoại đổi vai trò cho đúng tài khoản đích

**Precondition:** Dữ liệu nền và [`DS-05`](../00-moi-truong-va-du-lieu.md#ds-05--hồ-sơ-đầy-đủ-của-acc-target). ACC-ADMIN đã đăng nhập.

**Steps:**

1. Mở màn hình Tài khoản, bấm "Bộ lọc", nhập "lan@example.test".
2. Xoá danh sách request trong tab Network.
3. Bấm nút thao tác trên dòng ACC-TARGET, chọn "Đổi vai trò".
4. Quan sát hộp thoại và tab Network.

**Test data:** ACC-TARGET.

**Expected result:**

- Hộp thoại có tiêu đề "Thay đổi vai trò" và dòng phụ "Chọn vai trò mới và nhập lý do thay đổi".
- Hộp thoại hiển thị tên "Lan Nguyễn", email "lan@example.test" và vai trò hiện tại "Người dùng".
- Ô "Vai trò mới" cho chọn đúng một vai trò; ô lý do trống với chữ gợi ý "Vui lòng nhập lý do"; bộ đếm "0/500"; nút "Tiếp tục" bị tắt; ghi chú "Thay đổi sẽ có hiệu lực ở phiên đăng nhập mới.".
- Hộp thoại không hiển thị ngày sinh, giới tính, địa chỉ, số điện thoại hoặc giới thiệu bản thân.
- Tab Network có request `GET /roles/assignable?purpose=USER_ROLE_CHANGE`.
- Vai trò của ACC-TARGET không đổi.

**Actual result:**

**Bug:**

## `TC-013-01-02` — Danh sách "Vai trò mới" chỉ gồm vai trò được phép gán

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập và mở hộp thoại đổi vai trò cho ACC-TARGET.

**Steps:**

1. Mở danh sách của ô "Vai trò mới".
2. Ghi lại các lựa chọn.

**Test data:** Vai trò dùng chung ROLE-USER, ROLE-ORG, ROLE-ADMIN, ROLE-MOD, ROLE-DRAFT, ROLE-ARCHIVED.

**Expected result:**

- Danh sách gồm đúng "Người dùng", "Nhà tổ chức", "Quản trị viên", "Kiểm duyệt viên".
- Danh sách không có "Vai trò nháp", "Vai trò đã ngừng dùng" và không có lựa chọn superAdmin.

**Actual result:**

**Bug:**

## `TC-013-01-03` — Máy chủ trả danh mục vai trò được phép gán đúng điều kiện

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `GET {{base_url}}/roles/assignable?purpose=USER_ROLE_CHANGE` với `{{token_admin}}`.

**Test data:** Vai trò dùng chung ROLE-USER, ROLE-ORG, ROLE-ADMIN, ROLE-MOD, ROLE-DRAFT, ROLE-ARCHIVED.

**Expected result:**

- Phản hồi `200`, `success: true`.
- `data` gồm đúng bốn phần tử có `id` là `{{role_user}}`, `{{role_org}}`, `{{role_admin}}`, `{{role_mod}}`; mỗi phần tử chỉ gồm `id` và `name`.
- `data` không chứa `{{role_draft}}`, `{{role_archived}}` và không có phần tử nào cấp quyền superAdmin.

**Actual result:**

**Bug:**

## `TC-013-01-04` — Báo lỗi và không cho tiếp tục khi danh mục vai trò tải lỗi

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập. Trình duyệt đi qua proxy kiểm thử.

**Steps:**

1. Cấu hình proxy xử lý request `GET /roles/assignable` theo dòng dữ liệu.
2. Mở hộp thoại đổi vai trò cho ACC-TARGET.
3. Nhập lý do "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.".
4. Quan sát hộp thoại.
5. Gỡ cấu hình proxy, bấm "Thử tải lại".

**Test data:**

| Dòng | Cấu hình proxy |
|---|---|
| a | Trả phản hồi `500` |
| b | Huỷ phản hồi, không trả kết quả |

**Expected result:**

- Sau bước 4: hộp thoại vẫn mở; khung lỗi hiển thị tiêu đề "Không tải được danh mục vai trò" và nút "Thử tải lại"; ô "Vai trò mới" hiển thị "—" và bị khoá; nút "Tiếp tục" bị tắt.
- Sau bước 5: danh mục vai trò tải lại và chọn được; lý do đã nhập được giữ nguyên.
- Vai trò của ACC-TARGET không đổi.

**Actual result:**

**Bug:**

## `TC-013-01-05` — Xử lý danh mục vai trò rỗng như lỗi tải

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập. Proxy kiểm thử trả phản hồi `200` với `data` là mảng rỗng cho request `GET /roles/assignable`.

**Steps:**

1. Mở hộp thoại đổi vai trò cho ACC-TARGET.
2. Quan sát hộp thoại.

**Test data:** Phản hồi `{"success": true, "message": "OK", "data": []}`.

**Expected result:**

- Hộp thoại hiển thị khung lỗi "Không tải được danh mục vai trò" và nút "Thử tải lại", giống trạng thái tải lỗi.
- Ô "Vai trò mới" hiển thị "—" và bị khoá; nút "Tiếp tục" bị tắt.
- Vai trò của ACC-TARGET không đổi.

**Actual result:**

**Bug:**

## `TC-013-01-06` — Từ chối yêu cầu danh mục vai trò thiếu hoặc sai mục đích

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi request theo dòng dữ liệu với `{{token_admin}}`.

**Test data:**

| Dòng | Request |
|---|---|
| a | `GET {{base_url}}/roles/assignable` |
| b | `GET {{base_url}}/roles/assignable?purpose=OTHER` |

**Expected result:**

- Mỗi dòng trả `400`, `errorCode: VALIDATION_FAILED`, `data: null`.
- `details` có phần tử với `path` là `["purpose"]`.

**Actual result:**

**Bug:**
