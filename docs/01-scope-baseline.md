# Scope Baseline

## 1. Bối cảnh dự án

`DOMO` là dự án giúp người dùng thay đổi bản thân thông qua việc cùng tham gia các hoạt động, thử thách với cộng đồng. Đây không phải là ứng dụng theo dõi thói quen, quản lý việc cần làm hay cung cấp các thử thách độc lập.

Dự án dự định sẽ hướng tới bốn nguồn doanh thu chủ yếu: gói trả phí cho người dùng, dịch vụ dành cho creator, chiến dịch của thương hiệu và phiên bản dành cho doanh nghiệp. Để phát triển các nguồn doanh thu này, DOMO cần xây dựng hai nền tảng cốt lõi:

- Nguồn thử thách đa dạng, hấp dẫn và đáng tin cậy.
- Môi trường giúp người tham gia gắn bó đủ lâu để tích lũy thành tích.

MVP chưa tập trung tạo doanh thu. Mục tiêu của giai đoạn này là thiết lập nền tảng cho hai điều kiện trên bằng cách xác định rõ: ai được quyền tổ chức thử thách và ai được truy cập, sử dụng dữ liệu của người khác.

### Market Gap

| Cách các thử thách được tiến hành ở Việt Nam | Hạn chế hiện tại | Điểm khác biệt của DOMO |
|---|---|---|
| App theo dõi thói quen cá nhân | Người dùng tự thực hiện, thường không có đơn vị tổ chức, phần thưởng thực tế hoặc cơ chế công nhận kết quả | Có nhà tổ chức chịu trách nhiệm, cùng phần thưởng và sự công nhận công khai |
| Fanpage, nhóm Facebook kết hợp biểu mẫu và bảng tính | Thiếu quy trình nhất quán để duyệt minh chứng; bảng xếp hạng khó kiểm chứng; dữ liệu người tham gia phân tán ở nhiều nơi | Minh chứng được xử lý theo một quy trình duyệt; thành tích được lưu tập trung và có thể tra cứu lại |
| Trang web hoặc ứng dụng riêng cho từng chiến dịch của thương hiệu | Sau khi chiến dịch kết thúc, người tham gia khó tiếp tục sử dụng thành tích đã đạt được ở chiến dịch khác | Thành tích được tích lũy trên cùng một tài khoản qua nhiều thử thách và nhà tổ chức |
| Nền tảng thể thao có thử thách do câu lạc bộ tổ chức | Chủ yếu hỗ trợ các môn có thể đo bằng cảm biến, chưa phù hợp với thử thách về học tập, tài chính hoặc thiền | Hỗ trợ nhiều loại thử thách; nhà tổ chức xác định loại minh chứng cần nộp và duyệt kết quả |

Điểm hạn chế chung: **chưa ghi nhận nền tảng nào kết hợp được nhà tổ chức đã qua xác minh với hồ sơ thành tích cá nhân mà người tham gia có thể tiếp tục sử dụng qua nhiều thử thách.**

## 2. Mục tiêu - Business Requirement

`BR` là nhu cầu nghiệp vụ, trả lời câu hỏi *vì sao phải làm*. Mỗi BR nêu vấn đề doanh nghiệp đang gặp và giá trị nhận được khi giải quyết nó, không đề cập giải pháp kỹ thuật.

### Truy cập nền tảng

| ID | Nhu cầu nghiệp vụ | Vì sao cần | Tiêu chí thành công | Ưu tiên |
|---|---|---|---|---|
| `BR-001` | Giúp người dùng mới bắt đầu sử dụng DOMO nhanh chóng và duy trì quyền truy cập vào tài khoản, thành tích khi đổi thiết bị | Quy trình tạo tài khoản và khai hồ sơ có thể khiến người dùng dừng đăng ký. <br> Người dùng cũng có thể mất quyền truy cập khi đổi thiết bị hoặc mất tài khoản đăng nhập liên kết, kéo theo nguy cơ mất thành tích đã tích lũy | Người dùng đăng nhập và truy cập nội dung mà không phải tạo mật khẩu hoặc khai lại hồ sơ; <br> Trước khi đăng nhập, người dùng được thông báo về rủi ro mất quyền truy cập. <br> Khi đăng nhập trên thiết bị khác, người dùng truy cập đúng tài khoản cũ và các thành tích vẫn được giữ nguyên | Must |

### Vi phạm, phân quyền và truy vết

| ID | Nhu cầu nghiệp vụ | Vì sao cần | Tiêu chí thành công | Ưu tiên |
|---|---|---|---|---|
| `BR-002` | Cho phép xử lý kịp thời tài khoản và thử thách vi phạm, đồng thời khôi phục nguyên trạng tài khoản nếu bị xử lý nhầm | Phần thưởng có thể tạo động cơ gian lận hoặc tổ chức thử thách lừa đảo, nên bộ phận vận hành cần có khả năng xử lý kịp thời. <br> Nếu tài khoản bị xử lý nhầm, cần khôi phục được để tránh gây thiệt hại cho người dùng | Tài khoản bị đình chỉ **không thể đăng nhập**; khi được khôi phục, dữ liệu tài khoản vẫn nguyên vẹn. <br> Bộ phận vận hành có thể tạm dừng hoặc gỡ bất kỳ thử thách công khai nào vi phạm mà không cần duyệt trước tất cả thử thách | Must |
| `BR-003` | Bảo đảm quyền của mỗi tài khoản phù hợp với trách nhiệm hiện tại và duy trì quyền kiểm soát tối cao của DOMO | Vai trò và trách nhiệm của người dùng khác nhau, đồng thời có thể thay đổi. <br> Việc cấp quyền không phù hợp có thể ảnh hưởng đến hoạt động. <br> Do quản trị viên có quyền hạn lớn, DOMO cũng cần duy trì một tài khoản có thể can thiệp nếu quyền quản trị bị sử dụng sai | **Sau lần đăng nhập kế tiếp** kể từ khi thay đổi vai trò, tài khoản không thể sử dụng chức năng ngoài vai trò hiện tại. <br> Luôn tồn tại **đúng một** tài khoản quản trị gốc (superAdmin); không tài khoản nào khác có thể thay đổi hoặc tác động đến tài khoản này | Must |
| `BR-004` | Bảo đảm có thể truy vết các quyết định và thao tác quản trị | Khi phát sinh tranh chấp, bộ phận vận hành cần căn cứ để xác định và giải thích quyết định đã được đưa ra | **Mọi** thao tác quản trị đều có audit log ghi lại người thực hiện, đối tượng, nội dung thay đổi, lý do, thời điểm và kết quả | Must |

### Hồ sơ và dữ liệu cá nhân

| ID | Nhu cầu nghiệp vụ | Vì sao cần | Tiêu chí thành công | Ưu tiên |
|---|---|---|---|---|
| `BR-005` | Duy trì một hồ sơ duy nhất cho mỗi người tham gia và cho phép sử dụng lại thông tin khi đăng ký thử thách | Việc từng ban tổ chức tự thu thập thông tin khiến người tham gia phải khai báo nhiều lần và có thể phát sinh dữ liệu không nhất quán. <br> Điều này làm tăng khả năng người dùng bỏ dở đăng ký và gây khó khăn khi cần đối chiếu thông tin | Mỗi tài khoản gắn với đúng một hồ sơ; việc cập nhật hồ sơ không tạo hồ sơ người tham gia mới. <br> Khi đăng ký thử thách, người tham gia không phải nhập lại thông tin đã có trong hồ sơ | Must |
| `BR-006` | Thu thập dữ liệu cá nhân ở mức cần thiết, minh bạch mục đích sử dụng và giới hạn quyền truy cập theo nhu cầu | Người dùng cần biết DOMO lưu và sử dụng thông tin cá nhân như thế nào. <br> Thu thập dữ liệu nhạy cảm không cần thiết làm tăng trách nhiệm bảo vệ dữ liệu mà không bảo đảm ngăn được gian lận. <br> Việc chia sẻ sai phạm vi cũng tạo rủi ro về an toàn và bảo mật dữ liệu | Người dùng xem và sửa được toàn bộ thông tin DOMO lưu về mình, đồng thời biết mục đích sử dụng của từng phần. <br> Hồ sơ **không lưu** số, ảnh hoặc mã QR CCCD. <br> Nhà tổ chức chỉ nhận thông tin cần cho thử thách của mình; <br> Dữ liệu riêng của nhà tổ chức này không hiển thị với nhà tổ chức khác | Must |

### Nhà tổ chức đáng tin

| ID | Nhu cầu nghiệp vụ | Vì sao cần | Tiêu chí thành công | Ưu tiên |
|---|---|---|---|---|
| `BR-007` | Chỉ cho phép nhà tổ chức đã được thẩm định tổ chức thử thách, cung cấp quy trình thẩm định rõ ràng và xử lý được nhà tổ chức vi phạm | Nhà tổ chức không đáng tin có thể đưa ra cam kết về phần thưởng nhưng không thực hiện, ảnh hưởng đến người tham gia và uy tín DOMO. <br> Quy trình thẩm định thiếu hướng dẫn cũng có thể khiến nhà tổ chức đủ điều kiện bỏ dở hồ sơ | **Chỉ** nhà tổ chức đã qua thẩm định mới được công bố thử thách. <br> Người nộp biết hồ sơ đang ở bước nào và cần bổ sung nội dung gì; khi được yêu cầu bổ sung, chỉ cần sửa phần liên quan. <br> Bộ phận vận hành có thể đình chỉ và khôi phục tư cách nhà tổ chức; nhà tổ chức bị đình chỉ không thể công bố thử thách mới | Must |

### Vòng đời thử thách

| ID | Nhu cầu nghiệp vụ | Vì sao cần | Tiêu chí thành công | Ưu tiên |
|---|---|---|---|---|
| `BR-008` | Cho phép nhà tổ chức tạo, công bố và vận hành thử thách trên DOMO | Việc quản lý thử thách bằng bài đăng, tin nhắn và bảng tính tốn công sức, khó mở rộng và có thể quá tải đội DOMO. <br> Xây dựng trang web riêng cho từng chiến dịch cũng làm tăng chi phí | Nhà tổ chức có thể tự tạo, công bố, tạm dừng và kết thúc thử thách, đồng thời xem danh sách người tham gia trên DOMO mà không cần quản lý các hoạt động này bằng tin nhắn hoặc bảng tính | Must |
| `BR-009` | Giúp người dùng tìm thử thách phù hợp, nắm rõ điều kiện trước khi đăng nhập và chủ động quản lý việc tham gia | Người dùng cần biết nội dung và lợi ích của thử thách trước khi cung cấp thông tin hoặc đăng nhập. <br> Điều kiện không rõ ràng có thể khiến họ bỏ cuộc; <br> Danh mục không thống nhất khiến việc tìm kiếm kém hiệu quả | Khách có thể xem luật chơi, thời hạn, cách chứng minh hoàn thành và phần thưởng trước khi đăng nhập; chỉ cần đăng nhập khi chọn tham gia. <br> Người dùng biết trước mình có đáp ứng điều kiện tham gia hay không và có thể tự rời thử thách. <br> Tất cả thử thách thuộc danh mục chung do DOMO quản lý; bộ phận vận hành có thể thêm, ẩn và khôi phục danh mục | Must |
| `BR-010` | Bảo đảm kết quả thử thách có căn cứ và việc trao giải được ghi nhận | Việc xét minh chứng và trao giải có thể gây tranh chấp giữa người tham gia và nhà tổ chức, đặc biệt khi phần thưởng được trao ngoài DOMO. <br> Nhà tổ chức tham gia thử thách của chính mình cũng có thể làm giảm niềm tin vào tính công bằng | Mỗi lần hoàn thành được ghi nhận dựa trên minh chứng đã được chấp nhận. <br> Minh chứng bị từ chối có lý do và người tham gia được nộp lại. <br> Với mỗi thử thách đã kết thúc, kết quả được chốt và có trạng thái trao giải của từng người thắng. <br> Nhà tổ chức **không thể** tham gia thử thách của chính mình | Must |

### Giữ chân người tham gia và nhà tổ chức

| ID | Nhu cầu nghiệp vụ | Vì sao cần | Tiêu chí thành công | Ưu tiên |
|---|---|---|---|---|
| `BR-011` | Lưu giữ và tích lũy thành tích của người tham gia trên một tài khoản theo bộ chuẩn thống nhất, đồng thời cung cấp thông tin tiến độ | Nếu thành tích không được lưu sau mỗi thử thách, người tham gia khó thấy giá trị tích lũy và có thể thiếu động lực quay lại. <br> Thành tích cần có ý nghĩa nhất quán giữa các nhà tổ chức; <br> Thông tin tiến độ giúp người tham gia biết mình đang ở đâu trong thử thách | Mỗi lần hoàn thành thử thách được ghi vào tài khoản bằng huy hiệu thuộc bộ chuẩn của DOMO và vẫn xem được sau khi thử thách kết thúc. <br> Người tham gia xem được tiến độ và thứ hạng của mình trong từng thử thách đang tham gia | Must |
| `BR-012` | Cung cấp cho nhà tổ chức và DOMO số liệu cần thiết để đánh giá hiệu quả và lập kế hoạch | Nhà tổ chức cần biết kết quả thử thách để đánh giá việc tổ chức đợt tiếp theo. <br> DOMO cần số liệu về người dùng và nhà tổ chức để đánh giá mức độ sử dụng nền tảng và xác định bước phát triển tiếp theo | Với mỗi thử thách, nhà tổ chức xem được số người tham gia, tỷ lệ hoàn thành và số minh chứng đang chờ duyệt. <br> Bộ phận vận hành xem được số người dùng và số nhà tổ chức của nền tảng tại bất kỳ thời điểm nào | Must |
| `BR-013` | Thông báo kịp thời cho người tham gia và nhà tổ chức về các thay đổi liên quan đến họ | Thiếu thông tin cập nhật có thể khiến người dùng không biết hồ sơ đang được xử lý đến đâu, không hiểu lý do bị hạn chế hoặc bỏ lỡ kết quả và thời hạn | Người bị ảnh hưởng nhận được thông báo về kết quả thẩm định hồ sơ, kết quả duyệt minh chứng, thay đổi vai trò hoặc trạng thái tài khoản. <br> Thông báo nêu lý do đối với quyết định có lý do cần truyền đạt | Must |

### Quản trị nền tảng

| ID | Nhu cầu nghiệp vụ | Vì sao cần | Tiêu chí thành công | Ưu tiên |
|---|---|---|---|---|
| `BR-014` | Cho phép bộ phận vận hành quản lý nội dung công khai mà không phụ thuộc đội kỹ thuật | Việc phải nhờ đội kỹ thuật mỗi khi đăng hướng dẫn, thể lệ hoặc tin mới làm chậm quá trình cập nhật và tăng khối lượng công việc, dù đây là kênh giới thiệu DOMO và nhà tổ chức | Bộ phận vận hành có thể tự soạn, lưu nháp, công bố và phân loại bài viết; mỗi bài hiển thị đúng người viết | Should |

## 3. Trong phạm vi

### Actor nghiệp vụ

| Actor | Là ai | Kết quả nghiệp vụ | Module trực tiếp |
|---|---|---|---|
| **Khách** | Người chưa đăng nhập và chưa có tài khoản | Xem nội dung công khai và tìm hiểu nền tảng trước khi đăng nhập | `M-01`, `M-08`, `M-19` |
| **Người dùng** | Vai trò cơ sở của mọi tài khoản | Quản lý hồ sơ, tham gia thử thách, ghi nhận hoạt động, theo dõi tiến độ, giữ thành tích | `M-01`, `M-04`, `M-14`, `M-19`–`M-23` |
| **Nhà tổ chức** | Người dùng đáp ứng điều kiện tổ chức thử thách | Tạo, xuất bản, vận hành, kiểm duyệt và kết thúc thử thách của mình; không thấy dữ liệu riêng của bên khác | `M-05`, `M-13`, `M-14`, `M-24`–`M-28` |
| **Quản trị viên** | Nhân sự thuộc bộ phận vận hành DOMO | Quản trị tài khoản, quyền, nhà tổ chức, danh mục, nội dung và vi phạm theo năng lực được cấp | `M-02`, `M-03`, `M-06`–`M-08`, `M-15`, `M-16`, `M-29` |

- superAdmin là cờ bảo vệ trên đúng một tài khoản, không phải một actor hay một vai riêng.
- Một tài khoản chỉ mang một vai trò tại một thời điểm.
- Nhà tổ chức kế thừa quyền của Người dùng. Quản trị viên **không** mặc nhiên có quyền Nhà tổ chức; chỉ vận hành thử thách khi được cấp năng lực tương ứng.

### Hệ thống hỗ trợ

| Hệ thống | Vai trò trong MVP | Module |
|---|---|---|
| **Google / Firebase Auth** | Phương thức đăng nhập duy nhất | `M-01` |
| **Object Storage** | Lưu ảnh, giấy tờ và minh chứng ngoài cơ sở dữ liệu nghiệp vụ | `M-04`, `M-05`, `M-21` |
| **SMTP / Mailer** | Gửi email cho các sự kiện được hỗ trợ trong MVP | `M-14`, nhận sự kiện từ `M-03`, `M-05`, `M-06` |

### Module

| Module | Kết quả nghiệp vụ | Actor chính |
|---|---|---|
| `M-01` Đăng nhập và xác thực | Người dùng đăng nhập và quay lại bằng cùng tài khoản Google, đồng thời được thông tin về rủi ro mất quyền truy cập | Khách, Người dùng |
| `M-04` Hồ sơ cá nhân và định danh | Người dùng duy trì hồ sơ và chia sẻ đúng thông tin cần cho từng mục đích; MVP không thu thập CCCD hoặc xác minh danh tính người tham gia | Người dùng, Quản trị viên |
| `M-19` Khám phá thử thách | Người dùng tìm thử thách phù hợp và xem đủ thông tin trước khi tham gia | Khách, Người dùng |
| `M-20` Tham gia thử thách | Người dùng tham gia và chủ động rời thử thách cá nhân theo quy định | Người dùng |
| `M-21` Ghi nhận hoạt động và minh chứng | Người dùng ghi nhận hoạt động và cung cấp minh chứng để xét kết quả | Người dùng |
| `M-22` Tiến độ và xếp hạng cá nhân | Người dùng theo dõi tiến độ và thứ hạng của mình trong thử thách | Người dùng |
| `M-23` Thành tích và quyền lợi | Thành tích của người dùng được ghi nhận và lưu giữ qua nhiều thử thách | Người dùng |
| `M-14` Thông báo | Các bên liên quan nhận thông tin kịp thời về những thay đổi cần lưu ý hoặc xử lý | Người dùng, Nhà tổ chức |
| `M-05` Hồ sơ nhà tổ chức | Người tham gia biết chủ thể chịu trách nhiệm cho thử thách qua hồ sơ đã thẩm định; MVP chỉ tiếp nhận cá nhân/KOL và nhóm/CLB | Nhà tổ chức |
| `M-24` Tạo thử thách | Nhà tổ chức xác định mục tiêu, nội dung, luật, thời gian và tiêu chí của thử thách | Nhà tổ chức |
| `M-25` Xuất bản và phân phối thử thách | Nhà tổ chức đưa thử thách từ trạng thái chuẩn bị đến người dùng | Nhà tổ chức |
| `M-26` Vận hành thử thách | Nhà tổ chức quản lý vòng đời thử thách và xử lý người tham gia không phù hợp | Nhà tổ chức |
| `M-27` Kiểm duyệt minh chứng | Bên chịu trách nhiệm có thể chấp nhận hoặc từ chối minh chứng và nêu rõ lý do | Nhà tổ chức |
| `M-28` Kết quả và trao thưởng | Nhà tổ chức chốt kết quả, xuất dữ liệu và ghi nhận việc trao thưởng ngoài hệ thống | Nhà tổ chức |
| `M-13` Báo cáo cho nhà tổ chức | Nhà tổ chức theo dõi số người tham gia, tỷ lệ hoàn thành và minh chứng chờ xử lý | Nhà tổ chức |
| `M-06` Kiểm duyệt nhà tổ chức | Chỉ chủ thể đáp ứng yêu cầu mới được tổ chức thử thách; vận hành có thể đình chỉ hoặc khôi phục tư cách khi cần | Quản trị viên |
| `M-03` Quản trị tài khoản người dùng | Vận hành quản lý trạng thái tài khoản, vai trò và quyền truy cập | Quản trị viên |
| `M-02` Vai trò và phân quyền | Vận hành áp dụng giới hạn trách nhiệm phù hợp với từng vai trò | Quản trị viên |
| `M-15` Giám sát thử thách toàn nền tảng | Vận hành can thiệp thử thách công khai vi phạm mà không cần duyệt trước tất cả thử thách | Quản trị viên |
| `M-07` Quản lý danh mục | DOMO quản lý danh mục phân loại và bộ huy hiệu chuẩn dùng chung | Quản trị viên |
| `M-08` Quản lý nội dung | Vận hành duy trì nội dung công khai phục vụ truyền thông và hướng dẫn | Quản trị viên, Khách |
| `M-16` Nhật ký thao tác và truy vết | Vận hành điều tra sai lệch, xác định trách nhiệm và tra cứu căn cứ của quyết định quản trị | Quản trị viên |
| `M-29` Báo cáo toàn nền tảng | Vận hành theo dõi quy mô và tình trạng hoạt động cơ bản của nền tảng | Quản trị viên |

### Kênh và nền tảng

| Kênh | Trong MVP | Phạm vi |
|---|---|---|
| **Web** | Có | Hỗ trợ đầy đủ cả bốn actor |
| **Android** | Có, bản cài nội bộ | Chỉ cho Khách và Người dùng: `M-01`, `M-04`, `M-14`, `M-19`–`M-23`. Nhà tổ chức và Quản trị viên không có giao diện trên Android |
| **iOS** | Không | Chưa triển khai trong MVP; xem (`O4`) |
| **Email** | Có, chỉ để thông báo | Gửi các thông báo thuộc phạm vi `M-14` gửi |
| **Kênh khác** | Không | Không bao gồm SMS, chatbot, nội dung nhúng trên web hoặc email marketing |

## 4. Ngoài phạm vi

| ID | Hạng mục ngoài phạm vi | Module | Lý do |
|---|---|---|---|
| `O1` | Đăng nhập bằng Facebook hoặc số điện thoại | `M-01` | MVP chỉ hỗ trợ đăng nhập bằng Google |
| `O2` | Hỗ trợ nhiều phương thức đăng nhập hoặc gộp tài khoản | `M-01` | MVP chỉ hỗ trợ đăng nhập bằng Google |
| `O3` | Đăng nhập bằng Apple | `M-01` | Chưa triển khai bản iOS |
| `O4` | Ứng dụng iOS | Kênh | Chưa triển khai trong MVP |
| `O5` | Khôi phục tài khoản khi người dùng mất quyền truy cập Google | `M-01` | MVP không có phương thức định danh thay thế |
| `O6` | Chuyển giao quyền đại diện CLB cho người khác | `M-05` | CLB tự bàn giao email cho người đại diện mới |
| `O7` | Nhà tổ chức là doanh nghiệp | `M-05` | MVP chỉ tiếp nhận cá nhân/KOL và nhóm/CLB |
| `O8` | Phân quyền nhân viên thuộc nhà tổ chức | `M-05`, `M-02` | MVP giả định mỗi nhà tổ chức có một người vận hành |
| `O9` | Màn hình quản lý lý do từ chối | `M-06`, `M-07` | MVP sử dụng danh sách lý do cố định |
| `O10` | Đối chiếu CCCD với dữ liệu dân cư | `M-04` | MVP không thu thập CCCD |
| `O11` | Thanh toán, ví, đối soát và KYC | `M-28` | Phần thưởng được trao ngoài hệ thống |
| `O12` | Phát hành ứng dụng trên CH Play | Kênh | MVP sử dụng bản cài đặt nội bộ |
| `O13` | Trợ lý AI | Tích hợp | Không thuộc vòng chức năng cốt lõi của MVP |
| `O14` | Thông báo đẩy (push notification) | `M-14` | MVP sử dụng thông báo trong ứng dụng và email |
| `O15` | Một tài khoản có nhiều vai trò đồng thời hoặc có màn hình chuyển vai | `M-02`, `M-03`, `M-05` | MVP giới hạn mỗi tài khoản ở một vai trò tại một thời điểm |
| `O16` | Quản lý thiết bị và phiên đăng nhập, bao gồm đăng xuất từ xa | `M-01` | MVP chỉ hỗ trợ cơ chế phiên đăng nhập cơ bản |
| `O17` | Yêu thích, bảng tin đề xuất và bộ lọc nhiều tầng | `M-19` | MVP chỉ bao gồm chức năng khám phá cốt lõi |
| `O18` | Hàng chờ; thi theo đội/nhóm; tự lập đội hoặc chuyển đội | `M-20`, `M-24` | MVP chỉ hỗ trợ thử thách cá nhân |
| `O19` | Nộp minh chứng bằng tệp tùy ý hoặc video; sửa minh chứng sau khi đã duyệt | `M-21` | MVP chỉ nhận ảnh và số liệu nhập thủ công |
| `O20` | Chuỗi ngày liên tiếp; xếp hạng đội hoặc công khai; nhiều công thức tính điểm | `M-22`, `M-24` | MVP chỉ hỗ trợ xếp hạng cá nhân |
| `O21` | Cấp chứng nhận PDF có mã tra cứu | `M-23` | MVP ghi nhận thành tích trong hệ thống |
| `O22` | Tùy chỉnh từng loại thông báo người dùng muốn nhận | `M-14` | MVP sử dụng danh sách sự kiện thông báo cố định |
| `O23` | Trang công khai riêng cho từng nhà tổ chức | `M-05` | Thông tin nhà tổ chức được giới thiệu ngay trong thử thách |
| `O24` | Biểu mẫu, tiêu chí và quy trình thẩm định riêng theo từng loại nhà tổ chức | `M-05`, `M-06` | MVP sử dụng một biểu mẫu và quy trình chung |
| `O25` | Thu phí tham gia; điều kiện tham gia phức tạp; mẫu thử thách và chức năng nhân bản | `M-24` | Không thuộc thiết kế cốt lõi của MVP |
| `O26` | Thử thách riêng tư hoặc chỉ truy cập bằng liên kết mời; lên lịch xuất bản; quản lý chiến dịch | `M-25` | MVP chỉ hỗ trợ xuất bản công khai theo cách thủ công |
| `O27` | Gia hạn thử thách sau khi đã phát sinh tiến độ hoặc kết quả | `M-26` | MVP chưa có quy tắc xử lý ảnh hưởng của việc gia hạn |
| `O28` | Phân nhóm kiểm duyệt; duyệt hàng loạt; quy trình khiếu nại nhiều bước; phát hiện gian lận tự động | `M-27` | MVP xử lý từng minh chứng riêng lẻ |
| `O29` | Sửa phần thưởng; tích điểm; kho quà; tự động trao thưởng | `M-28` | Phần thưởng được trao ngoài hệ thống |
| `O30` | Phễu chuyển đổi, phân tích cohort, số liệu thời gian thực và so sánh | `M-13`, `M-29` | MVP chỉ cung cấp các chỉ số cơ bản |
| `O31` | Tự động xác minh nhà tổ chức; quy trình xử lý vi phạm nhiều bước | `M-06` | MVP thẩm định thủ công |
| `O32` | Kết thúc ngay phiên đăng nhập đang hoạt động khi tài khoản bị vô hiệu hóa | `M-03`, `M-01` | Theo quyết định tại CR-001, chức năng này được hoãn |
| `O33` | Tạo, sửa loại quyền và màn hình quản lý quyền | `M-02` | Danh mục quyền được quản lý thông qua M-02 |
| `O34` | Duyệt trước mọi thử thách; đề xuất thử thách lên trang chủ | `M-15` | MVP giám sát sau khi thử thách được công khai |
| `O35` | Tạo huy hiệu tùy chỉnh | `M-07` | MVP sử dụng bộ huy hiệu chuẩn có sẵn |
| `O36` | CMS đầy đủ; banner; mẫu nội dung; quản lý phiên bản điều khoản | `M-08` | MVP chỉ hỗ trợ bài viết và danh mục nội dung |
| `O37` | Tra cứu, lọc và xuất nhật ký thao tác nâng cao | `M-16` | MVP chỉ hỗ trợ nhật ký cơ bản |
| `O38` | Bình luận trong thử thách | Chưa có module | Cần bổ sung cơ chế kiểm duyệt nội dung do người dùng tạo |
| `O39` | Thả cảm xúc; theo dõi; mạng lưới quan hệ; giới thiệu bạn bè | Chưa có module | Không thuộc vòng chức năng cốt lõi của MVP |
| `O40` | Hệ thống ticket hỗ trợ | Chưa có module | MVP chưa bao gồm bộ phận hỗ trợ người dùng |
