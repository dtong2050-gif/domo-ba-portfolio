# Kiểm thử hệ thống và API

Bộ tài liệu này quy định các test scenario và test case kiểm thử hệ thống và API cho chức năng tra cứu tài khoản, chọn tài khoản cho thao tác quản trị và đổi vai trò.
Mỗi user story có một thư mục riêng; mỗi file trong thư mục là một test scenario gồm nhiều test case, và mỗi test case truy vết về scenario của user story mà nó kiểm chứng.
Toàn bộ test case phải đạt trước khi bắt đầu [kiểm thử chấp nhận người dùng](../09-uat-scenarios.md).

## 1. Phạm vi

| Mục | Nội dung |
|---|---|
| User story | [`US-011`](../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md) Tra cứu danh sách tài khoản<br>[`US-012`](../06-user-stories/us-012-chon-tai-khoan-cho-thao-tac-quan-tri.md) Chọn tài khoản cho thao tác quản trị<br>[`US-013`](../06-user-stories/us-013-doi-vai-tai-khoan.md) Đổi vai cho một tài khoản |
| Màn hình | [`SCR-002`](../07-screen-specs/scr-002-quan-tri-tai-khoan.md) Màn quản trị tài khoản<br>[`SCR-003`](../07-screen-specs/scr-003-hop-thoai-doi-vai.md) Hộp thoại đổi vai trò<br>[`SCR-004`](../07-screen-specs/scr-004-xac-nhan-doi-vai.md) Hộp thoại xác nhận đổi vai |
| Endpoint | `GET /users`, `POST /users/selection-previews`, `GET /roles/assignable`, `PATCH /users/{id}/role`, theo [đặc tả API](../08-api-spec-extract.md) |
| Yêu cầu phi chức năng | `NFR-P-06`: nhật ký thao tác không chứa trường định danh nhạy cảm |
| Ngoài phạm vi | Thực thi lệnh vô hiệu hoá và khôi phục tài khoản<br>`NFR-P-05`: số thao tác và thời gian đổi vai trò, được đo trong [`UAT-021`](../09-uat-scenarios.md#uat-021--cấp-quyền-quản-trị-cho-nhân-sự-vận-hành-mới) |

## 2. Quy ước

| Mục | Quy ước |
|---|---|
| Mã test scenario | `TS-<mã US>-<số thứ tự>`, ví dụ `TS-013-02` |
| Mã test case | `TC-<mã US>-<số thứ tự scenario>-<số thứ tự test case>`, ví dụ `TC-013-02-03` |
| Test type | **Positive:** thao tác và dữ liệu hợp lệ; hệ thống phải thực hiện đúng<br>**Negative:** thao tác hoặc dữ liệu không hợp lệ, không được phép, hoặc hệ thống gặp sự cố; hệ thống phải từ chối hoặc xử lý an toàn<br>**Boundary:** giá trị nằm tại hoặc sát biên của khoảng hợp lệ |
| Priority | Theo mức ưu tiên của scenario user story được truy vết: Cao là High, Trung bình là Medium, Thấp là Low. Test case truy vết nhiều scenario lấy mức cao nhất. Test case chưa có scenario tương ứng do QA xác định |
| Status | **Pass:** kết quả thực tế khớp với Expected result<br>**Failed:** có ít nhất một sai khác so với Expected result<br>Để trống khi chưa thực hiện |
| US | Mã scenario của user story mà test case kiểm chứng. Test case kiểm tra quy ước trong đặc tả API nhưng chưa có scenario tương ứng ghi "Đặc tả API" |
| Test case nhiều dòng dữ liệu | Thực hiện Steps lần lượt với từng dòng trong Test data. Test case có Status Pass khi mọi dòng dữ liệu đạt |
| Actual result | Ghi khi thực hiện test case. Với test case nhiều dòng dữ liệu, ghi kết quả theo từng dòng |
| Bug | Mã phiếu lỗi trên công cụ quản lý lỗi và mô tả ngắn. Chỉ ghi khi Status là Failed |
| Bộ dữ liệu | `DS-xx` là bộ dữ liệu dùng chung, quy định tại [môi trường và dữ liệu](00-moi-truong-va-du-lieu.md#6-bộ-dữ-liệu-dùng-chung) |

## 3. Danh sách test scenario

Môi trường, công cụ, tài khoản, vai trò, biến Postman và bộ dữ liệu dùng chung được quy định tại [môi trường và dữ liệu](00-moi-truong-va-du-lieu.md).

| User story | Mã | Test scenario | Số test case |
|---|---|---|---|
| [`US-011`](us-011-tra-cuu-danh-sach-tai-khoan/README.md) | [`TS-011-01`](us-011-tra-cuu-danh-sach-tai-khoan/ts-011-01-hien-thi-danh-sach-mac-dinh.md) | Hiển thị danh sách mặc định | 2 |
| [`US-011`](us-011-tra-cuu-danh-sach-tai-khoan/README.md) | [`TS-011-02`](us-011-tra-cuu-danh-sach-tai-khoan/ts-011-02-tim-kiem-va-loc.md) | Tìm kiếm và lọc | 6 |
| [`US-011`](us-011-tra-cuu-danh-sach-tai-khoan/README.md) | [`TS-011-03`](us-011-tra-cuu-danh-sach-tai-khoan/ts-011-03-sap-xep-va-phan-trang.md) | Sắp xếp và phân trang | 5 |
| [`US-011`](us-011-tra-cuu-danh-sach-tai-khoan/README.md) | [`TS-011-04`](us-011-tra-cuu-danh-sach-tai-khoan/ts-011-04-bo-loc-cot-hien-thi-va-lam-moi.md) | Bộ lọc, cột hiển thị và làm mới | 3 |
| [`US-011`](us-011-tra-cuu-danh-sach-tai-khoan/README.md) | [`TS-011-05`](us-011-tra-cuu-danh-sach-tai-khoan/ts-011-05-trang-thai-rong-va-loi-tai.md) | Trạng thái rỗng và lỗi tải | 2 |
| [`US-011`](us-011-tra-cuu-danh-sach-tai-khoan/README.md) | [`TS-011-06`](us-011-tra-cuu-danh-sach-tai-khoan/ts-011-06-kiem-tra-tham-so-tai-may-chu.md) | Kiểm tra tham số tra cứu tại máy chủ | 4 |
| [`US-011`](us-011-tra-cuu-danh-sach-tai-khoan/README.md) | [`TS-011-07`](us-011-tra-cuu-danh-sach-tai-khoan/ts-011-07-phan-quyen-va-phien-dang-nhap.md) | Phân quyền và phiên đăng nhập | 4 |
| [`US-012`](us-012-chon-tai-khoan-cho-thao-tac-quan-tri/README.md) | [`TS-012-01`](us-012-chon-tai-khoan-cho-thao-tac-quan-tri/ts-012-01-menu-thao-tac-theo-loai-tai-khoan.md) | Menu thao tác theo loại tài khoản | 6 |
| [`US-012`](us-012-chon-tai-khoan-cho-thao-tac-quan-tri/README.md) | [`TS-012-02`](us-012-chon-tai-khoan-cho-thao-tac-quan-tri/ts-012-02-che-do-chon-nhieu-dong.md) | Chế độ chọn nhiều dòng | 7 |
| [`US-012`](us-012-chon-tai-khoan-cho-thao-tac-quan-tri/README.md) | [`TS-012-03`](us-012-chon-tai-khoan-cho-thao-tac-quan-tri/ts-012-03-giu-va-xoa-danh-sach-chon.md) | Giữ và xoá danh sách chọn | 3 |
| [`US-012`](us-012-chon-tai-khoan-cho-thao-tac-quan-tri/README.md) | [`TS-012-04`](us-012-chon-tai-khoan-cho-thao-tac-quan-tri/ts-012-04-mo-lenh-hang-loat-va-chot-danh-sach.md) | Mở lệnh hàng loạt và chốt danh sách tài khoản đích | 7 |
| [`US-013`](us-013-doi-vai-tai-khoan/README.md) | [`TS-013-01`](us-013-doi-vai-tai-khoan/ts-013-01-mo-hop-thoai-va-danh-muc-vai-tro.md) | Mở hộp thoại và danh mục vai trò | 6 |
| [`US-013`](us-013-doi-vai-tai-khoan/README.md) | [`TS-013-02`](us-013-doi-vai-tai-khoan/ts-013-02-kiem-tra-ly-do.md) | Kiểm tra lý do đổi vai trò | 8 |
| [`US-013`](us-013-doi-vai-tai-khoan/README.md) | [`TS-013-03`](us-013-doi-vai-tai-khoan/ts-013-03-xac-nhan-va-doi-vai-tro-thanh-cong.md) | Xác nhận và đổi vai trò thành công | 8 |
| [`US-013`](us-013-doi-vai-tai-khoan/README.md) | [`TS-013-04`](us-013-doi-vai-tai-khoan/ts-013-04-tu-choi-theo-quy-tac-bao-ve.md) | Từ chối theo quy tắc bảo vệ | 8 |
| [`US-013`](us-013-doi-vai-tai-khoan/README.md) | [`TS-013-05`](us-013-doi-vai-tai-khoan/ts-013-05-dong-thoi-va-gui-lap.md) | Thao tác đồng thời và gửi lặp | 3 |
| [`US-013`](us-013-doi-vai-tai-khoan/README.md) | [`TS-013-06`](us-013-doi-vai-tai-khoan/ts-013-06-loi-he-thong-va-mat-phan-hoi.md) | Lỗi hệ thống và mất phản hồi | 2 |
| [`US-013`](us-013-doi-vai-tai-khoan/README.md) | [`TS-013-07`](us-013-doi-vai-tai-khoan/ts-013-07-phien-dang-nhap-va-xac-thuc.md) | Phiên đăng nhập và xác thực | 4 |

## 4. Độ phủ scenario user story

| User story | Số scenario | Scenario có test case | Số test scenario | Số test case |
|---|---|---|---|---|
| [`US-011`](us-011-tra-cuu-danh-sach-tai-khoan/README.md) | 17 | 17 | 7 | 26 |
| [`US-012`](us-012-chon-tai-khoan-cho-thao-tac-quan-tri/README.md) | 13 | 13 | 4 | 23 |
| [`US-013`](us-013-doi-vai-tai-khoan/README.md) | 20 | 20 | 7 | 39 |
| **Tổng** | **50** | **50** | **18** | **88** |

## 5. Truy vết từ scenario user story tới test case

| Scenario | Test case |
|---|---|
| `US011-SC-01` | `TC-011-01-01`, `TC-011-01-02` |
| `US011-SC-02` | `TC-011-02-01`, `TC-011-02-02`, `TC-011-02-03` |
| `US011-SC-03` | `TC-011-02-04` |
| `US011-SC-04` | `TC-011-03-01`, `TC-011-03-02` |
| `US011-SC-05` | `TC-011-02-05`, `TC-011-02-06` |
| `US011-SC-06` | `TC-011-03-03` |
| `US011-SC-07` | `TC-011-03-04` |
| `US011-SC-08` | `TC-011-06-01`, `TC-011-06-02`, `TC-011-06-03`, `TC-011-06-04` |
| `US011-SC-09` | `TC-011-03-05` |
| `US011-SC-10` | `TC-011-04-01` |
| `US011-SC-11` | `TC-011-04-02` |
| `US011-SC-12` | `TC-011-04-03` |
| `US011-SC-13` | `TC-011-05-01` |
| `US011-SC-14` | `TC-011-05-02` |
| `US011-SC-15` | `TC-011-07-01`, `TC-011-07-02` |
| `US011-SC-16` | `TC-011-07-03` |
| `US011-SC-17` | `TC-011-07-04` |
| `US012-SC-01` | `TC-012-01-01`, `TC-012-01-06` |
| `US012-SC-02` | `TC-012-01-02`, `TC-012-01-06` |
| `US012-SC-03` | `TC-012-01-03`, `TC-012-01-04`, `TC-012-01-06`, `TC-012-04-04` |
| `US012-SC-04` | `TC-012-02-01` |
| `US012-SC-05` | `TC-012-02-02`, `TC-012-02-03`, `TC-012-04-03` |
| `US012-SC-06` | `TC-012-04-01`, `TC-012-04-02`, `TC-012-04-03` |
| `US012-SC-07` | `TC-012-03-01` |
| `US012-SC-08` | `TC-012-02-04` |
| `US012-SC-09` | `TC-012-02-05` |
| `US012-SC-10` | `TC-012-02-06` |
| `US012-SC-11` | `TC-012-03-02` |
| `US012-SC-12` | `TC-012-01-05`, `TC-012-02-07`, `TC-012-04-04` |
| `US012-SC-13` | `TC-012-03-03` |
| `US013-SC-01` | `TC-013-03-02`, `TC-013-03-03` |
| `US013-SC-02` | `TC-013-01-01` |
| `US013-SC-03` | `TC-013-01-02`, `TC-013-01-03` |
| `US013-SC-04` | `TC-013-01-04`, `TC-013-01-05` |
| `US013-SC-05` | `TC-013-02-01`, `TC-013-02-02`, `TC-013-02-05`, `TC-013-02-06`, `TC-013-02-07` |
| `US013-SC-06` | `TC-013-02-03`, `TC-013-02-04`, `TC-013-02-08` |
| `US013-SC-07` | `TC-013-02-06`, `TC-013-03-01` |
| `US013-SC-08` | `TC-013-04-01`, `TC-013-04-02`, `TC-013-04-08` |
| `US013-SC-09` | `TC-013-04-03`, `TC-013-04-04`, `TC-013-04-08` |
| `US013-SC-10` | `TC-013-04-05`, `TC-013-04-06` |
| `US013-SC-11` | `TC-013-03-04`, `TC-013-03-05` |
| `US013-SC-12` | `TC-013-05-01`, `TC-013-05-02` |
| `US013-SC-13` | `TC-013-06-01` |
| `US013-SC-14` | `TC-013-06-02` |
| `US013-SC-15` | `TC-013-03-06` |
| `US013-SC-16` | `TC-013-03-07` |
| `US013-SC-17` | `TC-013-05-03` |
| `US013-SC-18` | `TC-013-07-01`, `TC-013-07-03` |
| `US013-SC-19` | `TC-013-07-02`, `TC-013-07-03` |
| `US013-SC-20` | `TC-013-04-07` |
| `NFR-P-06` | `TC-013-03-08` |

## 6. Test case chưa có scenario user story tương ứng

Các test case sau kiểm tra quy ước trong đặc tả API nhưng chưa có scenario tương ứng trong user story. BA bổ sung scenario vào user story liên quan để hoàn tất truy vết.

| Test case | Nội dung kiểm tra | User story cần bổ sung scenario |
|---|---|---|
| `TC-012-04-05` | Kiểm tra dữ liệu của yêu cầu chốt danh sách | `US-012` |
| `TC-012-04-06` | Giới hạn số dòng xem trước từ 1 đến 100 | `US-012` |
| `TC-012-04-07` | Xác thực và phân quyền khi chốt danh sách | `US-012` |
| `TC-013-01-06` | Kiểm tra tham số mục đích của danh mục vai trò | `US-013` |
| `TC-013-07-04` | Từ chối tài khoản thiếu quyền quản trị tài khoản khi lấy danh mục vai trò và đổi vai trò | `US-013` |

## 7. Kết quả thực thi

| Thông tin | Nội dung |
|---|---|
| Đường dẫn môi trường kiểm thử | |
| Phiên bản phần mềm | |
| Thời gian thực hiện | |
| Người thực hiện | |

| Tổng số test case | Pass | Failed | Chưa thực hiện |
|---|---|---|---|
| 88 | | | |
