# `US-012` — Chọn tài khoản cho thao tác quản trị

Story này nối màn tra cứu [`SCR-002`](../07-screen-specs/scr-002-quan-tri-tai-khoan.md) với các lệnh quản trị: trên mỗi dòng được mở lệnh gì, và khi chọn nhiều dòng thì danh sách tài khoản đích gồm những ai.

## Phát biểu

> **Là** quản trị viên có quyền quản trị tài khoản

> **Tôi muốn** xem các thao tác được phép trên từng tài khoản và chọn danh sách tài khoản cho thao tác hàng loạt

> **Để** mở đúng lệnh quản trị trên đúng tài khoản đích, không tác động tới tài khoản được bảo vệ


## Luồng trong phạm vi

| Bước | Nội dung |
|---|---|
| Bắt đầu | Quản trị viên có quyền quản trị tài khoản đang xem kết quả tra cứu trên màn hình quản trị tài khoản |
| Quyết định chính | Hệ thống hiển thị menu theo trạng thái và điều kiện bảo vệ của từng tài khoản. Khi bộ lọc trạng thái có giá trị cụ thể, quản trị viên có thể chọn hoặc bỏ chọn các tài khoản hợp lệ trong toàn bộ kết quả đã lọc |
| Kết quả | Story kết thúc khi menu phù hợp được hiển thị, danh sách chọn được cập nhật, hoặc danh sách tài khoản đích được gửi sang hộp thoại xác nhận vô hiệu hoá hoặc khôi phục. Story không bao gồm bước xác nhận và thực thi lệnh |

## US Inventory

| Mã | Loại | Ưu tiên | Rủi ro nếu thiếu | Mockup |
|---|---|---|---|---|
| `US012-SC-01` | Happy path | Cao | Không mở được lệnh phù hợp hoặc mở nhầm thao tác | [menu_activated-account](../../mockups/scr-002/scr-002_menu_activated-account.png) |
| `US012-SC-02` | Business rule | Cao | Hiển thị thao tác không phù hợp với trạng thái tài khoản | [menu_disabled-account](../../mockups/scr-002/scr-002_menu_disabled-account.png) |
| `US012-SC-03` | Authorization / Security | Cao | Quản trị viên tự tác động lên chính mình, hoặc tài khoản superAdmin bị thay đổi | [not-choose-activated-account](../../mockups/scr-002/scr-002_not-choose-activated-account.png) |
| `US012-SC-04` | UX | Trung bình | Danh sách chọn trộn hai trạng thái, không xác định được lệnh hàng loạt | [filter-on](../../mockups/scr-002/scr-002_filter-on.png) |
| `US012-SC-05` | Happy path | Cao | Chọn thiếu trang, chọn nhầm tài khoản được bảo vệ hoặc hiện sai nút lệnh | [choose-all_activated-account](../../mockups/scr-002/scr-002_choose-all_activated-account.png), [choose-all_disabled-account](../../mockups/scr-002/scr-002_choose-all_disabled-account.png) |
| `US012-SC-06` | Business rule | Cao | Gửi sai danh sách đích hoặc thực thi lệnh trước khi xác nhận | [choose-all_activated-account](../../mockups/scr-002/scr-002_choose-all_activated-account.png), [choose-all_disabled-account](../../mockups/scr-002/scr-002_choose-all_disabled-account.png) |
| `US012-SC-07` | UX | Cao | Mất lựa chọn ở các trang khác | [choose-all_activated-account](../../mockups/scr-002/scr-002_choose-all_activated-account.png) |
| `US012-SC-08` | UX | Trung bình | Quản trị viên hiểu sai phạm vi đang chọn | [not-choose-activated-account](../../mockups/scr-002/scr-002_not-choose-activated-account.png), [choose-all_activated-account](../../mockups/scr-002/scr-002_choose-all_activated-account.png) |
| `US012-SC-09` | Business rule | Cao | Chỉ cập nhật trang đang xem hoặc không bỏ chọn hết | [not-choose-activated-account](../../mockups/scr-002/scr-002_not-choose-activated-account.png), [choose-all_activated-account](../../mockups/scr-002/scr-002_choose-all_activated-account.png) |
| `US012-SC-10` | Business rule | Trung bình | Thao tác trên một dòng làm mất hoặc thêm nhầm lựa chọn khác | [choose-all_activated-account](../../mockups/scr-002/scr-002_choose-all_activated-account.png) |
| `US012-SC-11` | Business rule | Cao | Lệnh áp dụng lên tài khoản không còn thuộc kết quả hiện tại | [not-choose-activated-account](../../mockups/scr-002/scr-002_not-choose-activated-account.png) |
| `US012-SC-12` | Authorization / Security | Cao | Tự khôi phục bị gom vào lệnh hàng loạt, hoặc bị chặn hoàn toàn | [not-choose-disabled-account](../../mockups/scr-002/scr-002_not-choose-disabled-account.png) |
| `US012-SC-13` | UX | Trung bình | Danh sách chọn vẫn còn sau khi bỏ chọn và bị dùng ngoài ý muốn | [not-choose-activated-account](../../mockups/scr-002/scr-002_not-choose-activated-account.png) |

```gherkin
Feature: US-012 — Chọn tài khoản cho thao tác quản trị

  Background:
    Given quản trị viên có quyền quản trị tài khoản
    And quản trị viên đang xem danh sách tài khoản trên màn hình quản trị tài khoản

  Scenario: [US012-SC-01] Mở menu của tài khoản đang hoạt động và được phép quản trị
    Given tài khoản "ACC-101" có trạng thái "Đang hoạt động"
    And "ACC-101" không phải tài khoản của quản trị viên và không phải tài khoản superAdmin
    When quản trị viên bấm nút "Thao tác" trên dòng "ACC-101"
    Then menu hiển thị đúng hai lựa chọn theo thứ tự "Đổi vai trò", "Vô hiệu hoá"
    And menu không hiển thị lựa chọn "Khôi phục"
    And vai trò và trạng thái của "ACC-101" chưa thay đổi

  Scenario: [US012-SC-02] Mở menu của tài khoản đã vô hiệu hoá
    Given tài khoản "ACC-201" có trạng thái "Đã vô hiệu hoá"
    And "ACC-201" không phải tài khoản của quản trị viên và không phải tài khoản superAdmin
    When quản trị viên bấm nút "Thao tác" trên dòng "ACC-201"
    Then menu chỉ hiển thị lựa chọn "Khôi phục"
    And menu không hiển thị lựa chọn "Đổi vai trò" hoặc "Vô hiệu hoá"
    And trạng thái của "ACC-201" vẫn là "Đã vô hiệu hoá"

  Scenario Outline: [US012-SC-03] Không hiển thị thao tác bị cấm trên tài khoản được bảo vệ
    Given dòng đích là tài khoản "<ma>", <dac_diem>
    And "<ma>" có trạng thái "Đang hoạt động"
    When bảng hiển thị dòng "<ma>"
    Then dòng đó không có nút thao tác  
    And dòng đó không có lựa chọn "Đổi vai trò" hoặc "Vô hiệu hoá"
    And không có hộp thoại đổi vai trò hoặc hộp thoại vô hiệu hoá nào được mở cho "<ma>"
    And vai trò và trạng thái của "<ma>" không thay đổi

    Examples:
      | ma       | dac_diem                           |
      | ACC-SELF | "là chính tài khoản của quản trị viên" |
      | ACC-ROOT | "là tài khoản superAdmin"              |

  Scenario Outline: [US012-SC-04] Không mở chế độ chọn nhiều dòng khi chưa có trạng thái cụ thể
    Given kết quả tra cứu gồm các tài khoản:
      | mã      | trạng thái     |
      | ACC-A01 | Đang hoạt động |
      | ACC-A02 | Đang hoạt động |
      | ACC-A03 | Đang hoạt động |
      | ACC-D01 | Đã vô hiệu hoá |
      | ACC-D02 | Đã vô hiệu hoá |
      | ACC-D03 | Đã vô hiệu hoá |
    And số dòng mỗi trang là 10
    When quản trị viên <thao_tac_bo_loc>
    Then kết quả gồm đúng "ACC-A01", "ACC-A02", "ACC-A03", "ACC-D01", "ACC-D02", "ACC-D03" trên một trang
    And ô chọn tất cả không hiển thị
    And không có tài khoản nào được chọn

    Examples:
      | thao_tac_bo_loc                    |
      | không chọn bộ lọc trạng thái       |
      | xoá bộ lọc trạng thái đang áp dụng |

  Scenario Outline: [US012-SC-05] Mở chế độ chọn nhiều dòng theo trạng thái
    Given hệ thống có 23 tài khoản ở trạng thái "<trang_thai>" với số dòng mỗi trang là 10:
      | nhóm                         | mã                  | số lượng | được phép chọn |
      | tài khoản của quản trị viên  | ACC-SELF            | 1        | không          |
      | tài khoản superAdmin         | ACC-ROOT            | 1        | không          |
      | tài khoản thường             | ACC-003 đến ACC-023 | 21       | có             |
    When quản trị viên chọn bộ lọc trạng thái "<trang_thai>"
    Then kết quả đã lọc gồm đúng 23 tài khoản trên và chia thành 3 trang
    And ô chọn tất cả hiển thị
    When quản trị viên đánh dấu ô chọn tất cả
    Then danh sách chọn gồm đúng 21 tài khoản "ACC-003" đến "ACC-023" trên cả 3 trang
    And "ACC-SELF" và "ACC-ROOT" không nằm trong danh sách chọn
    And thanh hành động hiển thị ngay dưới khu vực bộ lọc với số tài khoản đang chọn là 21
    And thanh hành động chỉ có nút "<nut_lenh>" và nút "Bỏ chọn"

    Examples:
      | trang_thai     | nut_lenh    |
      | Đang hoạt động | Vô hiệu hoá |
      | Đã vô hiệu hoá | Khôi phục   |

  Scenario Outline: [US012-SC-06] Mở đúng hộp thoại từ thanh hành động
    Given danh sách chọn gồm đúng "ACC-301" và "ACC-302" ở trạng thái "<trang_thai>"
    And thanh hành động hiển thị số tài khoản đang chọn là 2
    When quản trị viên bấm "<nut_lenh>"
    Then hệ thống mở <hop_thoai>
    And hộp thoại nhận đúng danh sách tài khoản đích gồm "ACC-301" và "ACC-302"
    And trạng thái của "ACC-301" và "ACC-302" chưa thay đổi
    And chưa có lệnh vô hiệu hoá hoặc khôi phục nào được gửi

    Examples:
      | trang_thai     | nut_lenh    | hop_thoai                        |
      | Đang hoạt động | Vô hiệu hoá | hộp thoại xác nhận vô hiệu hoá   |
      | Đã vô hiệu hoá | Khôi phục   | hộp thoại xác nhận khôi phục     |

  Scenario: [US012-SC-07] Giữ danh sách chọn khi chuyển trang
    Given bộ lọc trạng thái "Đang hoạt động" trả về 15 tài khoản được phép chọn với số dòng mỗi trang là 10
    And quản trị viên đã chọn "ACC-401" và "ACC-402" ở trang 1
    When quản trị viên chuyển sang trang 2 mà không thay đổi điều kiện tra cứu
    Then danh sách chọn vẫn gồm đúng "ACC-401" và "ACC-402"
    And thanh hành động vẫn hiển thị số tài khoản đang chọn là 2

  Scenario Outline: [US012-SC-08] Ô chọn tất cả phản ánh đúng phạm vi đang chọn
    Given toàn bộ kết quả đã lọc có đúng 5 tài khoản được phép chọn "ACC-501" đến "ACC-505"
    And danh sách chọn hiện tại là <dang_chon>
    When quản trị viên xem ô chọn tất cả trên tiêu đề bảng
    Then ô chọn tất cả ở trạng thái "<trang_thai_o_chon>"
    And danh sách chọn vẫn là <dang_chon>

    Examples:
      | dang_chon             | trang_thai_o_chon |
      | rỗng                  | chưa chọn         |
      | "ACC-501, ACC-502"    | chọn một phần     |
      | "ACC-501 đến ACC-505" | chọn tất cả       |

  Scenario Outline: [US012-SC-09] Bấm ô chọn tất cả cập nhật toàn bộ danh sách chọn
    Given toàn bộ kết quả đã lọc có đúng 5 tài khoản được phép chọn "ACC-601" đến "ACC-605"
    And ô chọn tất cả ở trạng thái "<truoc>"
    And danh sách chọn hiện tại là <chon_truoc>
    When quản trị viên bấm ô chọn tất cả
    Then danh sách chọn là <chon_sau>
    And ô chọn tất cả ở trạng thái "<sau>"

    Examples:
      | truoc         | chon_truoc            | chon_sau              | sau         |
      | chưa chọn     | rỗng                  | "ACC-601 đến ACC-605" | chọn tất cả |
      | chọn một phần | "ACC-601, ACC-602"    | "ACC-601 đến ACC-605" | chọn tất cả |
      | chọn tất cả   | "ACC-601 đến ACC-605" | rỗng                  | chưa chọn   |

  Scenario Outline: [US012-SC-10] Chọn hoặc bỏ chọn một dòng không làm đổi lựa chọn khác
    Given toàn bộ kết quả đã lọc có đúng 5 tài khoản được phép chọn "ACC-701" đến "ACC-705"
    And danh sách chọn ban đầu là <chon_truoc>
    When quản trị viên chuyển ô chọn của dòng "ACC-703" sang "<o_chon_dong>"
    Then danh sách chọn gồm đúng <chon_sau>
    And ô chọn tất cả ở trạng thái "chọn một phần"
    And thanh hành động hiển thị số tài khoản đang chọn là <so_luong>

    Examples:
      | chon_truoc            | o_chon_dong | chon_sau                             | so_luong |
      | "ACC-701, ACC-702"    | đã chọn     | "ACC-701, ACC-702, ACC-703"          | 3        |
      | "ACC-701 đến ACC-705" | chưa chọn   | "ACC-701, ACC-702, ACC-704, ACC-705" | 4        |

  Scenario Outline: [US012-SC-11] Bỏ chọn tất cả khi điều kiện tra cứu thay đổi
    Given danh sách chọn gồm đúng "ACC-801" và "ACC-802"
    And thanh hành động hiển thị số tài khoản đang chọn là 2
    When <thay_doi>
    Then danh sách chọn rỗng
    And thanh hành động không còn hiển thị
    And không có danh sách tài khoản đích nào được gửi cho lệnh quản trị

    Examples:
      | thay_doi                                                  |
      | quản trị viên đổi từ khoá tìm kiếm                        |
      | quản trị viên đổi vai lọc                                 |
      | quản trị viên đổi trạng thái lọc                          |
      | quản trị viên đổi chiều sắp xếp                           |
      | quản trị viên đổi số dòng mỗi trang                       |

  Scenario: [US012-SC-12] Không đưa tài khoản của quản trị viên vào danh sách khôi phục hàng loạt
    Given "ACC-SELF" là tài khoản của quản trị viên, có trạng thái "Đã vô hiệu hoá" và phiên đăng nhập hiện tại còn hợp lệ
    And "ACC-901" là tài khoản thường có trạng thái "Đã vô hiệu hoá"
    And bộ lọc trạng thái "Đã vô hiệu hoá" đang được áp dụng
    When quản trị viên đánh dấu ô chọn tất cả
    Then danh sách chọn chỉ gồm "ACC-901"
    And "ACC-SELF" không nằm trong danh sách chọn
    And việc tự khôi phục "ACC-SELF" chỉ thực hiện qua lựa chọn "Khôi phục" trên chính dòng đó
    And chưa mở hộp thoại xác nhận khôi phục và chưa thay đổi trạng thái tài khoản nào

  Scenario: [US012-SC-13] Bỏ chọn toàn bộ từ thanh hành động
    Given danh sách chọn gồm đúng "ACC-951", "ACC-952" và "ACC-953"
    And thanh hành động hiển thị số tài khoản đang chọn là 3
    When quản trị viên bấm "Bỏ chọn"
    Then danh sách chọn rỗng
    And ô chọn tất cả ở trạng thái "chưa chọn"
    And thanh hành động không còn hiển thị
    And không có danh sách tài khoản đích nào được gửi cho lệnh quản trị
```
