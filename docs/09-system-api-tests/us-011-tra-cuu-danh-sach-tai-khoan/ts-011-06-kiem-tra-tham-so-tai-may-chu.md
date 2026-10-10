# `TS-011-06` — Kiểm tra tham số tra cứu tại máy chủ

**Liên quan:** [danh sách scenario của `US-011`](README.md), [user story `US-011`](../../06-user-stories/us-011-tra-cuu-danh-sach-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** máy chủ chỉ nhận tham số tra cứu hợp lệ khi request được gửi trực tiếp, không đi qua giao diện, và không tự đổi giá trị sai về giá trị mặc định.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-011-06-01` | Chấp nhận các giá trị số dòng mỗi trang hợp lệ | Positive | High | | `US011-SC-08` |
| `TC-011-06-02` | Từ chối số dòng mỗi trang ngoài tập giá trị hợp lệ | Negative | High | | `US011-SC-08` |
| `TC-011-06-03` | Từ chối chiều sắp xếp ngoài asc và desc | Negative | High | | `US011-SC-08` |
| `TC-011-06-04` | Kiểm tra giá trị biên của số trang | Boundary | High | | `US011-SC-08` |

## `TC-011-06-01` — Chấp nhận các giá trị số dòng mỗi trang hợp lệ

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `GET {{base_url}}/users?pageSize=<giá trị>` với `{{token_admin}}`, lần lượt theo dòng dữ liệu.

**Test data:**

| Dòng | `pageSize` |
|---|---|
| a | 10 |
| b | 20 |
| c | 50 |
| d | 100 |

**Expected result:**

- Phản hồi `200`, `success: true`.
- `meta.pageSize` bằng giá trị đã gửi; số phần tử trong `data` không vượt quá giá trị đó.

**Actual result:**

**Bug:**

## `TC-011-06-02` — Từ chối số dòng mỗi trang ngoài tập giá trị hợp lệ

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `GET {{base_url}}/users?pageSize=<giá trị>` với `{{token_admin}}`, lần lượt theo dòng dữ liệu.

**Test data:**

| Dòng | `pageSize` |
|---|---|
| a | 1 |
| b | 25 |
| c | 101 |
| d | 10.5 |

**Expected result:**

- Phản hồi `400`, `errorCode: VALIDATION_FAILED`, `data: null`.
- `details` có phần tử với `path` là `["pageSize"]`.
- Máy chủ không trả danh sách với số dòng mặc định 10.

**Actual result:**

**Bug:**

## `TC-011-06-03` — Từ chối chiều sắp xếp ngoài asc và desc

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `GET {{base_url}}/users?sortOrder=newest` với `{{token_admin}}`.

**Test data:** `sortOrder=newest`.

**Expected result:**

- Phản hồi `400`, `errorCode: VALIDATION_FAILED`, `data: null`.
- `details` có phần tử với `path` là `["sortOrder"]`.
- Máy chủ không trả danh sách theo chiều mặc định `desc`.

**Actual result:**

**Bug:**

## `TC-011-06-04` — Kiểm tra giá trị biên của số trang

**Precondition:** Dữ liệu nền.

**Steps:**

1. Gửi `GET {{base_url}}/users?page=<giá trị>` với `{{token_admin}}`, lần lượt theo dòng dữ liệu.

**Test data:**

| Dòng | `page` | Kết quả |
|---|---|---|
| a | 0 | Từ chối |
| b | 1 | Chấp nhận |

**Expected result:**

- Dòng a: phản hồi `400`, `errorCode: VALIDATION_FAILED`, `data: null`; `details` có phần tử với `path` là `["page"]`.
- Dòng b: phản hồi `200`, `meta.page` bằng 1.

**Actual result:**

**Bug:**
