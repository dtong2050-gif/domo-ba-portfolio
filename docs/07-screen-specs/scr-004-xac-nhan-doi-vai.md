# `SCR-004` — Hộp thoại xác nhận đổi vai

## 1. Thông tin chung

| Mục | Nội dung |
|---|---|
| Mã màn hình | `SCR-004` |
| Mục đích | Cho phép quản trị viên đọc lại quyết định đổi vai trò lần cuối — đổi cho ai, từ vai trò nào sang vai trò nào, vì lý do gì — rồi mới xác nhận. Đây là bước **duy nhất** thực sự đổi vai trò; hai màn trước chỉ để xem và nhập thông tin |
| Actor | Quản trị viên có quyền quản trị tài khoản, superAdmin |
| Điều kiện truy cập | Chỉ mở được bằng nút "Tiếp tục" ở `SCR-003`, sau khi đã chọn vai và nhập lý do hợp lệ. Khi quản trị viên bấm "Xác nhận", hệ thống kiểm tra lại từ đầu: người thao tác còn quyền không, tài khoản đích còn được phép đổi không, vai mới còn dùng được không |
| Màn hình trước/sau | Trước: `SCR-003`. Sau: bấm "Quay lại" → về `SCR-003`, lý do đã nhập vẫn còn. Đổi xong, không có gì để đổi, hoặc bấm "Đóng và tải lại" → đóng cả hai hộp thoại, về `SCR-002` và danh sách được tải lại |
| Mockup | [Thư mục mockup `SCR-004`](../../mockups/scr-004/) — 8 frame trong thư mục và 1 frame dùng chung từ `SCR-002`, liệt kê ở mục 2 |
| User story liên quan | [`US-013`](../06-user-stories/us-013-doi-vai-tai-khoan.md): `US013-SC-01` đổi thành công<br>`US013-SC-07` nội dung cần xác nhận<br>`US013-SC-05`, `US013-SC-08`, `US013-SC-09`, `US013-SC-10`, `US013-SC-12` các trường hợp bị từ chối<br>`US013-SC-11` không có gì thay đổi<br>`US013-SC-13` đổi không thành công<br>`US013-SC-14` không rõ kết quả<br>`US013-SC-16` khi nào vai mới có hiệu lực<br>`US013-SC-17` chống bấm hai lần<br>`US013-SC-18` phiên đăng nhập hết hạn |
| Quy tắc nghiệp vụ liên quan | `RULE-006` bảo vệ tài khoản superAdmin<br>`RULE-007` không tự đổi vai hoặc tự vô hiệu hoá |

## 2. Mockup

| Frame | Nội dung |
|---|---|
| [scr-004_sample](../../mockups/scr-004/scr-004_sample.png) | Frame tham chiếu cho mục 3: frame `ready` có đánh số #1–#8 |
| [scr-004_change-role-confirmation_ready](../../mockups/scr-004/scr-004_change-role-confirmation_ready.png) | Vừa mở: hiện đủ tài khoản đích, vai cũ → vai mới và lý do để kiểm lại; nút "Quay lại" và "Xác nhận" |
| [scr-004_change-role-confirmation_pending](../../mockups/scr-004/scr-004_change-role-confirmation_pending.png) | Đã bấm "Xác nhận", đang chờ hệ thống xử lý: hai nút bị khoá, nút chính ghi "Đang xử lý..." |
| [scr-004_change-role-confirmation_success](../../mockups/scr-004/scr-004_change-role-confirmation_success.png) | Đổi vai thành công: cả hai hộp thoại đã đóng, danh sách tải lại và hiện thông báo đổi vai thành công |
| [scr-004_change-role-confirmation_no_change](../../mockups/scr-004/scr-004_change-role-confirmation_no_change.png) | Vai mới trùng vai đang có: cả hai hộp thoại đã đóng, danh sách tải lại và hiện thông báo "Không có thay đổi" |
| [scr-004_change-role-confirmation_try-again](../../mockups/scr-004/scr-004_change-role-confirmation_try-again.png) | Đổi không thành công, hệ thống xác nhận chưa có gì thay đổi: nút "Quay lại" và "Thử lại" |
| [scr-004_change-role-confirmation_rejected](../../mockups/scr-004/scr-004_change-role-confirmation_rejected.png) | Hệ thống từ chối vì một quy tắc bảo vệ. Frame vẽ trường hợp "vai đã chọn không còn dùng được"; các trường hợp khác cùng bố cục, chỉ khác chữ và nút — xem `SCR004-ST-06` |
| [scr-004_change-role-confirmation_timeout](../../mockups/scr-004/scr-004_change-role-confirmation_timeout.png) | Không nhận được phản hồi, không biết vai đã đổi hay chưa: chỉ còn nút "Đóng và tải lại" |
| [scr-002_session-expired](../../mockups/scr-002/scr-002_session-expired.png) | Phiên đăng nhập hết hạn hoặc không hợp lệ khi xác nhận: cả hai hộp thoại đã đóng, hệ thống chuyển về trang đăng nhập và hiện thông báo yêu cầu đăng nhập lại |

## 3. Mô tả thành phần

Mô tả dựa trên frame [scr-004_sample](../../mockups/scr-004/scr-004_sample.png). #9 đến #11 lấy từ các frame [try-again](../../mockups/scr-004/scr-004_change-role-confirmation_try-again.png), [rejected](../../mockups/scr-004/scr-004_change-role-confirmation_rejected.png) và [timeout](../../mockups/scr-004/scr-004_change-role-confirmation_timeout.png).

Hộp thoại này **không có ô nhập nào**. Mọi thông tin mang sang từ `SCR-003` và chỉ để đọc. Muốn sửa thì bấm "Quay lại".

| # | Tên | Loại | Bắt buộc | Mặc định | Rule / Validation | Ghi chú |
|---|---|---|---|---|---|---|
| 1 | Tiêu đề | Label | – | "Xác nhận thay đổi vai trò", "Kiểm tra lại thông tin trước khi xác nhận" | Đổi theo trạng thái | - |
| 2 | Nút đóng ✕ | Button, chỉ có biểu tượng | – | Bật | Có chức năng giống nút thoát của trạng thái đang hiện: "Quay lại" (#7) hoặc "Đóng và tải lại" (#11). **Ẩn** khi đang xử lý và khi không rõ kết quả | Phím Esc và bấm ra ngoài hộp thoại có chức năng giống ✕ |
| 3 | Tài khoản đích | Label | – | – | Chỉ đọc: tên (in đậm) và email | Luôn hiện ở mọi trạng thái |
| 4 | Vai cũ → vai mới | Hai nhãn màu, nối bằng mũi tên › | – | – | Luôn hiện **đủ cả hai vai**, không rút gọn còn mỗi vai mới. Vai cũ nhãn xám, vai mới nhãn xanh | - |
| 5 | Lý do | Label + khung chỉ đọc | – | – | Hiện đầy đủ lý do đã bỏ khoảng trắng đầu cuối — đúng nội dung sẽ được lưu. Không cắt bớt, tự xuống dòng | Chỉ hiện ở trạng thái Sẵn sàng và Đang xử lý |
| 6 | Dòng ghi chú | Label, có biểu tượng ( i ) | – | "Vai trò mới có hiệu lực từ lần đăng nhập tiếp theo." | Khi đang xử lý đổi thành "Đang gửi yêu cầu — vui lòng không đóng cửa sổ này.". Ẩn khi đã có kết quả | Người bị đổi vai vẫn dùng quyền cũ cho tới khi đăng nhập lại (`US013-SC-16`) |
| 7 | Nút "Quay lại" | Button | – | Bật | Có ở trạng thái Sẵn sàng, Đổi không thành công, và một số trường hợp bị từ chối. Bị khoá khi đang xử lý | Về `SCR-003`, giữ lý do đã nhập |
| 8 | Nút "Xác nhận" | Button chính | – | Bật | Bấm là đổi vai ngay. Trong lúc xử lý đổi chữ thành "Đang xử lý..." và bị khoá | Chỉ có ở trạng thái Sẵn sàng và Đang xử lý |
| 9 | Khung kết quả | Khung thông báo | – | Ẩn | Đổi vai trò không thành công hoặc bị từ chối: hiện khung viền đỏ, biểu tượng danger. Không rõ kết quả: khung viền xám, nền trắng | Nằm ngay dưới #1. Nội dung theo từng trạng thái ở mục 5 |
| 10 | Nút "Thử lại" | Button chính | – | Ẩn | Chỉ có khi đổi vai trò không thành công | – |
| 11 | Nút "Đóng và tải lại" | Button chính | – | Ẩn | Có khi không rõ kết quả và ở phần lớn các trường hợp bị từ chối | Đóng cả hai hộp thoại, về `SCR-002` và tải lại danh sách |

## 4. Hành động và điều hướng

| Hành động | Điều kiện | Kết quả |
|---|---|---|
| Bấm "Xác nhận" (#8) | Ở trạng thái Sẵn sàng | Gửi yêu cầu đổi vai trò → `SCR004-ST-02`. Hệ thống kiểm tra lại mọi điều kiện, nếu thỏa mãn mới được phép đổi vai trò. Kết quả được chia làm 2 trường hợp: đổi vai trò thành công và không đổi vai trò gì|
| Bấm "Xác nhận" thêm lần nữa | Đang xử lý | Không có tác dụng, nút đang bị khoá (`US013-SC-17`) |
| Bấm "Quay lại" (#7), ✕, Esc hoặc bấm ra ngoài | Ở trạng thái Sẵn sàng, Đổi không thành công, hoặc trường hợp bị từ chối có nút "Quay lại" | Về `SCR-003`: giữ lý do đã nhập, tải lại danh sách vai để chọn lại nếu cần |
| Bấm "Quay lại", ✕, Esc hoặc bấm ra ngoài | Đang xử lý, hoặc không rõ kết quả | Không có tác dụng |
| Bấm "Thử lại" (#10) | Đổi không thành công | Gửi lại đúng quyết định cũ → `SCR004-ST-02` |
| Bấm "Đóng và tải lại" (#11) | Không rõ kết quả, hoặc trường hợp bị từ chối có nút này | Đóng cả hai hộp thoại, về `SCR-002` và tải lại danh sách với đúng bộ lọc đang dùng |
| Hệ thống báo đã đổi vai | Đang xử lý | → `SCR004-ST-03` |
| Hệ thống báo vai mới trùng vai đang có | Đang xử lý | → `SCR004-ST-04` |
| Hệ thống báo lỗi và xác nhận chưa có gì thay đổi | Đang xử lý | → `SCR004-ST-05` |
| Hệ thống từ chối vì một quy tắc bảo vệ | Đang xử lý | → `SCR004-ST-06`, nội dung theo lý do từ chối |
| Hệ thống từ chối vì phiên đăng nhập đã hết hạn hoặc không hợp lệ | Đang xử lý | → `SCR004-ST-08` |
| Không nhận được phản hồi, hoặc hệ thống báo lỗi mà không xác nhận được là chưa có gì thay đổi | Đang xử lý | → `SCR004-ST-07` |

Ngoài việc khoá nút trong lúc xử lý, hệ thống phải tự đảm bảo mỗi quyết định chỉ được thực hiện một lần.

## 5. Trạng thái màn hình

### 5.1. Tổng quan trạng thái

| Mã | Trạng thái | Điều kiện xảy ra | Hiển thị | Hành động cho phép | Frame |
|---|---|---|---|---|---|
| `SCR004-ST-01` | Sẵn sàng | Vừa mở từ "Tiếp tục" của `SCR-003` | Tiêu đề mặc định; ba thông tin #3, #4, #5; ghi chú "Vai trò mới có hiệu lực từ lần đăng nhập tiếp theo."; nút "Quay lại" và "Xác nhận" | "Xác nhận"; "Quay lại", ✕, Esc, bấm ra ngoài | [ready](../../mockups/scr-004/scr-004_change-role-confirmation_ready.png) |
| `SCR004-ST-02` | Đang xử lý | Từ lúc bấm "Xác nhận" hoặc "Thử lại" tới khi có kết quả | Tiêu đề "Đang thay đổi vai trò"; giữ ba thông tin; ghi chú "Đang gửi yêu cầu — vui lòng không đóng cửa sổ này."; "Quay lại" và "Đang xử lý..." bị khoá; không có ✕ | Không; chờ kết quả | [pending](../../mockups/scr-004/scr-004_change-role-confirmation_pending.png) |
| `SCR004-ST-03` | Thành công | Hệ thống đã đổi vai | Đóng cả hai hộp thoại, về `SCR-002`, tải lại danh sách và hiện thông báo với tiêu đề "Đã thay đổi vai trò cho {tên tài khoản}" và nội dung "Vai trò mới: {vai trò mới}" | – | [success](../../mockups/scr-004/scr-004_change-role-confirmation_success.png) |
| `SCR004-ST-04` | Không có gì thay đổi | Vai mới trùng vai tài khoản đang có | Đóng cả hai hộp thoại, về `SCR-002`, tải lại danh sách và hiện thông báo "Không có thay đổi" | – | [no_change](../../mockups/scr-004/scr-004_change-role-confirmation_no_change.png) |
| `SCR004-ST-05` | Đổi không thành công | Hệ thống gặp lỗi khi đổi và xác nhận đã huỷ toàn bộ, chưa có gì thay đổi | Tiêu đề "Thay đổi vai trò thất bại"; khung đỏ "Không thể thay đổi vai trò", "Hệ thống chưa ghi nhận thay đổi nào. Vui lòng thử lại."; giữ tài khoản đích và vai cũ → vai mới, ẩn lý do | "Thử lại"; "Quay lại", ✕, Esc, bấm ra ngoài | [try-again](../../mockups/scr-004/scr-004_change-role-confirmation_try-again.png) |
| `SCR004-ST-06` | Bị từ chối | Hệ thống từ chối trước khi đổi, vì một quy tắc bảo vệ | Tiêu đề "Không thể thay đổi vai trò"; khung đỏ nêu **một** lý do đúng với trường hợp; giữ tài khoản đích và vai cũ → vai mới, ẩn lý do; **một** nút theo bảng ở 5.2 | "Quay lại" hoặc "Đóng và tải lại", tuỳ trường hợp | [rejected](../../mockups/scr-004/scr-004_change-role-confirmation_rejected.png) |
| `SCR004-ST-07` | Không rõ kết quả | Không nhận được phản hồi, hoặc hệ thống báo lỗi mà không xác nhận được là chưa có gì thay đổi | Tiêu đề "Không xác định được kết quả"; khung xám "Không nhận được phản hồi từ hệ thống", "Thay đổi có thể đã được áp dụng hoặc chưa. Hãy tải lại danh sách trước khi thử lại."; giữ tài khoản đích và vai cũ → vai mới, ẩn lý do; không có ✕ | Chỉ "Đóng và tải lại" | [timeout](../../mockups/scr-004/scr-004_change-role-confirmation_timeout.png) |
| `SCR004-ST-08` | Phiên đăng nhập không hợp lệ | Hệ thống từ chối lệnh vì phiên đăng nhập đã hết hạn hoặc không hợp lệ, trước khi kiểm tra quyền và điều kiện bảo vệ | Đóng cả hai hộp thoại, chuyển thẳng về trang đăng nhập; trang đăng nhập hiện "Phiên đăng nhập đã hết hạn", "Vui lòng đăng nhập lại." Vai của tài khoản đích không đổi | Đăng nhập lại | [session-expired](../../mockups/scr-002/scr-002_session-expired.png) |

### 5.2. Đặc tả chi tiết trạng thái

**`SCR004-ST-02` — Đang xử lý**

- **Khi nào xảy ra:** quản trị viên bấm "Xác nhận", hoặc bấm "Thử lại" sau khi đổi không thành công.
- **Người dùng thấy gì:** tiêu đề "Đang thay đổi vai trò"; ba thông tin vẫn hiện; nút chính ghi "Đang xử lý...", cả hai nút bị khoá; nút ✕ biến mất.
- **Người dùng làm gì tiếp:** chờ. Không có cách nào đóng hộp thoại cho tới khi có kết quả.
- **Ràng buộc:** mỗi quyết định chỉ được gửi một lần (`US013-SC-17`). Không cho đóng giữa chừng.

**`SCR004-ST-05` — Đổi không thành công**

- **Khi nào xảy ra:** hệ thống gặp lỗi trong lúc đổi, nhưng **xác nhận được** là đã huỷ toàn bộ — vai của tài khoản vẫn như cũ.
- **Người dùng thấy gì:** tiêu đề "Thay đổi vai trò thất bại"; khung đỏ "Không thể thay đổi vai trò", "Hệ thống chưa ghi nhận thay đổi nào. Vui lòng thử lại."; tài khoản đích và vai cũ → vai mới vẫn hiện, lý do được ẩn.
- **Người dùng làm gì tiếp:** bấm "Thử lại" để gửi lại đúng quyết định cũ, hoặc "Quay lại" để sửa.
- **Ràng buộc:** vai của tài khoản đích vẫn là vai cũ ở mọi nơi; người bị đổi vai không nhận thông báo nào (`US013-SC-13`). Chỉ khi hệ thống xác nhận được là chưa có gì thay đổi thì mới vào trạng thái này. Nếu không chắc, phải vào `SCR004-ST-07`.

**`SCR004-ST-06` — Bị từ chối**

- **Khi nào xảy ra:** khi kiểm tra lại lúc bấm "Xác nhận", hệ thống thấy một điều kiện không còn đúng nên từ chối, chưa đổi gì.
- **Người dùng thấy gì:** tiêu đề "Không thể thay đổi vai trò"; khung đỏ nêu đúng **một** lý do theo bảng dưới; tài khoản đích và vai cũ → vai mới vẫn hiện, lý do được ẩn; đúng **một** nút.
- **Người dùng làm gì tiếp:** nếu vấn đề nằm ở vai mới hoặc lý do thì quay lại `SCR-003` để sửa. Các trường hợp khác: tải lại danh sách rồi mới làm quyết định mới.
- **Ràng buộc:** vai của tài khoản đích không đổi; người bị đổi vai không nhận thông báo nào. Mỗi lý do phải nói đúng nguyên nhân — tài khoản đã bị vô hiệu hoá thì phải báo "cần khôi phục trước", **không** được báo là không tìm thấy tài khoản (`US013-SC-09`).

| Lý do từ chối | Tiêu đề khung | Nội dung khung | Nút | Kịch bản |
|---|---|---|---|---|
| Vai mới vừa chuyển về nháp hoặc bị ngừng dùng | "Vai trò đã chọn không còn khả dụng" | "Hãy quay lại và chọn một vai trò khác." | "Quay lại" | `US013-SC-10` |
| Lý do ngắn hơn 20 hoặc dài hơn 500 ký tự (giao diện đã chặn, nhưng hệ thống vẫn kiểm lại) | "Lý do chưa hợp lệ" | "Lý do cần dài từ 20 đến 500 ký tự." | "Quay lại" | `US013-SC-05` |
| Có người khác vừa đổi vai của tài khoản này | "Vai trò của tài khoản vừa được thay đổi" | "Vai trò của tài khoản vừa được thay đổi. Thay đổi chưa được áp dụng. Vui lòng tải lại danh sách." | "Đóng và tải lại" | `US013-SC-12` |
| Tài khoản đích vừa bị vô hiệu hoá | "Tài khoản hiện đang bị vô hiệu hóa" | "Tài khoản này đã bị vô hiệu hoá. Vui lòng tải lại hoặc khôi phục tài khoản trước khi đổi vai trò." | "Đóng và tải lại" | `US013-SC-09` |
| Tài khoản đích không còn tồn tại | "Không tìm thấy tài khoản" | "Tài khoản này không còn tồn tại. Vui lòng tải lại danh sách." | "Đóng và tải lại" | `US013-SC-09` |
| Tài khoản đích là chính người đang thao tác | "Tài khoản đích không hợp lệ" | "Không thể thay đổi quyền hoặc vô hiệu hoá chính tài khoản của bạn." | "Đóng và tải lại" | `US013-SC-08` |
| Tài khoản đích là superAdmin | "Tài khoản đích không hợp lệ" | "Tài khoản superAdmin được bảo vệ và không thể bị thay đổi bởi tài khoản khác." | "Đóng và tải lại" | `US013-SC-08` |

**`SCR004-ST-07` — Không rõ kết quả**

- **Khi nào xảy ra:** yêu cầu đã được gửi đi nhưng không nhận được phản hồi — ví dụ mất mạng, chờ quá lâu — hoặc hệ thống báo lỗi mà không xác nhận được là chưa có gì thay đổi.
- **Người dùng thấy gì:** tiêu đề "Không xác định được kết quả"; khung viền xám "Không nhận được phản hồi từ hệ thống", "Thay đổi có thể đã được áp dụng hoặc chưa. Hãy tải lại danh sách trước khi thử lại."; tài khoản đích và vai cũ → vai mới vẫn hiện để so với danh sách, lý do được ẩn; chỉ còn nút "Đóng và tải lại"; nút ✕ biến mất.
- **Người dùng làm gì tiếp:** bấm "Đóng và tải lại", xem vai thật của tài khoản trên danh sách, rồi mới quyết định có cần đổi lại hay không.
- **Ràng buộc:** hệ thống không tự gửi lại, và không được báo là thất bại (`US013-SC-14`). "Xác nhận", "Quay lại", ✕, Esc và bấm ra ngoài đều bị khoá.



