# Kiểm thử chấp nhận người dùng (UAT)

Ca nghiệm thu chuyển tiêu chí nghiệm thu của user story thành các bước kiểm tra cụ thể trên sản phẩm thật, kèm kết quả mong đợi đo được.
Mỗi bước truy vết về kịch bản hoặc yêu cầu mà nó kiểm chứng, nên biết ngay tiêu chí nào đã được kiểm và tiêu chí nào chưa.
Kết quả chạy là căn cứ để PM/PO nghiệm thu và quyết định phát hành.

## 1. Phạm vi và điều kiện đầu vào

**Phạm vi:** `US-011`, `US-012`, `US-013`.

**Môi trường:** trang quản trị trên web. Mỗi lần chạy ghi lại đường dẫn, phiên bản phần mềm và thời điểm chạy.

**Tài khoản cần chuẩn bị:**

| Tài khoản | Vai trò trong bài kiểm tra |
|---|---|
| Người chạy kiểm thử | Quản trị viên có đủ quyền quản trị tài khoản |
| Tài khoản đang hoạt động | Tài khoản đích hợp lệ của lệnh đổi vai trò |
| Tài khoản đã vô hiệu hoá | Kiểm tra bộ lọc trạng thái; tài khoản đích bị từ chối khi đổi vai trò |
| Tài khoản superAdmin | Kiểm tra `RULE-006`: luôn bị từ chối khi làm tài khoản đích |
| Tài khoản không có quyền quản trị tài khoản | Kiểm tra từ chối truy cập |
| Tài khoản Nhà tổ chức sở hữu ít nhất một thử thách | Kiểm tra hạ vai trò không làm mất dữ liệu sở hữu |
| Ít nhất 12 tài khoản phụ | Kiểm tra phân trang và sắp xếp |

**Dữ liệu cần chuẩn bị:** ba vai trò gốc (Người dùng, Nhà tổ chức, Quản trị viên), một vai trò tự tạo đang dùng, một vai trò tự tạo ở dạng nháp, một vai trò tự tạo đã ngừng dùng.

**Quy tắc an toàn dữ liệu:** chỉ dùng dữ liệu giả có tiền tố nhận biết được; không ghi email thật, mã đăng nhập hay thông tin cá nhân vào tài liệu; đếm số bản ghi trước khi chạy và dọn sạch sau khi chạy.

## 2. Ca nghiệm thu

### `UAT-021` — Tra cứu danh sách tài khoản

**Mục đích:** xác nhận quản trị viên tìm được đúng tài khoản cần xử lý, và màn hình không hiển thị dữ liệu ngoài phạm vi cần thiết.

**Điều kiện đầu vào:**

- Dữ liệu có cả tài khoản đang hoạt động lẫn đã vô hiệu hoá, thuộc nhiều vai trò khác nhau, và đủ nhiều để chia ít nhất 2 trang
- Có ít nhất hai tài khoản trùng ngày tham gia

| # | Bước | Kết quả mong đợi | Truy vết |
|---|---|---|---|
| 1 | Mở màn hình danh sách tài khoản | Bảng hiện tên và email, vai trò, trạng thái, ngày tham gia; mặc định sắp xếp mới đến cũ, 10 dòng mỗi trang<br>Không hiện ngày sinh, giới tính, địa chỉ, số điện thoại, giới thiệu bản thân | [`US011-SC-01`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 2 | Tìm theo tên, rồi theo email, gõ lẫn chữ hoa chữ thường | Hiển thị đúng tài khoản, không phân biệt chữ hoa chữ thường | [`US011-SC-02`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 3 | Lọc theo vai trò, theo trạng thái, rồi kết hợp với từ khoá | Kết quả thoả mãn tất cả điều kiện cùng lúc<br>Tổng số kết quả khi lọc trạng thái "Tất cả" bằng số tài khoản đang hoạt động cộng số tài khoản đã vô hiệu hoá | [`US011-SC-03`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 4 | Đổi chiều sắp xếp theo ngày tham gia | Sắp xếp được theo cả hai chiều; hai tài khoản trùng ngày luôn xếp theo mã tài khoản | [`US011-SC-04`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 5 | Đổi trang, và đổi số dòng mỗi trang qua các mức 10, 20, 50, 100 | Ô số dòng mỗi trang chỉ có 10, 20, 50, 100, không nhập tay được<br>Tổng số kết quả đúng ở mọi mức; danh sách không dừng ở 100 bản ghi đầu<br>Đổi số dòng mỗi trang thì quay về trang 1 | [`US011-SC-05`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [`US011-SC-06`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [`US011-SC-07`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 6 | Đang ở trang 2 trở đi, đổi từ khoá hoặc bộ lọc | Quay về trang 1; trong lúc chờ không hiện kết quả của điều kiện cũ | [`US011-SC-09`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 7 | Ẩn rồi hiện khu vực bộ lọc; ẩn rồi hiện từng cột tuỳ chọn | Ẩn khu vực bộ lọc không xoá điều kiện đang áp dụng<br>Ẩn hoặc hiện cột không tải lại dữ liệu; cột Tài khoản và Thao tác luôn hiển thị | [`US011-SC-10`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [`US011-SC-11`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 8 | Bấm nút làm mới, rồi so số liệu trên màn hình với phản hồi `GET /users` của máy chủ | Tra cứu lại đúng điều kiện và trang đang xem; số liệu khớp phản hồi của máy chủ<br>Không hiện dữ liệu cũ như kết quả mới | [`US011-SC-12`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 9 | Tìm một từ khoá không khớp tài khoản nào, rồi bấm "Xoá tìm kiếm và bộ lọc" | Hiện trạng thái rỗng kèm thông báo, không hiện bảng trống<br>Xoá tìm kiếm và bộ lọc giữ nguyên chiều sắp xếp và số dòng mỗi trang | [`US011-SC-13`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 10 | Gửi thẳng yêu cầu [`GET /users`](08-api-spec-extract.md) lên máy chủ, không qua giao diện<br>**Chuẩn bị:** mã đăng nhập của người chạy kiểm thử<br>**Đối chứng:** gửi `GET /users?pageSize=10`<br>**Gửi lần lượt:**<br>a. `pageSize` = `1`, `25`, `101`, `10.5`<br>b. `sortOrder=newest` (ngoài `asc`/`desc`) | Đối chứng trả `200` kèm danh sách<br>a, b. Mỗi yêu cầu trả `400 VALIDATION_FAILED`; phản hồi không có dữ liệu tài khoản; máy chủ không tự đổi về 10 hay `desc` | [`US011-SC-08`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |

### `UAT-022` — Chọn đúng tài khoản cho thao tác quản trị

**Mục đích:** xác nhận quản trị viên chỉ mở được lệnh phù hợp với từng tài khoản, và danh sách tài khoản đích của thao tác hàng loạt gồm đúng những tài khoản được chọn.

**Điều kiện đầu vào:**

- Kết quả tra cứu trải trên ít nhất 2 trang, gồm tài khoản của chính người thao tác, tài khoản superAdmin, tài khoản đang hoạt động và tài khoản đã vô hiệu hoá
- Tài khoản của người thao tác được thử ở hai trạng thái: đang hoạt động, và đã vô hiệu hoá nhưng phiên đăng nhập còn hợp lệ
- Tài khoản superAdmin được thử ở hai trạng thái: đang hoạt động và đã vô hiệu hoá

| # | Bước | Kết quả mong đợi | Truy vết |
|---|---|---|---|
| 1 | Mở menu thao tác trên từng loại tài khoản | Tài khoản đang hoạt động có "Đổi vai trò" và "Vô hiệu hoá"; tài khoản đã vô hiệu hoá chỉ có "Khôi phục"<br>Dòng của chính mình khi đang hoạt động không có nút thao tác; dòng superAdmin không có nút thao tác ở cả hai trạng thái<br>Tài khoản của chính mình khi đã vô hiệu hoá có "Khôi phục" trên menu của dòng | [`US012-SC-01`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [`US012-SC-02`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [`US012-SC-03`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [`US012-SC-12`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md) |
| 2 | Để bộ lọc trạng thái ở "Tất cả", rồi lần lượt chọn "Đang hoạt động" và "Đã vô hiệu hoá" | Chỉ khi lọc theo một trạng thái cụ thể mới chọn được nhiều dòng | [`US012-SC-04`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md) |
| 3 | Chọn và bỏ chọn từng dòng, bấm ô chọn tất cả, chuyển qua các trang | Ô chọn tất cả hiện đúng ba trạng thái: chưa chọn, chọn một phần, chọn tất cả; bấm vào thì chọn hoặc bỏ toàn bộ kết quả đã lọc<br>Chọn tất cả phủ mọi tài khoản hợp lệ trên mọi trang, tự loại chính mình và superAdmin<br>Chọn hoặc bỏ một dòng không làm đổi lựa chọn khác; lựa chọn giữ nguyên khi chuyển trang<br>Tài khoản của chính mình không nằm trong danh sách khôi phục hàng loạt | [`US012-SC-05`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [`US012-SC-07`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [`US012-SC-08`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [`US012-SC-09`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [`US012-SC-10`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [`US012-SC-12`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md) |
| 4 | Đổi từ khoá hoặc bộ lọc khi đang có dòng được chọn | Danh sách chọn bị xoá, thanh hành động biến mất | [`US012-SC-11`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md) |
| 5 | Chọn lại vài dòng, bấm nút lệnh trên thanh hành động | Mở hộp thoại xác nhận với đúng danh sách đã chọn; chưa tài khoản nào bị thay đổi | [`US012-SC-06`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md) |
| 6 | Chọn lại vài dòng rồi bấm "Bỏ chọn" | Xoá toàn bộ lựa chọn và ẩn thanh hành động | [`US012-SC-13`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md) |

### `UAT-023` — Đổi vai trò cho một tài khoản

**Mục đích:** xác nhận lệnh đổi vai trò chỉ chạy khi đủ điều kiện, chạy trọn vẹn một lần, và quyền mới có hiệu lực đúng thời điểm.

**Điều kiện đầu vào:**

- Tài khoản đích đang hoạt động, không phải chính người đang thao tác và không phải superAdmin, và đang có một phiên đăng nhập mở
- Có sẵn một vai trò đang dùng, khác vai trò hiện tại của tài khoản đích
- Một tài khoản nhà tổ chức đang sở hữu ít nhất một thử thách

| # | Bước | Kết quả mong đợi | Truy vết |
|---|---|---|---|
| 1 | Mở hộp thoại đổi vai trò từ dòng của tài khoản đích | Hộp thoại mở cho đúng tài khoản của dòng đã chọn và hiện vai trò hiện tại<br>Không hiện ngày sinh, giới tính, địa chỉ, số điện thoại, giới thiệu bản thân | [`US013-SC-02`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 2 | Mở danh sách "Vai trò mới" | Chọn được mọi vai trò hợp lệ; không có vai trò nháp, vai trò đã ngừng dùng hay lựa chọn superAdmin | [`US013-SC-03`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 3 | Nhập lý do dài 19, 20, 500 và 501 ký tự | Lý do 19 và 501 ký tự bị từ chối; 20 và 500 ký tự được chấp nhận | [`US013-SC-05`](06-user-stories/us-013-doi-vai-tai-khoan.md), [`US013-SC-06`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 4 | Nhập lý do hợp lệ và bấm "Tiếp tục" | Hộp xác nhận hiện đúng tài khoản đích, vai trò cũ → vai trò mới và lý do | [`US013-SC-07`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 5 | Bấm "Xác nhận" nhanh hai lần | Chỉ tạo đúng một lệnh<br>Vai trò mới được lưu trọn vẹn, mọi nơi hiển thị vai trò đều cập nhật | [`US013-SC-01`](06-user-stories/us-013-doi-vai-tai-khoan.md), [`US013-SC-17`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 6 | Đo từ lúc mở màn hình danh sách tài khoản đến lúc đổi vai trò xong | Không quá 5 thao tác và không quá 60 giây | `NFR-P-05` |
| 7 | Trên phiên đang mở của tài khoản đích, mở một chức năng chỉ vai trò mới được dùng; rồi đăng xuất, đăng nhập lại và mở lại chức năng đó | Phiên đang mở vẫn giữ quyền cũ; đăng nhập lại mới nhận quyền mới | [`US013-SC-16`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 8 | Mở lại hộp thoại, chọn đúng vai trò hiện tại và xác nhận | Báo "Không có thay đổi", không ghi như một thay đổi và không gửi thông báo | [`US013-SC-11`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 9 | Hạ vai trò của tài khoản nhà tổ chức xuống Người dùng | Quan hệ sở hữu và dữ liệu thử thách giữ nguyên | [`US013-SC-15`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 10 | Đọc nhật ký thao tác (audit log) của các lệnh vừa chạy | Nhật ký không chứa số điện thoại, địa chỉ hay ngày sinh | `NFR-P-06` |

### `UAT-024` — Từ chối người thực hiện hoặc tài khoản đích không được phép

**Mục đích:** xác nhận thao tác bị cấm bị chặn trên giao diện và bị máy chủ từ chối khi gửi thẳng yêu cầu.

**Điều kiện đầu vào:**

- Một tài khoản không có quyền quản trị tài khoản
- Hai loại tài khoản đích bị cấm: chính người đang thao tác và tài khoản superAdmin
- Hai tài khoản đích không hợp lệ: một tài khoản đã vô hiệu hoá và một mã tài khoản không tồn tại
- Một tài khoản đích hợp lệ: đang hoạt động, không phải chính người thao tác hay superAdmin
- Có cách làm hết hạn phiên đăng nhập của người thao tác
- Công cụ gửi yêu cầu HTTP thẳng tới máy chủ

| # | Bước | Kết quả mong đợi | Truy vết |
|---|---|---|---|
| 1 | Mở màn hình quản trị tài khoản bằng tài khoản không có quyền | Không hiện bảng danh sách; báo không đủ quyền và không có nút "Thử lại" | [`US011-SC-15`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |
| 2 | Bằng tài khoản có quyền, xem dòng của chính mình và dòng superAdmin | Không có nút thao tác trên hai dòng này | [`US012-SC-03`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md) |
| 3 | Gửi thẳng yêu cầu lên máy chủ, không qua giao diện<br>**Chuẩn bị:**<br>• Mã đăng nhập của tài khoản cần dùng<br>• Thông tin tài khoản đích: gửi `GET /users?q=<email của tài khoản đích>`, ghi lại `id`, `roleId` (vai trò hiện tại) và `currentRoleRevision`<br>• Lệnh đổi vai trò là `PATCH /users/{id}/role`, body gồm: `roleId` là vai trò mới, khác vai trò hiện tại, lấy `id` từ `GET /roles/assignable?purpose=USER_ROLE_CHANGE`; `currentRoleRevision` là giá trị đã ghi của tài khoản đích; `reason` dài 20 ký tự<br>**Đối chứng:** gửi lệnh đổi vai trò cho tài khoản đích hợp lệ với `roleId` bằng vai trò hiện tại<br>**Gửi lần lượt:**<br>a. Xem danh sách `GET /users` bằng mã đăng nhập của tài khoản không có quyền<br>b. Đổi vai trò cho chính mình<br>c. Đổi vai trò cho tài khoản superAdmin<br>Sau khi gửi, tra lại b và c bằng `GET /users?q=<email>` | Đối chứng trả `200 NO_CHANGE`<br>a. `403 PERMISSION_DENIED`, không có dữ liệu tài khoản<br>b. `403 USER_SELF_MANAGEMENT_FORBIDDEN`<br>c. `403 USER_ACCOUNT_PROTECTED`<br>Vai trò của chính mình và superAdmin không đổi | [`US011-SC-15`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [`US013-SC-08`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 4 | Gửi thẳng lên máy chủ, chuẩn bị và đối chứng như bước 3:<br>a. Đổi vai trò cho tài khoản đã vô hiệu hoá<br>b. Đổi vai trò cho một mã tài khoản không tồn tại: `{id}` đúng định dạng UUID, body dùng lại của a<br>Sau khi gửi, tra lại tài khoản ở a | Đối chứng trả `200 NO_CHANGE`<br>a. `409 USER_TARGET_DISABLED`<br>b. `404 NOT_FOUND`<br>Tài khoản ở a vẫn đã vô hiệu hoá, vai trò không đổi | [`US013-SC-09`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 5 | Gửi thẳng lên máy chủ như bước 3 ba yêu cầu: xem danh sách tài khoản `GET /users`, xem danh sách vai trò được gán `GET /roles/assignable?purpose=USER_ROLE_CHANGE` và lệnh đổi vai trò đối chứng. Mỗi yêu cầu gửi 4 lần, với mã đăng nhập:<br>a. hợp lệ<br>b. không có (bỏ header `Authorization`)<br>c. đã hết hạn<br>d. bị sửa một ký tự | a. `200`<br>b. `401 AUTH_TOKEN_MISSING`<br>c, d. `401 AUTH_TOKEN_INVALID`; không yêu cầu nào trả `403`<br>Phản hồi b–d không có dữ liệu; vai trò của tài khoản đích không đổi; không có bản ghi mới trong nhật ký thao tác | [`US011-SC-16`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [`US013-SC-18`](06-user-stories/us-013-doi-vai-tai-khoan.md), [`US013-SC-19`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 6 | Mở màn hình danh sách ở hai tab; ở tab 2 mở hộp xác nhận đổi vai trò. Làm hết hạn phiên của người thao tác, rồi bấm nút làm mới ở tab 1 và bấm "Xác nhận" ở tab 2 | Cả hai tab chuyển thẳng về trang đăng nhập và hiện "Phiên đăng nhập đã hết hạn", "Vui lòng đăng nhập lại."<br>Không hiện thông báo không đủ quyền; vai trò của tài khoản đích không đổi | [`US011-SC-17`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [`US013-SC-18`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 7 | Gửi thẳng lên máy chủ như bước 3 lệnh đổi vai trò cho tài khoản đích hợp lệ, với `reason` lần lượt:<br>a. toàn khoảng trắng; 19 ký tự; 501 ký tự<br>b. 20 ký tự<br>c. 500 ký tự, `roleId` là vai trò ban đầu<br>Trước b và c, tra lại tài khoản đích để lấy `roleId` và `currentRoleRevision` mới | a. `400 VALIDATION_FAILED`, vai trò của tài khoản đích không đổi<br>b, c. `200 CHANGED`; sau c tài khoản đích trở về vai trò ban đầu | [`US013-SC-05`](06-user-stories/us-013-doi-vai-tai-khoan.md), [`US013-SC-06`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 8 | Gửi thẳng lên máy chủ yêu cầu đối chứng như bước 3; rồi gửi lại cùng body, thêm trường `"isSuperAdmin": true` | Đối chứng trả `200 NO_CHANGE`<br>Có trường thêm: `400 VALIDATION_FAILED`<br>Tra lại tài khoản đích: `isSuperAdmin=false`, vai trò không đổi | [`US013-SC-20`](06-user-stories/us-013-doi-vai-tai-khoan.md) |

### `UAT-025` — An toàn khi dữ liệu đổi, mất phản hồi hoặc lệnh bị gửi lặp

**Mục đích:** xác nhận hệ thống không ghi nhầm khi dữ liệu đổi giữa chừng, không gửi lại lệnh khi chưa biết lệnh trước đã chạy hay chưa, và không hiển thị dữ liệu cũ hay lỗi như kết quả mới.

**Điều kiện đầu vào:**

- Hai phiên đăng nhập quản trị trên hai trình duyệt, mở song song
- Có cách ngắt phản hồi của máy chủ sau khi lệnh đã được gửi đi, và cách làm máy chủ lỗi trong lúc ghi
- Có cách làm máy chủ trả lỗi khi lấy danh sách tài khoản hoặc danh mục vai trò

| # | Bước | Kết quả mong đợi | Truy vết |
|---|---|---|---|
| 1 | Mở cùng một tài khoản đích trên cả hai phiên; đổi vai trò ở phiên A, rồi ở phiên B bấm "Xác nhận" mà không tải lại | Lệnh ở phiên B bị từ chối, không ghi đè thay đổi của phiên A | [`US013-SC-12`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 2 | Gửi một lệnh hợp lệ rồi ngắt phản hồi giữa chừng; sau đó tải lại màn hình và đối chiếu dữ liệu thật | Hộp xác nhận hiện "Không xác định được kết quả", không báo thất bại và không tự gửi lại<br>Phải tải lại danh sách trước khi được làm tiếp | [`US013-SC-14`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 3 | Gửi một lệnh hợp lệ, làm máy chủ lỗi trong lúc ghi | Không bản ghi nào bị đổi; vai trò của tài khoản đích vẫn là vai trò cũ ở mọi nơi hiển thị<br>Hộp xác nhận báo thay đổi vai trò thất bại, chưa ghi nhận thay đổi nào, và có nút "Thử lại" | [`US013-SC-13`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 4 | Kiểm tra nhật ký thao tác của các lệnh trên | Mỗi lệnh có bản ghi mang đúng kết quả thật: bị từ chối, thất bại hoặc thành công | `StR-006` |
| 5 | Chuyển vai trò đã chọn trong hộp xác nhận về nháp hoặc ngừng dùng, rồi bấm "Xác nhận" | Lệnh bị từ chối, vai trò của tài khoản đích giữ nguyên, và chỉ cho quay lại chọn vai trò khác | [`US013-SC-10`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 6 | Mở hộp thoại đổi vai trò khi danh mục vai trò tải lỗi hoặc trả về rỗng | Hộp thoại báo lỗi, cho thử tải lại và không cho sang bước xác nhận | [`US013-SC-04`](06-user-stories/us-013-doi-vai-tai-khoan.md) |
| 7 | Làm máy chủ trả lỗi khi đang tra cứu danh sách | Bảng hiện trạng thái lỗi kèm "Thử lại", giữ nguyên từ khoá và không hiển thị dữ liệu cũ như kết quả mới | [`US011-SC-14`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) |

## 3. Kết quả thực thi

| Ca | Trạng thái | Ngày chạy | Ghi chú |
|---|---|---|---|
| `UAT-021` | Chưa chạy | — | — |
| `UAT-022` | Chưa chạy | — | — |
| `UAT-023` | Chưa chạy | — | — |
| `UAT-024` | Chưa chạy | — | — |
| `UAT-025` | Chưa chạy | — | — |

Điều kiện để một ca được tính là đạt: dữ liệu lưu đúng, màn hình hiển thị đúng trạng thái tương ứng, và nhật ký thao tác ghi đúng kết quả thật.
