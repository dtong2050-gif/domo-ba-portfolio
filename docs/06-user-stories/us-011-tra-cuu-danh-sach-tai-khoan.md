# `US-011` — Tra cứu danh sách tài khoản

## Phát biểu

> **Là** quản trị viên có quyền quản trị tài khoản

> **Tôi muốn** xem, tìm, lọc, sắp xếp và phân trang danh sách tài khoản

> **Để** kiểm tra quyền và xác định đúng tài khoản cần xử lý mà không tiếp cận dữ liệu ngoài phạm vi cần thiết


## Luồng trong phạm vi

| Bước | Nội dung |
|---|---|
| Bắt đầu | Quản trị viên có quyền quản trị tài khoản mở danh sách tài khoản |
| Quyết định chính | Dùng từ khoá, vai lọc, trạng thái lọc, chiều sắp xếp và phân trang để thu hẹp rồi duyệt toàn bộ tập kết quả |
| Bàn giao | Hệ thống hiển thị đúng tài khoản và thông tin tối thiểu cần cho việc kiểm tra |

## US Inventory

| Mã | Loại | Ưu tiên | Rủi ro nếu thiếu | Mockup |
|---|---|---|---|---|
| `US011-SC-01` | Happy path | Cao | Sai dữ liệu, lộ trường nhạy cảm hoặc không thấy đích | [default](../../mockups/scr-002/scr-002_default.png) |
| `US011-SC-02` | Business rule | Cao | Bỏ sót tài khoản do cách viết hoa thường khác nhau | [filter-on](../../mockups/scr-002/scr-002_filter-on.png) |
| `US011-SC-03` | Business rule | Cao | Trả thừa tài khoản không thoả đồng thời mọi điều kiện | [filter-on](../../mockups/scr-002/scr-002_filter-on.png) |
| `US011-SC-04` | Business rule | Cao | Lặp hoặc mất dòng giữa các trang | [default](../../mockups/scr-002/scr-002_default.png) |
| `US011-SC-05` | Edge case | Cao | Tìm kiếm và lọc chỉ chạy trên 100 bản ghi đầu | [default](../../mockups/scr-002/scr-002_default.png) |
| `US011-SC-06` | Validation | Trung bình | Phân trang sai với số dòng đã chọn | [menu_pagination](../../mockups/scr-002/scr-002_menu_pagination.png) |
| `US011-SC-07` | Validation | Cao | Gửi yêu cầu sai hoặc âm thầm đổi giá trị | [menu_pagination](../../mockups/scr-002/scr-002_menu_pagination.png) |
| `US011-SC-08` | Validation | Cao | Kênh truy cập khác vượt qua kiểm tra của giao diện | Không có giao diện |
| `US011-SC-09` | UX | Trung bình | Hiển thị trang rỗng hoặc dữ liệu của lần tra cứu cũ | [loading](../../mockups/scr-002/scr-002_loading.png) |
| `US011-SC-10` | UX | Thấp | Mất điều kiện đang áp hoặc tải lại trang ngoài ý muốn | [default](../../mockups/scr-002/scr-002_default.png), [filter-on](../../mockups/scr-002/scr-002_filter-on.png) |
| `US011-SC-11` | UX | Thấp | Ẩn nhầm cột bắt buộc hoặc gọi lại dữ liệu | [display-hide-column](../../mockups/scr-002/scr-002_display-hide-column.png) |
| `US011-SC-12` | UX | Trung bình | Tải sai tập dữ liệu hoặc tải lại toàn trang | [loading](../../mockups/scr-002/scr-002_loading.png) |
| `US011-SC-13` | Edge case | Trung bình | Người dùng tưởng hệ thống bị lỗi | [notfound](../../mockups/scr-002/scr-002_notfound.png) |
| `US011-SC-14` | Recovery | Cao | Dữ liệu cũ bị trình bày như kết quả mới | [error](../../mockups/scr-002/scr-002_error.png) |
| `US011-SC-15` | Authorization / Security | Cao | Lộ danh sách tài khoản qua các kênh truy cập | [access-denied](../../mockups/scr-002/scr-002_access-denied.png) |
| `US011-SC-16` | Authorization / Security | Cao | Lấy được danh sách tài khoản khi chưa đăng nhập hoặc phiên đăng nhập không hợp lệ | Không có giao diện |
| `US011-SC-17` | Authorization / Security | Cao | Báo nhầm thành không đủ quyền, người dùng không biết phải đăng nhập lại | [session-expired](../../mockups/scr-002/scr-002_session-expired.png) |

```gherkin
Feature: US-011 — Tra cứu danh sách tài khoản

  Scenario: [US011-SC-01] Mở danh sách với trạng thái mặc định
    Given tài khoản "U-001" đã đăng nhập và có quyền quản trị tài khoản
    And hệ thống có các tài khoản:
      | mã    | tên       | email              | vai trò       | trạng thái     | ngày tham gia          | là superAdmin |
      | U-001 | Admin An  | admin@example.test | Quản trị viên | Đang hoạt động | 2026-08-01T08:00+07:00 | false         |
      | U-002 | Huy Tran  | huy@example.test   | Người dùng    | Đang hoạt động | 2026-08-04T08:00+07:00 | false         |
      | U-003 | Lan Le    | lan@example.test   | Quản trị viên | Đã vô hiệu hoá | 2026-08-03T08:00+07:00 | false         |
      | U-004 | Root Domo | root@example.test  | Quản trị viên | Đang hoạt động | 2026-08-02T08:00+07:00 | true          |
    When "U-001" mở màn hình quản trị tài khoản
    Then bảng hiển thị đủ bốn dòng theo thứ tự "U-002, U-003, U-004, U-001"
    And bảng hiển thị tên và email, vai trò, trạng thái, ngày tham gia và cột thao tác
    And danh sách sắp xếp theo ngày tham gia từ mới đến cũ, mỗi trang 10 dòng
    And dòng "U-002" có nút mở menu thao tác, còn dòng superAdmin "U-004" vẫn hiển thị nhưng không có lựa chọn quản trị nào
    And dữ liệu danh sách không chứa ngày sinh, giới tính, địa chỉ, số điện thoại hoặc giới thiệu bản thân
    And không có vai trò hoặc trạng thái tài khoản nào bị thay đổi

  Scenario Outline: [US011-SC-02] Tìm theo tên hoặc email không phân biệt hoa thường
    Given hệ thống có các tài khoản:
      | mã    | tên      | email               |
      | U-101 | Huy Tran | huy@example.test    |
      | U-102 | Huy Le   | huy.le@example.test |
      | U-103 | Lan Le   | lan@example.test    |
    And quản trị viên đang ở trang đầu của màn hình quản trị tài khoản
    When quản trị viên nhập <tu_khoa> vào ô tìm kiếm
    Then danh sách chỉ hiển thị <xuat_hien>
    And không hiển thị <khong_xuat_hien>
    And kết quả tự cập nhật mà không cần bấm Enter hoặc nút "Lọc"
    And từ khoá được áp dụng trên toàn bộ dữ liệu trước khi phân trang

    Examples:
      | tu_khoa            | xuat_hien      | khong_xuat_hien |
      | "hUy"              | "U-101, U-102" | "U-103"         |
      | "HUY@EXAMPLE.TEST" | "U-101"        | "U-102, U-103"  |

  Scenario Outline: [US011-SC-03] Kết hợp đồng thời từ khoá, vai và trạng thái
    Given hệ thống có các tài khoản:
      | định_danh | tên       | email                  | vai           | trạng thái     |
      | U-201     | Huy Tran  | huy@example.test       | Người dùng    | Đang hoạt động |
      | U-202     | Huy Admin | huy.admin@example.test | Quản trị viên | Đã vô hiệu hoá |
      | U-203     | Lan User  | lan@example.test       | Người dùng    | Đã vô hiệu hoá |
      | U-204     | Hanh Tran | hanh@example.test      | Người dùng    | Đang hoạt động |
    When quản trị viên tra cứu với từ khoá <tu_khoa>, vai trò <vai> và trạng thái <trang_thai>
    Then danh sách chỉ hiển thị <xuat_hien>
    And không hiển thị <khong_xuat_hien>
    And cả ba điều kiện được áp dụng trên toàn bộ tập dữ liệu trước khi phân trang

    Examples:
      | tu_khoa | vai             | trang_thai       | xuat_hien | khong_xuat_hien       |
      | "huy"   | "Người dùng"    | "Đang hoạt động" | "U-201"   | "U-202, U-203, U-204" |
      | ""      | "Người dùng"    | "Đã vô hiệu hoá" | "U-203"   | "U-201, U-202, U-204" |
      | "huy"   | "Quản trị viên" | "Đã vô hiệu hoá" | "U-202"   | "U-201, U-203, U-204" |

  Scenario Outline: [US011-SC-04] Sắp xếp theo ngày tham gia, trùng ngày thì xếp theo mã tài khoản
    Given hệ thống có các tài khoản:
      | mã    | ngày tham gia          |
      | U-301 | 2026-08-01T08:00+07:00 |
      | U-302 | 2026-08-01T08:00+07:00 |
      | U-303 | 2026-08-02T08:00+07:00 |
    And "U-301" và "U-302" có cùng ngày tham gia, trong đó "U-301" có mã nhỏ hơn
    When quản trị viên chọn chiều sắp xếp <chieu_sap_xep>
    Then danh sách có thứ tự <thu_tu>
    And thứ tự được áp dụng trước khi phân trang
    And hai tài khoản trùng ngày tham gia được xếp theo mã tài khoản, cùng chiều với <chieu_sap_xep>
    And tra cứu lại cùng điều kiện vẫn cho ra cùng thứ tự

    Examples:
      | chieu_sap_xep | thu_tu                |
      | "Cũ đến mới"  | "U-301, U-302, U-303" |
      | "Mới đến cũ"  | "U-303, U-302, U-301" |

  Scenario Outline: [US011-SC-05] Duyệt kết quả tra cứu vượt quá 100 tài khoản
    Given hệ thống có 205 tài khoản "AN-001" đến "AN-205" mang vai "Người dùng" và trạng thái "Đang hoạt động"
    And hệ thống có 20 tài khoản "BINH-001" đến "BINH-020" mang vai "Quản trị viên" và trạng thái "Đã vô hiệu hoá"
    When quản trị viên <thao_tac_tra_cuu> với số dòng mỗi trang là 100
    Then màn hình cho biết tổng số kết quả là 205 và tổng số trang là 3
    And trang 1 có "AN-001" đến "AN-100", trang 2 có "AN-101" đến "AN-200", trang 3 có "AN-201" đến "AN-205"
    And không hiển thị tài khoản "BINH-001" đến "BINH-020"
    And không tài khoản nào bị lặp hoặc bị bỏ sót giữa ba trang

    Examples:
      | thao_tac_tra_cuu                                     |
      | nhập từ khoá "an"                                    |
      | chọn vai "Người dùng" và trạng thái "Đang hoạt động" |

  Scenario Outline: [US011-SC-06] Thay đổi số dòng trên mỗi trang
    Given danh sách có 205 tài khoản
    When quản trị viên đặt số dòng mỗi trang là <so_dong_moi_trang>
    Then màn hình quay về trang 1
    And trang 1 hiển thị đúng <so_dong> dòng
    And số dòng mỗi trang đang áp dụng là <so_dong_moi_trang>

    Examples:
      | so_dong_moi_trang | so_dong |
      | 10                | 10      |
      | 20                | 20      |
      | 50                | 50      |
      | 100               | 100     |

  Scenario: [US011-SC-07] Giao diện chỉ cho chọn số dòng mỗi trang hợp lệ
    Given màn hình quản trị tài khoản đang hiển thị một kết quả tra cứu thành công
    When quản trị viên mở danh sách chọn "Số kết quả mỗi trang"
    Then danh sách chỉ có các lựa chọn 10, 20, 50 và 100
    And giá trị mặc định là 10 khi chưa có lựa chọn trước đó
    And giao diện không cho người thao tác nhập giá trị tuỳ ý

  Scenario Outline: [US011-SC-08] Từ chối điều kiện tra cứu sai tại máy chủ
    Given yêu cầu được gửi thẳng đến server, không đi qua giao diện (UI)
    When yêu cầu đặt <dieu_kien_tra_cuu> thành <gia_tri>
    Then máy chủ từ chối yêu cầu trước khi tra cứu danh sách
    And phản hồi không chứa dữ liệu tài khoản
    And máy chủ không tự thay <gia_tri> bằng giá trị mặc định

    Examples:
      | dieu_kien_tra_cuu | gia_tri                                        |
      | số dòng mỗi trang | 1                                              |
      | số dòng mỗi trang | 25                                             |
      | số dòng mỗi trang | 101                                            |
      | số dòng mỗi trang | 10.5                                           |
      | chiều sắp xếp     | một giá trị ngoài "Cũ đến mới" và "Mới đến cũ" |

  Scenario Outline: [US011-SC-09] Quay về trang đầu khi điều kiện tra cứu thay đổi
    Given quản trị viên đang ở trang 3 và bảng đang hiển thị tài khoản "U-025"
    When quản trị viên thay đổi <tham_so> thành <gia_tri_moi>
    Then lần tra cứu tiếp theo chạy trên trang đầu với <tham_so> bằng <gia_tri_moi>
    And trong lúc chờ kết quả, bảng không hiển thị "U-025" như kết quả của điều kiện tra cứu mới

    Examples:
      | tham_so           | gia_tri_moi      |
      | từ khoá           | "huy"            |
      | bộ lọc vai trò           | "Người dùng"     |
      | bộ lọc trạng thái   | "Đang hoạt động" |
      | chiều sắp xếp     | "Cũ đến mới"     |
      | số dòng mỗi trang | 50               |

  Scenario: [US011-SC-10] Hiện và ẩn khu vực tìm kiếm, bộ lọc
    Given khu vực tìm kiếm và bộ lọc đang ẩn nhưng từ khoá "huy" vẫn đang được áp dụng
    When quản trị viên bấm nút "Bộ lọc"
    Then khu vực tìm kiếm và bộ lọc hiển thị với từ khoá "huy"
    And toàn bộ trang không tải lại
    When quản trị viên bấm lại nút "Bộ lọc"
    Then khu vực tìm kiếm và bộ lọc bị ẩn
    And từ khoá "huy" vẫn được áp dụng

  Scenario Outline: [US011-SC-11] Hiện và ẩn các cột tuỳ chọn
    Given bảng đang hiển thị các cột Tài khoản, Vai trò, Trạng thái, Ngày tham gia và Thao tác
    When quản trị viên <thao_tac> cột <cot_tuy_chon> trong menu "Cột hiển thị"
    Then cột <cot_tuy_chon> được <ket_qua>
    And cột Tài khoản và Thao tác vẫn hiển thị
    And hệ thống không tra cứu lại danh sách

    Examples:
      | thao_tac | cot_tuy_chon    | ket_qua  |
      | bỏ chọn  | "Vai trò"       | ẩn       |
      | bỏ chọn  | "Trạng thái"    | ẩn       |
      | bỏ chọn  | "Ngày tham gia" | ẩn       |
      | chọn     | "Vai trò"       | hiển thị |

  Scenario: [US011-SC-12] Làm mới đúng điều kiện tra cứu hiện tại
    Given quản trị viên đang xem trang 2 với từ khoá "an", vai trò "Người dùng", trạng thái "Đang hoạt động", chiều mới đến cũ và số dòng mỗi trang là 10
    And bảng đang hiển thị kết quả của lần tra cứu thành công gần nhất
    When quản trị viên bấm nút làm mới
    Then hệ thống tra cứu lại đúng từ khoá "an", vai trò "Người dùng", trạng thái "Đang hoạt động", chiều mới đến cũ, trang 2 và số dòng mỗi trang 10
    And thân bảng hiển thị trạng thái đang tải, không hiển thị dữ liệu cũ như kết quả mới
    And thanh công cụ và bộ lọc vẫn dùng được; nút chuyển trang tạm tắt
    And toàn bộ trang không tải lại
    And bảng hiển thị dữ liệu của lần tra cứu mới khi tra cứu thành công

  Scenario: [US011-SC-13] Không có kết quả và xoá điều kiện tra cứu
    Given quản trị viên đang dùng từ khoá "khong-ton-tai", vai trò "Người dùng", trạng thái "Đang hoạt động", chiều cũ đến mới và số dòng mỗi trang là 50
    When lần tra cứu hoàn tất với tổng số kết quả bằng 0
    Then bảng không hiển thị dữ liệu của lần tra cứu trước
    And màn hình hiển thị "Không có bản ghi nào phù hợp với từ khoá hoặc bộ lọc hiện tại."
    And màn hình hiển thị nút "Xoá tìm kiếm và bộ lọc"
    When quản trị viên bấm "Xoá tìm kiếm và bộ lọc"
    Then từ khoá được xoá, vai trò và trạng thái trở về "Tất cả"
    And màn hình quay về trang 1
    And chiều cũ đến mới và số dòng mỗi trang 50 được giữ nguyên

  Scenario: [US011-SC-14] Không lấy được danh sách
    Given quản trị viên đang tra cứu từ khoá "huy"
    And lần tra cứu thành công gần nhất đã hiển thị tài khoản "U-101"
    When hệ thống gặp lỗi khi lấy danh sách, không phải lỗi thiếu quyền quản trị hay phiên đăng nhập không hợp lệ
    Then vùng nội dung bảng hiển thị trạng thái lỗi kèm nút "Thử lại"
    And bảng không hiển thị "U-101" như kết quả của lần tra cứu mới
    And từ khoá "huy" được giữ nguyên
    And không có dữ liệu tài khoản hoặc trạng thái tài khoản nào bị thay đổi

  Scenario: [US011-SC-15] Tài khoản không có quyền quản trị yêu cầu danh sách
    Given tài khoản "U-901" đã đăng nhập nhưng không có quyền quản trị tài khoản
    When "U-901" gửi yêu cầu lấy danh sách tài khoản
    Then máy chủ từ chối yêu cầu vì không đủ quyền
    And phản hồi không chứa dữ liệu tài khoản
    And màn hình không hiển thị bảng danh sách
    And màn hình không hiển thị nút "Thử lại"

  Scenario Outline: [US011-SC-16] Máy chủ từ chối yêu cầu danh sách khi phiên đăng nhập thiếu hoặc không hợp lệ
    Given yêu cầu lấy danh sách tài khoản <tinh_trang_phien>
    When yêu cầu được gửi tới máy chủ
    Then máy chủ từ chối yêu cầu với mã "<ma_loi>"
    And máy chủ không kiểm tra quyền quản trị tài khoản
    And phản hồi không chứa dữ liệu tài khoản

    Examples:
      | tinh_trang_phien                  | ma_loi             |
      | không kèm thông tin đăng nhập     | AUTH_TOKEN_MISSING |
      | kèm phiên đăng nhập đã hết hạn    | AUTH_TOKEN_INVALID |
      | kèm phiên đăng nhập đã bị sửa đổi | AUTH_TOKEN_INVALID |

  Scenario: [US011-SC-17] Phiên đăng nhập hết hạn khi đang ở màn danh sách
    Given "ACC-ADMIN" đang xem màn danh sách tài khoản
    And phiên đăng nhập của "ACC-ADMIN" đã hết hạn
    When "ACC-ADMIN" bấm nút làm mới
    Then hệ thống chuyển thẳng "ACC-ADMIN" về trang đăng nhập
    And trang đăng nhập hiển thị "Phiên đăng nhập đã hết hạn" và "Vui lòng đăng nhập lại."
    And hệ thống không hiển thị thông báo không đủ quyền
    And không có dữ liệu tài khoản nào được hiển thị
```
