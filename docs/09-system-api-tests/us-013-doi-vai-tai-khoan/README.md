# `US-013` — Đổi vai cho một tài khoản

Thư mục này gồm các test scenario kiểm thử hệ thống và API cho [`US-013`](../../06-user-stories/us-013-doi-vai-tai-khoan.md).
Mỗi file là một test scenario; mỗi test scenario gồm nhiều test case.

**Liên quan:** màn hình [`SCR-002`](../../07-screen-specs/scr-002-quan-tri-tai-khoan.md), [`SCR-003`](../../07-screen-specs/scr-003-hop-thoai-doi-vai.md), [`SCR-004`](../../07-screen-specs/scr-004-xac-nhan-doi-vai.md); endpoint `GET /roles/assignable`, `PATCH /users/{id}/role`, `GET /users` theo [đặc tả API](../../08-api-spec-extract.md); [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md); [mục lục](../README.md)

| Mã | Test scenario | Nội dung kiểm tra | Số test case | Scenario user story |
|---|---|---|---|---|
| [`TS-013-01`](ts-013-01-mo-hop-thoai-va-danh-muc-vai-tro.md) | Mở hộp thoại và danh mục vai trò | Hộp thoại đổi vai trò mở cho đúng tài khoản đích, danh mục vai trò chỉ gồm vai trò được phép gán, và hộp thoại không cho tiếp tục khi danh mục tải lỗi hoặc rỗng. | 6 | `US013-SC-02`, `US013-SC-03`, `US013-SC-04`, Đặc tả API |
| [`TS-013-02`](ts-013-02-kiem-tra-ly-do.md) | Kiểm tra lý do đổi vai trò | Giao diện và máy chủ áp dụng cùng ràng buộc lý do dài từ 20 đến 500 ký tự sau khi bỏ khoảng trắng đầu cuối. | 8 | `US013-SC-05`, `US013-SC-06`, `US013-SC-07` |
| [`TS-013-03`](ts-013-03-xac-nhan-va-doi-vai-tro-thanh-cong.md) | Xác nhận và đổi vai trò thành công | Hộp xác nhận hiển thị đúng quyết định; lệnh hợp lệ đổi vai trò trọn vẹn, ghi nhật ký và gửi thông báo; quyền mới có hiệu lực đúng thời điểm và dữ liệu sở hữu không bị ảnh hưởng. | 8 | `US013-SC-07`, `US013-SC-01`, `US013-SC-11`, `US013-SC-15`, `US013-SC-16`, `NFR-P-06` |
| [`TS-013-04`](ts-013-04-tu-choi-theo-quy-tac-bao-ve.md) | Từ chối theo quy tắc bảo vệ | Máy chủ kiểm tra lại toàn bộ điều kiện bảo vệ tại thời điểm ghi, từ chối đúng nguyên nhân, và không thay đổi dữ liệu khi tài khoản đích hoặc vai trò mới không hợp lệ. | 8 | `US013-SC-08`, `US013-SC-09`, `US013-SC-10`, `US013-SC-20` |
| [`TS-013-05`](ts-013-05-dong-thoi-va-gui-lap.md) | Thao tác đồng thời và gửi lặp | Hệ thống không ghi đè quyết định mới hơn bằng thông tin cũ, và mỗi quyết định chỉ được gửi một lần. | 3 | `US013-SC-12`, `US013-SC-17` |
| [`TS-013-06`](ts-013-06-loi-he-thong-va-mat-phan-hoi.md) | Lỗi hệ thống và mất phản hồi | Khi máy chủ lỗi trong lúc ghi, không có thay đổi một phần; khi không xác định được kết quả, giao diện không tự gửi lại và không báo nhầm là thất bại. | 2 | `US013-SC-13`, `US013-SC-14` |
| [`TS-013-07`](ts-013-07-phien-dang-nhap-va-xac-thuc.md) | Phiên đăng nhập và xác thực | Khi phiên đăng nhập không hợp lệ hoặc tài khoản thiếu quyền, máy chủ từ chối trước khi đọc hoặc ghi dữ liệu, và giao diện chuyển người dùng về trang đăng nhập khi phiên hết hạn. | 4 | `US013-SC-18`, `US013-SC-19`, Đặc tả API |
| **Tổng** | | | **39** | |
