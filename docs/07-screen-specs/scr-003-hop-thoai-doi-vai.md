# `SCR-003` — Hộp thoại đổi vai trò

## 1. Thông tin chung

| Mục | Nội dung |
|---|---|
| Mã màn hình | `SCR-003` |
| Mục đích | Cho quản trị viên chọn vai mới cho đúng một tài khoản và ghi lại lý do. Hộp thoại này **chưa đổi vai của ai**; việc đổi vai chỉ xảy ra khi bấm "Xác nhận" ở `SCR-004` |
| Actor | Quản trị viên có quyền quản trị tài khoản, superAdmin |
| Điều kiện truy cập | Mở từ lệnh "Đổi vai trò" trong menu thao tác của một dòng ở `SCR-002`. Lệnh này chỉ có trên dòng đang hoạt động, không phải của chính mình và không phải superAdmin (`RULE-006`, `RULE-007`) |
| Màn hình trước/sau | Trước: `SCR-002`, menu thao tác của một dòng. Sau: "Tiếp tục" → `SCR-004` mở đè lên hộp thoại này; "Huỷ", ✕, Esc hoặc bấm ra ngoài → đóng, về `SCR-002` |
| Mockup | [Thư mục mockup `SCR-003`](../../mockups/scr-003/) — 6 frame trong thư mục và 1 frame dùng chung từ `SCR-002`, liệt kê ở mục 2 |
| User story liên quan | [`US-013`](../06-user-stories/us-013-doi-vai-tai-khoan.md) đổi vai cho một tài khoản: `US013-SC-01` đổi thành công<br>`US013-SC-02` mở hộp thoại<br>`US013-SC-03` danh sách vai<br>`US013-SC-04` lỗi danh sách<br>`US013-SC-05`, `US013-SC-06` ràng buộc lý do<br>`US013-SC-11` chọn trùng vai<br>`US013-SC-16` thời điểm có hiệu lực<br>`US013-SC-19` phiên đăng nhập hết hạn |
| Quy tắc nghiệp vụ liên quan | `RULE-006` bảo vệ tài khoản superAdmin<br>`RULE-007` không tự đổi vai hoặc tự vô hiệu hoá |

## 2. Mockup

| Frame | Nội dung |
|---|---|
| [scr-003_sample](../../mockups/scr-003/scr-003_sample.png) | Frame tham chiếu cho mục 3: frame `eligible` có đánh số #1–#11 |
| [scr-003_change-role-dialog_catalog-loading](../../mockups/scr-003/scr-003_change-role-dialog_catalog-loading.png) | Vừa mở hộp thoại, danh mục vai đang tải: ô "Vai trò mới" ghi "Đang tải danh mục vai trò..." và bị khoá; "Tiếp tục" tắt |
| [scr-003_change-role-dialog_ready](../../mockups/scr-003/scr-003_change-role-dialog_ready.png) | Danh mục đã tải xong, chưa chọn vai: ô "Vai trò mới" hiện chữ gợi ý "Chọn vai trò mới"; lý do còn trống; "Tiếp tục" tắt |
| [scr-003_change-role-dialog_eligible](../../mockups/scr-003/scr-003_change-role-dialog_eligible.png) | Đã chọn vai và lý do hợp lệ, "Tiếp tục" bật |
| [scr-003_change-role-dialog_reason-validation-errors](../../mockups/scr-003/scr-003_change-role-dialog_reason-validation-errors.png) | Lý do ngoài khoảng 20–500: viền ô đỏ, "Tiếp tục" tắt. Frame xếp chồng **ba** biến thể thông báo để tham chiếu; thực tế mỗi lúc chỉ hiện một (xem #7) |
| [scr-003_change-role-dialog_catalog-load-failed](../../mockups/scr-003/scr-003_change-role-dialog_catalog-load-failed.png) | Không tải được danh mục vai: khung lỗi đỏ kèm nút "Thử tải lại"; ô "Vai trò mới" hiện "—" và bị khoá |
| [scr-002_session-expired](../../mockups/scr-002/scr-002_session-expired.png) | Phiên đăng nhập hết hạn hoặc không hợp lệ: hộp thoại đã đóng, hệ thống chuyển về trang đăng nhập và hiện thông báo yêu cầu đăng nhập lại |

## 3. Mô tả thành phần

Mô tả dựa trên frame [scr-003_sample](../../mockups/scr-003/scr-003_sample.png). #12 lấy từ frame [catalog-load-failed](../../mockups/scr-003/scr-003_change-role-dialog_catalog-load-failed.png).

| # | Tên | Loại | Bắt buộc | Mặc định | Rule / Validation | Ghi chú |
|---|---|---|---|---|---|---|
| 1 | Tiêu đề | Label | – | "Thay đổi vai trò", "Chọn vai trò mới và nhập lý do thay đổi" | Chỉ đọc | – |
| 2 | Nút đóng ✕ | Button, chỉ có biểu tượng | – | Bật | Luôn bật | Cùng kết quả với "Huỷ" (#10) |
| 3 | Tài khoản đích | Label | – | – | Chỉ đọc: tên (in đậm) và email, lấy từ dòng đã chọn ở `SCR-002` | Không hiển thị ngày sinh, giới tính, địa chỉ, số điện thoại hay giới thiệu bản thân (`US013-SC-02`) |
| 4 | Vai trò hiện tại | Nhãn màu | – | – | Chỉ đọc, lấy từ dòng đã chọn ở `SCR-002`. Nếu trong lúc đang thao tác có người khác đổi vai của tài khoản này, `SCR-004` sẽ báo và không ghi đè | Cùng màu nhãn với cột "Vai trò" ở `SCR-002` |
| 5 | Vai trò mới | Dropdown | **Có** | Chưa chọn vai nào, chữ gợi ý xám "Chọn vai trò mới" | Chọn đúng một vai. Danh sách **chỉ** gồm vai trò đã công bố và đang dùng. Không có vai nháp, vai đã ngừng dùng, và không có lựa chọn superAdmin — superAdmin là thuộc tính của tài khoản, không phải một vai trò (`US013-SC-03`). Được chọn trùng vai hiện tại (`US013-SC-11`) | Khác danh sách vai ở bộ lọc #5 của `SCR-002`. Khoá khi danh mục đang tải hoặc tải lỗi. Trong ô chỉ hiện một nội dung, theo trạng thái: "Đang tải danh mục vai trò..." (`SCR003-ST-01`)<br>"Chọn vai trò mới" (nếu chưa chọn vai trò nào)<br>tên vai (đã chọn vai trò)<br>"—" (không tải được danh mục vai trò)(`SCR003-ST-05`) |
| 6 | Lý do | Textarea | **Có** | Trống, gợi ý "Vui lòng nhập lý do" | Từ 20 đến 500 ký tự **sau khi bỏ khoảng trắng đầu cuối**, 20 và 500 đều hợp lệ. Khi bấm "Xác nhận" ở `SCR-004`, hệ thống kiểm tra lại đúng quy tắc này (`US013-SC-05`, `US013-SC-06`) | Không chặn gõ quá 500 ký tự — người dùng dán một đoạn dài vẫn thấy được mình thừa bao nhiêu để cắt bớt |
| 7 | Dòng gợi ý / lỗi lý do | Label | – | "Tối thiểu 20 ký tự", chữ xám | Ở trạng thái lỗi chuyển sang chữ đỏ, viền #6 đỏ, và hiện **một** thông báo tùy theo độ dài đã bỏ khoảng trắng người thao tác nhập vào: 0 ký tự → "Vui lòng nhập lý do"<br>1–19 ký tự → "Tối thiểu 20 ký tự"<br>trên 500 ký tự → "Tối đa 500 ký tự" | Lỗi hiện khi rời ô #6 lần đầu với giá trị không hợp lệ; từ đó cập nhật theo từng lần gõ. Ô chưa từng được nhập thì không báo lỗi |
| 8 | Bộ đếm ký tự | Label | – | "0/500" | Đếm **độ dài sau khi bỏ khoảng trắng đầu cuối**, cùng cách đếm với quy tắc 20–500 ở #6 | Đổi đỏ cùng lúc với #7 |
| 9 | Ghi chú hiệu lực | Label, có biểu tượng ( i ) | – | "Thay đổi sẽ có hiệu lực ở phiên đăng nhập mới." | Chỉ đọc | Phiên đang mở của tài khoản đích vẫn giữ quyền cũ (`US013-SC-16`) |
| 10 | Nút "Huỷ" | Button | – | Bật | Luôn bật | Đóng mà không hỏi lại |
| 11 | Nút "Tiếp tục" | Button chính | – | Tắt | Chỉ bật khi #5 đã có giá trị **và** #6 đạt 20–500 ký tự **và** hộp thoại đang ở `SCR003-ST-04` | Bấm chỉ mở `SCR-004`, **vai chưa đổi** |
| 12 | Khung lỗi tải danh mục | Alert, có nút "Thử tải lại" | – | Ẩn | Chỉ hiện ở `SCR003-ST-05`, nằm giữa #5 và #6. Tiêu đề "Không tải được danh mục vai trò"; nội dung "Hệ thống không dùng danh mục đã lưu trước đó để tránh gán sai vai. Vui lòng thử tải lại; nếu vẫn lỗi, hãy đóng hộp thoại và thử lại sau." | Không dùng lại danh sách vai của lần mở trước |

Bộ đếm (#8) không tính khoảng trắng ở hai đầu, giống hệt cách kiểm tra 20–500.

Quản trị viên cấp được vai quản trị cho người khác, miễn vai đó có trong danh sách #5.

## 4. Hành động và điều hướng

| Hành động | Điều kiện | Kết quả |
|---|---|---|
| Mở hộp thoại | Chọn "Đổi vai trò" ở menu thao tác của `SCR-002` | Hiện #3, #4 từ dòng đã chọn và tải danh mục vai → `SCR003-ST-01` |
| Mở danh sách "Vai trò mới" (#5) | Danh mục đã tải xong | Hiện các vai được phép gán |
| Chọn một vai ở #5 | Danh mục đã tải xong | Ghi nhận vai; bật "Tiếp tục" nếu lý do đã hợp lệ → `SCR003-ST-04` |
| Nhập hoặc sửa lý do (#6) | Luôn | Cập nhật bộ đếm #8; cập nhật #7 nếu đã vào trạng thái lỗi; bật hoặc tắt "Tiếp tục" theo #11. Được nhập cả khi danh mục đang tải hoặc tải lỗi |
| Rời ô lý do (#6) | Lý do ngoài khoảng 20–500 | → `SCR003-ST-03` |
| Bấm "Thử tải lại" (#12) | Ở `SCR003-ST-05` | Tải lại danh mục → `SCR003-ST-01`. Lý do đã nhập được giữ |
| Bấm "Tiếp tục" (#11) | Nút đang bật | Bỏ khoảng trắng đầu cuối của lý do rồi mở `SCR-004` với tài khoản đích, vai cũ → vai mới và lý do. **Vai chưa đổi** |
| Bấm "Huỷ" (#10), ✕ (#2), phím Esc hoặc bấm ra ngoài | Mọi trạng thái | Đóng, không thay đổi gì, bỏ nội dung đã nhập, về `SCR-002`; bảng không tải lại |
| Quay lại từ `SCR-004` bằng "Quay lại", ✕, Esc hoặc bấm ra ngoài | – | Về hộp thoại này, **giữ** lý do đã nhập, tải lại danh mục vai → `SCR003-ST-01`. Vai đã chọn còn trong danh mục mới thì giữ; không còn thì bỏ chọn để người dùng chọn lại (`US013-SC-10`) |

Bốn cách đóng cho cùng một kết quả và không hỏi "Bạn có chắc không?".

## 5. Trạng thái màn hình

### 5.1. Tổng quan trạng thái

| Mã | Trạng thái | Điều kiện xảy ra | Hiển thị | Hành động cho phép | Frame |
|---|---|---|---|---|---|
| `SCR003-ST-01` | Đang tải danh mục | Vừa mở hộp thoại, bấm "Thử tải lại", hoặc quay lại từ `SCR-004` | #3, #4 hiện đủ; #5 ghi "Đang tải danh mục vai trò..." và bị khoá; "Tiếp tục" tắt | Nhập lý do; đóng hộp thoại | [catalog-loading](../../mockups/scr-003/scr-003_change-role-dialog_catalog-loading.png) |
| `SCR003-ST-02` | Sẵn sàng | Danh mục đã tải xong nhưng chưa đủ điều kiện: chưa chọn vai, hoặc lý do chưa đạt mà chưa vào trạng thái lỗi | Hiện đủ thông tin; #5 hiện "Chọn vai trò mới" khi chưa chọn vai; #7 chữ xám "Tối thiểu 20 ký tự"; "Tiếp tục" tắt | Chọn vai, nhập lý do, đóng | [ready](../../mockups/scr-003/scr-003_change-role-dialog_ready.png) |
| `SCR003-ST-03` | Lỗi lý do | Đã rời ô lý do với độ dài ngoài 20–500, hoặc đang sửa sau khi đã vào trạng thái này | Viền #6 đỏ; #7 và #8 đỏ với **một** trong ba thông báo; "Tiếp tục" tắt. Giữ nguyên nội dung đã nhập | Sửa lý do, chọn vai, đóng | [reason-validation-errors](../../mockups/scr-003/scr-003_change-role-dialog_reason-validation-errors.png) |
| `SCR003-ST-04` | Đủ điều kiện | Đã chọn vai và lý do đạt 20–500 ký tự | #7, #8 về màu xám; "Tiếp tục" bật | Mọi hành động ở mục 4 | [eligible](../../mockups/scr-003/scr-003_change-role-dialog_eligible.png) |
| `SCR003-ST-05` | Lỗi tải danh mục | Không tải được danh sách vai vì hệ thống lỗi hoặc không phản hồi, hoặc danh sách trả về rỗng | Khung #12 kèm "Thử tải lại"; #5 hiện "—" và bị khoá; "Tiếp tục" tắt. Không đóng hộp thoại | "Thử tải lại"; nhập lý do; đóng | [catalog-load-failed](../../mockups/scr-003/scr-003_change-role-dialog_catalog-load-failed.png) |
| `SCR003-ST-06` | Phiên đăng nhập không hợp lệ | Hệ thống từ chối tải danh mục vai vì phiên đăng nhập đã hết hạn hoặc không hợp lệ | Đóng hộp thoại, chuyển thẳng về trang đăng nhập; trang đăng nhập hiện "Phiên đăng nhập đã hết hạn", "Vui lòng đăng nhập lại." Không dùng danh mục đã lưu trước đó | Đăng nhập lại | [session-expired](../../mockups/scr-002/scr-002_session-expired.png) |

### 5.2. Đặc tả chi tiết trạng thái

**`SCR003-ST-03` — Lỗi lý do**

- **Khi nào xảy ra:** người dùng rời ô lý do khi độ dài sau khi bỏ khoảng trắng đầu cuối là 0, từ 1 đến 19, hoặc trên 500.
- **Người dùng thấy gì:** viền ô lý do đỏ; dưới ô là **một** thông báo đỏ theo thứ tự ưu tiên: 0 ký tự → "Vui lòng nhập lý do"; 1–19 → "Tối thiểu 20 ký tự"; trên 500 → "Tối đa 500 ký tự". Bộ đếm cùng dòng chuyển đỏ, ví dụ "11/500" hoặc "515/500".
- **Người dùng làm gì tiếp:** sửa lý do. Khi độ dài về lại 20–500 thì thông báo và viền đỏ biến mất ngay, không cần rời ô.
- **Ràng buộc:** lý do chỉ gồm khoảng trắng được tính là 0 ký tự, không phải lý do hợp lệ. Hai giá trị 20 và 500 đều hợp lệ.

**`SCR003-ST-05` — Lỗi tải danh mục**

- **Khi nào xảy ra:** không tải được danh sách vai vì hệ thống lỗi hoặc không phản hồi (không phải do thiếu quyền), hoặc danh sách vai trả về rỗng.
- **Người dùng thấy gì:** khung lỗi #12 giữa ô "Vai trò mới" và ô lý do, kèm nút "Thử tải lại"; ô "Vai trò mới" hiện "—" và bị khoá.
- **Người dùng làm gì tiếp:** bấm "Thử tải lại", hoặc đóng hộp thoại và thử lại sau. Lý do đã nhập được giữ qua các lần thử tải lại.
- **Ràng buộc:** danh sách rỗng là lỗi dữ liệu hoặc cấu hình, không phải trạng thái nghiệp vụ bình thường (`US013-SC-04`). Không được dùng danh mục đã lưu từ lần mở trước thay cho danh mục mới.

