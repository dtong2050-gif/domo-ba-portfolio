# Yêu cầu, ràng buộc và quy tắc nghiệp vụ

Tài liệu tập hợp yêu cầu stakeholder, yêu cầu chức năng, yêu cầu phi chức năng, quy tắc nghiệp vụ, ràng buộc và giả định.
Mỗi mục có mã định danh riêng và được truy vết trong ma trận truy vết tới user story, màn hình, test scenario và kịch bản nghiệm thu.
Đây là căn cứ để viết user story, đặc tả màn hình và ca kiểm thử.

## 1. Stakeholder requirement

`StR` là nhu cầu của từng nhóm stakeholder để đạt các `BR` ở [phạm vi nền §2](01-scope-baseline.md#2-mục-tiêu---business-requirement). Mỗi StR phục vụ ít nhất một BR.

| ID | Phát biểu | Phục vụ | Ưu tiên |
|---|---|---|---|
| `StR-003` | Bộ phận vận hành cần đình chỉ và khôi phục quyền truy cập của tài khoản khi xử lý vi phạm hoặc điều chỉnh quyết định trước đó | `BR-002` | Must |
| `StR-004` | Quản trị viên cần phân công và thay đổi vai trò của tài khoản, để mỗi người chỉ dùng được những chức năng phù hợp với trách nhiệm được giao | `BR-003` | Must |
| `StR-005` | Bộ phận vận hành cần giành lại quyền kiểm soát hệ thống khi tài khoản quản trị bị lạm dụng hoặc bị phân quyền sai | `BR-003` | Must |
| `StR-006` | Bộ phận vận hành cần tra lại được ai đã thực hiện thao tác quản trị nào, trên tài khoản nào, vì lý do gì và vào lúc nào | `BR-004` | Must |

## 2. Functional requirement

`FR` trả lời cho câu hỏi WHAT, mô tả hệ thống `phải làm gì` để đáp ứng các `StR` ở trên. Mỗi FR gắn với ít nhất một StR và được viết ở mức có thể kiểm thử được.

### `FR-011` — Tra cứu danh sách tài khoản

**Phục vụ:** `StR-003`, `StR-004` — Hệ thống phải cho phép quản trị viên tra cứu danh sách tài khoản để kiểm tra quyền và xử lý vi phạm.

| # | Tiêu chí |
|---|---|
| 1 | Danh sách hiển thị tên, email, vai trò, trạng thái, ngày tham gia và menu thao tác. Khu vực quản trị không cung cấp màn hình xem chi tiết tài khoản; dữ liệu định danh nhạy cảm không nằm trong dữ liệu của màn danh sách |
| 2 | Cho phép tìm theo tên hoặc email, không phân biệt chữ hoa và chữ thường; lọc theo vai trò và trạng thái hoạt động hoặc đã vô hiệu hóa. Kết quả cập nhật ngay khi người dùng nhập hoặc chọn điều kiện, không cần nút Lọc |
| 3 | Cho phép sắp xếp theo ngày tham gia tăng dần hoặc giảm dần. Mặc định hiển thị tài khoản mới nhất trước. Khi ngày tham gia trùng nhau, áp dụng thứ tự phụ ổn định để các dòng không đổi vị trí giữa các trang |
| 4 | Khi không có kết quả, hiển thị thông báo trạng thái rỗng thay vì bảng trống |
| 5 | Phân trang phía máy chủ trên toàn bộ tập dữ liệu. Cho phép chọn kích thước trang 10, 20, 50 hoặc 100 bản ghi; mặc định là 10. Khi đổi kích thước trang, quay về trang 1 |
| 6 | Danh sách hỗ trợ truy cập toàn bộ bản ghi, không bị giới hạn ở 100 bản ghi đầu tiên |
| 7 | Chỉ khi đã chọn một trạng thái cụ thể trong bộ lọc, quản trị viên mới có thể chọn nhiều tài khoản trong toàn bộ kết quả đã lọc, kể cả ở các trang khác nhau. Hệ thống tự loại tài khoản của người thao tác và tài khoản superAdmin khỏi danh sách có thể chọn. Khi điều kiện tra cứu thay đổi, hệ thống xóa toàn bộ lựa chọn. Sau khi chọn, hệ thống chỉ mở hộp thoại xác nhận và chưa thay đổi tài khoản nào |

### `FR-012` — Đổi vai cho một tài khoản

**Phục vụ:** `StR-004`, `StR-005` — Hệ thống phải cho phép quản trị viên đổi vai của một tài khoản khác sau khi xác nhận thao tác và nhập lý do.

| # | Tiêu chí |
|---|---|
| 1 | Không cho phép người thao tác tự đổi vai trò hoặc thực hiện thao tác lên tài khoản superAdmin |
| 2 | Chỉ cho phép gán vai trò đã được công bố và chưa ngừng sử dụng. Danh sách vai trò không hiển thị vai trò ở trạng thái nháp hoặc đã ngừng sử dụng. superAdmin là thuộc tính của tài khoản, không phải vai trò và không thể được cấp qua chức năng này. Máy chủ từ chối mọi yêu cầu cấp thuộc tính superAdmin |
| 3 | Hộp thoại đổi vai trò được mở từ menu thao tác của đúng tài khoản đích. Hộp thoại hiển thị vai trò hiện tại, cho phép chọn vai trò mới và yêu cầu nhập lý do dài từ 20 đến 500 ký tự sau khi loại bỏ khoảng trắng ở đầu và cuối. Cả giao diện và máy chủ đều từ chối lý do nằm ngoài giới hạn này. Trước khi lưu, hộp thoại xác nhận hiển thị tài khoản đích, vai trò cũ, vai trò mới và lý do |
| 4 | Nếu có lỗi ở bất kỳ bước nào, vai trò của tài khoản không được thay đổi |
| 5 | Vai trò mới có hiệu lực từ lần đăng nhập kế tiếp. Phiên đang mở tiếp tục dùng quyền cũ cho đến khi người dùng đăng nhập lại hoặc phiên hết hạn |
| 6 | Khi hạ vai trò của nhà tổ chức, hệ thống chỉ gỡ các quyền tương ứng; không xóa quan hệ sở hữu hoặc dữ liệu thử thách. Nếu vai trò nhà tổ chức được cấp lại, dữ liệu cũ vẫn còn nguyên |
| 7 | Sau khi cập nhật, tất cả nguồn lưu trữ vai trò phải trả về cùng một vai trò hiện hành |

## 3. Non-functional requirement

`NFR` là tiêu chí chất lượng của hệ thống: scalability, availability, security, compatibility, usability, performance, compliance, maintainability. Mỗi NFR có ngưỡng đo lường cụ thể thay vì mô tả định tính.

| ID | Tiêu chí | Phát biểu | Cách xác minh | Phục vụ |
|---|---|---|---|---|
| `NFR-P-05` | Usability | Khi quản trị viên đang ở màn danh sách tài khoản và chưa được đào tạo riêng, thao tác đổi vai cho một tài khoản không vượt quá **5 thao tác và 60 giây** | Đếm số thao tác và đo thời gian trong `UAT-021`, theo cùng phương pháp đo ngưỡng thao tác đăng nhập hiện hành | `StR-004` |
| `NFR-P-06` | Compliance | Bản ghi dấu vết thao tác quản trị không được ghi bất kỳ trường định danh nhạy cảm nào, phù hợp với yêu cầu pháp luật Việt Nam về xử lý dữ liệu cá nhân | Rà soát cấu trúc bản ghi trước khi phát hành và kiểm tra bản ghi thực tế trong `TC-013-03-08` | `StR-006` |

## 4. Business rule

`RULE-` - Business rule là các quy tắc nghiệp vụ mà tổ chức đặt ra, tồn tại độc lập với việc có hệ thống hay không.

### `RULE-006` — Tài khoản superAdmin được bảo vệ và có quyền gán mọi vai trò

**Phát biểu:** Hệ thống luôn có đúng một tài khoản superAdmin. Không tài khoản nào khác được thực hiện thao tác quản trị lên tài khoản này. Ngược lại, superAdmin được gán vai trò cho mọi tài khoản, kể cả vai trò Quản trị viên.

**Áp dụng khi:**
- Hiển thị menu thao tác trên màn hình danh sách tài khoản;
- Mở hộp thoại đổi vai trò;
- Xử lý lệnh quản trị có đích là superAdmin;
- Tài khoản superAdmin gán vai trò cho tài khoản khác.

**Không áp dụng khi:** khôi phục quyền truy cập cho chính tài khoản superAdmin. Việc này thực hiện trực tiếp trên cơ sở dữ liệu, nằm ngoài phạm vi ứng dụng.

**Hành vi khi vi phạm:**
- Ẩn toàn bộ lựa chọn quản trị trên dòng superAdmin; không mở hộp thoại đổi vai trò.
- Nếu tài khoản khác gửi thẳng yêu cầu lên máy chủ để thay đổi superAdmin, máy chủ từ chối trước khi ghi dữ liệu.

**Thông báo cho người dùng:** *"Tài khoản superAdmin được bảo vệ và không thể bị thay đổi bởi tài khoản khác."*

#### Decision table

| # | Chủ thể | Đích | Thao tác | Kết quả |
|---|---------|------|----------|---------|
| 1 | Tài khoản khác | superAdmin | Đổi vai trò hoặc vô hiệu hoá | Từ chối; dữ liệu không đổi |
| 2 | Tài khoản khác | superAdmin | Mở menu thao tác | Không hiển thị lựa chọn quản trị; dòng vẫn hiển thị |
| 3 | superAdmin | Người dùng, Nhà tổ chức, Quản trị viên | Gán vai trò | Cho phép theo luồng quản trị vai trò |
| 4 | Quản trị viên | Tài khoản thường | Thao tác quản trị tài khoản | Xét theo các quy tắc còn lại |

**Điều kiện bất biến:** `số lượng tài khoản superAdmin luôn bằng 1`.

#### Ngoại lệ và quyền ghi đè

Không tài khoản nào trong ứng dụng ghi đè được quy tắc này. Nếu mất quyền truy cập superAdmin, cách duy nhất là can thiệp trực tiếp vào cơ sở dữ liệu. Hệ thống không có màn hình khôi phục, tài khoản dự phòng hay luồng ủy quyền.

### `RULE-007` — Không cho phép tự đổi vai trò hoặc tự vô hiệu hoá

**Phát biểu:** Tài khoản không được tự đổi vai trò hoặc tự vô hiệu hóa. Trước khi ghi dữ liệu, hệ thống so sánh định danh người thực hiện với từng tài khoản đích.

**Áp dụng khi:**
- Đổi vai trò;
- Vô hiệu hoá một tài khoản;
- Vô hiệu hoá hàng loạt cùng lúc.

**Không áp dụng khi:** khôi phục tài khoản đang bị vô hiệu hoá.

**Hành vi khi vi phạm:**

- Từ chối toàn bộ yêu cầu trước khi ghi dữ liệu.
- Với yêu cầu hàng loạt, từ chối toàn bộ danh sách, không xử lý một phần.

**Thông báo cho người dùng:** *"Không thể thay đổi quyền hoặc vô hiệu hoá chính tài khoản của bạn."*

#### Decision table

| # | Người thực hiện là tài khoản đích | Thao tác | Kết quả |
|---|-----------------------------------|----------|---------|
| 1 | Có | Đổi vai trò | Từ chối; vai trò không đổi |
| 2 | Có | Vô hiệu hoá một tài khoản | Từ chối; trạng thái không đổi |
| 3 | Có, tài khoản nằm trong danh sách chọn | Vô hiệu hoá hàng loạt | Từ chối toàn bộ yêu cầu; không cập nhật tài khoản nào |
| 4 | Có | Tự khôi phục tài khoản khi phiên đăng nhập còn hiệu lực | Cho phép sau khi xác nhận và nhập lý do hợp lệ; quy tắc này không áp dụng |
| 5 | Không | Một trong các thao tác ở trên | Kiểm tra tiếp quyền của người thực hiện, `RULE-006` và các quy tắc liên quan khác |

**Điều kiện kiểm tra:**
- Với yêu cầu đơn lẻ: từ chối khi `actorId = targetUserId` và thao tác là đổi vai trò hoặc vô hiệu hoá.
- Với yêu cầu hàng loạt: từ chối khi `actorId nằm trong targetUserIds`.

#### Ngoại lệ và quyền ghi đè

Không tài khoản nào ghi đè được quy tắc này, kể cả superAdmin.

## 5. Ràng buộc

Ràng buộc là những giới hạn bắt buộc trong thiết kế, phát sinh từ công nghệ, nền tảng, pháp luật hoặc nguồn lực dự án và không thể thay đổi trong phạm vi hiện tại.

| ID | Ràng buộc | Ảnh hưởng |
|---|---|---|
| `C-01` | Việc xử lý dữ liệu cá nhân phải tuân thủ pháp luật Việt Nam về dữ liệu cá nhân | Dữ liệu của màn danh sách tài khoản và bản ghi dấu vết thao tác không được chứa trường định danh nhạy cảm. Trước khi thu thập dữ liệu, phải xác định thời hạn lưu trữ, đối tượng được quyền xem và thời điểm xóa |
| `C-03` | Người dùng chỉ đăng nhập bằng tài khoản Google | Không có phương thức khôi phục quyền truy cập thay thế. Nếu quản trị viên tự khóa tài khoản hoặc phân quyền sai, biện pháp còn lại là can thiệp trực tiếp vào cơ sở dữ liệu. Là căn cứ của `StR-005` |
| `C-05` | DOMO chưa có bộ phận hỗ trợ hoặc kênh tiếp nhận khiếu nại | Người bị đình chỉ tài khoản hiện chưa có kênh khiếu nại. Vì vậy, hướng dẫn liên hệ trong thông báo được hoãn lại cho đến khi có bộ phận chịu trách nhiệm tiếp nhận và xử lý |

## 6. Giả định

Những điều được xem là đúng tại thời điểm phân tích nhưng chưa được kiểm chứng. Mỗi giả định ghi rõ rủi ro nếu sai và cách xử lý tương ứng.

| ID | Giả định | Ảnh hưởng nếu sai | Trạng thái |
|---|---|---|---|
| `A-05` | Vô hiệu hóa tài khoản là đủ để ngăn người gian lận tạo tài khoản mới | Vì không có định danh xác nhận danh tính thật của người dùng, việc vô hiệu hóa chỉ chặn được một tài khoản cụ thể, không ngăn được cùng một người tạo tài khoản khác. Cơ chế chống gian lận ở cấp người dùng cần được xác định thành yêu cầu riêng | **Không còn hiệu lực (Invalid):** Đã xác minh rằng người vi phạm vẫn có thể tạo tài khoản mới và tiếp tục gian lận |
