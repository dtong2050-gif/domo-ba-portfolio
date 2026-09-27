# Ma trận truy vết yêu cầu

Ma trận truy vết nối mỗi yêu cầu với nhu cầu nghiệp vụ, user story, màn hình và ca nghiệm thu tương ứng.
Nhờ đó có thể lần theo hai chiều, từ mục tiêu kinh doanh xuống ca kiểm thử và ngược lại, để phát hiện yêu cầu chưa được kiểm thử hoặc không gắn với mục tiêu nào.
Khi một yêu cầu thay đổi, ma trận cho biết ngay những story, màn hình và ca nghiệm thu cần cập nhật.

## Tổng quan độ phủ

**4/4 yêu cầu stakeholder đã có ít nhất một ca nghiệm thu (100%).**

| Loại | Số lượng | Mã |
|---|---|---|
| Business Requirement | 3 | `BR-002`, `BR-003`, `BR-004` |
| Stakeholder Requirement | 4 | `StR-003`, `StR-004`, `StR-005`, `StR-006` |
| Functional Requirement | 2 | `FR-011`, `FR-012` |
| Non-functional Requirement | 2 | `NFR-P-05`, `NFR-P-06` |
| Business Rule | 2 | `RULE-006`, `RULE-007` |
| User Story | 3 | `US-011`, `US-012`, `US-013` |
| Tiêu chí nghiệm thu (SC-) | 50 | — |
| Màn hình và hộp thoại | 3 | `SCR-002`, `SCR-003`, `SCR-004` |
| Ca nghiệm thu người dùng | 5 | `UAT-021`, `UAT-022`, `UAT-023`, `UAT-024`, `UAT-025` |
| Yêu cầu thay đổi | 1 | [`CR-001`](10-change-decision-log.md#cr-001--rút-hiệu-lực-cắt-phiên-đang-mở-khỏi-mvp) → `BR-002`, `FR-012` |
| Quyết định | 2 | [`BD-001`](10-change-decision-log.md#bd-001--ba-vai-trò-gốc-chỉ-được-sửa-bộ-quyền), [`BD-002`](10-change-decision-log.md#bd-002--email-được-cấp-lại-cho-người-khác-vẫn-nhận-tài-khoản-cũ) |

## Bảng A — Từ nhu cầu nghiệp vụ tới user story

| `BR-` | Nhu cầu nghiệp vụ | `StR-` | `FR-` | `NFR-` | `US-` |
|---|---|---|---|---|---|
| `BR-002` | Cho phép xử lý kịp thời tài khoản và thử thách vi phạm, đồng thời khôi phục nguyên trạng tài khoản nếu bị xử lý nhầm | `StR-003` | `FR-011` | — | `US-011`, `US-012` |
| `BR-003` | Bảo đảm quyền của mỗi tài khoản phù hợp với trách nhiệm hiện tại và duy trì quyền kiểm soát tối cao của DOMO | `StR-004` | `FR-011`, `FR-012` | `NFR-P-05` | `US-011`, `US-012`, `US-013` |
| `BR-003` | Bảo đảm quyền của mỗi tài khoản phù hợp với trách nhiệm hiện tại và duy trì quyền kiểm soát tối cao của DOMO | `StR-005` | `FR-012` | — | `US-011`, `US-012`, `US-013` |
| `BR-004` | Bảo đảm có thể truy vết các quyết định và thao tác quản trị | `StR-006` | — | `NFR-P-06` | `US-013` |

Nguồn: [nhu cầu nghiệp vụ](01-scope-baseline.md#2-mục-tiêu---business-requirement), [yêu cầu và quy tắc nghiệp vụ](03-requirements-and-rules.md).

## Bảng B — Từ user story tới nghiệm thu

| `US-` | Tiêu đề | `RULE-` | `SCR-` | `UAT-` |
|---|---|---|---|---|
| `US-011` | Tra cứu danh sách tài khoản | `RULE-006` | `SCR-002` | `UAT-021`, `UAT-024`, `UAT-025` |
| `US-012` | Chọn tài khoản cho thao tác quản trị | `RULE-006`, `RULE-007` | `SCR-002` | `UAT-022`, `UAT-024` |
| `US-013` | Đổi vai cho một tài khoản | `RULE-006`, `RULE-007` | `SCR-002`, `SCR-003`, `SCR-004` | `UAT-023`, `UAT-024`, `UAT-025` |

Nguồn: [quy tắc nghiệp vụ](03-requirements-and-rules.md#4-business-rule), [user story và tiêu chí nghiệm thu](06-user-stories/), [đặc tả màn hình](07-screen-specs/), [ca nghiệm thu](09-uat-test-cases.md).
