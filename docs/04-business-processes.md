# Quy trình nghiệp vụ

Các sơ đồ quy trình mô tả công việc đi qua những vai trò và hệ thống nào, theo thứ tự nào và kết thúc ra sao.
Sơ đồ làm rõ điểm bàn giao giữa các bên cùng các nhánh thay thế và ngoại lệ, những chỗ dễ bị bỏ sót khi chỉ đọc từng user story.
Nhờ đó BA, dev và tester cùng thống nhất một cách hiểu về nghiệp vụ trước khi thiết kế, lập trình và kiểm thử.

## 1. Đổi vai cho một tài khoản

### 1.1 Phạm vi

| Mục | Nội dung |
|---|---|
| Mục đích | Quản trị viên nâng hoặc hạ vai của một tài khoản ngay tại màn danh sách, có lý do và có xác nhận, để việc trao quyền giải thích được về sau |
| Người thực hiện | Quản trị viên có quyền quản trị tài khoản. superAdmin thực hiện theo cùng luồng |
| Người bị tác động | Chủ tài khoản đích, nhận thông báo kèm lý do |
| Điều kiện trước | Tài khoản đích tồn tại, đang hoạt động, không phải chính người thao tác, không phải superAdmin |
| Kết quả khi thành công | Tài khoản đích mang đúng một vai mới, mọi nơi lưu vai trả về cùng một giá trị, nhật ký có bản ghi, người bị đổi vai nhận thông báo |
| Phục vụ | `BR-003`, `BR-004`, `StR-004`, `StR-005`, `StR-006`, `FR-012`, `NFR-P-05`, `NFR-P-06`, `RULE-006`, `RULE-007`, [`US-013`](06-user-stories/us-013-doi-vai-tai-khoan.md), [`SCR-002`](07-screen-specs/scr-002-quan-tri-tai-khoan.md), [`SCR-003`](07-screen-specs/scr-003-hop-thoai-doi-vai.md), [`SCR-004`](07-screen-specs/scr-004-xac-nhan-doi-vai.md) |

### 1.2 Sơ đồ hoạt động

*[Hình 1 — PRC-001: Đổi vai cho một tài khoản](../diagrams/prc-001-activity-change-role.png)*

### 1.3 Chú giải luồng

**Luồng chính**

| Bước | Lane | Hoạt động | Đi tiếp khi |
|---|---|---|---|
| 1 | Quản trị viên | Tìm tài khoản đích trên màn danh sách, mở menu "Thao tác" và chọn "Đổi vai trò" | Hộp thoại đổi vai trò mở với đúng một tài khoản đích |
| 2 | Quản trị tài khoản | Hiển thị tên, email, vai trò hiện tại và tải danh sách vai trò được phép gán. Danh sách chỉ gồm vai trò đã công bố và đang dùng, không bao giờ có lựa chọn superAdmin | Danh sách có ít nhất một vai trò để chọn |
| 3 | Quản trị viên | Chọn một vai trò mới, nhập lý do và bấm "Tiếp tục" | Cổng "Danh sách vai trò dùng được và lý do hợp lệ?" trả Có: lý do dài 20–500 ký tự sau khi bỏ khoảng trắng đầu cuối |
| 4 | Quản trị viên | Đọc lại hộp xác nhận chỉ đọc gồm tài khoản đích, vai trò cũ → vai trò mới và lý do đã chuẩn hoá, rồi bấm "Xác nhận" | Lệnh được gửi đúng một lần, nút bị khoá trong lúc chờ |
| 5 | Quản trị tài khoản | Nhận lệnh và kiểm tra lại từ đầu qua ba cổng liên tiếp:<br>1. "Phiên đăng nhập còn hợp lệ?"<br>2. "Đủ điều kiện đổi vai?", gồm theo đúng thứ tự: dữ liệu gửi lên → tài khoản đích tồn tại → không phải chính người thao tác → không phải superAdmin → đang hoạt động → vai trò mới gán được → vai trò của đích chưa bị ai đổi<br>3. "Vai trò mới khác vai trò hiện tại?" | Cả ba cổng trả Có. Điều kiện đầu tiên không đạt quyết định mã lỗi trả về |
| 6 | Quản trị tài khoản | Cập nhật vai trò mới và đồng bộ mọi nơi lưu vai trò trong một giao dịch, tăng mốc phiên bản vai trò của tài khoản thêm 1 | Cổng "Cập nhật vai trò thành công?" trả Có: giao dịch hoàn tất, mọi nơi lưu vai trò trả về cùng một vai trò mới |
| 7 | Quản trị tài khoản | Trả kết quả `CHANGED` cho màn quản trị, đồng thời bàn giao sang bước 8 và 9 | Không chờ nhật ký và thông báo xong mới trả |
| 8 | Nhật ký thao tác | Ghi bản ghi `UPDATE` với kết quả `success` | Tra lại được ai đổi vai trò của ai, từ vai trò nào sang vai trò nào, vì sao, lúc nào |
| 9 | Thông báo | Gửi thông báo trong ứng dụng cho người bị đổi vai, kèm đúng lý do đã nhập. Không gửi email | Người bị đổi vai biết vai trò mới và lý do |
| 10 | Quản trị viên | Hai hộp thoại đóng, danh sách tải lại theo đúng bộ lọc đang dùng, hiện "Đã thay đổi vai trò cho {tên tài khoản}" | Thấy vai trò mới trên dòng tài khoản. Kết thúc |

**Luồng thay thế**

| Mã | Khi nào | Xử lý | Nhật ký | Thông báo |
|---|---|---|---|---|
| `A1` | Ở hộp xác nhận, quản trị viên bấm "Quay lại" | Về hộp thoại đổi vai trò, giữ lý do đã nhập, danh sách vai trò được tải lại để chọn lại nếu cần | Không ghi | Không gửi |
| `A2` | Vai trò mới trùng vai trò tài khoản đang mang | Trả `NO_CHANGE`, không cập nhật vai trò, không tăng mốc phiên bản. Đóng hai hộp thoại, tải lại danh sách, hiện "Không có thay đổi" | `no_op` | Không gửi |
| `A3` | Quản trị viên đóng hộp thoại thay vì bấm "Xác nhận" | Không gửi lệnh nào, vai trò giữ nguyên | Không ghi | Không gửi |

**Luồng ngoại lệ**

| Mã | Tình huống | Xử lý | User story | Quản trị viên thấy | Nhật ký |
|---|---|---|---|---|---|
| `E1` | Danh sách vai trò tải lỗi hoặc trả về rỗng | Chưa cho sang hộp xác nhận. Danh sách rỗng được coi là lỗi cấu hình | [`US013-SC-04`](06-user-stories/us-013-doi-vai-tai-khoan.md) | Trạng thái lỗi và nút "Thử tải lại" | Không ghi |
| `E2` | Lý do dưới 20 hoặc trên 500 ký tự sau khi bỏ khoảng trắng đầu cuối | Giao diện chặn ngay trên hộp thoại. Máy chủ vẫn tự kiểm tra lại | [`US013-SC-05`](06-user-stories/us-013-doi-vai-tai-khoan.md) | "Lý do cần dài từ 20 đến 500 ký tự.", nút "Quay lại" | `rejected` nếu lệnh tới máy chủ |
| `E3` | Phiên đăng nhập của quản trị viên hết hạn hoặc không hợp lệ | Từ chối trước mọi kiểm tra khác | [`US013-SC-18`](06-user-stories/us-013-doi-vai-tai-khoan.md) | Hai hộp thoại đóng, trang đăng nhập hiện "Phiên đăng nhập đã hết hạn" và "Vui lòng đăng nhập lại." | Không ghi |
| `E4` | Lệnh gửi thẳng tới máy chủ kèm trường không có trong đặc tả, ví dụ trường cấp quyền superAdmin | Từ chối, tài khoản đích không mang cờ superAdmin | [`US013-SC-20`](06-user-stories/us-013-doi-vai-tai-khoan.md) | Không qua giao diện | `rejected` |
| `E5` | Tài khoản đích không tồn tại | Từ chối | [`US013-SC-09`](06-user-stories/us-013-doi-vai-tai-khoan.md) | "Không tìm thấy tài khoản", nút "Đóng và tải lại" | `rejected` |
| `E6` | Tài khoản đích là chính người thao tác | Từ chối theo `RULE-007`. Giao diện đã ẩn nút thao tác trên dòng của chính mình, nhánh này chỉ xảy ra khi gửi thẳng yêu cầu lên máy chủ | [`US013-SC-08`](06-user-stories/us-013-doi-vai-tai-khoan.md) | "Không thể thay đổi quyền hoặc vô hiệu hoá chính tài khoản của bạn.", nút "Đóng và tải lại" | `rejected` |
| `E7` | Tài khoản đích là superAdmin | Từ chối theo `RULE-006`. Giao diện đã ẩn nút thao tác trên dòng superAdmin | [`US013-SC-08`](06-user-stories/us-013-doi-vai-tai-khoan.md) | "Tài khoản superAdmin được bảo vệ và không thể bị thay đổi bởi tài khoản khác.", nút "Đóng và tải lại" | `rejected` |
| `E8` | Tài khoản đích vừa bị vô hiệu hoá sau khi hộp thoại được mở | Từ chối. Phải báo đúng nguyên nhân, **không** báo "Không tìm thấy tài khoản" | [`US013-SC-09`](06-user-stories/us-013-doi-vai-tai-khoan.md) | "Tài khoản này đã bị vô hiệu hoá. Vui lòng tải lại hoặc khôi phục tài khoản trước khi đổi vai trò.", nút "Đóng và tải lại" | `rejected` |
| `E9` | Vai trò mới vừa chuyển về nháp hoặc bị ngừng dùng | Từ chối | [`US013-SC-10`](06-user-stories/us-013-doi-vai-tai-khoan.md) | "Vai trò đã chọn không còn khả dụng", nút "Quay lại" để chọn vai trò khác | `rejected` |
| `E10` | Người khác đã đổi vai trò của tài khoản đích sau lúc hộp thoại được mở | Từ chối thay vì ghi đè quyết định mới hơn bằng thông tin cũ | [`US013-SC-12`](06-user-stories/us-013-doi-vai-tai-khoan.md) | "Vai trò của tài khoản vừa được thay đổi", nút "Đóng và tải lại" | `rejected` |
| `E11` | Lỗi khi cập nhật, máy chủ xác nhận đã hoàn tác toàn bộ | Vai trò giữ nguyên ở mọi nơi | [`US013-SC-13`](06-user-stories/us-013-doi-vai-tai-khoan.md) | "Không thể thay đổi vai trò", nút "Thử lại" gửi lại đúng quyết định cũ | `failed` |
| `E12` | Không nhận được phản hồi, hoặc máy chủ báo lỗi mà không xác nhận được là chưa cập nhật | Giao diện **không** tự gửi lại và **không** báo là thất bại | [`US013-SC-14`](06-user-stories/us-013-doi-vai-tai-khoan.md) | "Không xác định được kết quả", chỉ còn "Đóng và tải lại" để xem vai trò thật trên danh sách | Theo kết quả thật ở máy chủ |
| `E13` | Gửi thông báo cho người bị đổi vai thất bại | **Không** hoàn tác vai trò, lệnh đã thành công. Việc gửi lại thuộc phần thông báo | — | Không ảnh hưởng | `success` giữ nguyên |
| `E14` | Ghi nhật ký lỗi sau khi vai trò đã đổi | Lỗi phải được ghi nhận để vận hành xử lý. Kết quả của lệnh giữ nguyên | — | Không ảnh hưởng | Cần ghi bù |

### 1.4 Hiệu lực quyền sau khi đổi vai

Phiên đăng nhập ghi vai và quyền tại thời điểm đăng nhập (xem [mục 3](#3-đăng-nhập-bằng-google)), nên vai được ghi ngay nhưng quyền mới chỉ có hiệu lực từ lần đăng nhập kế tiếp (`FR-012` #5).

*[Hình 2 — PRC-002: Hiệu lực quyền của người bị đổi vai](../diagrams/prc-002-state-role-effect.png)*

| Từ | Sự kiện | Đến | Điều kiện | Ai kích hoạt |
|---|---|---|---|---|
| Vai cũ đang có hiệu lực | Lệnh đổi vai trò thành công | Vai trò mới đã được cập nhật, phiên đang mở vẫn dùng quyền cũ | Đã xác nhận, lý do hợp lệ, qua đủ ba cổng kiểm tra ở bước 5 | Quản trị viên |
| Vai trò mới đã được cập nhật, phiên đang mở vẫn dùng quyền cũ | Tiếp tục thao tác trên phiên cũ | Vai trò mới đã được cập nhật, phiên đang mở vẫn dùng quyền cũ | Phiên cũ còn hạn | Người bị đổi vai |
| Vai trò mới đã được cập nhật, phiên đang mở vẫn dùng quyền cũ | Đăng nhập lại | Vai trò mới có hiệu lực | Phiên mới mang vai trò và quyền mới | Người bị đổi vai |
| Vai trò mới đã được cập nhật, phiên đang mở vẫn dùng quyền cũ | Phiên cũ hết hạn | Vai trò mới có hiệu lực | Chậm nhất 7 ngày kể từ lần đăng nhập gần nhất | Hệ thống |
| Vai trò mới có hiệu lực | Lệnh đổi vai trò kế tiếp thành công | Vai trò mới đã được cập nhật, phiên đang mở vẫn dùng quyền cũ | Như lần đổi vai trò trước | Quản trị viên |

**Hạ vai không cắt quyền tức thì**, theo [`CR-001`](10-change-decision-log.md#cr-001--rút-hiệu-lực-cắt-phiên-đang-mở-khỏi-mvp).

## 2. Vô hiệu hoá và khôi phục tài khoản

### 2.1 Phạm vi

| Mục | Nội dung |
|---|---|
| Mục đích | Bộ phận vận hành đình chỉ quyền truy cập của tài khoản vi phạm và mở lại khi xử lý nhầm, **không xoá dữ liệu** của người dùng |
| Người thực hiện | Quản trị viên có quyền quản trị tài khoản. superAdmin thực hiện theo cùng luồng |
| Người bị tác động | Chủ các tài khoản đích, nhận thông báo kèm lý do |
| Điều kiện trước | Lệnh vô hiệu hoá chỉ nhận tài khoản đang hoạt động, lệnh khôi phục chỉ nhận tài khoản đã vô hiệu hoá. Không tài khoản đích nào là superAdmin. Lệnh vô hiệu hoá không chứa chính người thao tác |
| Kết quả khi thành công | Mọi tài khoản đích cùng đổi trạng thái trong một lần ghi, mỗi tài khoản có một bản ghi nhật ký, mỗi người bị tác động nhận thông báo |
| Phục vụ | `BR-002`, `StR-003`, `StR-006`, `FR-011` #7, `RULE-006`, `RULE-007`, [`US-012`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md), [`SCR-002`](07-screen-specs/scr-002-quan-tri-tai-khoan.md) |

### 2.2 Sơ đồ hoạt động

*[Hình 3 — PRC-003: Vô hiệu hoá và khôi phục tài khoản](../diagrams/prc-003-activity-disable-restore.png)*

### 2.3 Chú giải luồng

**Luồng chính** — vô hiệu hoá hoặc khôi phục một tài khoản từ menu của dòng

| Bước | Lane | Hoạt động | Đi tiếp khi |
|---|---|---|---|
| 1 | Quản trị viên | Mở menu "Thao tác" trên dòng đích. Tài khoản đang hoạt động có "Đổi vai trò" và "Vô hiệu hoá"; tài khoản đã vô hiệu hoá chỉ có "Khôi phục" | Chọn đúng lệnh theo trạng thái hiện tại |
| 2 | Quản trị tài khoản | Mở hộp thoại xác nhận của lệnh tương ứng cho tài khoản đích | Chưa tài khoản nào bị thay đổi |
| 3 | Quản trị viên | Đọc số lượng và danh sách tài khoản đích, nhập lý do và bấm "Xác nhận" | Cổng "Lý do hợp lệ?" trả Có: lý do dài 20–500 ký tự sau khi bỏ khoảng trắng đầu cuối |
| 4 | Quản trị tài khoản | Nhận lệnh và kiểm tra lại từ đầu qua ba cổng liên tiếp:<br>1. "Phiên đăng nhập còn hợp lệ?"<br>2. "Đủ điều kiện thực hiện lệnh?", gồm theo đúng thứ tự: dữ liệu gửi lên → mọi tài khoản đích tồn tại → không có superAdmin → lệnh vô hiệu hoá không chứa chính người thao tác<br>3. "Trạng thái mọi tài khoản đích khớp với lệnh?" | Cả ba cổng trả Có. Chỉ cần một tài khoản đích vi phạm là **cả lệnh** không được cập nhật |
| 5 | Quản trị tài khoản | Cập nhật trạng thái toàn bộ danh sách trong một giao dịch. Vô hiệu hoá ghi thời điểm vô hiệu hoá; khôi phục xoá giá trị đó | Cổng "Cập nhật trạng thái thành công?" trả Có. Không xoá dữ liệu nào của tài khoản |
| 6 | Quản trị tài khoản | Trả kết quả thành công cho màn quản trị, đồng thời bàn giao sang bước 7 và 8 | Không chờ nhật ký và thông báo xong mới trả |
| 7 | Nhật ký thao tác | Ghi một bản ghi cho mỗi tài khoản đích, loại hành động `DELETE` với vô hiệu hoá, `RESTORE` với khôi phục, kết quả `success`. Các bản ghi của cùng một lệnh dùng chung mã quyết định | Tra lại được từng tài khoản bị tác động bởi quyết định nào |
| 8 | Thông báo | Gửi thông báo trong ứng dụng kèm lý do cho từng tài khoản đích. Riêng lệnh vô hiệu hoá gửi thêm email | Người bị tác động biết trạng thái mới và lý do, người bị vô hiệu hoá biết ngay cả khi không mở được ứng dụng |
| 9 | Quản trị viên | Hộp thoại đóng, danh sách tải lại theo đúng bộ lọc đang dùng | Thấy trạng thái mới trên các dòng. Kết thúc |

**Luồng thay thế**

| Mã | Khi nào | Xử lý | Nhật ký | Thông báo |
|---|---|---|---|---|
| `A1` | Xử lý nhiều tài khoản cùng lúc | Chỉ bật được chế độ chọn nhiều khi bộ lọc trạng thái có giá trị cụ thể. Chọn được qua nhiều trang; "chọn tất cả" phủ toàn bộ kết quả đã lọc và **tự loại** chính người thao tác cùng superAdmin. Đổi điều kiện tra cứu thì xoá toàn bộ lựa chọn. Khi bấm nút lệnh trên thanh hành động, hệ thống chốt danh sách thành một bản cố định có hạn 10 phút rồi mới mở hộp xác nhận. Ở bước 4, máy chủ kiểm tra thêm danh sách đã chốt còn hiệu lực, ngay sau khi kiểm tra dữ liệu gửi lên | Như luồng chính | Như luồng chính |
| `A2` | Quản trị viên tự khôi phục tài khoản của chính mình khi phiên đăng nhập còn hiệu lực | Cho phép theo ngoại lệ của `RULE-007`. Chỉ làm được qua "Khôi phục" trên dòng của chính mình, không gom được vào lệnh nhiều tài khoản | Như luồng chính | Như luồng chính |
| `A3` | Quản trị viên đóng hộp thoại thay vì bấm "Xác nhận" | Không gửi lệnh nào, trạng thái giữ nguyên | Không ghi | Không gửi |

**Luồng ngoại lệ**

| Mã | Tình huống | Xử lý | Nhật ký |
|---|---|---|---|
| `E1` | Lý do dưới 20 hoặc trên 500 ký tự, hoặc danh sách đích rỗng hay có mã trùng | Giao diện chặn lý do ngay trên hộp thoại. Máy chủ vẫn tự kiểm tra lại cả lý do lẫn danh sách đích; bị từ chối thì quản trị viên bấm "Quay lại" để sửa | `rejected` nếu lệnh tới máy chủ |
| `E2` | Phiên đăng nhập của quản trị viên hết hạn hoặc không hợp lệ | Từ chối trước mọi kiểm tra khác, chuyển về trang đăng nhập | Không ghi |
| `E3` | Lệnh hàng loạt có danh sách đã chốt quá 10 phút mới được xác nhận | Từ chối, quản trị viên bấm "Đóng và tải lại" rồi chọn và chốt lại danh sách | `rejected` |
| `E4` | Có ít nhất một tài khoản đích không tồn tại | Từ chối toàn bộ lệnh | `rejected` |
| `E5` | Danh sách có tài khoản superAdmin | Từ chối toàn bộ lệnh theo `RULE-006` | `rejected` |
| `E6` | Lệnh vô hiệu hoá chứa chính người thao tác | Từ chối toàn bộ lệnh theo `RULE-007`, kể cả các tài khoản đích hợp lệ còn lại | `rejected` |
| `E7` | Trạng thái của ít nhất một tài khoản đích không còn khớp lệnh, ví dụ người khác vừa khôi phục trước | Không cập nhật tài khoản nào, trả "Trạng thái không đổi" rồi đóng hộp thoại và tải lại danh sách. **Không** báo "Không tìm thấy tài khoản" | `no_op` cho từng tài khoản đích |
| `E8` | Lỗi khi cập nhật giữa chừng | Hoàn tác toàn bộ danh sách, không để danh sách cập nhật một phần. Nút "Thử lại" gửi lại đúng lệnh cũ | `failed` |
| `E9` | Không nhận được phản hồi | Giao diện **không** tự gửi lại, chỉ còn "Đóng và tải lại" để xem trạng thái thật | Theo kết quả thật ở máy chủ |
| `E10` | Gửi thông báo hoặc email thất bại | **Không** hoàn tác trạng thái tài khoản, lệnh đã thành công. Việc gửi lại thuộc phần thông báo | `success` giữ nguyên |
| `E11` | Ghi nhật ký lỗi sau khi trạng thái đã đổi | Lỗi phải được ghi nhận để vận hành xử lý. Kết quả của lệnh giữ nguyên | Cần ghi bù |

### 2.4 Trạng thái tài khoản

*[Hình 4 — PRC-004: Vòng đời trạng thái tài khoản](../diagrams/prc-004-state-account-status.png)*

| Từ | Sự kiện | Đến | Điều kiện | Ai kích hoạt |
|---|---|---|---|---|
| — | Đăng nhập Google lần đầu | Đang hoạt động | Tài khoản được tạo cùng vai Người dùng trong một bước | Khách |
| Đang hoạt động | Vô hiệu hoá | Đã vô hiệu hoá | Đã xác nhận, lý do hợp lệ, đích không phải superAdmin, không phải chính người thao tác | Quản trị viên |
| Đã vô hiệu hoá | Khôi phục | Đang hoạt động | Đã xác nhận, lý do hợp lệ, đích không phải superAdmin. Được tự khôi phục chính mình | Quản trị viên |
| Đã vô hiệu hoá | Người dùng tự đăng nhập lại | Đã vô hiệu hoá | Bị từ chối khi đăng nhập, trạng thái giữ nguyên | Người dùng |

## 3. Đăng nhập bằng Google

### 3.1 Phạm vi

| Mục | Nội dung |
|---|---|
| Mục đích | Người dùng vào DOMO bằng tài khoản Google, không tạo mật khẩu, và nhận đúng tài khoản cũ trên mọi thiết bị |
| Người tham gia | Người dùng, ứng dụng DOMO trên web hoặc Android, Google Sign-In, Firebase Auth, máy chủ DOMO, cơ sở dữ liệu DOMO |
| Ranh giới trách nhiệm | Google trả lời "người này là ai". DOMO tự quyết "người này có được vào không" |
| Kết quả khi thành công | Người dùng có phiên đăng nhập DOMO hạn 7 ngày, phiên mang vai và quyền của tài khoản tại thời điểm đăng nhập |
| Phục vụ | `BR-001`, `BR-002`, `C-03` |

### 3.2 Sơ đồ tuần tự

*[Hình 5 — PRC-005: Đăng nhập bằng Google](../diagrams/prc-005-sequence-google-login.png)*

### 3.3 Chú giải luồng

**Luồng chính** — người dùng đã có tài khoản DOMO đăng nhập lại, trên cùng hoặc khác thiết bị

| Bước | Từ → Đến | Hành động | Ghi chú |
|---|---|---|---|
| 1–3 | Người dùng ↔ Ứng dụng | Bấm nút Google, đọc cảnh báo rủi ro, bấm "Tiếp tục" | Cảnh báo hiện **mỗi lần** bấm nút. Chỉ mở Google sau khi bấm "Tiếp tục" |
| 4–6 | Ứng dụng ↔ Google, Người dùng → Google | Mở màn chọn tài khoản, người dùng chọn và xác minh | Luôn hiện màn chọn tài khoản |
| 7 | Ứng dụng → Firebase | Chuyển thông tin xác thực Google cho Firebase Auth | — |
| 8 | Firebase → Ứng dụng | Trả mã xác thực Firebase | Ứng dụng không dùng phiên của Firebase |
| 9 | Ứng dụng → Máy chủ | Gửi `POST /auth/google` kèm mã xác thực | Chỉ gửi mã, không gửi thông tin người dùng |
| Giữa 9 và 10 | Máy chủ | Xác minh chữ ký, dự án Firebase và thời hạn của mã, rồi đọc mã định danh Firebase, email, trạng thái xác minh email, tên hiển thị từ mã | Không hợp lệ thì dừng ở `E2` |
| 10 | Máy chủ → Firebase | Hỏi mã đã bị thu hồi chưa | — |
| 11 | Firebase → Máy chủ | Xác nhận mã chưa bị thu hồi | Đã thu hồi thì dừng ở `E2`. Sau đó kiểm tra `E3`, `E4` |
| 12 | Máy chủ → CSDL | Tìm tài khoản theo mã định danh Firebase | Liên kết theo **mã định danh Firebase**, không theo email |
| 13 | CSDL → Máy chủ | Trả tài khoản tìm thấy | Đã vô hiệu hoá thì dừng ở `E5`. Không ghi đè tên và email bằng dữ liệu Google |
| 14 | Máy chủ | Tạo phiên DOMO hạn 7 ngày, ghi vai và quyền hiện tại vào phiên | Đổi vai chỉ có hiệu lực từ lần đăng nhập kế tiếp, xem [mục 1.4](#14-hiệu-lực-quyền-sau-khi-đổi-vai) |
| 15–16 | Máy chủ → Ứng dụng → Người dùng | Trả phiên và thông tin tài khoản, vào màn chủ | Không bắt hoàn thiện hồ sơ |

**Luồng thay thế**

| Mã | Khi nào | Hệ thống xử lý | Người dùng thấy |
|---|---|---|---|
| `A1` | Bấm "Huỷ" ở cảnh báo rủi ro | Không gọi Google, không tạo phiên | Vẫn ở màn đăng nhập |
| `A2` | Đóng màn chọn tài khoản Google | Không coi là lỗi, không tạo phiên | Vẫn ở màn đăng nhập, **không** có thông báo lỗi |
| `A3` | Không thấy tài khoản theo mã định danh Firebase, nhưng có tài khoản cùng email | Chỉ nhận lại khi email đã được Google xác minh và tài khoản chưa bị vô hiệu hoá. Gắn mã định danh Firebase vào tài khoản đó, **không** tạo tài khoản thứ hai | Vào đúng tài khoản cũ |
| `A4` | Đăng nhập lần đầu, chưa có tài khoản nào | Tạo tài khoản mới với vai Người dùng và trạng thái Đang hoạt động trong một bước, lấy tên hiển thị từ Google | Vào màn chủ, không phải khai hồ sơ |

**Luồng ngoại lệ**

Mọi ngoại lệ dưới đây đều kết thúc giống nhau: **không** mở phiên, **không** tạo tài khoản, không để lại dữ liệu dở dang, người dùng thử lại được ngay.

| Mã | Tình huống | Người dùng thấy |
|---|---|---|
| `E1` | Google không phản hồi, quá thời gian chờ, hoặc từ chối xác thực | "Không kết nối được tới Google. Vui lòng thử lại." khi quá thời gian chờ; "Không đăng nhập được bằng tài khoản Google này." khi Google từ chối. Bấm lại vẫn qua cảnh báo rủi ro |
| `E2` | Mã xác thực sai chữ ký, không cấp cho dự án Firebase của DOMO, hết hạn, đã bị thu hồi trên Firebase, hoặc tài khoản Firebase đã bị tắt | Quay về đầu luồng đăng nhập |
| `E3` | Tài khoản Google không có email | Không đăng nhập được |
| `E4` | Email chưa được Google xác minh, kể cả khi trùng email một tài khoản DOMO đang có | Màn đăng nhập hiện "Tài khoản Google này chưa xác minh email nên chưa dùng để đăng nhập DOMO được.", không tạo và không mở tài khoản DOMO nào |
| `E5` | Tài khoản DOMO đã bị vô hiệu hoá, dù Google xác thực thành công | Thông báo nêu đúng lý do tài khoản đã bị vô hiệu hoá, không dùng chung câu với lỗi kết nối |
