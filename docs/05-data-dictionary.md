# Mô hình dữ liệu và từ điển dữ liệu

Mô hình dữ liệu cho thấy các thực thể chính và quan hệ giữa chúng; từ điển dữ liệu định nghĩa ý nghĩa, kiểu và quy tắc của từng trường.
Mỗi khái niệm chỉ có một tên và một định nghĩa, tránh việc màn hình, API và cơ sở dữ liệu hiểu cùng một dữ liệu theo nhiều cách.
Đây là nguồn tham chiếu chung cho đặc tả màn hình, đặc tả API và kiểm thử.

## 1. Sơ đồ quan hệ thực thể

*[Hình 1 — ERD: Quan hệ giữa các bảng](../diagrams/erd-account-admin.png)*

### Bản số quan hệ

| Quan hệ | Đọc từ trái sang | Đọc từ phải sang |
|---|---|---|
| **`roles` — `users`** | Một vai trò được gán cho **nhiều** tài khoản, và có thể **chưa ai** mang | Một tài khoản mang **đúng một** vai trò |
| **`users` — `user_action_logs`** | Một tài khoản có thể có **nhiều** bản ghi nhật ký thao tác, hoặc chưa có bản ghi nào | Một bản ghi luôn gắn với **đúng một** người thực hiện |

### Ràng buộc

| Ràng buộc | Nội dung |
|---|---|
| Mỗi tài khoản mang **đúng một** vai trò | Đổi vai trò là thay thế, quyền không cộng dồn từ nhiều vai trò. Quản trị viên **không** mặc nhiên có quyền Nhà tổ chức |
| Không có tài khoản "chưa có vai trò" | Tạo tài khoản và gán vai trò Người dùng là **một bước duy nhất** |
| Không dùng tên hay email làm khoá | Mọi lệnh tham chiếu bằng mã. Tên chỉ dùng để hiển thị |
| Tài khoản không có trạng thái "đã xoá" | Chỉ có đang hoạt động và đã vô hiệu hoá. Không xoá vật lý |
| Vai trò có **hai trục trạng thái độc lập** | Trục công bố: nháp hoặc đã công bố<br>Trục vòng đời: đang dùng hoặc đã ngừng dùng |
| Chỉ vai trò **đã công bố và chưa ngừng dùng** mới gán mới được | Danh sách vai trò được phép gán chỉ gồm các vai trò này |
| Chuyển vai trò từ đã công bố về nháp chỉ ngăn **gán mới** | Các tài khoản đang mang vai trò đó vẫn giữ nguyên quan hệ |
| Ba vai trò gốc chỉ được sửa bộ quyền | Người dùng, Nhà tổ chức, Quản trị viên không được ngừng dùng, không đổi tên, mã định danh, mô tả hay trạng thái công bố ([`BD-001`](10-change-decision-log.md#bd-001--ba-vai-trò-gốc-chỉ-được-sửa-bộ-quyền)) |

## 2. Từ điển dữ liệu

### Bảng `users` — Tài khoản

Mỗi dòng là một tài khoản, tự tạo ở lần đăng nhập Google đầu tiên với vai trò Người dùng. Người dùng quản lý thông tin cá nhân; Quản trị viên quản lý vai trò và trạng thái (`M-03`).

**Khoá chính:** `id`

| Tên nghiệp vụ | Tên kỹ thuật | Kiểu | Giá trị hợp lệ | Quy tắc | Ví dụ | Trace |
|---|---|---|---|---|---|---|
| Mã tài khoản | `id` | `uuid` | Tự sinh, tăng dần theo thời gian tạo | Khoá cho mọi lệnh tác động lên tài khoản | — | `FR-011`, `FR-012` |
| Email | `email` | `text` | Bắt buộc. Duy nhất toàn bảng, kể cả tài khoản đã vô hiệu hoá | Một email là một tài khoản. Đăng nhập lại bằng email của tài khoản đã vô hiệu hoá bị từ chối | `nguoi.dung@example.test` | `FR-011` |
| Mã định danh Firebase | `firebaseUid` | `text` | Duy nhất toàn bảng | Liên kết tài khoản với danh tính Firebase Auth cấp khi người dùng đăng nhập bằng Google | — | `M-01` |
| Tên hiển thị | `name` | `text` | Được phép để trống | Không duy nhất, hai tài khoản có thể trùng tên | `Nguyễn Văn A` | `FR-011` |
| Tên người dùng | `username` | `text` | Được phép để trống. Duy nhất trong các tài khoản đang hoạt động | Tự đặt khi cập nhật hồ sơ | `vana` | `M-04` |
| Ảnh đại diện | `avatarUrl` | `text` | Được phép để trống; rỗng thì dùng ảnh mặc định | Đường dẫn tệp trên dịch vụ lưu trữ tệp | — | `M-04` |
| Ngày sinh | `dateOfBirth` | `date` | Được phép để trống | — | `1998-04-12` | `M-04`, `FR-011` |
| Giới tính | `gender` | `text` | Được phép để trống. Văn bản tự do | — | `Nữ` | `M-04`, `FR-011` |
| Địa chỉ | `address` | `text` | Được phép để trống | — | — | `M-04`, `FR-011` |
| Số điện thoại | `phone` | `text` | Được phép để trống | — | — | `M-04`, `FR-011` |
| Giới thiệu | `bio` | `text` | Được phép để trống. Văn bản tự do | — | — | `M-04` |
| Vai trò hiện tại | `roleId` | `uuid` → `roles.id` | Bắt buộc. Luôn đúng một vai trò trong `roles` | Đổi vai trò thay giá trị này; vai trò mới phải đã công bố và chưa ngừng dùng | `a1b2c3d4-0000-4000-8000-000000000001` (vai trò Người dùng) | `FR-011`, `FR-012` |
| Mốc phiên bản vai trò | `currentRoleRevision` | `integer` | Từ 1 trở lên. Mặc định 1 khi tạo tài khoản | Tăng 1 mỗi lần `roleId` thay đổi, kể cả khi tài khoản được chuyển sang vai trò thay thế lúc ngừng dùng vai trò cũ. Lệnh không có thay đổi thì giữ nguyên. Lệnh đổi vai trò gửi kèm giá trị đã nhận lúc mở hộp thoại; khác giá trị hiện tại thì lệnh bị từ chối, không ghi đè | `7` | `FR-012` |
| Cờ superAdmin | `isSuperAdmin` | `boolean` | Mặc định `false` | Không gán được qua giao diện. Tài khoản mang cờ này không bị tài khoản nào khác thay đổi | `false` | `RULE-006` |
| Ngày tham gia | `createdAt` | `timestamptz` | Tự ghi khi tạo tài khoản | **Không đổi** khi đăng nhập lại, đổi vai trò hay cập nhật hồ sơ. Dùng để sắp xếp danh sách | `2026-08-21T01:00:00.000Z` | `FR-011` |
| Thời điểm cập nhật | `updatedAt` | `timestamptz` | Tự ghi mỗi lần cập nhật | Được cập nhật khi đổi vai trò | `2026-08-21T03:05:00.000Z` | `FR-012` |
| Thời điểm vô hiệu hoá | `deletedAt` | `timestamptz` | Rỗng khi đang hoạt động | Có giá trị khi tài khoản bị vô hiệu hoá; khôi phục thì trở về rỗng | — | `FR-011` |

### Bảng `roles` — Vai trò

Danh mục vai trò; mỗi vai trò là một nhóm năng lực được gán cho tài khoản. Hệ thống **không cấp quyền riêng lẻ** trực tiếp cho từng người.

**Khoá chính:** `id`

| Tên nghiệp vụ | Tên kỹ thuật | Kiểu | Giá trị hợp lệ | Quy tắc | Ví dụ | Trace |
|---|---|---|---|---|---|---|
| Mã vai trò | `id` | `uuid` | Tự sinh, tăng dần theo thời gian tạo | Khoá cho mọi tham chiếu tới vai trò, gồm `users.roleId` | — | `FR-011`, `FR-012` |
| Tên vai trò | `name` | `text` | Bắt buộc. 1–100 ký tự sau khi bỏ khoảng trắng đầu cuối | Duy nhất, không phân biệt chữ hoa chữ thường và khoảng trắng đầu cuối, kể cả với vai trò đã ngừng dùng | `Quản trị viên` | `FR-011`, `FR-012` |
| Mã định danh vai trò | `slug` | `text` | Bắt buộc. Chỉ gồm chữ thường, số và dấu gạch ngang | Duy nhất, kể cả với vai trò đã ngừng dùng. `user`, `organizer`, `admin` là ba vai trò gốc | `admin` | `FR-012` |
| Mô tả | `description` | `text` | Được phép để trống. Tối đa 500 ký tự | — | `Vận hành nền tảng` | `M-02` |
| Trạng thái công bố | `status` | `text` | `draft` — Nháp<br>`public` — Đã công bố. Mặc định `draft` | Chỉ vai trò `public` và chưa ngừng dùng mới gán mới được. Bộ lọc vai trò ở danh sách tài khoản nhận cả vai trò `draft` | `public` | `FR-011`, `FR-012` |
| Bộ quyền | `permissions` | `jsonb` | Danh sách ít nhất một mã năng lực dạng `tài_nguyên:hành_động`, không trùng | **Quyền quản trị tài khoản** gồm `user:read_all` (tra cứu) và `user:manage_role` (đổi vai trò) | `user:read_all`, `user:manage_role` | `FR-011`, `FR-012` |
| Phiên bản vai trò | `revision` | `integer` | Từ 1 trở lên. Mặc định 1 | Tăng 1 mỗi lần cập nhật vai trò thành công. Chặn ghi đè khi hai người cùng sửa một vai trò | `3` | `M-02` |
| Thời điểm tạo | `createdAt` | `timestamptz` | Tự ghi khi tạo vai trò | — | `2026-08-01T01:00:00.000Z` | `M-02` |
| Thời điểm cập nhật | `updatedAt` | `timestamptz` | Tự ghi mỗi lần cập nhật | — | `2026-08-15T07:20:00.000Z` | `M-02` |
| Thời điểm ngừng dùng | `deletedAt` | `timestamptz` | Rỗng khi vai trò đang dùng | Có giá trị khi vai trò đã ngừng dùng; khôi phục thì trở về rỗng. Vai trò đã ngừng dùng bị loại khỏi cả bộ lọc vai trò ở danh sách tài khoản lẫn danh sách vai trò được phép gán. Ba vai trò gốc không được ngừng dùng | — | `FR-011`, `FR-012` |

### Bảng `user_action_logs` — Nhật ký thao tác (audit log)

Mỗi dòng ghi một quyết định quản trị: ai làm, tác động lên ai, thay đổi gì, vì sao, lúc nào, kết quả ra sao.

**Khoá chính:** `id`

| Tên nghiệp vụ | Tên kỹ thuật | Kiểu | Giá trị hợp lệ | Quy tắc | Ví dụ | Trace |
|---|---|---|---|---|---|---|
| Mã bản ghi | `id` | `uuid` | Tự sinh, tăng dần theo thời gian ghi | — | — | `StR-006` |
| Người thực hiện | `actorId` | `uuid` → `users.id` | Bắt buộc. Mã một tài khoản đã đăng nhập | Chỉ ghi mã, không ghi email hay tên | — | `StR-006`, `NFR-P-06` |
| Loại hành động | `action` | `enum` | `CREATE`, `UPDATE`, `DELETE`, `RESTORE` | Đổi vai trò là `UPDATE`, vô hiệu hoá là `DELETE`, khôi phục là `RESTORE` | `UPDATE` | `StR-006` |
| Loại đối tượng | `entity` | `text` | Bắt buộc. Tên bảng bị tác động | Với thao tác trên tài khoản là `users` | `users` | `StR-006` |
| Đối tượng bị tác động | `entityId` | `text` | Bắt buộc | Mã của đối tượng bị tác động; không ghi email hay tên | — | `StR-006`, `NFR-P-06` |
| Kết quả | `result` | `enum` | `success`, `no_op`, `rejected`, `failed` | Mỗi lệnh ghi rõ kết quả của mình, không dùng giá trị mặc định | `success` | `StR-006`, `FR-012` |
| Lý do | `reason` | `text` | Được phép để trống. Với đổi vai trò, vô hiệu hoá, khôi phục: **20–500 ký tự** sau khi bỏ khoảng trắng đầu cuối | Lý do quản trị viên nhập, hoặc nguyên nhân bị từ chối, thất bại hay không có thay đổi | `Phân công vận hành nội dung` | `StR-006`, `FR-012` |
| Mã quyết định | `correlationId` | `uuid` | Được phép để trống | Nối các bản ghi thuộc cùng một quyết định, ví dụ một lệnh hàng loạt. Rỗng khi lệnh chỉ tác động một tài khoản | — | `StR-006` |
| Thay đổi | `changes` | `jsonb` | `{ before, after }`, chỉ gồm các trường thực sự thay đổi | Chỉ điền khi kết quả là `success`. Không chứa số điện thoại, địa chỉ hay ngày sinh. Với đổi vai trò chỉ gồm `roleId` | `roleId`: vai trò cũ → vai trò mới | `StR-006`, `NFR-P-06` |
| Phương thức yêu cầu | `method` | `text` | Được phép để trống | Phương thức của yêu cầu gây ra thao tác | `PATCH` | `StR-006` |
| Đường dẫn yêu cầu | `path` | `text` | Được phép để trống | Chỉ chứa mã, không chứa email hay tên | `/users/{id}/role` | `StR-006`, `NFR-P-06` |
| Thời điểm ghi | `createdAt` | `timestamptz` | Tự ghi khi tạo bản ghi | — | `2026-08-21T03:05:00.000Z` | `StR-006` |

**Giá trị của `result` theo kết quả lệnh đổi vai trò**

| Kết quả lệnh | Nghĩa | `result` | Ghi dữ liệu |
|---|---|---|---|
| **Thành công** | Vai trò đã đổi, mọi nơi lưu vai trò đã đồng bộ | `success` | Có |
| **Không có thay đổi** | Vai trò mới trùng vai trò đang mang | `no_op` | Không |
| **Bị từ chối** | Một điều kiện bảo vệ chặn trước khi ghi | `rejected` | Không |
| **Thất bại an toàn** | Lỗi khi ghi, và hệ thống **xác nhận đã hoàn tác toàn bộ** | `failed` | Không |
