# `US-012` — Chọn tài khoản cho thao tác quản trị

Thư mục này gồm các test scenario kiểm thử hệ thống và API cho [`US-012`](../../06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md).
Mỗi file là một test scenario; mỗi test scenario gồm nhiều test case.

**Liên quan:** màn hình [`SCR-002`](../../07-screen-specs/scr-002-quan-tri-tai-khoan.md); endpoint `GET /users`, `POST /users/selection-previews` theo [đặc tả API](../../08-api-spec-extract.md); [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md); [mục lục](../README.md)

| Mã | Test scenario | Nội dung kiểm tra | Số test case | Scenario user story |
|---|---|---|---|---|
| [`TS-012-01`](ts-012-01-menu-thao-tac-theo-loai-tai-khoan.md) | Menu thao tác theo loại tài khoản | Menu thao tác trên mỗi dòng chỉ chứa các lệnh được phép theo trạng thái tài khoản, theo tài khoản của chính người thao tác và theo tài khoản superAdmin. | 6 | `US012-SC-01`, `US012-SC-02`, `US012-SC-03`, `US012-SC-12` |
| [`TS-012-02`](ts-012-02-che-do-chon-nhieu-dong.md) | Chế độ chọn nhiều dòng | Chế độ chọn nhiều dòng chỉ bật khi lọc theo một trạng thái cụ thể; ô chọn tất cả và ô chọn từng dòng cập nhật đúng danh sách chọn trên mọi trang và không bao gồm tài khoản được bảo vệ. | 7 | `US012-SC-04`, `US012-SC-05`, `US012-SC-08`, `US012-SC-09`, `US012-SC-10`, `US012-SC-12` |
| [`TS-012-03`](ts-012-03-giu-va-xoa-danh-sach-chon.md) | Giữ và xoá danh sách chọn | Danh sách chọn được giữ khi chuyển trang, và bị xoá toàn bộ khi điều kiện tra cứu thay đổi hoặc khi người thao tác bỏ chọn. | 3 | `US012-SC-07`, `US012-SC-11`, `US012-SC-13` |
| [`TS-012-04`](ts-012-04-mo-lenh-hang-loat-va-chot-danh-sach.md) | Mở lệnh hàng loạt và chốt danh sách tài khoản đích | Thanh hành động mở đúng hộp thoại xác nhận với đúng danh sách đã chọn, và máy chủ chốt danh sách theo đúng quy tắc loại trừ, kiểm tra dữ liệu và xác thực. | 7 | `US012-SC-06`, `US012-SC-05`, `US012-SC-03`, `US012-SC-12`, Đặc tả API |
| **Tổng** | | | **23** | |
