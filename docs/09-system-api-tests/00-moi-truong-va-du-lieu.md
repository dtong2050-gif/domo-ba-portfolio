# Môi trường, công cụ và dữ liệu

Tài liệu này quy định môi trường, công cụ và dữ liệu dùng chung cho mọi test case trong [bộ kiểm thử hệ thống và API](README.md).
Test case chỉ ghi phần dữ liệu riêng của mình và tham chiếu tới tài khoản, vai trò, biến và bộ dữ liệu quy định tại đây.

## 1. Môi trường và công cụ

| Hạng mục | Nội dung |
|---|---|
| Môi trường | Môi trường kiểm thử đã cài phiên bản cần kiểm thử; trang quản trị trên web |
| Trình duyệt | Chrome bản mới nhất. Dùng DevTools để theo dõi request trong tab Network và giả lập mạng chậm |
| Gửi yêu cầu API | Postman, dùng environment với các biến tại mục 4 |
| Chặn và sửa phản hồi | Proxy kiểm thử (mitmproxy hoặc Charles), dùng để trả lỗi `500`, trả danh sách rỗng hoặc huỷ phản hồi của một request |
| Gây lỗi khi ghi | Cơ chế gây lỗi trong giao dịch ghi vai trò, do đội phát triển cung cấp trên môi trường kiểm thử |
| Cơ sở dữ liệu | Quyền đọc và cập nhật dữ liệu kiểm thử trên các bảng `users`, `roles`, `user_action_logs` qua DBeaver |
| Làm hết hạn phiên trên giao diện | Thay mã đăng nhập mà trình duyệt đang lưu bằng `{{token_expired}}` |

## 2. Tài khoản dùng chung

| Mã | Tên | Email | Vai trò | Trạng thái | Ghi chú |
|---|---|---|---|---|---|
| ACC-ADMIN | QA Admin | `qa.admin@example.test` | Quản trị viên | Đang hoạt động | Người thực hiện chính |
| ACC-ADMIN-2 | QA Admin 2 | `qa.admin2@example.test` | Quản trị viên | Đang hoạt động | Phiên quản trị thứ hai, dùng cho thao tác đồng thời |
| ACC-NOPERM | QA User | `qa.user@example.test` | Người dùng | Đang hoạt động | Không có quyền quản trị tài khoản |
| ACC-TARGET | Lan Nguyễn | `lan@example.test` | Người dùng | Đang hoạt động | Tài khoản đích hợp lệ |
| ACC-ORG | CLB Chạy bộ | `org@example.test` | Nhà tổ chức | Đang hoạt động | Sở hữu thử thách CHALLENGE-101, có người tham gia và minh chứng |
| ACC-DISABLED | Disabled Demo | `disabled@example.test` | Người dùng | Đã vô hiệu hoá | |
| ACC-ROOT | Root Domo | `root@example.test` | Quản trị viên | Đang hoạt động | Tài khoản superAdmin |

## 3. Vai trò dùng chung

| Mã | Tên vai trò | Trạng thái công bố | Vòng đời | Ghi chú |
|---|---|---|---|---|
| ROLE-USER | Người dùng | Đã công bố | Đang dùng | Vai trò gốc, không có quyền quản trị tài khoản |
| ROLE-ORG | Nhà tổ chức | Đã công bố | Đang dùng | Vai trò gốc |
| ROLE-ADMIN | Quản trị viên | Đã công bố | Đang dùng | Vai trò gốc, có `user:read_all` và `user:manage_role` |
| ROLE-MOD | Kiểm duyệt viên | Đã công bố | Đang dùng | Vai trò tự tạo |
| ROLE-DRAFT | Vai trò nháp | Nháp | Đang dùng | Vai trò tự tạo |
| ROLE-ARCHIVED | Vai trò đã ngừng dùng | Đã công bố | Đã ngừng dùng | Vai trò tự tạo |

## 4. Biến và body dùng chung trong Postman

| Biến | Giá trị |
|---|---|
| `{{base_url}}` | Địa chỉ máy chủ của môi trường kiểm thử |
| `{{token_admin}}`, `{{token_admin2}}`, `{{token_root}}`, `{{token_noperm}}` | Mã đăng nhập hợp lệ của ACC-ADMIN, ACC-ADMIN-2, ACC-ROOT, ACC-NOPERM |
| `{{token_expired}}` | Mã đăng nhập đã hết hạn của ACC-ADMIN |
| `{{token_noperm_expired}}` | Mã đăng nhập đã hết hạn của ACC-NOPERM |
| `{{token_tampered}}` | `{{token_admin}}` bị sửa một ký tự ở phần chữ ký |
| `{{id_admin}}`, `{{id_target}}`, `{{id_org}}`, `{{id_disabled}}`, `{{id_root}}` | Mã tài khoản tương ứng tại mục 2 |
| `{{id_missing}}` | `00000000-0000-4000-8000-000000000000`, đúng định dạng UUID, không thuộc tài khoản nào |
| `{{role_user}}`, `{{role_org}}`, `{{role_admin}}`, `{{role_mod}}`, `{{role_draft}}`, `{{role_archived}}` | Mã vai trò tương ứng tại mục 3 |
| `{{rev_<tài khoản>}}` | Giá trị `currentRoleRevision` của tài khoản, đọc bằng `GET /users` ngay trước khi gửi lệnh |

**Body đổi vai trò chuẩn** dùng cho `PATCH /users/{id}/role`. Test case thay từng trường theo Test data.

```json
{
  "roleId": "{{role_admin}}",
  "currentRoleRevision": {{rev_target}},
  "reason": "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới."
}
```

Mọi request gửi kèm header `Authorization: Bearer <mã đăng nhập>` theo Steps và header `Content-Type: application/json` khi có body.

## 5. Quy định dữ liệu và cách đối chiếu

- Chỉ dùng dữ liệu giả; email thuộc tên miền `example.test`.
- Mỗi test case bắt đầu từ dữ liệu nền gồm tài khoản tại mục 2 và vai trò tại mục 3, trừ khi Precondition ghi khác.
- Precondition có mã `DS-xx` dùng bộ dữ liệu tại mục 6.
- Precondition ghi "Dữ liệu tài khoản chỉ gồm..." được thực hiện trên bản sao cơ sở dữ liệu dành riêng cho test case đó.
- Sau mỗi test case, QA khôi phục dữ liệu về dữ liệu nền.
- "Vai trò không đổi" trong Expected result nghĩa là `roleId` và `currentRoleRevision` của tài khoản trong phản hồi `GET /users` và trong bảng `users` giữ nguyên giá trị trước khi thực hiện.
- "Không có bản ghi nhật ký mới" nghĩa là số bản ghi trong `user_action_logs` không đổi so với trước khi thực hiện.
- "Không gửi thông báo" nghĩa là tài khoản đích không có thông báo mới trong danh sách thông báo trong ứng dụng.
- Không ghi mã đăng nhập, mật khẩu hoặc dữ liệu thật vào tài liệu và kết quả kiểm thử.

## 6. Bộ dữ liệu dùng chung

Bộ dữ liệu được nạp vào môi trường theo Precondition của test case. Precondition ghi rõ dữ liệu tài khoản của môi trường gồm dữ liệu nền và bộ dữ liệu, hay chỉ gồm một số tài khoản và bộ dữ liệu.

| Mã | Nội dung | Test case sử dụng |
|---|---|---|
| [`DS-01`](#ds-01--tài-khoản-tìm-kiếm-theo-tên-và-email) | Tài khoản tìm kiếm theo tên và email | `TC-011-02-01`, `TC-011-02-02`, `TC-011-02-03`, `TC-011-04-01` |
| [`DS-02`](#ds-02--tài-khoản-cho-tra-cứu-vượt-quá-100-bản-ghi) | Tài khoản cho tra cứu vượt quá 100 bản ghi | `TC-011-02-05`, `TC-011-02-06`, `TC-011-03-03`, `TC-011-03-05`, `TC-011-04-03` |
| [`DS-03`](#ds-03--tài-khoản-trùng-ngày-tham-gia) | Tài khoản trùng ngày tham gia | `TC-011-03-01`, `TC-011-03-02` |
| [`DS-04`](#ds-04--tài-khoản-cho-chọn-nhiều-dòng) | Tài khoản cho chọn nhiều dòng | `TC-012-02-02`, `TC-012-02-03`, `TC-012-04-03` |
| [`DS-05`](#ds-05--hồ-sơ-đầy-đủ-của-acc-target) | Hồ sơ đầy đủ của ACC-TARGET | `TC-011-01-02`, `TC-013-01-01`, `TC-013-03-08` |

### `DS-01` — Tài khoản tìm kiếm theo tên và email

| Mã | Tên | Email | Vai trò | Trạng thái |
|---|---|---|---|---|
| U-101 | Huy Tran | `huy@example.test` | Người dùng | Đang hoạt động |
| U-102 | Huy Le | `huy.le@example.test` | Người dùng | Đang hoạt động |
| U-103 | Lan Le | `lan.le@example.test` | Người dùng | Đang hoạt động |

### `DS-02` — Tài khoản cho tra cứu vượt quá 100 bản ghi

- 205 tài khoản AN-001 đến AN-205: tên "An 001" đến "An 205", email `an001@example.test` đến `an205@example.test`, vai trò Người dùng, Đang hoạt động.
- 20 tài khoản BINH-001 đến BINH-020: tên "Binh 001" đến "Binh 020", email `binh001@example.test` đến `binh020@example.test`, vai trò Quản trị viên, Đã vô hiệu hoá.
- Ngày tham gia giảm dần từ AN-001 (mới nhất) tới AN-205. BINH-001 đến BINH-020, ACC-ADMIN và ACC-ROOT có ngày tham gia cũ hơn AN-205.

### `DS-03` — Tài khoản trùng ngày tham gia

| Mã | Tên | Email | Ngày tham gia |
|---|---|---|---|
| U-301 | Sort 301 | `sort301@example.test` | 01/08/2026 08:00 |
| U-302 | Sort 302 | `sort302@example.test` | 01/08/2026 08:00 |
| U-303 | Sort 303 | `sort303@example.test` | 02/08/2026 08:00 |

- U-301 được tạo trước U-302 nên có mã tài khoản nhỏ hơn.
- Không tài khoản nào khác có tên hoặc email chứa "sort".

### `DS-04` — Tài khoản cho chọn nhiều dòng

21 tài khoản ACC-003 đến ACC-023, email `acc003@example.test` đến `acc023@example.test`, vai trò Người dùng. Trạng thái của 21 tài khoản do Precondition của từng test case quy định.

### `DS-05` — Hồ sơ đầy đủ của ACC-TARGET

| Trường | Giá trị |
|---|---|
| Ngày sinh | `1998-04-12` |
| Giới tính | `Nữ` |
| Địa chỉ | `12 Đường Kiểm Thử` |
| Số điện thoại | `0900000000` |
| Giới thiệu | `Hồ sơ kiểm thử` |
