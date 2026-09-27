# `SCR-002` — Màn quản trị tài khoản

## 1. Thông tin chung

| Mục | Nội dung |
|---|---|
| Mã màn hình | `SCR-002` |
| Mục đích | Cho phép quản trị viên xem và tìm đúng tài khoản cần xử lý trong toàn bộ danh sách, rồi mở các lệnh quản trị (đổi vai, vô hiệu hoá, khôi phục) ngay trên dòng của tài khoản đó. Màn này chỉ đọc dữ liệu, không tự thay đổi tài khoản nào |
| Actor | Quản trị viên có quyền quản trị tài khoản, superAdmin |
| Điều kiện truy cập | Đã đăng nhập bằng tài khoản có quyền quản trị tài khoản. Không có quyền thì mục "Tài khoản" không hiện trên thanh điều hướng, và máy chủ từ chối trả danh sách |
| Màn hình trước/sau | Trước: thanh điều hướng bên trái › Quản lý người dùng › Tài khoản. Sau: `SCR-003` Hộp thoại đổi vai trò, mở từ menu của một dòng; hộp thoại xác nhận vô hiệu hoá hoặc khôi phục. Mọi hộp thoại đều mở đè lên màn này, khi đóng là quay lại `SCR-002` |
| Mockup | [Thư mục mockup `SCR-002`](../../mockups/scr-002/) — 16 frame, liệt kê ở mục 2 |
| User story liên quan | [`US-011`](../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) tra cứu danh sách<br>[`US-012`](../06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md) menu thao tác, chọn nhiều dòng và mở lệnh<br>[`US-013`](../06-user-stories/us-013-doi-vai-tai-khoan.md) đổi vai, bắt đầu từ menu của một dòng |
| Quy tắc nghiệp vụ liên quan | `RULE-006` bảo vệ tài khoản superAdmin<br>`RULE-007` không tự đổi vai hoặc tự vô hiệu hoá|

## 2. Mockup

| Frame | Nội dung |
|---|---|
| [scr-002_sample](../../mockups/scr-002/scr-002_sample.png) | Frame tham chiếu cho mục 3: mở cùng lúc mọi thành phần và đánh số #1–#24. Không phải một trạng thái thật của màn |
| [scr-002_default](../../mockups/scr-002/scr-002_default.png) | Trạng thái mặc định của `SCR-002`: bảng có dữ liệu, khu vực tìm kiếm và bộ lọc đang bị ẩn |
| [scr-002_filter-on](../../mockups/scr-002/scr-002_filter-on.png) | Đã bấm nút "Bộ lọc" ở góc trên bên trái rồi chọn vai trò cần lọc |
| [scr-002_loading](../../mockups/scr-002/scr-002_loading.png) | Thân bảng hiện các skeleton loader; thanh công cụ vẫn hiện, nút chuyển trang tắt |
| [scr-002_notfound](../../mockups/scr-002/scr-002_notfound.png) | Không có tài khoản nào khớp điều kiện tìm kiếm hoặc bộ lọc |
| [scr-002_error](../../mockups/scr-002/scr-002_error.png) | Tải danh sách thất bại, màn hình hiển thị nút "Thử lại" |
| [scr-002_access-denied](../../mockups/scr-002/scr-002_access-denied.png) | Người dùng không có quyền truy cập: thanh công cụ và bảng bị thay bằng thông báo |
| [scr-002_session-expired](../../mockups/scr-002/scr-002_session-expired.png) | Phiên đăng nhập hết hạn hoặc không hợp lệ: hệ thống đã chuyển về trang đăng nhập và hiện thông báo yêu cầu đăng nhập lại |
| [scr-002_display-hide-column](../../mockups/scr-002/scr-002_display-hide-column.png) | Menu "Cột hiển thị" đang mở |
| [scr-002_menu_activated-account](../../mockups/scr-002/scr-002_menu_activated-account.png) | Menu thao tác đang mở trên một tài khoản đang hoạt động |
| [scr-002_menu_disabled-account](../../mockups/scr-002/scr-002_menu_disabled-account.png) | Menu thao tác đang mở trên một tài khoản đã bị vô hiệu hoá |
| [scr-002_menu_pagination](../../mockups/scr-002/scr-002_menu_pagination.png) | Menu chọn số kết quả hiển thị trên mỗi trang được mở |
| [scr-002_not-choose-activated-account](../../mockups/scr-002/scr-002_not-choose-activated-account.png) | Đang lọc tài khoản theo trạng thái "Đang hoạt động": cột ô chọn đã xuất hiện nhưng chưa có dòng nào được chọn |
| [scr-002_not-choose-disabled-account](../../mockups/scr-002/scr-002_not-choose-disabled-account.png) | Đang lọc tài khoản theo trạng thái "Đã vô hiệu hoá": cột ô chọn đã hiện nhưng chưa có dòng nào được chọn |
| [scr-002_choose-all_activated-account](../../mockups/scr-002/scr-002_choose-all_activated-account.png) | Đã chọn nhiều tài khoản đang hoạt động, thanh hành động hiển thị nút "Vô hiệu hoá" |
| [scr-002_choose-all_disabled-account](../../mockups/scr-002/scr-002_choose-all_disabled-account.png) | Đã chọn nhiều tài khoản đã bị vô hiệu hoá, thanh hành động hiển thị nút "Khôi phục" |

## 3. Mô tả thành phần

Mô tả dựa trên frame [scr-002_sample](../../mockups/scr-002/scr-002_sample.png)

| # | Tên | Loại | Bắt buộc | Mặc định | Rule / Validation | Ghi chú |
|---|---|---|---|---|---|---|
| 1 | Nút "Bộ lọc" | Button | – | Khu vực tìm kiếm và bộ lọc đang ẩn | Bấm để hiện hoặc ẩn khu vực chứa #4–#8 | Nút đổi màu khi khu vực đang mở. Ẩn khu vực này **không** xoá điều kiện đang áp dụng |
| 2 | Nút "Cột hiển thị" | Button | – | – | Bấm để mở hoặc đóng menu #22 | Nút đổi màu khi menu đang mở |
| 3 | Nút làm mới | Button, chỉ có biểu tượng ↻ | – | Bật | Tắt khi bảng đang tải | Nằm ở góc phải thanh công cụ, không có chữ |
| 4 | Ô tìm kiếm | Textbox | Không | Trống, gợi ý "Tìm theo tên hoặc email" | Tìm theo tên **hoặc** email; không phân biệt chữ hoa chữ thường; tự bỏ khoảng trắng thừa ở hai đầu; tìm trên toàn bộ dữ liệu trước khi chia trang | Kết quả tự cập nhật khi ngừng gõ, không cần bấm Enter. Từ khoá không tạo thẻ ở #7 |
| 5 | Bộ lọc vai trò | Dropdown | Không | "Tất cả vai trò" | Chọn một vai, hoặc "Tất cả vai trò". Danh sách gồm cả vai đang ở dạng nháp | Khác danh sách vai được phép gán ở `SCR-003` |
| 6 | Bộ lọc trạng thái | Dropdown | Không | "Tất cả trạng thái" | "Tất cả trạng thái", "Đang hoạt động" hoặc "Đã vô hiệu hoá" | Chọn một trạng thái cụ thể mới bật chế độ chọn nhiều dòng (`SCR002-ST-07`) |
| 7 | Thẻ bộ lọc đang áp | Chip, có nút ✕ | – | Không có thẻ | Chỉ tạo thẻ cho bộ lọc: mỗi bộ lọc #5, #6 đang khác "Tất cả" có một thẻ dạng "{tên bộ lọc}: {giá trị}". Bấm ✕ trên thẻ để đưa riêng bộ lọc đó về "Tất cả" | Ví dụ trong frame: "Trạng thái: Đang hoạt động" |
| 8 | Nút "Xoá bộ lọc" | Button dạng chữ | – | Tắt | Bật khi #5 hoặc #6 đang khác "Tất cả vai trò" hoặc "Tất cả trạng thái" | Chỉ đưa hai bộ lọc về "Tất cả"; **giữ nguyên** từ khoá ở #4 |
| 9 | Thanh hành động | Thanh | – | Ẩn | Chỉ hiện khi có ít nhất một dòng được chọn. Gồm đúng ba phần: dòng chữ "Đã chọn {n} tài khoản", nút lệnh #23 và nút #24 | {n} đếm trên toàn bộ kết quả đã lọc, không chỉ trang đang xem |
| 10 | Ô chọn tất cả | Checkbox | Không | Không hiện | Chỉ hiện khi #6 có giá trị cụ thể và bảng có ít nhất một dòng. Có dòng nhưng không dòng nào chọn được thì hiện nhưng bị khoá | Nằm ở tiêu đề cột. Chọn **mọi tài khoản hợp lệ trong mọi trang**, không chỉ trang đang xem |
| 11 | Ô chọn từng dòng | Checkbox | Không | Chưa chọn | Dòng của chính người đang thao tác và dòng superAdmin luôn bị khoá, không bao giờ vào danh sách đã chọn | Dòng đã chọn được tô nền; ô bị khoá hiện màu xám. Xem `RULE-006`, `RULE-007` |
| 12 | Cột "Tài khoản" | Label | – | – | Chỉ đọc. Tên (in đậm) trên, email dưới | Luôn hiển thị. Cạnh tên có thẻ "Bạn" ở dòng của chính người đang xem, thẻ "superAdmin" nền tối ở dòng superAdmin |
| 13 | Cột "Vai trò" | Nhãn màu | – | – | Chỉ đọc; mỗi vai một màu nhãn | Dòng superAdmin hiện đúng vai thật của tài khoản đó. Tắt được qua #22 |
| 14 | Cột "Trạng thái" | Nhãn màu | – | – | Chỉ đọc: "Đang hoạt động" hoặc "Đã vô hiệu hoá" | Tắt được qua #22 |
| 15 | Cột "Ngày tham gia" | Label, tiêu đề bấm được | – | Sắp xếp mới → cũ | Là ngày tạo tài khoản, không phải ngày cập nhật gần nhất; hiển thị dạng dd/mm/yyyy. Hai tài khoản trùng ngày thì xếp theo mã tài khoản, cùng chiều sắp xếp | Mũi tên cạnh tiêu đề chỉ chiều đang sắp. Tắt được qua #22 |
| 16 | Cột "Thao tác" | Cột | – | – | Chứa nút #20 của từng dòng | Luôn hiển thị, không tắt được qua #22 |
| 17 | Số kết quả mỗi trang | Dropdown | Không | 10 | Chỉ có 10, 20, 50, 100; không cho nhập tay | – |
| 18 | Chỉ số kết quả | Label | – | – | Chỉ đọc: "{from} – {to} trong {total} tài khoản" | {total} là tổng số kết quả đã lọc, không bị cắt ở 100 |
| 19 | Nút chuyển trang « ‹ › » | Button | – | – | Tắt nút tương ứng khi đang ở trang đầu hoặc trang cuối | – |
| 20 | Nút thao tác (…) | Button, chỉ có biểu tượng | – | – | Bấm để mở menu #21 của đúng dòng đó | Không hiện trên dòng superAdmin và dòng của chính mình khi đang hoạt động (`RULE-006`, `RULE-007`) |
| 21 | Menu thao tác | Menu | – | Đóng | Nội dung đổi theo loại dòng: tài khoản đang hoạt động có "Đổi vai trò" và "Vô hiệu hoá"; tài khoản đã bị vô hiệu hoá chỉ có "Khôi phục". Chi tiết từng loại dòng ở mục 4 | Chọn một lệnh chỉ mở hộp thoại, chưa thay đổi gì. "Vô hiệu hoá" chữ đỏ |
| 22 | Menu "Cột hiển thị" | Menu checkbox | – | Cả ba cột đều được tick | Gồm ba cột tuỳ chọn #13, #14, #15, bật tắt độc lập; được tắt cả ba | Cột #12 và #16 không có trong menu nên không tắt được |
| 23 | Nút lệnh hàng loạt | Button | – | – | Chỉ có **một** nút, theo #6: "Vô hiệu hoá" khi lọc "Đang hoạt động", "Khôi phục" khi lọc "Đã vô hiệu hoá" | Nút "Vô hiệu hoá" viền đỏ. Bấm chỉ mở hộp thoại xác nhận, chưa thay đổi gì |
| 24 | Nút "Bỏ chọn" | Button | – | – | Bỏ hết lựa chọn trên mọi trang | Thanh hành động #9 biến mất ngay |

Màn này **không hiển thị** ngày sinh, địa chỉ, số điện thoại hay bất kỳ thông tin hồ sơ nào khác. Dữ liệu máy chủ gửi xuống chỉ đủ để hiển thị bảng và biết dòng nào được phép thao tác.

## 4. Hành động và điều hướng

| Hành động | Điều kiện | Kết quả |
|---|---|---|
| Bấm "Bộ lọc" (#1) | Luôn | Hiện hoặc ẩn khu vực tìm kiếm và bộ lọc. Không tải lại trang, không xoá điều kiện đang áp |
| Tick hoặc bỏ tick trong menu "Cột hiển thị" (#22) | Luôn | Cột tương ứng hiện hoặc ẩn ngay. Không tải lại dữ liệu |
| Bấm nút làm mới (#3) | Bảng không đang tải | Tải lại đúng từ khoá, bộ lọc, chiều sắp xếp, trang và số dòng hiện tại → `SCR002-ST-02` |
| Gõ từ khoá, đổi bộ lọc vai hoặc trạng thái, đổi chiều sắp xếp, đổi số dòng mỗi trang | Luôn | Quay về trang 1, bỏ hết lựa chọn, tải lại → `SCR002-ST-02`. Không hiển thị kết quả của điều kiện cũ trong lúc chờ |
| Bấm ✕ trên thẻ bộ lọc (#7) | Thẻ đang hiện | Đưa riêng bộ lọc đó về "Tất cả", thẻ biến mất; quay về trang 1, bỏ hết lựa chọn, tải lại → `SCR002-ST-02`. Từ khoá và bộ lọc còn lại giữ nguyên |
| Bấm "Xoá bộ lọc" (#8) | Có ít nhất một bộ lọc khác "Tất cả" | Đưa hai bộ lọc về "Tất cả", các thẻ ở #7 biến mất; **giữ nguyên** từ khoá, chiều sắp xếp và số dòng mỗi trang. Quay về trang 1, bỏ hết lựa chọn, tải lại → `SCR002-ST-02` |
| Bấm "Xoá tìm kiếm và bộ lọc" ở `SCR002-ST-04` | Có ít nhất một điều kiện đang áp | Xoá từ khoá, đưa hai bộ lọc về "Tất cả", quay về trang 1, bỏ hết lựa chọn. **Giữ nguyên** chiều sắp xếp và số dòng mỗi trang |
| Bấm tiêu đề "Ngày tham gia" (#15) | Luôn | Đảo chiều sắp xếp giữa mới → cũ và cũ → mới. Sắp trên toàn bộ dữ liệu trước khi chia trang, rồi quay về trang 1 |
| Bấm nút chuyển trang (#19) | Nút không bị tắt | Chuyển sang trang tương ứng. Các dòng đã chọn vẫn được giữ |
| Bấm ô chọn tất cả (#10) | Chưa chọn, hoặc mới chọn một phần | Chọn mọi tài khoản hợp lệ trong mọi trang, tự loại dòng của chính mình và dòng superAdmin. Thanh hành động (#9) hiện ra → `SCR002-ST-08` |
| Bấm ô chọn tất cả (#10) | Đã chọn hết | Bỏ chọn tất cả trên mọi trang. Thanh hành động biến mất |
| Bấm ô chọn một dòng (#11) | Dòng không bị khoá | Thêm hoặc bỏ đúng dòng đó; giữ nguyên các lựa chọn khác, kể cả ở trang khác |
| Bấm "Vô hiệu hoá" (#23) trên thanh hành động | #6 là "Đang hoạt động" | Chốt danh sách đã chọn rồi mở hộp thoại xác nhận vô hiệu hoá. Chưa tài khoản nào bị thay đổi |
| Bấm "Khôi phục" (#23) trên thanh hành động | #6 là "Đã vô hiệu hoá" | Chốt danh sách đã chọn rồi mở hộp thoại xác nhận khôi phục. Chưa tài khoản nào bị thay đổi |
| Bấm "Bỏ chọn" (#24) | Có ít nhất một dòng được chọn | Bỏ hết lựa chọn. Thanh hành động biến mất |
| Bấm nút thao tác (#20) | Dòng của tài khoản có trạng thái "Đang hoạt động" | Menu có "Đổi vai trò" và "Vô hiệu hoá", đúng thứ tự này |
| Bấm nút thao tác (#20) | Dòng của tài khoản có trạng thái "Đã vô hiệu hóa" | Menu chỉ có "Khôi phục". Không được phép đổi vai cho tài khoản đang bị khoá |
| Bấm nút thao tác (#20) | Dòng của chính mình, đang hoạt động | **Không có nút "Thao tác"**. Cả hai lệnh đều bị `RULE-007` cấm |
| Bấm nút thao tác (#20) | Dòng của chính mình, đã bị vô hiệu hoá nhưng vẫn còn phiên đăng nhập | Menu chỉ có "Khôi phục". Tự khôi phục là ngoại lệ của `RULE-007` |
| Bấm nút thao tác (#20) | Dòng superAdmin, bất kỳ trạng thái | **Không có nút "Thao tác"**; dòng vẫn nằm trong danh sách (`RULE-006`) |
| Chọn "Đổi vai trò" trong menu thao tác (#21) | Menu đang mở | Mở `SCR-003` cho đúng tài khoản của dòng đó. Chưa thay đổi gì |
| Chọn "Vô hiệu hoá" hoặc "Khôi phục" trong menu thao tác (#21) | Menu đang mở | Mở hộp thoại xác nhận tương ứng. Chưa thay đổi gì |
| Bấm "Thử lại" | Ở `SCR002-ST-05` | Tải lại đúng điều kiện hiện tại → `SCR002-ST-02` |
| Quay lại từ `SCR-003` bằng "Huỷ", ✕, Esc hoặc bấm ra ngoài | Luôn | Về màn này; bảng không tải lại |
| Quay lại sau khi đổi vai thành công hoặc không có thay đổi | – | Về màn này; bảng tải lại đúng điều kiện hiện tại |

## 5. Trạng thái màn hình

### 5.1. Tổng quan trạng thái

| Mã | Trạng thái | Điều kiện xảy ra | Hiển thị | Hành động cho phép | Frame |
|---|---|---|---|---|---|
| `SCR002-ST-01` | Đang tải | Lần lấy dữ liệu đầu tiên khi vừa vào màn | Thân bảng hiện các dòng xám giữ chỗ; chân bảng ghi "Đang tải danh sách..." thay cho chỉ số kết quả; bốn nút chuyển trang tắt. Tiêu đề bảng và thanh công cụ vẫn hiện; không có cột ô chọn; không có màn chờ toàn trang | Dùng thanh công cụ và bộ lọc; nút làm mới tắt | [scr-002_loading](../../mockups/scr-002/scr-002_loading.png) |
| `SCR002-ST-02` | Đang tải lại | Bấm nút làm mới, đổi điều kiện tìm kiếm, đổi trang hoặc số dòng mỗi trang | Giống `SCR002-ST-01`; **không** giữ dữ liệu cũ trong vùng bảng | Như `SCR002-ST-01` | [scr-002_loading](../../mockups/scr-002/scr-002_loading.png) |
| `SCR002-ST-03` | Có dữ liệu | Có ít nhất một tài khoản khớp điều kiện | Bảng theo cấu hình cột hiện tại. Dòng superAdmin có nền xám nhạt, thẻ "superAdmin" cạnh tên và không có nút "Thao tác"; dòng của chính mình có thẻ "Bạn"; chân bảng ghi "1 – N trong M tài khoản" | Mọi hành động ở mục 4 | [scr-002_default](../../mockups/scr-002/scr-002_default.png), [scr-002_filter-on](../../mockups/scr-002/scr-002_filter-on.png), [scr-002_display-hide-column](../../mockups/scr-002/scr-002_display-hide-column.png), [scr-002_menu_activated-account](../../mockups/scr-002/scr-002_menu_activated-account.png), [scr-002_menu_disabled-account](../../mockups/scr-002/scr-002_menu_disabled-account.png), [scr-002_menu_pagination](../../mockups/scr-002/scr-002_menu_pagination.png) |
| `SCR002-ST-04` | Rỗng | Tra cứu xong nhưng không tài khoản nào khớp từ khoá và bộ lọc | "Không tìm thấy kết quả", "Không có bản ghi nào phù hợp với từ khoá hoặc bộ lọc hiện tại." và nút "Xoá tìm kiếm và bộ lọc"; chân bảng ghi "0 trong 0 tài khoản". Không có cột ô chọn, không hiển thị dữ liệu của lần tìm trước | Xoá hoặc sửa điều kiện | [scr-002_notfound](../../mockups/scr-002/scr-002_notfound.png) |
| `SCR002-ST-05` | Lỗi tải | Máy chủ không phản hồi, quá thời gian chờ, hoặc trả lỗi không phải lỗi thiếu quyền hay phiên đăng nhập không hợp lệ | "Không thể tải danh sách tài khoản.", "Vui lòng thử lại." và nút "Thử lại" trong thân bảng; chân bảng ghi "— trong — tài khoản"; bốn nút chuyển trang tắt. Giữ điều kiện tìm kiếm hiện tại; **không** hiển thị dữ liệu cũ | "Thử lại"; dùng thanh công cụ và bộ lọc | [scr-002_error](../../mockups/scr-002/scr-002_error.png) |
| `SCR002-ST-06` | Không đủ quyền | Máy chủ từ chối vì người dùng không có quyền quản trị tài khoản | Thanh công cụ và bảng được thay bằng "Bạn không có quyền quản lý tài khoản." và "Bạn không thể thực hiện thao tác này."; chỉ còn tiêu đề màn. **Không** có nút "Thử lại" | Không; chỉ rời màn | [scr-002_access-denied](../../mockups/scr-002/scr-002_access-denied.png) |
| `SCR002-ST-07` | Chế độ chọn nhiều dòng | #6 có giá trị cụ thể và bảng đang ở `SCR002-ST-03` | Hiện cột ô chọn (#10, #11); dòng của chính mình và dòng superAdmin có ô chọn bị khoá | Chọn từng dòng hoặc chọn tất cả | [scr-002_not-choose-activated-account](../../mockups/scr-002/scr-002_not-choose-activated-account.png), [scr-002_not-choose-disabled-account](../../mockups/scr-002/scr-002_not-choose-disabled-account.png) |
| `SCR002-ST-08` | Đã chọn | Có ít nhất một dòng được chọn | Thanh hành động (#9) hiện ngay dưới khu vực bộ lọc | Mở lệnh hàng loạt; "Bỏ chọn" | [scr-002_choose-all_activated-account](../../mockups/scr-002/scr-002_choose-all_activated-account.png), [scr-002_choose-all_disabled-account](../../mockups/scr-002/scr-002_choose-all_disabled-account.png) |
| `SCR002-ST-09` | Phiên đăng nhập không hợp lệ | Máy chủ từ chối vì chưa đăng nhập, phiên đăng nhập đã hết hạn hoặc không hợp lệ | Rời màn này, chuyển thẳng về trang đăng nhập; trang đăng nhập hiện "Phiên đăng nhập đã hết hạn", "Vui lòng đăng nhập lại." Không hiển thị bảng hay thông báo không đủ quyền | Đăng nhập lại | [session-expired](../../mockups/scr-002/scr-002_session-expired.png) |

### 5.2. Đặc tả chi tiết trạng thái

**`SCR002-ST-02` — Đang tải lại**

- **Khi nào xảy ra:** người dùng bấm nút làm mới, đổi bất kỳ điều kiện tìm kiếm nào, chuyển trang hoặc đổi số dòng mỗi trang.
- **Người dùng thấy gì:** giống hệt `SCR002-ST-01` — hiển thị skeleton loader trong thân bảng, chân bảng ghi "Đang tải danh sách...", thanh công cụ vẫn hiện.
- **Người dùng làm gì tiếp:** chờ kết quả, hoặc tiếp tục đổi điều kiện; lần đổi mới nhất là lần được hiển thị.
- **Ràng buộc:** vùng bảng **không** được giữ dữ liệu của lần tra cứu trước.

**`SCR002-ST-05` — Lỗi tải**

- **Khi nào xảy ra:** máy chủ không phản hồi, quá thời gian chờ, hoặc trả về lỗi không phải lỗi thiếu quyền hay phiên đăng nhập không hợp lệ.
- **Người dùng thấy gì:** "Không thể tải danh sách tài khoản." và "Vui lòng thử lại." trong thân bảng, kèm nút "Thử lại". Chân bảng ghi "— trong — tài khoản"; bốn nút chuyển trang tắt. Từ khoá và bộ lọc vẫn giữ nguyên.
- **Người dùng làm gì tiếp:** bấm "Thử lại", hoặc sửa điều kiện tìm kiếm rồi tra cứu lại.
- **Ràng buộc:** không hiển thị dữ liệu của lần tra cứu thành công trước đó như thể là kết quả mới.

**`SCR002-ST-06` — Không đủ quyền**

- **Khi nào xảy ra:** máy chủ từ chối trả danh sách vì người dùng không có quyền quản trị tài khoản.
- **Người dùng thấy gì:** cả thanh công cụ ("Bộ lọc", "Cột hiển thị", nút làm mới) lẫn bảng được thay bằng "Bạn không có quyền quản lý tài khoản." và "Bạn không thể thực hiện thao tác này."
- **Người dùng làm gì tiếp:** rời màn. Hệ thống không tự thử lại và không có nút "Thử lại".
- **Ràng buộc:** phải tách khỏi `SCR002-ST-05`.



