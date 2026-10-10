# `TS-013-02` — Kiểm tra lý do đổi vai trò

**Liên quan:** [danh sách scenario của `US-013`](README.md), [user story `US-013`](../../06-user-stories/us-013-doi-vai-tai-khoan.md), [môi trường và dữ liệu](../00-moi-truong-va-du-lieu.md), [mục lục](../README.md)

**Nội dung kiểm tra:** giao diện và máy chủ áp dụng cùng ràng buộc lý do dài từ 20 đến 500 ký tự sau khi bỏ khoảng trắng đầu cuối.

| Test case ID | Tiêu đề | Test type | Priority | Status | US |
|---|---|---|---|---|---|
| `TC-013-02-01` | Từ chối lý do chỉ gồm khoảng trắng | Negative | High | | `US013-SC-05` |
| `TC-013-02-02` | Từ chối lý do 19 ký tự | Boundary | High | | `US013-SC-05` |
| `TC-013-02-03` | Chấp nhận lý do 20 ký tự | Boundary | Medium | | `US013-SC-06` |
| `TC-013-02-04` | Chấp nhận lý do 500 ký tự | Boundary | Medium | | `US013-SC-06` |
| `TC-013-02-05` | Từ chối lý do 501 ký tự | Boundary | High | | `US013-SC-05` |
| `TC-013-02-06` | Đếm độ dài lý do sau khi bỏ khoảng trắng đầu cuối | Boundary | High | | `US013-SC-05`, `US013-SC-07` |
| `TC-013-02-07` | Máy chủ từ chối lý do ngoài khoảng 20 đến 500 ký tự | Boundary | High | | `US013-SC-05` |
| `TC-013-02-08` | Máy chủ chấp nhận lý do dài 20 và 500 ký tự | Boundary | Medium | | `US013-SC-06` |

## `TC-013-02-01` — Từ chối lý do chỉ gồm khoảng trắng

**Precondition:** Dữ liệu nền. ACC-ADMIN đã đăng nhập, mở hộp thoại đổi vai trò cho ACC-TARGET và chọn "Quản trị viên".

**Steps:**

1. Nhập Test data vào ô lý do.
2. Bấm ra ngoài ô lý do.
3. Quan sát ô lý do, dòng thông báo, bộ đếm và nút "Tiếp tục".

**Test data:** Chuỗi gồm 5 khoảng trắng.

**Expected result:**

- Viền ô lý do chuyển đỏ; dòng thông báo hiển thị "Vui lòng nhập lý do" màu đỏ.
- Bộ đếm hiển thị "0/500" màu đỏ.
- Nút "Tiếp tục" bị tắt.

**Actual result:**

**Bug:**

## `TC-013-02-02` — Từ chối lý do 19 ký tự

**Precondition:** Như `TC-013-02-01`.

**Steps:**

1. Nhập Test data vào ô lý do.
2. Bấm ra ngoài ô lý do.
3. Quan sát ô lý do, dòng thông báo, bộ đếm và nút "Tiếp tục".
4. Bấm vào ô lý do, gõ thêm một ký tự "a".

**Test data:** Chuỗi gồm 19 ký tự "a".

**Expected result:**

- Sau bước 3: viền ô lý do đỏ; dòng thông báo hiển thị "Tối thiểu 20 ký tự" màu đỏ; bộ đếm "19/500" màu đỏ; nút "Tiếp tục" bị tắt.
- Sau bước 4: viền đỏ và thông báo lỗi biến mất ngay mà không cần rời ô; bộ đếm "20/500" màu xám; nút "Tiếp tục" được bật.

**Actual result:**

**Bug:**

## `TC-013-02-03` — Chấp nhận lý do 20 ký tự

**Precondition:** Như `TC-013-02-01`.

**Steps:**

1. Nhập Test data vào ô lý do.
2. Bấm ra ngoài ô lý do, quan sát ô lý do và nút "Tiếp tục".
3. Bấm "Tiếp tục".

**Test data:** Chuỗi gồm 20 ký tự "a".

**Expected result:**

- Sau bước 2: không có thông báo lỗi; bộ đếm "20/500" màu xám; nút "Tiếp tục" được bật.
- Sau bước 3: hộp thoại xác nhận mở ra, hiển thị đúng lý do đã nhập.

**Actual result:**

**Bug:**

## `TC-013-02-04` — Chấp nhận lý do 500 ký tự

**Precondition:** Như `TC-013-02-01`.

**Steps:**

1. Dán Test data vào ô lý do.
2. Bấm ra ngoài ô lý do, quan sát ô lý do và nút "Tiếp tục".
3. Bấm "Tiếp tục".

**Test data:** Chuỗi gồm 500 ký tự "a".

**Expected result:**

- Sau bước 2: không có thông báo lỗi; bộ đếm "500/500" màu xám; nút "Tiếp tục" được bật.
- Sau bước 3: hộp thoại xác nhận hiển thị đủ 500 ký tự, không bị cắt bớt.

**Actual result:**

**Bug:**

## `TC-013-02-05` — Từ chối lý do 501 ký tự

**Precondition:** Như `TC-013-02-01`.

**Steps:**

1. Dán Test data vào ô lý do.
2. Bấm ra ngoài ô lý do.
3. Quan sát nội dung ô lý do, dòng thông báo, bộ đếm và nút "Tiếp tục".

**Test data:** Chuỗi gồm 501 ký tự "a".

**Expected result:**

- Ô lý do giữ đủ 501 ký tự đã dán, không tự cắt bớt.
- Viền ô lý do đỏ; dòng thông báo hiển thị "Tối đa 500 ký tự" màu đỏ; bộ đếm "501/500" màu đỏ.
- Nút "Tiếp tục" bị tắt.

**Actual result:**

**Bug:**

## `TC-013-02-06` — Đếm độ dài lý do sau khi bỏ khoảng trắng đầu cuối

**Precondition:** Như `TC-013-02-01`.

**Steps:**

1. Nhập lý do theo dòng dữ liệu.
2. Bấm ra ngoài ô lý do, quan sát bộ đếm, dòng thông báo và nút "Tiếp tục".
3. Với dòng b, bấm "Tiếp tục" và quan sát lý do trên hộp thoại xác nhận.

**Test data:**

| Dòng | Lý do nhập vào | Độ dài sau khi bỏ khoảng trắng đầu cuối |
|---|---|---|
| a | 1 khoảng trắng, theo sau là 19 ký tự "a" | 19 |
| b | 2 khoảng trắng, 20 ký tự "a", 2 khoảng trắng | 20 |

**Expected result:**

- Dòng a: bộ đếm "19/500" màu đỏ; thông báo "Tối thiểu 20 ký tự"; nút "Tiếp tục" bị tắt.
- Dòng b: bộ đếm "20/500" màu xám; không có thông báo lỗi; nút "Tiếp tục" được bật. Hộp thoại xác nhận hiển thị lý do gồm đúng 20 ký tự "a", không có khoảng trắng ở đầu và cuối.

**Actual result:**

**Bug:**

## `TC-013-02-07` — Máy chủ từ chối lý do ngoài khoảng 20 đến 500 ký tự

**Precondition:** Dữ liệu nền. Đọc `{{rev_target}}` trước mỗi request.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_admin}}` và body đổi vai trò chuẩn, thay `reason` theo dòng dữ liệu.
2. Kiểm tra phản hồi, vai trò của ACC-TARGET và bản ghi mới nhất trong `user_action_logs` có `entityId` là `{{id_target}}`.

**Test data:**

| Dòng | `reason` |
|---|---|
| a | Chuỗi gồm 5 khoảng trắng |
| b | Chuỗi gồm 19 ký tự "a" |
| c | Chuỗi gồm 501 ký tự "a" |

**Expected result:**

- Mỗi dòng trả `400`, `errorCode: VALIDATION_FAILED`, `data: null`; `details` có phần tử với `path` là `["reason"]`.
- Vai trò của ACC-TARGET không đổi.
- Bản ghi nhật ký của lệnh có `result: rejected`, không phải `failed`.
- Không gửi thông báo.

**Actual result:**

**Bug:**

## `TC-013-02-08` — Máy chủ chấp nhận lý do dài 20 và 500 ký tự

**Precondition:** Dữ liệu nền. Đọc `{{rev_target}}` trước mỗi request.

**Steps:**

1. Gửi `PATCH {{base_url}}/users/{{id_target}}/role` với `{{token_admin}}` và body đổi vai trò chuẩn, thay `roleId` và `reason` theo dòng a.
2. Đọc lại `{{rev_target}}`, gửi request theo dòng b.
3. Sau mỗi request, kiểm tra phản hồi và bản ghi mới nhất trong `user_action_logs` có `entityId` là `{{id_target}}`.

**Test data:**

| Dòng | `roleId` | `reason` |
|---|---|---|
| a | `{{role_admin}}` | Chuỗi gồm 20 ký tự "a" |
| b | `{{role_user}}` | Chuỗi gồm 500 ký tự "a" |

**Expected result:**

- Mỗi dòng trả `200`, `data.outcome: CHANGED`, `data.affectedCount: 1`.
- `currentRoleRevision` của ACC-TARGET tăng 1 sau mỗi request.
- Bản ghi nhật ký có `result: success` và `reason` lưu đủ 20 hoặc 500 ký tự đã gửi.
- Sau dòng b, ACC-TARGET trở lại vai trò Người dùng.

**Actual result:**

**Bug:**
