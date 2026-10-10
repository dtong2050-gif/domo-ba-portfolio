# Kịch bản kiểm thử chấp nhận người dùng (UAT)

Tài liệu này quy định các kịch bản nghiệm thu chức năng tra cứu tài khoản, chọn tài khoản cho thao tác quản trị và đổi vai trò.
Bộ phận vận hành trực tiếp thực hiện các kịch bản trên giao diện; mỗi kịch bản là một tình huống công việc thực tế và xác nhận một hoặc nhiều nhu cầu của stakeholder.
Kết quả nghiệm thu là căn cứ để PO quyết định phát hành.

## 1. Phạm vi và người tham gia

### 1.1 Phạm vi

| Mục | Nội dung |
|---|---|
| Chức năng nghiệm thu | Tra cứu danh sách tài khoản ([`US-011`](06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md))<br>Chọn tài khoản cho thao tác quản trị ([`US-012`](06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md))<br>Đổi vai cho một tài khoản ([`US-013`](06-user-stories/us-013-doi-vai-tai-khoan.md)) |
| Kênh | Trang quản trị trên web |
| Nguồn đối chiếu kết quả | Màn hình Quản trị tài khoản, thông báo trong ứng dụng của người bị đổi vai trò, Nhật ký thao tác |
| Ngoài phạm vi | Vô hiệu hoá và khôi phục tài khoản sau khi bấm "Xác nhận". Chức năng này thuộc một user story khác. `UAT-025` chỉ kiểm tra danh sách tài khoản trong hộp thoại xác nhận vô hiệu hoá, sau đó đóng hộp thoại mà không xác nhận. |

### 1.2 Người tham gia

| Vai trò | Trách nhiệm |
|---|---|
| Quản trị viên vận hành | Thực hiện các bước của nhân vật Hà Phạm và ghi kết quả từng bước |
| Thành viên bộ phận vận hành được phân công | Thực hiện các bước của các nhân vật Minh Trần, Tuấn Lê, Lan Võ và Khoa Đặng trên tài khoản tương ứng |
| Người giữ tài khoản quản trị gốc | Thực hiện các bước của tài khoản quản trị gốc trong `UAT-024` |
| PO | Theo dõi quá trình nghiệm thu, quyết định kết quả từng kịch bản và ký xác nhận nghiệm thu |
| QA | Chuẩn bị môi trường và dữ liệu, cung cấp đường dẫn trực tiếp tới màn hình Tài khoản, đo số thao tác và thời gian trong `UAT-021`, ghi nhận và theo dõi việc xử lý sai khác |

## 2. Điều kiện bắt đầu và kết thúc

**Điều kiện bắt đầu.** Đợt nghiệm thu chỉ bắt đầu khi đáp ứng đủ các điều kiện sau:

1. Toàn bộ [ca kiểm thử hệ thống và API](09-system-api-tests/README.md) của `US-011`, `US-012`, `US-013` đã được thực hiện và đạt.
2. Không còn lỗi mức Critical hoặc High đang mở thuộc phạm vi tại mục 1.1.
3. Môi trường nghiệm thu đã cài đặt phiên bản dự kiến phát hành.
4. Tài khoản và dữ liệu tại mục 3 đã sẵn sàng.

**Điều kiện kết thúc.** Đợt nghiệm thu được xem là hoàn thành khi đáp ứng đủ các điều kiện sau:

1. Cả năm kịch bản có kết quả "Đạt" hoặc "Đạt có điều kiện".
2. Mọi sai khác phát hiện trong đợt nghiệm thu đã được ghi tại mục 5.2, có người phụ trách và thời hạn xử lý.
3. Các bên tại mục 5.3 đã ký xác nhận.

## 3. Nhân vật và dữ liệu

### 3.1 Nhân vật

| Nhân vật | Email | Vai trò ban đầu | Trạng thái ban đầu | Kịch bản |
|---|---|---|---|---|
| Hà Phạm | `ha.pham@example.test` | Quản trị viên | Đang hoạt động | `UAT-021`, `UAT-022`, `UAT-023`, `UAT-025` |
| Minh Trần | `minh.tran@example.test` | Người dùng | Đang hoạt động | `UAT-021` |
| Tuấn Lê | `tuan.le@example.test` | Quản trị viên | Đang hoạt động | `UAT-022` |
| Lan Võ, đại diện CLB Chạy bộ Sài Gòn | `lan.vo@example.test` | Nhà tổ chức | Đang hoạt động | `UAT-023` |
| Khoa Đặng | `khoa.dang@example.test` | Quản trị viên | Đang hoạt động | `UAT-024` |
| Root Domo, tài khoản quản trị gốc (superAdmin) | `root@example.test` | Quản trị viên | Đang hoạt động | `UAT-024` |
| 25 tài khoản nghi gian lận | `runner01@fastrun.test` đến `runner25@fastrun.test` | Người dùng | Đang hoạt động | `UAT-025` |

### 3.2 Thiết lập dữ liệu

- Vai trò "Quản trị viên" có quyền quản trị tài khoản.
- Tài khoản Lan Võ sở hữu thử thách "Chạy 100 km tháng 10/2026", có ít nhất 3 người tham gia và ít nhất 1 minh chứng đã nộp.
- Ngoài 25 tài khoản nghi gian lận, không tài khoản nào có email chứa "fastrun.test".
- Trước `UAT-021`, Hà Phạm chưa được hướng dẫn riêng về chức năng đổi vai trò.

### 3.3 Quy định thực hiện

- Chỉ sử dụng dữ liệu tại mục 3.1 và 3.2. Không sử dụng email, họ tên hoặc thông tin cá nhân thật.
- Mỗi kịch bản bắt đầu từ trạng thái ban đầu tại mục 3.1. QA khôi phục dữ liệu về trạng thái này trước mỗi kịch bản và sau khi kết thúc đợt nghiệm thu.
- `UAT-021` được thực hiện đầu tiên. Các kịch bản còn lại được thực hiện theo thứ tự bất kỳ.
- Người thực hiện ghi "Đạt" tại cột "Kết quả" khi kết quả thực tế khớp với cột "Kết quả mong đợi". Khi có sai khác, người thực hiện ghi số bước và mô tả sai khác tại mục 5.2; PO quyết định kết quả "Đạt có điều kiện" hoặc "Không đạt" cho bước đó theo quy ước tại mục 5.1.

## 4. Kịch bản

| Mã | Kịch bản | Nhu cầu được xác nhận |
|---|---|---|
| `UAT-021` | Cấp quyền quản trị cho nhân sự vận hành mới | [`StR-004`](03-requirements-and-rules.md#1-stakeholder-requirement), [`NFR-P-05`](03-requirements-and-rules.md#3-non-functional-requirement) |
| `UAT-022` | Thu hồi quyền quản trị khi rà soát định kỳ | [`StR-004`](03-requirements-and-rules.md#1-stakeholder-requirement), [`StR-006`](03-requirements-and-rules.md#1-stakeholder-requirement), [`CR-001`](10-change-decision-log.md#cr-001--rút-hiệu-lực-cắt-phiên-đang-mở-khỏi-mvp) |
| `UAT-023` | Hạ và cấp lại vai trò Nhà tổ chức | [`StR-004`](03-requirements-and-rules.md#1-stakeholder-requirement), [`FR-012`](03-requirements-and-rules.md#2-functional-requirement) #6 |
| `UAT-024` | Giành lại quyền kiểm soát khi tài khoản quản trị bị sử dụng sai mục đích | [`StR-005`](03-requirements-and-rules.md#1-stakeholder-requirement), [`RULE-006`, `RULE-007`](03-requirements-and-rules.md#4-business-rule), [`CR-001`](10-change-decision-log.md#cr-001--rút-hiệu-lực-cắt-phiên-đang-mở-khỏi-mvp) |
| `UAT-025` | Lập danh sách tài khoản gian lận cần vô hiệu hoá | [`StR-003`](03-requirements-and-rules.md#1-stakeholder-requirement) |

### `UAT-021` — Cấp quyền quản trị cho nhân sự vận hành mới

**Tình huống:** Minh Trần gia nhập bộ phận vận hành từ ngày 01/10/2026 và đã đăng nhập DOMO; tài khoản đang mang vai trò Người dùng. Trưởng bộ phận vận hành gửi yêu cầu cấp vai trò Quản trị viên cho Minh Trần, trong đó ghi email "Minh.Tran@Example.test".

**Trạng thái ban đầu:** Minh Trần đang đăng nhập DOMO trên máy của mình.

| # | Người thực hiện | Thao tác | Kết quả mong đợi | Kết quả |
|---|---|---|---|---|
| 1 | Hà Phạm | Mở Quản lý người dùng › Tài khoản | Màn hình hiển thị danh sách tài khoản | |
| 2 | Hà Phạm | Bấm "Bộ lọc", nhập "Minh.Tran@Example.test" vào ô tìm kiếm | Danh sách chỉ còn một dòng: Minh Trần, `minh.tran@example.test`, vai trò "Người dùng", trạng thái "Đang hoạt động" | |
| 3 | Hà Phạm | Bấm nút thao tác (…) trên dòng Minh Trần, chọn "Đổi vai trò" | Hộp thoại "Thay đổi vai trò" hiển thị tên, email và vai trò hiện tại "Người dùng" của Minh Trần | |
| 4 | Hà Phạm | Chọn "Quản trị viên" tại ô "Vai trò mới", nhập lý do "Cấp quyền quản trị cho nhân sự vận hành mới theo yêu cầu của trưởng bộ phận vận hành.", bấm "Tiếp tục" | Hộp thoại "Xác nhận thay đổi vai trò" hiển thị Minh Trần, vai trò cũ "Người dùng" › vai trò mới "Quản trị viên", đúng lý do vừa nhập và dòng "Vai trò mới có hiệu lực từ lần đăng nhập tiếp theo." | |
| 5 | Hà Phạm | Bấm "Xác nhận" | Hai hộp thoại đóng. Hệ thống hiển thị thông báo "Đã thay đổi vai trò cho Minh Trần", "Vai trò mới: Quản trị viên". Dòng Minh Trần hiển thị vai trò "Quản trị viên" | |
| 6 | QA | Tổng hợp số thao tác và thời gian của Hà Phạm, tính từ khi danh sách hiển thị ở bước 1 đến khi thông báo thành công hiển thị ở bước 5 | Không quá 5 thao tác và không quá 60 giây | |
| 7 | Minh Trần | Trên phiên đăng nhập đang mở, tải lại trang | Chưa có mục Quản lý người dùng › Tài khoản. Quyền Quản trị viên chưa có hiệu lực trên phiên đang mở | |
| 8 | Minh Trần | Mở danh sách thông báo | Có thông báo về việc đổi vai trò, nêu vai trò mới "Quản trị viên" và đúng lý do đã nhập ở bước 4 | |
| 9 | Minh Trần | Đăng xuất, đăng nhập lại, mở Quản lý người dùng › Tài khoản | Màn hình Tài khoản hiển thị danh sách tài khoản | |

### `UAT-022` — Thu hồi quyền quản trị khi rà soát định kỳ

**Tình huống:** Trong đợt rà soát quyền định kỳ hằng tháng, Hà Phạm đối chiếu danh sách tài khoản mang vai trò Quản trị viên với danh sách nhân sự bộ phận vận hành. Tuấn Lê đã chuyển sang bộ phận Marketing từ ngày 01/10/2026 nhưng tài khoản vẫn mang vai trò Quản trị viên.

**Trạng thái ban đầu:** Tuấn Lê đang đăng nhập DOMO trên máy của mình và đang mở màn hình Tài khoản.

| # | Người thực hiện | Thao tác | Kết quả mong đợi | Kết quả |
|---|---|---|---|---|
| 1 | Hà Phạm | Mở Quản lý người dùng › Tài khoản, bấm "Bộ lọc", chọn vai trò "Quản trị viên" và trạng thái "Đang hoạt động" | Danh sách chỉ gồm các tài khoản mang vai trò Quản trị viên và đang hoạt động, trong đó có Tuấn Lê | |
| 2 | Hà Phạm | Bấm nút thao tác (…) trên dòng Tuấn Lê, chọn "Đổi vai trò", chọn "Người dùng" tại ô "Vai trò mới", nhập lý do "Chuyển công tác sang bộ phận Marketing từ ngày 01/10/2026, không còn nhiệm vụ vận hành.", bấm "Tiếp tục" | Hộp thoại "Xác nhận thay đổi vai trò" hiển thị Tuấn Lê, vai trò cũ "Quản trị viên" › vai trò mới "Người dùng" và đúng lý do vừa nhập | |
| 3 | Hà Phạm | Bấm "Xác nhận" | Hệ thống hiển thị thông báo "Đã thay đổi vai trò cho Tuấn Lê", "Vai trò mới: Người dùng". Danh sách tải lại theo bộ lọc đang dùng và không còn dòng Tuấn Lê | |
| 4 | Tuấn Lê | Trên màn hình Tài khoản đang mở, bấm nút làm mới (↻) | Danh sách tài khoản vẫn hiển thị. Phiên đăng nhập đang mở giữ quyền của vai trò cũ cho tới khi Tuấn Lê đăng nhập lại hoặc phiên hết hạn, tối đa 7 ngày, theo `CR-001`. PO xác nhận chấp nhận khoảng thời gian này | |
| 5 | Tuấn Lê | Mở danh sách thông báo | Có thông báo về việc đổi vai trò, nêu vai trò mới "Người dùng" và đúng lý do đã nhập ở bước 2 | |
| 6 | Tuấn Lê | Đăng xuất, đăng nhập lại | Không còn mục Quản lý người dùng › Tài khoản | |
| 7 | Hà Phạm | Theo đề nghị giải trình của Tuấn Lê, mở Nhật ký thao tác và tìm thao tác đổi vai trò của Tuấn Lê | Bản ghi nêu đủ: người thực hiện Hà Phạm, tài khoản Tuấn Lê, vai trò "Quản trị viên" → "Người dùng", lý do đã nhập ở bước 2, thời điểm thực hiện và kết quả thành công | |

### `UAT-023` — Hạ và cấp lại vai trò Nhà tổ chức

**Tình huống:** CLB Chạy bộ Sài Gòn, do Lan Võ đại diện, đề nghị tạm ngừng tổ chức thử thách trên DOMO. Hà Phạm hạ vai trò tài khoản của Lan Võ về Người dùng. Sau đó, CLB đề nghị hoạt động trở lại và Hà Phạm cấp lại vai trò Nhà tổ chức.

**Trạng thái ban đầu:** Lan Võ đang đăng nhập DOMO.

| # | Người thực hiện | Thao tác | Kết quả mong đợi | Kết quả |
|---|---|---|---|---|
| 1 | Lan Võ | Mở mục quản lý thử thách của Nhà tổ chức, mở thử thách "Chạy 100 km tháng 10/2026", ghi lại số người tham gia và số minh chứng | Thử thách hiển thị đầy đủ thông tin, số người tham gia và số minh chứng | |
| 2 | Hà Phạm | Tìm tài khoản Lan Võ trên màn hình Tài khoản, đổi vai trò sang "Người dùng" với lý do "Tạm ngừng tổ chức thử thách theo đề nghị của CLB Chạy bộ Sài Gòn." | Hệ thống hiển thị thông báo "Đã thay đổi vai trò cho Lan Võ", "Vai trò mới: Người dùng" | |
| 3 | Lan Võ | Đăng xuất, đăng nhập lại | Không còn mục quản lý thử thách của Nhà tổ chức. Lan Võ vẫn sử dụng được các chức năng của Người dùng | |
| 4 | Hà Phạm | Đổi vai trò của Lan Võ sang "Nhà tổ chức" với lý do "Cấp lại vai trò Nhà tổ chức theo đề nghị hoạt động trở lại của CLB Chạy bộ Sài Gòn." | Hệ thống hiển thị thông báo "Đã thay đổi vai trò cho Lan Võ", "Vai trò mới: Nhà tổ chức" | |
| 5 | Lan Võ | Đăng xuất, đăng nhập lại, mở mục quản lý thử thách của Nhà tổ chức | Thử thách "Chạy 100 km tháng 10/2026" vẫn thuộc CLB Chạy bộ Sài Gòn. Thông tin thử thách, số người tham gia và số minh chứng khớp với số liệu đã ghi ở bước 1 | |

### `UAT-024` — Giành lại quyền kiểm soát khi tài khoản quản trị bị sử dụng sai mục đích

**Tình huống:** Bộ phận vận hành phát hiện tài khoản quản trị viên của Khoa Đặng được dùng để đổi vai trò của nhiều tài khoản mà không có yêu cầu hợp lệ. Người giữ tài khoản quản trị gốc thu hồi quyền quản trị của Khoa Đặng. Tài khoản Khoa Đặng không được tự thay đổi quyền của chính mình và không được tác động tới tài khoản quản trị gốc.

**Trạng thái ban đầu:** Khoa Đặng đang đăng nhập DOMO trên máy của mình.

| # | Người thực hiện | Thao tác | Kết quả mong đợi | Kết quả |
|---|---|---|---|---|
| 1 | Khoa Đặng | Mở Quản lý người dùng › Tài khoản, bấm "Bộ lọc", nhập "khoa.dang@example.test" vào ô tìm kiếm | Dòng Khoa Đặng có thẻ "Bạn" và không có nút thao tác (…) | |
| 2 | Khoa Đặng | Thay từ khoá bằng "root@example.test" | Dòng Root Domo có thẻ "superAdmin", nền xám nhạt và không có nút thao tác (…) | |
| 3 | Khoa Đặng | Xoá từ khoá, chọn trạng thái "Đang hoạt động", bấm ô chọn tất cả ở tiêu đề bảng | Ô chọn trên dòng Khoa Đặng và dòng Root Domo bị khoá. Số tài khoản đã chọn trên thanh hành động bằng tổng số tài khoản ở chân bảng trừ 2 | |
| 4 | Khoa Đặng | Bấm "Bỏ chọn" | Thanh hành động biến mất. Không tài khoản nào bị thay đổi | |
| 5 | Người giữ tài khoản quản trị gốc | Đăng nhập bằng tài khoản Root Domo, tìm tài khoản Khoa Đặng trên màn hình Tài khoản, đổi vai trò sang "Người dùng" với lý do "Thu hồi quyền quản trị do tài khoản bị sử dụng sai mục đích, chờ kết quả xác minh." | Hệ thống hiển thị thông báo "Đã thay đổi vai trò cho Khoa Đặng", "Vai trò mới: Người dùng" | |
| 6 | Khoa Đặng | Trên màn hình Tài khoản đang mở, bấm nút làm mới (↻) | Danh sách tài khoản vẫn hiển thị. Phiên đăng nhập đang mở giữ quyền của vai trò cũ cho tới khi Khoa Đặng đăng nhập lại hoặc phiên hết hạn, tối đa 7 ngày, theo `CR-001`. PO xác nhận chấp nhận khoảng thời gian này đối với tài khoản bị sử dụng sai mục đích | |
| 7 | Khoa Đặng | Đăng xuất, đăng nhập lại | Không còn mục Quản lý người dùng › Tài khoản | |
| 8 | Khoa Đặng | Mở đường dẫn trực tiếp tới màn hình Tài khoản do QA cung cấp | Màn hình hiển thị "Bạn không có quyền quản lý tài khoản." và "Bạn không thể thực hiện thao tác này.". Không hiển thị danh sách tài khoản và không có nút "Thử lại" | |

### `UAT-025` — Lập danh sách tài khoản gian lận cần vô hiệu hoá

**Tình huống:** Bộ phận vận hành phát hiện 25 tài khoản cùng tên miền email "fastrun.test" đăng ký trong cùng một ngày, cùng tham gia một thử thách có phần thưởng và nộp các minh chứng trùng nhau. Sau khi đối chiếu, tài khoản `runner07@fastrun.test` được xác định là thành viên thật của câu lạc bộ và không thuộc diện xử lý. Hà Phạm lập danh sách 24 tài khoản còn lại để trình lệnh vô hiệu hoá.

**Trạng thái ban đầu:** Cả 25 tài khoản đang ở trạng thái "Đang hoạt động".

| # | Người thực hiện | Thao tác | Kết quả mong đợi | Kết quả |
|---|---|---|---|---|
| 1 | Hà Phạm | Mở Quản lý người dùng › Tài khoản, bấm "Bộ lọc", nhập "fastrun.test" vào ô tìm kiếm | Danh sách gồm 25 tài khoản có email chứa "fastrun.test"; chân bảng ghi "1 – 10 trong 25 tài khoản" | |
| 2 | Hà Phạm | Chọn trạng thái "Đang hoạt động" | Danh sách vẫn gồm 25 tài khoản; mỗi dòng có ô chọn | |
| 3 | Hà Phạm | Bấm ô chọn tất cả ở tiêu đề bảng | Thanh hành động hiển thị "Đã chọn 25 tài khoản", nút "Vô hiệu hoá" và nút "Bỏ chọn" | |
| 4 | Hà Phạm | Chuyển tới trang có tài khoản `runner07@fastrun.test`, bỏ chọn dòng này | Dòng `runner07@fastrun.test` không còn được chọn. Thanh hành động hiển thị "Đã chọn 24 tài khoản" | |
| 5 | Hà Phạm | Lần lượt mở cả ba trang của danh sách | Mọi tài khoản trừ `runner07@fastrun.test` vẫn được chọn trên cả ba trang. Thanh hành động vẫn hiển thị "Đã chọn 24 tài khoản" | |
| 6 | Hà Phạm | Bấm "Vô hiệu hoá" trên thanh hành động | Hộp thoại xác nhận vô hiệu hoá mở ra với đúng 24 tài khoản, không có `runner07@fastrun.test` | |
| 7 | Hà Phạm | Đóng hộp thoại xác nhận mà không xác nhận | Cả 25 tài khoản vẫn ở trạng thái "Đang hoạt động" | |

## 5. Kết quả và xác nhận nghiệm thu

### 5.1 Quy ước kết quả

Cột "Kết quả" của từng bước và kết quả của từng kịch bản tại mục 5.2 chỉ nhận một trong ba giá trị sau.

| Kết quả | Áp dụng cho một bước | Áp dụng cho một kịch bản |
|---|---|---|
| Đạt | Kết quả thực tế khớp với kết quả mong đợi | Mọi bước có kết quả "Đạt" |
| Đạt có điều kiện | Có sai khác; PO chấp nhận phát hành kèm điều kiện và thời hạn khắc phục ghi tại mục 5.2 | Không có bước "Không đạt" và có ít nhất một bước "Đạt có điều kiện" |
| Không đạt | Có sai khác và PO không chấp nhận phát hành khi sai khác chưa được khắc phục | Có ít nhất một bước "Không đạt" |

### 5.2 Kết quả thực hiện

| Thông tin | Nội dung |
|---|---|
| Đường dẫn môi trường nghiệm thu | |
| Phiên bản phần mềm | |
| Thời gian thực hiện | |

| Kịch bản | Kết quả | Bước có sai khác | Mô tả sai khác | Mã phiếu lỗi | Ý kiến của PO | Thời hạn khắc phục |
|---|---|---|---|---|---|---|
| `UAT-021` | | | | | | |
| `UAT-022` | | | | | | |
| `UAT-023` | | | | | | |
| `UAT-024` | | | | | | |
| `UAT-025` | | | | | | |

### 5.3 Xác nhận nghiệm thu

Các bên ký tên dưới đây xác nhận đã thực hiện hoặc theo dõi đợt nghiệm thu và thống nhất với kết quả ghi tại mục 5.2.

| Vai trò | Họ và tên | Chữ ký | Ngày |
|---|---|---|---|
| PO | | | |
| Đại diện bộ phận vận hành | | | |
| QA | | | |

**Quyết định của PO.** Cột "Quyết định" chỉ nhận một trong ba giá trị: "Đồng ý phát hành", "Đồng ý phát hành có điều kiện", "Không đồng ý phát hành".

| Quyết định | Điều kiện kèm theo |
|---|---|
| | |
