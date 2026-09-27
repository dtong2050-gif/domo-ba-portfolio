# `US-013` — Đổi vai cho một tài khoản

## Phát biểu

> **Là** quản trị viên có quyền quản trị tài khoản

> **Tôi muốn** đổi vai của một tài khoản khác sau khi nhập lý do và xác nhận quyết định

> **Để** trao hoặc thu hồi đúng quyền mà không làm mất dữ liệu nghiệp vụ của tài khoản


## Luồng trong phạm vi

| Bước | Nội dung |
|---|---|
| Bắt đầu | Từ menu thao tác của một tài khoản đích hợp lệ, quản trị viên chọn "Đổi vai trò" để mở hộp thoại với đúng một tài khoản đích |
| Quyết định chính | Chọn một vai trò được phép gán và đang dùng, nhập lý do 20–500 ký tự sau khi bỏ khoảng trắng đầu cuối, kiểm tra lại tài khoản đích, vai trò cũ → vai trò mới và lý do trên hộp xác nhận, rồi xác nhận. Hệ thống kiểm tra lại quyền và toàn bộ điều kiện bảo vệ **tại thời điểm xử lý**, không dựa vào kết quả đã kiểm trên giao diện |
| Kết quả | Lệnh hợp lệ cập nhật vai trò như một thay đổi trọn vẹn và tải lại danh sách; kết quả thực tế được gửi cho module audit, kết quả thành công được gửi cho phần thông báo. Phiên đăng nhập đang mở của tài khoản đích vẫn giữ quyền cũ; quyền mới có hiệu lực từ lần đăng nhập kế tiếp |


## US Inventory

| Mã | Loại | Ưu tiên | Rủi ro nếu thiếu | Mockup |
|---|---|---|---|---|
| `US013-SC-01` | Happy path | Cao | Ghi sai vai trò, hai nguồn lưu vai trò lệch nhau, hoặc không gửi sự kiện cho module liên quan | [eligible](../../mockups/scr-003/scr-003_change-role-dialog_eligible.png), [confirmation_ready](../../mockups/scr-004/scr-004_change-role-confirmation_ready.png), [confirmation_success](../../mockups/scr-004/scr-004_change-role-confirmation_success.png) |
| `US013-SC-02` | UX | Cao | Thao tác nhầm tài khoản hoặc hiển thị dữ liệu ngoài phạm vi | [ready](../../mockups/scr-003/scr-003_change-role-dialog_ready.png) |
| `US013-SC-03` | Business rule | Cao | Gán vai trò nháp, vai trò đã ngừng dùng hoặc cấp nhầm quyền superAdmin | Chưa có frame |
| `US013-SC-04` | Recovery | Cao | Tiếp tục thao tác với danh sách vai trò lỗi hoặc rỗng | [catalog-load-failed](../../mockups/scr-003/scr-003_change-role-dialog_catalog-load-failed.png) |
| `US013-SC-05` | Validation | Cao | Lệnh thiếu căn cứ hoặc lý do vượt giới hạn | [reason-validation-errors](../../mockups/scr-003/scr-003_change-role-dialog_reason-validation-errors.png) |
| `US013-SC-06` | Validation | Trung bình | Giao diện và máy chủ xử lý giá trị biên không thống nhất | [eligible](../../mockups/scr-003/scr-003_change-role-dialog_eligible.png) |
| `US013-SC-07` | UX | Cao | Xác nhận nhầm tài khoản đích, vai trò hoặc lý do | [confirmation_ready](../../mockups/scr-004/scr-004_change-role-confirmation_ready.png) |
| `US013-SC-08` | Authorization / Security | Cao | Quản trị viên tự nâng quyền hoặc tác động tới tài khoản được bảo vệ | [confirmation_rejected](../../mockups/scr-004/scr-004_change-role-confirmation_rejected.png) |
| `US013-SC-09` | Business rule | Cao | Báo sai nguyên nhân hoặc ghi lên tài khoản đích không hợp lệ | [confirmation_rejected](../../mockups/scr-004/scr-004_change-role-confirmation_rejected.png) |
| `US013-SC-10` | Concurrency | Cao | Gán vai trò đã đổi trạng thái sau khi mở hộp thoại | [confirmation_rejected](../../mockups/scr-004/scr-004_change-role-confirmation_rejected.png) |
| `US013-SC-11` | Business rule | Trung bình | Ghi một thay đổi không có thật hoặc gửi thông báo sai | [confirmation_no_change](../../mockups/scr-004/scr-004_change-role-confirmation_no_change.png) |
| `US013-SC-12` | Concurrency | Cao | Ghi đè một thay đổi vai trò mới hơn | [confirmation_rejected](../../mockups/scr-004/scr-004_change-role-confirmation_rejected.png) |
| `US013-SC-13` | Recovery | Cao | Hai nguồn lưu vai trò lệch nhau hoặc chỉ cập nhật được một phần | [confirmation_try-again](../../mockups/scr-004/scr-004_change-role-confirmation_try-again.png) |
| `US013-SC-14` | Recovery | Cao | Gửi lại một lệnh có thể đã được ghi nhận | [confirmation_timeout](../../mockups/scr-004/scr-004_change-role-confirmation_timeout.png) |
| `US013-SC-15` | Business rule | Cao | Mất quan hệ sở hữu hoặc mất dữ liệu thử thách | Không có giao diện |
| `US013-SC-16` | Business rule | Cao | Hiểu sai thời điểm quyền mới có hiệu lực | Không có giao diện |
| `US013-SC-17` | Concurrency | Trung bình | Tạo nhiều lệnh trùng nhau do bấm lặp | [confirmation_pending](../../mockups/scr-004/scr-004_change-role-confirmation_pending.png) |
| `US013-SC-18` | Authorization / Security | Cao | Ghi lệnh đổi vai khi không xác định được người thực hiện, hoặc báo nhầm thành thất bại hay không rõ kết quả | [session-expired](../../mockups/scr-002/scr-002_session-expired.png) |
| `US013-SC-19` | Authorization / Security | Cao | Dùng danh mục vai cũ hoặc để người thao tác tiếp tục nhập khi phiên đăng nhập đã hết hạn | [session-expired](../../mockups/scr-002/scr-002_session-expired.png) |
| `US013-SC-20` | Authorization / Security | Cao | Kênh truy cập khác cấp được quyền superAdmin qua lệnh đổi vai | Không có giao diện |

```gherkin
Feature: US-013 — Đổi vai cho một tài khoản

  Background:
    Given tài khoản "ACC-ADMIN" đã đăng nhập và có quyền quản trị tài khoản
    And tài khoản "ACC-TARGET" tồn tại, đang hoạt động, không phải "ACC-ADMIN" và không phải tài khoản superAdmin
    And "ACC-TARGET" đang có vai trò "Người dùng"

  Scenario Outline: [US013-SC-01] Đổi sang một vai trò được phép gán
    Given vai trò "<vai_tro_moi>" đã được công bố, đang dùng và được phép gán
    And lý do đã chuẩn hoá là "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới."
    When "ACC-ADMIN" chọn "<vai_tro_moi>", tiếp tục sang hộp "xác nhận" và xác nhận
    Then hệ thống trả kết quả thành công cho "ACC-TARGET"
    And "ACC-TARGET" có đúng vai trò "<vai_tro_moi>"
    And mọi nơi hiển thị thông tin vai trò đều cập nhật theo "<vai_tro_moi>"
    And danh sách được tải lại đúng điều kiện tra cứu hiện tại
    And hệ thống gửi tài khoản đích, loại sự kiện và lý do đã chuẩn hoá cho module thông báo
    And hệ thống gửi kết quả thực tế cùng người thực hiện, tài khoản đích, vai trò cũ → vai trò mới và lý do cho module audit

    Examples:
      | vai_tro_moi     |
      | Quản trị viên   |
      | Kiểm duyệt viên |

  Scenario: [US013-SC-02] Mở đúng hộp thoại đổi vai trò từ menu của tài khoản đích
    Given dòng "ACC-TARGET" đang hiển thị tên "Lan Nguyễn", email "lan@example.test" và vai trò "Người dùng" trên màn hình quản trị tài khoản
    When "ACC-ADMIN" mở menu thao tác của dòng "ACC-TARGET" và chọn "Đổi vai trò"
    Then hệ thống mở hộp thoại đổi vai trò cho đúng "ACC-TARGET"
    And hộp thoại hiển thị "Lan Nguyễn", "lan@example.test" và vai trò hiện tại "Người dùng"
    And hộp thoại có ô chọn đúng một vai trò mới và ô lý do bắt buộc
    And hộp thoại không hiển thị ngày sinh, giới tính, địa chỉ, số điện thoại hoặc giới thiệu bản thân
    And vai trò của "ACC-TARGET" chưa thay đổi

  Scenario: [US013-SC-03] Danh sách vai trò chỉ gồm vai trò được phép gán
    Given danh mục vai trò có các bản ghi:
      | vai trò               | trạng thái công bố | trạng thái vòng đời | loại vai   |
      | Quản trị viên         | Đã công bố         | Đang dùng           | Vai gốc    |
      | Kiểm duyệt viên       | Đã công bố         | Đang dùng           | Vai tự tạo |
      | Vai trò nháp          | Nháp               | Đang dùng           | Vai tự tạo |
      | Vai trò đã ngừng dùng | Đã công bố         | Đã ngừng dùng       | Vai tự tạo |
    When "ACC-ADMIN" mở danh sách "Vai trò mới" trong hộp thoại đổi vai trò
    Then danh sách hiển thị đúng "Quản trị viên" và "Kiểm duyệt viên"
    And danh sách không hiển thị "Vai trò nháp" hoặc "Vai trò đã ngừng dùng"
    And danh sách không có lựa chọn superAdmin

  Scenario Outline: [US013-SC-04] Không dùng được danh sách vai trò
    Given việc tải danh sách vai trò có kết quả "<ket_qua_tai>"
    When hộp thoại đổi vai trò xử lý kết quả đó
    Then hộp thoại chuyển sang trạng thái "<trang_thai>"
    And <hanh_dong_tiep_theo>
    And hộp thoại không cho "ACC-ADMIN" tiếp tục sang hộp xác nhận
    And vai trò của "ACC-TARGET" chưa thay đổi

    Examples:
      | ket_qua_tai                         | trang_thai     | hanh_dong_tiep_theo                                                                              |
      | lỗi khi tải danh sách               | Lỗi            | hộp thoại hiển thị nút "Thử tải lại"                                                                 |
      | danh sách rỗng                      | Lỗi            | hệ thống ghi nhận đây là lỗi dữ liệu hoặc cấu hình                                               |

  Scenario Outline: [US013-SC-05] Từ chối lý do ngoài khoảng hợp lệ
    Given lý do mẫu "<mau_ly_do>" có đúng <do_dai_sau_chuan_hoa> ký tự sau khi bỏ khoảng trắng đầu cuối
    And "ACC-ADMIN" đã chọn vai trò "Quản trị viên"
    When "ACC-ADMIN" yêu cầu tiếp tục hoặc gửi thẳng lệnh đổi vai trò xuống máy chủ với lý do đó
    Then giao diện không cho tiếp tục và máy chủ từ chối nếu vẫn nhận được lệnh
    And kết quả được phân loại là bị từ chối, không phải thất bại an toàn (fail-safe)
    And vai trò của "ACC-TARGET" vẫn là "Người dùng"

    Examples:
      | mau_ly_do         | do_dai_sau_chuan_hoa |
      | toàn khoảng trắng | 0               |
      | 19 ký tự          | 19              |
      | 501 ký tự         | 501             |

   Scenario Outline: [US013-SC-06] Chấp nhận lý do tại hai giá trị biên
    Given lý do mẫu "<mau_ly_do>" có đúng <do_dai_sau_chuan_hoa> ký tự sau khi bỏ khoảng trắng đầu cuối
    And "ACC-ADMIN" đã chọn vai trò "Quản trị viên"
    When "ACC-ADMIN" yêu cầu tiếp tục sang hộp xác nhận
    Then giao diện cho phép mở hộp xác nhận
    And máy chủ chấp nhận độ dài lý do này khi lệnh được xác nhận

    Examples:
      | mau_ly_do | do_dai_sau_chuan_hoa |
      | 20 ký tự  | 20              |
      | 500 ký tự | 500             |

  Scenario: [US013-SC-07] Hộp xác nhận hiển thị đúng quyết định và lý do đã chuẩn hoá
    Given "ACC-TARGET" có tên "Lan Nguyễn" và email "lan@example.test"
    And "ACC-ADMIN" đã chọn vai trò "Quản trị viên"
    And "ACC-ADMIN" đã nhập lý do "  Điều chuyển sang vai trò phù hợp với nhiệm vụ mới.  "
    When "ACC-ADMIN" bấm "Tiếp tục"
    Then hộp xác nhận hiển thị "Lan Nguyễn" và "lan@example.test"
    And hiển thị đầy đủ "Người dùng → Quản trị viên"
    And hiển thị lý do đã chuẩn hoá "Điều chuyển sang vai trò phù hợp với nhiệm vụ mới."
    And mọi nội dung trên hộp xác nhận đều ở chế độ chỉ đọc
    And vai trò của "ACC-TARGET" chưa thay đổi trước khi "ACC-ADMIN" bấm "Xác nhận"

  Scenario Outline: [US013-SC-08] Từ chối tài khoản đích được bảo vệ
    Given đích của lệnh là "<dich_duoc_bao_ve>"
    When "ACC-ADMIN" yêu cầu đổi vai trò của tài khoản đích sang "Quản trị viên"
    Then hệ thống từ chối trước khi ghi dữ liệu với nguyên nhân "<ly_do_tu_choi>"
    And vai trò của tài khoản đích không thay đổi
    And không gửi sự kiện cho module thông báo

    Examples:
      | dich_duoc_bao_ve | ly_do_tu_choi                                                                |
      | ACC-ADMIN        | Không thể thay đổi quyền hoặc vô hiệu hoá chính tài khoản của bạn            |
      | ACC-SUPER        | Tài khoản superAdmin được bảo vệ và không thể bị thay đổi bởi tài khoản khác |

  Scenario Outline: [US013-SC-09] Phân biệt tài khoản đã vô hiệu hoá với tài khoản không tồn tại
    Given đích của lệnh là "<dich>", ở tình trạng "<tinh_trang>"
    When "ACC-ADMIN" xác nhận lệnh đổi vai trò
    Then hệ thống từ chối trước khi ghi dữ liệu
    And báo nguyên nhân "<nguyen_nhan_dung>"
    And không báo nguyên nhân "<nguyen_nhan_sai>"
    And không gửi sự kiện cho module thông báo

    Examples:
      | dich         | tinh_trang     | nguyen_nhan_dung                              | nguyen_nhan_sai                               |
      | ACC-DISABLED | Đã vô hiệu hoá | Cần khôi phục tài khoản trước khi đổi vai trò | Không tìm thấy tài khoản                      |
      | ACC-MISSING  | Không tồn tại  | Không tìm thấy tài khoản                      | Cần khôi phục tài khoản trước khi đổi vai trò |

  Scenario Outline: [US013-SC-10] Từ chối vai trò không còn được phép gán
    Given hộp xác nhận đang hiển thị vai trò mới "<vai_tro_moi>"
    And trước khi lệnh được xử lý, vai trò đó ở tình trạng "<tinh_trang_vai_tro>"
    When "ACC-ADMIN" bấm "Xác nhận"
    Then máy chủ từ chối lệnh trước khi ghi dữ liệu với nguyên nhân vai trò không còn khả dụng
    And "ACC-TARGET" vẫn có vai trò "Người dùng"
    And hộp xác nhận chỉ cho quay lại để tải lại danh sách vai trò và chọn lại

    Examples:
      | vai_tro_moi           | tinh_trang_vai_tro   |
      | Vai trò nháp          | đã chuyển sang nháp  |
      | Vai trò đã ngừng dùng | đã ngừng dùng        |

  Scenario: [US013-SC-11] Trả "Không có thay đổi" khi chọn đúng vai trò hiện tại
    Given vai trò mới trong quyết định cũng là "Người dùng"
    When "ACC-ADMIN" xác nhận lệnh
    Then hệ thống trả kết quả "Không có thay đổi"
    And không ghi vai trò như một thay đổi mới
    And danh sách được tải lại theo đúng điều kiện tra cứu hiện tại và hiển thị thông báo "Không có thay đổi"
    And không gửi sự kiện cho module thông báo
    And hệ thống gửi kết quả "Không có thay đổi" cho module audit

   Scenario: [US013-SC-12] Từ chối quyết định dựa trên thông tin vai trò đã cũ
    Given hộp xác nhận được mở khi vai trò hiện tại của "ACC-TARGET" là "Người dùng"
    And một lệnh khác đã đổi "ACC-TARGET" sang "Kiểm duyệt viên"
    When "ACC-ADMIN" xác nhận quyết định cũ để đổi sang "Quản trị viên"
    Then hệ thống từ chối lệnh vì vai trò hiện tại không còn khớp với thông tin trên hộp xác nhận
    And không ghi đè vai trò "Kiểm duyệt viên"
    And hộp xác nhận không cho thử lại với thông tin cũ
    And quản trị viên phải tải lại dữ liệu trước khi tạo quyết định mới

  Scenario: [US013-SC-13] Không để lại thay đổi một phần khi cập nhật thất bại
    Given lệnh đổi "ACC-TARGET" từ "Người dùng" sang "Quản trị viên" đã vượt qua toàn bộ điều kiện bảo vệ
    When hệ thống không thể hoàn tất trọn vẹn việc cập nhật vai trò
    Then mọi nơi hiển thị thông tin vai trò vẫn là "Người dùng"
    And hộp xác nhận hiển thị trạng thái thất bại an toàn và cho phép "Thử lại"
    And không gửi sự kiện cho module thông báo

  Scenario: [US013-SC-14] Không tự động gửi lại lệnh khi kết quả không xác định
    Given "ACC-ADMIN" đã gửi lệnh đổi "ACC-TARGET" sang "Quản trị viên"
    When giao diện mất phản hồi sau thời điểm thay đổi có thể đã được ghi nhận
    Then hộp xác nhận hiển thị trạng thái không xác định, không hiểu thành thất bại an toàn
    And giữ tài khoản đích và vai trò cũ → vai trò mới để đối soát
    And vô hiệu hoá nút "Xác nhận", nút "Quay lại", phím Esc và thao tác bấm ra ngoài hộp thoại
    And chỉ cho phép bấm "Đóng và tải lại"
    And hệ thống tải lại danh sách trước khi cho phép tạo lệnh mới

  Scenario: [US013-SC-15] Hạ vai trò Nhà tổ chức không xoá dữ liệu sở hữu
    Given "ACC-TARGET" đang có vai trò "Nhà tổ chức"
    And "ACC-TARGET" sở hữu thử thách "CHALLENGE-101" và dữ liệu vận hành liên quan
    When "ACC-ADMIN" đổi "ACC-TARGET" sang "Người dùng" thành công
    Then "ACC-TARGET" có vai trò "Người dùng"
    And "CHALLENGE-101" vẫn thuộc sở hữu của "ACC-TARGET"
    And dữ liệu của "CHALLENGE-101" không bị xoá hoặc thay đổi bởi lệnh đổi vai trò

  Scenario: [US013-SC-16] Phiên đăng nhập đang mở chưa đổi quyền ngay
    Given "ACC-TARGET" đang có một phiên đăng nhập hợp lệ với quyền của vai trò "Người dùng"
    When lệnh đổi "ACC-TARGET" sang "Quản trị viên" thành công
    Then danh sách hiển thị vai trò mới "Quản trị viên" sau khi tải lại
    And phiên đang mở của "ACC-TARGET" vẫn dùng quyền của vai trò "Người dùng"
    And quyền của "Quản trị viên" chỉ có hiệu lực khi "ACC-TARGET" đăng nhập lại hoặc khi phiên hiện tại hết hạn

  Scenario: [US013-SC-17] Chặn gửi trùng trong lúc lệnh đang được xử lý
    Given hộp xác nhận đang sẵn sàng với một quyết định hợp lệ
    When "ACC-ADMIN" bấm "Xác nhận"
    Then hộp xác nhận chuyển sang trạng thái đang gửi
    And chỉ một lệnh được gửi cho quyết định đó trong lúc hệ thống đang xử lý
    And nút "Xác nhận", nút "Quay lại", phím Esc và thao tác bấm ra ngoài đều bị vô hiệu hoá cho tới khi có kết quả

  Scenario: [US013-SC-18] Từ chối đổi vai khi phiên đăng nhập hết hạn lúc xác nhận
    Given hộp xác nhận đã mở với một quyết định đổi vai trò hợp lệ cho "ACC-TARGET"
    And phiên đăng nhập của "ACC-ADMIN" hết hạn trước khi hệ thống xử lý lệnh
    When "ACC-ADMIN" bấm "Xác nhận"
    Then máy chủ từ chối với mã "AUTH_TOKEN_INVALID" trước khi kiểm tra quyền và các điều kiện bảo vệ
    And vai trò của "ACC-TARGET" vẫn là "Người dùng"
    And không có bản ghi dấu vết thao tác nào được tạo
    And hệ thống đóng cả hai hộp thoại và chuyển thẳng "ACC-ADMIN" về trang đăng nhập
    And trang đăng nhập hiển thị "Phiên đăng nhập đã hết hạn" và "Vui lòng đăng nhập lại."
    And hệ thống không hiển thị trạng thái không đủ quyền, thất bại hay không xác định được kết quả

  Scenario: [US013-SC-19] Không tải danh mục vai khi phiên đăng nhập hết hạn
    Given "ACC-ADMIN" chọn "Đổi vai trò" trên dòng "ACC-TARGET"
    And phiên đăng nhập của "ACC-ADMIN" đã hết hạn
    When hộp thoại đổi vai trò tải danh mục vai
    Then máy chủ từ chối với mã "AUTH_TOKEN_INVALID"
    And hộp thoại không hiển thị danh sách vai và không dùng danh mục đã lưu trước đó
    And hệ thống đóng hộp thoại và chuyển thẳng "ACC-ADMIN" về trang đăng nhập
    And trang đăng nhập hiển thị "Phiên đăng nhập đã hết hạn" và "Vui lòng đăng nhập lại."

  Scenario: [US013-SC-20] Từ chối yêu cầu cấp superAdmin gửi thẳng tới máy chủ
    Given yêu cầu đổi vai trò cho "ACC-TARGET" được gửi thẳng tới máy chủ, không đi qua giao diện
    And yêu cầu kèm một trường đòi cấp superAdmin cho "ACC-TARGET"
    When máy chủ nhận yêu cầu
    Then máy chủ từ chối với mã "VALIDATION_FAILED" trước khi ghi dữ liệu
    And "ACC-TARGET" không mang cờ superAdmin
    And vai trò của "ACC-TARGET" vẫn là "Người dùng"
```
