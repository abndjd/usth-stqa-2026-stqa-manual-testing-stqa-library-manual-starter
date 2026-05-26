# Test Cases — Bảng trường hợp kiểm thử

| Thông tin | |
|---|---|
| **Nhóm** | Nhóm 10 |
| **Ngày tạo** | 26/05/2026 |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

*(Phần này do Người 1 tổng hợp dựa trên phân tích của cả nhóm - Sẽ điền sau khi chốt yêu cầu)*

---

## Bước 2: Test Cases chi tiết

### Người 2: Tester 1 — Phụ trách Core Module (Mượn/Trả sách)

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-01 | Kiểm tra mượn đạt mức tối đa | Đang mượn 2 cuốn | 1. Chọn sách có sẵn<br>2. Bấm mượn | Số sách sau khi mượn: 3 | Mượn thành công | REQ-04 | BVA |
| TC-02 | Kiểm tra mượn vượt mức tối đa | Đang mượn 3 cuốn | 1. Chọn sách có sẵn<br>2. Bấm mượn | Số lượng sách đang giữ: 3 | Báo lỗi "Vượt quá giới hạn 3 cuốn" | REQ-04 | BVA |
| TC-03 | Tìm kiếm theo tên sách có tồn tại | Đang ở trang tìm kiếm | 1. Nhập từ khóa<br>2. Bấm tìm kiếm | Từ khóa: `"Clean Code"` | Hiển thị sách "Clean Code" | REQ-03 | N/A |
| TC-04 | Tìm kiếm sách không tồn tại | Đang ở trang tìm kiếm | 1. Nhập từ khóa rác<br>2. Bấm tìm kiếm | Từ khóa: `"XYZ123456"` | Trả về danh sách rỗng, báo "Không tìm thấy" | REQ-03 | N/A |
| TC-05 | Kiểm tra mượn sách lần đầu | Đang mượn 0 cuốn | 1. Chọn sách có sẵn<br>2. Bấm mượn | Số lượng sách đang giữ: 0 | Mượn thành công | REQ-04 | BVA |
| TC-06 | Kiểm tra giới hạn dưới | Lỗi hệ thống | Gọi API mượn với số lượng âm | Số lượng: -1 | Trả về mã lỗi, từ chối giao dịch | REQ-04 | BVA |

---

### Người 3: Tester 2 — Phụ trách User Module & Xác thực

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-07 | Đăng nhập bằng Email chuẩn | Ở trang đăng nhập | 1. Nhập email hợp lệ<br>2. Nhập pass đúng<br>3. Bấm Đăng nhập | Email: `user@gmail.com`<br>Pass: `123456` | Đăng nhập thành công | REQ-01 | EP |
| TC-08 | Đăng nhập sai định dạng Email | Ở trang đăng nhập | 1. Nhập email thiếu `@`<br>2. Bấm Đăng nhập | Email: `usergmail.com`<br>Pass: `123456` | Báo lỗi "Định dạng email không hợp lệ" | REQ-01 | EP |
| TC-09 | Thêm thành viên hợp lệ | Đăng nhập quyền Admin | 1. Điền đủ thông tin<br>2. Bấm Thêm | Email: `new@gmail.com`, Tên: `Nguyễn A` | Thêm thành công, hiển thị trong danh sách | REQ-07 | EP |
| TC-10 | Thêm thành viên trùng Email | Đăng nhập quyền Admin | 1. Điền email đã tồn tại<br>2. Bấm Thêm | Email: `user@gmail.com` | Báo lỗi "Email đã được sử dụng" | REQ-07 | EP |
| TC-11 | Đăng nhập sai mật khẩu | Ở trang đăng nhập | 1. Nhập email đúng<br>2. Nhập pass sai<br>3. Bấm Đăng nhập | Email: `user@gmail.com`<br>Pass: `wrongpass` | Báo lỗi "Sai mật khẩu" | REQ-01 | EP |
| TC-12 | Thêm thành viên bỏ trống Email | Đăng nhập quyền Admin | 1. Bỏ trống ô Email<br>2. Bấm Thêm | Email: `""` | Báo lỗi "Email không được để trống" | REQ-07 | EP |

---

### Người 4: Tester 3 — Phụ trách Logic & Nghiệp vụ nâng cao

**Bảng Decision Table (REQ-05):**
| Điều kiện / Hành động | Rule 1 | Rule 2 | Rule 3 | Rule 4 |
|-----------------------|--------|--------|--------|--------|
| **Tài khoản hoạt động?** | Có | Có | Không | Không |
| **Sách có sẵn?** | Có | Không | Có | Không |
| **Cho phép mượn?** | **YES** | **NO** | **NO** | **NO** |

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-13 | Mượn sách khi hội đủ điều kiện | TK bình thường | 1. Vào chi tiết sách<br>2. Bấm Mượn | Sách "Có sẵn" | Mượn thành công | REQ-05 | Decision Table |
| TC-14 | Mượn sách không có sẵn | TK bình thường | 1. Vào chi tiết sách<br>2. Bấm Mượn | Sách "Đang mượn" | Nút Mượn bị ẩn / Báo lỗi | REQ-05 | Decision Table |
| TC-15 | Tài khoản bị khóa mượn sách | TK bị khóa | 1. Vào chi tiết sách<br>2. Bấm Mượn | Sách "Có sẵn" | Báo lỗi "Tài khoản bị khóa" | REQ-05 | Decision Table |
| TC-16 | Tài khoản khóa + sách không có | TK bị khóa | 1. Vào chi tiết sách<br>2. Bấm Mượn | Sách "Đang mượn" | Nút Mượn bị ẩn / Báo lỗi | REQ-05 | Decision Table |
| TC-17 | Xem chi tiết sách hợp lệ | Không yêu cầu | Bấm vào ảnh bìa sách bất kỳ | Sách ID `BOOK01` | Mở trang chi tiết hiển thị đủ thông tin | REQ-02 | N/A |
| TC-18 | Xem chi tiết sách đã bị xóa | Không yêu cầu | Truy cập URL sách đã xóa | Sách ID `DELETED_BOOK` | Trả về trang 404 / Không tìm thấy | REQ-02 | N/A |

---

### Người 5: QA Analyst & Báo cáo tổng hợp

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| TC-19 | Sinh ngày hạn trả tự động | Đang mượn sách | Thực hiện thao tác mượn | Ngày mượn là T | Hạn trả hiển thị T + 14 ngày | REQ-06 | N/A |
| TC-20 | Trả sách đúng hạn | Đang giữ sách | Thực hiện thao tác trả | Ngày trả <= (T+14) | Cập nhật trạng thái trả thành công, không phạt | REQ-06 | N/A |
| TC-21 | Trả sách quá hạn | Đang giữ sách | Thực hiện thao tác trả | Ngày trả > (T+14) | Trả thành công, ghi nhận cảnh báo/phạt | REQ-06 | N/A |
| TC-22 | Quét sách quá hạn tự động | Có sách quá hạn | Thủ thư bấm "Quét hệ thống" | N/A | Danh sách hiển thị đúng các sách vượt 14 ngày | REQ-08 | N/A |
| TC-23 | Trạng thái sau khi quét quá hạn | Đã quét hệ thống | Truy cập hồ sơ người dùng có sách quá hạn | User ID có sách quá hạn | Tài khoản bị đánh dấu "Tạm ngưng mượn" | REQ-08 | N/A |
| TC-24 | Quét hệ thống không có lỗi | Không có sách quá hạn | Thủ thư bấm "Quét hệ thống" | N/A | Báo cáo "Không có sách nào quá hạn" | REQ-08 | N/A |

---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật áp dụng |
|----------------|-------|---------|----------------------|
| Người 2 | 6 | REQ-03, REQ-04 | BVA |
| Người 3 | 6 | REQ-01, REQ-07 | EP |
| Người 4 | 6 | REQ-02, REQ-05 | Decision Table |
| Người 5 | 6 | REQ-06, REQ-08 | N/A |
| **Tổng** | **24** | **Bao phủ toàn bộ** | |****
