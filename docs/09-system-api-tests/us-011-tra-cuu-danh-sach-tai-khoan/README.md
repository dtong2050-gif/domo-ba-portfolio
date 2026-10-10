# `US-011` — Tra cứu danh sách tài khoản

Thư mục này gồm các test scenario kiểm thử hệ thống và API cho [`US-011`](../../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md).
Mỗi file là một test scenario; mỗi test scenario gồm nhiều test case.

**Liên quan:** màn hình [`SCR-002`](../../07-screen-specs/scr-002-quan-tri-tai-khoan.md); endpoint `GET /users` theo [đặc tả API](../../08-api-spec-extract.md); [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md); [mục lục](../README.md)

| Mã | Test scenario | Nội dung kiểm tra | Số test case | Scenario user story |
|---|---|---|---|---|
| [`TS-011-01`](ts-011-01-hien-thi-danh-sach-mac-dinh.md) | Hiển thị danh sách mặc định | Khi mở màn hình Tài khoản, danh sách hiển thị đúng cột, thứ tự và số dòng mặc định, và phản hồi của máy chủ không chứa thông tin cá nhân nhạy cảm. | 2 | `US011-SC-01` |
| [`TS-011-02`](ts-011-02-tim-kiem-va-loc.md) | Tìm kiếm và lọc | Từ khoá, bộ lọc vai trò và bộ lọc trạng thái trả đúng tập kết quả, được áp dụng trên toàn bộ dữ liệu trước khi chia trang. | 6 | `US011-SC-02`, `US011-SC-03`, `US011-SC-05` |
| [`TS-011-03`](ts-011-03-sap-xep-va-phan-trang.md) | Sắp xếp và phân trang | Thứ tự sắp xếp ổn định giữa các lần tra cứu, số dòng mỗi trang chỉ nhận giá trị hợp lệ, và danh sách quay về trang 1 khi điều kiện tra cứu thay đổi. | 5 | `US011-SC-04`, `US011-SC-06`, `US011-SC-07`, `US011-SC-09` |
| [`TS-011-04`](ts-011-04-bo-loc-cot-hien-thi-va-lam-moi.md) | Bộ lọc, cột hiển thị và làm mới | Thao tác ẩn hiện khu vực bộ lọc và cột hiển thị không làm mất điều kiện tra cứu hay tải lại dữ liệu; nút làm mới tra cứu lại đúng điều kiện đang áp dụng. | 3 | `US011-SC-10`, `US011-SC-11`, `US011-SC-12` |
| [`TS-011-05`](ts-011-05-trang-thai-rong-va-loi-tai.md) | Trạng thái rỗng và lỗi tải | Màn hình thông báo rõ khi không có kết quả hoặc không lấy được danh sách, và không hiển thị dữ liệu cũ như kết quả mới. | 2 | `US011-SC-13`, `US011-SC-14` |
| [`TS-011-06`](ts-011-06-kiem-tra-tham-so-tai-may-chu.md) | Kiểm tra tham số tra cứu tại máy chủ | Máy chủ chỉ nhận tham số tra cứu hợp lệ khi request được gửi trực tiếp, không đi qua giao diện, và không tự đổi giá trị sai về giá trị mặc định. | 4 | `US011-SC-08` |
| [`TS-011-07`](ts-011-07-phan-quyen-va-phien-dang-nhap.md) | Phân quyền và phiên đăng nhập | Chỉ tài khoản có quyền quản trị tài khoản và phiên đăng nhập hợp lệ mới xem được danh sách; màn hình phân biệt rõ thiếu quyền với phiên hết hạn. | 4 | `US011-SC-15`, `US011-SC-16`, `US011-SC-17` |
| **Tổng** | | | **26** | |
