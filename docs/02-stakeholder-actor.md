# Stakeholder và actor

## 1. Stakeholder

| **Nhóm** | **Vai trò** | **Ảnh hưởng** | **Mức độ quan tâm** | **Mối quan tâm chính** |
|---|---|---|---|---|
| **Sponsor** — Anh Đỗ Anh Minh | Chủ dự án; phê duyệt mục tiêu kinh doanh và phạm vi tổng thể, cấp nguồn lực, giải quyết bất đồng vượt thẩm quyền của PM/PO | cao | cao | Đảm bảo sản phẩm phục vụ mục tiêu kinh doanh; quyết định các thay đổi lớn về phạm vi và nguồn lực. Quan tâm đến kết quả tổng thể |
| **PM/PO** — Anh Nguyễn Hữu Huy | Chịu trách nhiệm về product vision, phạm vi và thứ tự ưu tiên backlog; nghiệm thu sản phẩm bàn giao; theo dõi tiến độ và ngân sách | cao | cao | Đảm bảo sản phẩm tạo ra giá trị. Quyết định phạm vi và thứ tự ưu tiên từng sprint; chịu trách nhiệm trực tiếp kết quả sản phẩm |
| **BA** — Anh Đặng Việt Hoàng | Thu thập và phân tích yêu cầu; xây dựng Use Case, User Story, tiêu chí chấp nhận và mô hình quy trình; duy trì RTM; kết nối PO với đội phát triển | trung bình | cao | Yêu cầu rõ ràng, nhất quán, có thể truy vết và đủ điều kiện để thiết kế, phát triển, kiểm thử và nghiệm thu |
| **Đội phát triển** — Anh Nguyễn Hữu Đại (Tech Lead) | Định hướng kỹ thuật; thiết kế, phát triển, triển khai và duy trì hệ thống cùng đội kỹ thuật | cao | cao | Yêu cầu khả thi, phạm vi ổn định trong sprint, chất lượng kỹ thuật, bảo mật, hiệu năng và khả năng vận hành |
| **Bộ phận tiếp thị** — Chị Nguyễn Thanh Thảo | Marketing Lead, đại diện bộ phận Tiếp thị | trung bình | cao | Giảm công việc thủ công; có thể truyền thông rõ về thử thách, nhà tổ chức và quyền lợi; thu hút người tham gia phù hợp |
| **Đại diện người dùng (`USER`)** — Chị Nguyễn Yến Nga | Người tham gia; khám phá thử thách, đăng ký, thực hiện hoạt động và theo dõi kết quả | trung bình | cao | Dễ tìm thử thách phù hợp, hiểu luật chơi, tham gia thuận tiện, được đánh giá công bằng và lưu giữ thành tích |
| **Đại diện nhà tổ chức (`ORGANIZER`)** — Chị Nguyễn Thiên Yến | Nhà tổ chức; tạo, công bố và vận hành thử thách, xét minh chứng và ghi nhận kết quả | trung bình | cao | Quy trình thẩm định rõ ràng; dễ tạo và quản lý thử thách; xem được dữ liệu cần thiết; kiểm duyệt và chốt kết quả minh bạch |
| **Quản trị viên (`ADMIN`)** — Bộ phận vận hành DOMO | Vận hành nền tảng; quản lý tài khoản và quyền, thẩm định nhà tổ chức, giám sát thử thách, xử lý vi phạm | cao | cao | Thao tác quản trị rõ ràng, kiểm soát quyền phù hợp, truy vết được quyết định và xử lý vi phạm kịp thời |
| **Đối tác tích hợp** | Google, dịch vụ lưu trữ file, dịch vụ gửi email; đối tác thanh toán khi thanh toán được đưa vào phạm vi | cao | thấp | Yêu cầu kỹ thuật, giới hạn sử dụng, tính sẵn sàng, bảo mật và chính sách của dịch vụ |

## 2. Ma trận quan tâm và ảnh hưởng

*[Hình 1 — Ma trận ảnh hưởng và quan tâm của stakeholder](../diagrams/stakeholder-matrix.png)*

## 3. Actor trong hệ thống

### Chú thích các mục

| **Mục** | **Ý nghĩa** |
|---|---|
| **Loại** | Cho biết actor là người đã hoặc chưa đăng nhập DOMO, hay là một hệ thống hỗ trợ không có tài khoản DOMO |
| **Mô tả** | Xác định actor là ai và vai trò của actor trong phạm vi hệ thống |
| **Mục tiêu nghiệp vụ** | Nêu kết quả actor muốn đạt được khi sử dụng DOMO. Với hệ thống hỗ trợ, mục này nêu mục đích DOMO sử dụng dịch vụ đó |
| **Quyền hạn** | Liệt kê các hoạt động actor được phép thực hiện |
| **Giới hạn** | Nêu các hoạt động actor không được phép thực hiện, kể cả khi các hoạt động đó có vẻ liên quan đến quyền của actor |
| **Actor liên quan** | Liệt kê các actor có quan hệ trực tiếp đến actor hiện tại |

### Actor nghiệp vụ

DOMO có ba vai trò gắn với tài khoản: **Người dùng, Nhà tổ chức và Quản trị viên**. Khách là người chưa đăng nhập, không phải một vai trò của tài khoản.

superAdmin là cờ bảo vệ gắn với đúng một tài khoản, không phải actor hay vai trò riêng. Tài khoản này không thể bị thay đổi bởi tài khoản khác (`RULE-006`). Mỗi tài khoản chỉ có một vai trò tại một thời điểm.

#### 1. Khách

| | |
|---|---|
| **Loại** | Người sử dụng chưa đăng nhập, chưa có tài khoản DOMO |
| **Mô tả** | Người truy cập DOMO trước khi đăng nhập |
| **Mục tiêu nghiệp vụ** | Tìm hiểu DOMO và nội dung công khai trước khi quyết định đăng nhập |
| **Quyền hạn** | Xem nội dung và thử thách công khai |
| **Giới hạn** | Không truy cập dữ liệu nghiệp vụ<br>DOMO không tạo hồ sơ người dùng cho Khách |
| **Actor liên quan** | Dịch vụ xác thực Google (xác thực đăng nhập; lần đăng nhập đầu tiên tạo tài khoản với vai trò Người dùng) |

#### 2. Người dùng (`USER`)

| | |
|---|---|
| **Loại** | Người đã đăng nhập và có tài khoản DOMO |
| **Mô tả** | Vai trò cơ bản của tài khoản DOMO, phục vụ việc khám phá và tham gia thử thách. Tài khoản mới được gán vai trò này khi đăng nhập lần đầu |
| **Mục tiêu nghiệp vụ** | Tìm và tham gia thử thách phù hợp với lĩnh vực quan tâm; có thể đăng ký để trở thành Nhà tổ chức |
| **Quyền hạn** | Xem và cập nhật hồ sơ của chính mình<br>Xem, tham gia và rời thử thách<br>Ghi nhận hoạt động và nộp minh chứng<br>Theo dõi tiến độ và thứ hạng của mình trong thử thách<br>Xem thành tích được ghi nhận qua các thử thách<br>Nhận thông báo về các thay đổi liên quan |
| **Giới hạn** | Không xem hoặc sửa hồ sơ của người khác<br>Không tạo thử thách khi chưa được cấp vai trò Nhà tổ chức và đáp ứng điều kiện thẩm định |
| **Actor liên quan** | Dịch vụ xác thực Google (xác thực đăng nhập)<br>Dịch vụ lưu trữ tệp (lưu ảnh đại diện và minh chứng)<br>Nhà tổ chức (bên tổ chức thử thách mà Người dùng tham gia) |

#### 3. Nhà tổ chức (`ORGANIZER`)

| | |
|---|---|
| **Loại** | Người đã đăng nhập và có tài khoản DOMO |
| **Mô tả** | Vai trò cho phép tạo và vận hành thử thách. Được cấp vai trò Nhà tổ chức không đồng nghĩa hồ sơ đã được DOMO thẩm định |
| **Mục tiêu nghiệp vụ** | Hoàn tất thẩm định của DOMO để đủ điều kiện tạo và vận hành thử thách |
| **Quyền hạn** | Có các quyền của Người dùng<br>Quản lý hồ sơ nhà tổ chức của mình<br>Tạo, xuất bản, vận hành và kết thúc thử thách của mình khi đáp ứng điều kiện thẩm định<br>Kiểm duyệt minh chứng của người tham gia<br>Chốt kết quả và ghi nhận tình trạng trao thưởng<br>Xem báo cáo về thử thách của mình |
| **Giới hạn** | Chỉ thao tác trên hồ sơ và thử thách của mình; không xem dữ liệu riêng của Nhà tổ chức khác<br>Không được tạo và công bố thử thách khi hồ sơ chưa được DOMO thẩm định |
| **Actor liên quan** | Người dùng (các quyền của Người dùng được kế thừa)<br>Quản trị viên (thẩm định hồ sơ và quản lý tư cách Nhà tổ chức)<br>Dịch vụ lưu trữ tệp (lưu trữ giấy tờ phục vụ thẩm định)<br>Dịch vụ gửi email (gửi thông báo liên quan đến hồ sơ và thẩm định) |

#### 4. Quản trị viên (`ADMIN`)

| | |
|---|---|
| **Loại** | Người đã đăng nhập, có tài khoản và thuộc bộ phận vận hành DOMO |
| **Mô tả** | Vai trò quản trị và vận hành nền tảng, bao gồm thẩm định hồ sơ, quản lý tài khoản, vai trò và nội dung |
| **Mục tiêu nghiệp vụ** | Duy trì chất lượng và độ tin cậy của DOMO bằng cách kiểm soát tư cách Nhà tổ chức, xử lý tài khoản vi phạm và quản lý nội dung, danh mục |
| **Quyền hạn** | Xem danh sách tài khoản<br>Đổi vai trò, vô hiệu hoá và khôi phục tài khoản<br>Thẩm định hồ sơ nhà tổ chức; đình chỉ hoặc khôi phục tư cách Nhà tổ chức<br>Giám sát thử thách đã công khai và can thiệp khi có vi phạm<br>Quản lý danh mục, huy hiệu và nội dung công khai<br>Xem nhật ký thao tác<br>Xem báo cáo toàn nền tảng |
| **Giới hạn** | Không tự đổi vai trò hoặc tự vô hiệu hoá tài khoản của chính mình (`RULE-007`)<br>Không tác động tới tài khoản superAdmin (`RULE-006`)<br>Không thẩm định hồ sơ của Nhà tổ chức mà mình là thành viên<br>Không mặc nhiên có quyền của Nhà tổ chức; chỉ vận hành thử thách khi được cấp năng lực tương ứng |
| **Actor liên quan** | Nhà tổ chức (bên được thẩm định và quản lý tư cách)<br>Người dùng (bên có tài khoản được quản trị)<br>Dịch vụ gửi email (gửi thông báo về quyết định quản trị)<br>Dịch vụ lưu trữ tệp (cung cấp quyền xem giấy tờ theo phạm vi được phép) |

### Hệ thống hỗ trợ

Các hệ thống dưới đây không có tài khoản DOMO. Mối quan hệ với DOMO được thiết lập thông qua tích hợp hoặc cấu hình hạ tầng, không phải thông qua vai trò tài khoản. Quyền hạn trong các mục dưới đây mô tả phạm vi dịch vụ mà hệ thống cung cấp cho DOMO.

#### 5. Dịch vụ xác thực Google — Google / Firebase Auth

| | |
|---|---|
| **Loại** | Hệ thống bên ngoài — dịch vụ xác thực |
| **Mô tả** | Phương thức đăng nhập duy nhất của DOMO. DOMO không cung cấp đăng nhập bằng email và mật khẩu |
| **Mục tiêu nghiệp vụ** | Sử dụng tài khoản Google để xác thực người đăng nhập mà không phải tự quản lý mật khẩu |
| **Quyền hạn** | Xác nhận kết quả xác thực và cung cấp thông tin định danh cần thiết cho DOMO |
| **Giới hạn** | Không truy cập dữ liệu nghiệp vụ của DOMO<br>Không quyết định vai trò tài khoản; DOMO tự gán vai trò sau khi nhận kết quả xác thực. Việc xác thực tài khoản Google không đồng nghĩa với xác minh danh tính pháp lý của người dùng |
| **Actor liên quan** | Khách (khởi tạo đăng nhập)<br>Người dùng, Nhà tổ chức, Quản trị viên (đăng nhập vào DOMO) |

#### 6. Dịch vụ lưu trữ tệp — Object Storage (MinIO)

| | |
|---|---|
| **Loại** | Hạ tầng lưu trữ do DOMO tự vận hành |
| **Mô tả** | Lưu ảnh đại diện, ảnh danh mục, minh chứng hoạt động và giấy tờ pháp lý của Nhà tổ chức |
| **Mục tiêu nghiệp vụ** | Lưu tệp bên ngoài cơ sở dữ liệu nghiệp vụ và giới hạn khả năng truy cập vào giấy tờ không công khai |
| **Quyền hạn** | Cho phép tải lên hoặc xem tệp thông qua liên kết tạm thời do DOMO cấp: liên kết tải lên có hiệu lực 5 phút và liên kết xem giấy tờ có hiệu lực 15 phút |
| **Giới hạn** | Không tự quyết định người nào được truy cập tệp; DOMO phải kiểm tra quyền trước khi cấp liên kết<br>Không công khai giấy tờ pháp lý |
| **Actor liên quan** | Người dùng, Nhà tổ chức và Quản trị viên — tải lên hoặc xem tệp theo quyền được cấp |

#### 7. Dịch vụ gửi email — SMTP / Mailer (Gmail)

| | |
|---|---|
| **Loại** | Hệ thống bên ngoài — gửi email qua máy chủ thư Gmail |
| **Mô tả** | Gửi email cho các sự kiện được hỗ trợ trong MVP, liên quan đến quản trị tài khoản, hồ sơ và thẩm định Nhà tổ chức |
| **Mục tiêu nghiệp vụ** | Thông báo cho các bên liên quan về những thay đổi cần lưu ý mà không yêu cầu họ tự kiểm tra DOMO |
| **Quyền hạn** | Gửi email theo nội dung và yêu cầu do DOMO cung cấp |
| **Giới hạn** | Không truy cập dữ liệu DOMO ngoài nội dung cần thiết để gửi email và không gửi dữ liệu trở lại DOMO<br>Nếu gửi email thất bại, trạng thái nghiệp vụ đã được cập nhật vẫn được giữ nguyên |
| **Actor liên quan** | Người dùng, Nhà tổ chức (người nhận thông báo)<br>Quản trị viên (có thể thực hiện thao tác dẫn đến thông báo) |
