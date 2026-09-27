# Nhật ký thay đổi và quyết định

Nhật ký ghi lại các yêu cầu thay đổi phạm vi và các quyết định có nhiều phương án, kèm lý do lựa chọn.
Mỗi bản ghi nêu bối cảnh, phương án đã cân nhắc, lựa chọn cuối và các hạng mục bị ảnh hưởng.
Nhờ đó người đọc sau biết quyết định dựa trên thông tin nào và thay đổi ảnh hưởng tới đâu.

## 1. Yêu cầu thay đổi

### CR-001 — Rút hiệu lực cắt phiên đang mở khỏi MVP

| Mục | Nội dung |
|---|---|
| Người đề xuất | Chủ dự án |
| Ngày đề xuất | 21/08/2026 |
| Trạng thái | Đã duyệt |

**Bối cảnh.** Bản chốt ban đầu cam kết tài khoản bị vô hiệu hoá mất quyền truy cập trong vòng 24 giờ, kể cả khi phiên đăng nhập còn hạn. Sau khi phần đăng nhập Google hoàn tất, chủ dự án mới đặt hàng chức năng vô hiệu hoá đầy đủ: màn quản trị ra lệnh, cắt phiên đang mở và email báo cho người bị vô hiệu hoá. Cả ba chưa được phân tích, và hai trong ba thuộc module khác (`M-03`, `M-14`).

**Phương án đã cân nhắc.**

| Phương án | Ưu | Nhược |
|---|---|---|
| 1. Làm trong MVP: cắt phiên phía máy chủ trong 24 giờ | Giữ nguyên cam kết; lệnh đình chỉ có hiệu lực ngay | Kéo `M-03` và `M-14` vào sớm; phải đổi cơ chế phiên đang chạy ổn định |
| 2. Hoãn tới sau MVP, giữ chặn ở cửa đăng nhập | Không đụng cơ chế phiên; tài liệu khớp với hệ thống đang chạy | Phiên đang mở của tài khoản vi phạm vẫn dùng được tới khi hết hạn, tối đa 7 ngày |
| 3. Bỏ hẳn | Không phải làm lại | Mất khả năng đình chỉ tài khoản vi phạm khi đã có người dùng thật |

**Lựa chọn.** Phương án 2. Phương án 1 đúng về nghiệp vụ nhưng cần hai module chưa tới lượt phân tích, và DOMO chưa có bộ phận hỗ trợ để vận hành việc đình chỉ theo cam kết thời gian (`C-05`). Phương án 3 bỏ mất một nhu cầu có thật.

Điều kiện kèm theo: nhu cầu quay lại backlog ngay khi MVP phát hành; trước khi mở rộng người dùng, quản lý sản phẩm phải xem lại khoảng hở 7 ngày.

**Hạng mục bị ảnh hưởng.**

| Hạng mục | Ảnh hưởng |
|---|---|
| `BR-002` | Tiêu chí thành công chỉ yêu cầu tài khoản bị đình chỉ không đăng nhập lại được, không yêu cầu cắt phiên đang mở |
| `FR-012` #5 | Vai mới cũng chỉ có hiệu lực từ lần đăng nhập kế tiếp, vì dùng chung cơ chế phiên |
| `O32` | Cắt ngay phiên đang mở khi tài khoản bị vô hiệu hoá nằm ngoài phạm vi MVP |
| [Quy trình đổi vai](04-business-processes.md#14-hiệu-lực-quyền-sau-khi-đổi-vai) | Nêu rõ khoảng lệch giữa lúc vai được ghi và lúc quyền mới có hiệu lực |

## 2. Quyết định

### BD-001 — Ba vai trò gốc chỉ được sửa bộ quyền

| Mục | Nội dung |
|---|---|
| Người quyết định | Chủ dự án |
| Ngày quyết định | 22/08/2026 |
| Trạng thái | Có hiệu lực |

**Bối cảnh.** Màn hình và API quản trị vai trò đã có sẵn nhưng chưa bảo vệ ba vai trò gốc Người dùng, Nhà tổ chức và Quản trị viên. Các luồng đăng nhập, trở thành Nhà tổ chức và gán vai trò đều dựa vào định danh của ba vai trò này, nên việc sửa tự do tên, mã định danh hay vòng đời có thể làm các luồng đó hỏng.

**Phương án đã cân nhắc.**

| Phương án | Ưu | Nhược |
|---|---|---|
| 1. Cố định mọi thuộc tính và vòng đời của ba vai trò gốc, chỉ cho sửa bộ quyền | Giữ ổn định định danh cốt lõi; vận hành vẫn điều chỉnh được trách nhiệm của từng vai trò | Phải tách lệnh cập nhật bộ quyền khỏi lệnh sửa vai trò dùng chung |
| 2. Chỉ cấm ngừng dùng, vẫn cho sửa tên, mã định danh, mô tả và trạng thái công bố | Ít thay đổi so với hệ thống đang chạy | Luồng đăng nhập và gán vai trò có thể không tìm thấy vai trò gốc |
| 3. Để quản trị vai trò ngoài phạm vi MVP | Không phải làm thêm | Chức năng đang có không có chuẩn nghiệm thu; vận hành không có cách cấu hình an toàn |

**Lựa chọn.** Phương án 1. Phương án 2 làm mất tính ổn định của vai trò gốc; phương án 3 để một chức năng đang chạy không có chuẩn nghiệm thu. Ranh giới áp dụng cho mọi người có quyền quản trị vai trò, kể cả superAdmin, và được kiểm tra tại máy chủ, không chỉ trên giao diện.

Điều kiện kèm theo: chỉ xem lại khi có yêu cầu thay đổi định danh hoặc vòng đời của ba vai trò gốc.

**Hạng mục bị ảnh hưởng.**

| Hạng mục | Ảnh hưởng |
|---|---|
| [Ràng buộc dữ liệu](05-data-dictionary.md#ràng-buộc) | Ba vai trò gốc chỉ được sửa bộ quyền |
| [Bảng `roles`](05-data-dictionary.md#bảng-roles--vai-trò) | `slug` xác định ba vai trò gốc `user`, `organizer`, `admin`; `deletedAt` không bao giờ có giá trị với ba vai trò này |

### BD-002 — Email được cấp lại cho người khác vẫn nhận tài khoản cũ

| Mục | Nội dung |
|---|---|
| Người quyết định | Chủ dự án |
| Ngày quyết định | 10/09/2026 |
| Trạng thái | Có hiệu lực |

**Bối cảnh.** Khi đăng nhập Google, hệ thống tìm tài khoản theo mã định danh Firebase trước; không thấy thì nhận lại tài khoản có cùng email đã được Google xác minh. Bước này dành cho tài khoản có từ trước khi người dùng đăng nhập bằng Google, nhưng không phân biệt được cùng một người quay lại với một người khác được cấp lại email, ví dụ email công ty bị thu hồi rồi giao cho nhân viên mới.

**Phương án đã cân nhắc.**

| Phương án | Ưu | Nhược |
|---|---|---|
| 1. Nhận nguyên tài khoản cũ | Không phải làm thêm; người đổi tài khoản Google nhưng giữ email vẫn về đúng dữ liệu của mình | Người được cấp lại email nhận luôn vai trò, lịch sử vi phạm và hồ sơ nhà tổ chức của chủ cũ |
| 2. Chặn khi email đã gắn với một mã định danh Firebase khác | Phát hiện được bằng dữ liệu sẵn có | Chặn oan người xoá rồi tạo lại tài khoản Google cùng email; cần kênh khiếu nại và thao tác gỡ chặn |
| 3. Tạo tài khoản mới theo mã định danh Firebase | Chủ mới không chạm được dữ liệu của chủ cũ | Phá quy tắc một email là một tài khoản; hai tài khoản cùng email tồn tại song song |

**Lựa chọn.** Phương án 1. Phương án 2 đổi một rủi ro hiếm lấy lỗi chặn oan thường gặp hơn, và cần người xử lý gỡ chặn trong khi DOMO chưa có bộ phận hỗ trợ (`C-05`). Phương án 3 mâu thuẫn với quy tắc một email là một tài khoản.

Điều kiện kèm theo: xem lại khi có khiếu nại thực tế về việc nhận nhầm tài khoản của người khác, hoặc số liệu cho thấy nhiều người đăng nhập bằng email tổ chức thay vì email cá nhân. Bắt buộc xem lại nếu DOMO mở phương thức đăng nhập ngoài Google.

**Hạng mục bị ảnh hưởng.**

| Hạng mục | Ảnh hưởng |
|---|---|
| [Bảng `users`](05-data-dictionary.md#bảng-users--tài-khoản), trường `email` | Một email là một tài khoản, duy nhất toàn bảng kể cả tài khoản đã vô hiệu hoá |
| [Đăng nhập bằng Google](04-business-processes.md#33-chú-giải-luồng), `A3` | Nhận lại tài khoản cùng email đã được Google xác minh, không tạo tài khoản thứ hai |
