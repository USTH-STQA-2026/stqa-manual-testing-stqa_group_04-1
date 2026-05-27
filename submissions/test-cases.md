# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).  
> Nguồn: SRS v1.0 — Hệ thống Quản lý Thư viện ABC  
> Kỹ thuật áp dụng: EP (Equivalence Partitioning) / BVA (Boundary Value Analysis) / Decision Table

| Thông tin | |
|---|---|
| **Nhóm** | `STQA-Group-04` |
| **Ngày tạo** | `15/05/2026` |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

> 📖 **Textbook:** Chương 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Trước khi viết Test Case**, nhóm **phải** phân tích miền đầu vào bằng bảng IDM bên dưới.  
> Mỗi chức năng cần xác định: **Đặc tính (Characteristic)**, **Phân vùng (Block/Partition)**, và **Giá trị đại diện (Value)**.

---

### IDM — Đăng nhập (REQ-01)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Email có tồn tại trong DB? | Có | `librarian@library.com` | Đăng nhập thành công |
| | Không | `noexist@test.com` | "Không tìm thấy thành viên" |
| Mật khẩu có đúng? | Đúng | `admin123` | Đăng nhập thành công |
| | Sai | `wrongpass` | "Mật khẩu không đúng" |
| Ô nhập có rỗng? | Không rỗng | (giá trị bất kỳ) | Xử lý bình thường |
| | Rỗng (cả 2 trường) | `""` / `""` | "Vui lòng nhập email và mật khẩu" |
| Vai trò sau đăng nhập | Thủ thư | `librarian@library.com` | AppBar hiển thị tên + "Thủ thư"; có tab Thành viên |
| | Thành viên | `ba.nguyen@email.com` | AppBar hiển thị tên + "Thành viên"; không có tab Thành viên |

---

### IDM — Xem danh sách sách (REQ-02)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Vai trò người xem | Thủ thư | `librarian@library.com` | Xem được tất cả 20 sách |
| | Thành viên | `ba.nguyen@email.com` | Xem được tất cả 20 sách |
| Trạng thái sách hiển thị | Có sẵn | BOOK001 | Hiển thị "Có sẵn" |
| | Đã mượn | BOOK003 | Hiển thị "Đã mượn" |
| | Thất lạc | BOOK007, BOOK020 | Hiển thị "Thất lạc" |
| Cập nhật real-time | Sau khi mượn | BOOK001 vừa được mượn | Trạng thái chuyển sang "Đã mượn" ngay |
| | Sau khi trả | BOOK003 vừa được trả | Trạng thái chuyển sang "Có sẵn" ngay |

---

### IDM — Tìm kiếm và lọc sách (REQ-03)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Từ khóa có tồn tại trong DB? | Có (tên sách) | `"Flutter"` | Hiển thị BOOK001 |
| | Có (tên tác giả) | `"Nguyễn Minh Đức"` | Hiển thị BOOK001, BOOK009 |
| | Không | `"xyznotexist999"` | "Không tìm thấy sách" |
| Phân biệt HOA/thường? | Chữ thường | `"flutter"` | Kết quả giống `"Flutter"` |
| | Chữ HOA | `"FLUTTER"` | Kết quả giống `"Flutter"` |
| | Hỗn hợp | `"fLuTtEr"` | Kết quả giống `"Flutter"` |
| Lọc theo thể loại | Thể loại có sách | `"Kinh tế"` | Chỉ hiển thị BOOK007, BOOK014, BOOK015 |
| | Kết hợp tìm kiếm + lọc | keyword=`"Lý"` + lọc=`"Công nghệ"` | Chỉ hiển thị BOOK008, BOOK011 |

---

### IDM — Mượn sách (REQ-04)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Trạng thái sách | Có sẵn | BOOK001 | Cho phép mượn |
| | Đã mượn | BOOK003 | Từ chối — thông báo lỗi sách không có sẵn |
| | Thất lạc | BOOK007 | Từ chối — thông báo lỗi sách không có sẵn |
| Trạng thái thành viên | Hoạt động | MEM002 (`ba.nguyen`) | Cho phép mượn |
| | Tạm ngưng | MEM004 (`cu.le`) | Từ chối — thông báo "Tạm ngưng" (không phải "Hết hạn") |
| | Hết hạn | MEM005 (`binh.pham`) | Từ chối — thông báo "Hết hạn" (không phải "Tạm ngưng") |
| Số sách đang mượn (BVA) | 0 (min) | MEM002 — 0 sách đang mượn | Cho phép mượn |
| | 1 | MEM002 — 1 sách đang mượn | Cho phép mượn |
| | 2 (max−1) | MEM002 — 2 sách đang mượn | Cho phép mượn — đây là sách thứ 3 |
| | 3 (max = biên) | MEM đã mượn đủ 3 sách | Từ chối — "Đã đạt giới hạn 3 sách" |

---

### IDM — Trả sách (REQ-05)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Thành viên trả đúng phiếu của mình? | Đúng | MEM002 trả BR001 | Trả thành công, sách về "Có sẵn" |
| Tình trạng phiếu khi trả | Còn hạn | BR003 (hạn 15/10/2024) | Trả thành công, không cảnh báo |
| | Quá hạn | BR001 (hạn 15/09/2024) | Trả thành công nhưng hiển thị cảnh báo quá hạn |

---

### IDM — Xử lý quá hạn (REQ-06)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Ai kích hoạt "Kiểm tra quá hạn"? | Thủ thư | `librarian@library.com` | Nút hiển thị, hoạt động |
| | Thành viên | `ba.nguyen@email.com` | Nút không xuất hiện |
| Phiếu có quá hạn không? | Quá hạn (dueDate ≤ hôm nay) | BR001 (hạn 15/09/2024) | Chuyển trạng thái sang "Quá hạn" |
| | Còn hạn (dueDate > hôm nay) | BR003 (hạn 15/10/2024, nếu còn hạn) | Giữ nguyên "Đang mượn" |

---

### IDM — Quản lý thành viên (REQ-07)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Họ tên | Hợp lệ | `"Nguyễn Test"` | Chấp nhận |
| | Rỗng | `""` | Thông báo lỗi thiếu thông tin |
| Email | Hợp lệ | `test.new@gmail.com` | Chấp nhận |
| | Thiếu dấu `.` trong domain | `user@domain` | Thông báo email không hợp lệ |
| | Thiếu `@` | `userdomain.com` | Thông báo email không hợp lệ |
| | Đã tồn tại trong DB | `ba.nguyen@email.com` | "Email đã tồn tại" |
| Số điện thoại (BVA, đúng 10 số) | 9 số (min−1) | `091234567` | Thông báo định dạng không đúng |
| | 10 số (đúng biên) | `0912345678` | Chấp nhận |
| | 11 số (max+1) | `09123456789` | Thông báo định dạng không đúng |
| Quyền truy cập form | Thủ thư | `librarian@library.com` | Thấy tab Thành viên + nút Thêm mới |
| | Thành viên | `ba.nguyen@email.com` | Không thấy tab Thành viên |

---

### IDM — Tra cứu phiếu mượn (REQ-08)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Vai trò tra cứu | Thủ thư | `librarian@library.com` | Thấy tất cả phiếu của mọi thành viên (BR001–BR005) |
| | Thành viên | `ba.nguyen@email.com` (MEM002) | Chỉ thấy BR001, BR004 — không thấy BR003 (MEM006) |
| Thông tin phiếu hiển thị | Đầy đủ | BR001 | Mã phiếu, sách mượn, ngày mượn, ngày hết hạn, trạng thái |

---

## Bước 2: Test Cases

> 💡 Mỗi TC ánh xạ về ít nhất 1 dòng IDM ở Bước 1. Dữ liệu test lấy từ **Seed Data SRS mục 3** — không dùng data tự bịa.

---

### Nhóm 1 — Đăng nhập (REQ-01) | 5 TC | Kỹ thuật: Decision Table

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|----------------|----------------|-----------------|------------------|-----|-----------|
| TC-01 | Đăng nhập thành công với vai trò Thủ thư | Hệ thống đã load, đang ở trang đăng nhập | 1. Mở https://stqa.rbc.vn 2. Nhập email 3. Nhập mật khẩu 4. Nhấn **Đăng nhập** | Email: `librarian@library.com` Mật khẩu: `admin123` | Chuyển sang trang chủ. AppBar hiển thị tên người dùng + vai trò **"Thủ thư"**. Có tab **Thành viên** trong menu. | REQ-01 | Decision Table |
| TC-02 | Đăng nhập thành công với vai trò Thành viên | Hệ thống đã load, đang ở trang đăng nhập | 1. Mở https://stqa.rbc.vn 2. Nhập email 3. Nhập mật khẩu 4. Nhấn **Đăng nhập** | Email: `ba.nguyen@email.com` Mật khẩu: `password123` | Chuyển sang trang chủ. AppBar hiển thị tên + vai trò **"Thành viên"**. **Không** có tab Thành viên trong menu. | REQ-01 | Decision Table |
| TC-03 | Từ chối — email không tồn tại trong hệ thống | Đang ở trang đăng nhập | 1. Nhập email không có trong DB 2. Nhập mật khẩu bất kỳ 3. Nhấn **Đăng nhập** | Email: `noexist@test.com` Mật khẩu: `abc123` | Hệ thống hiển thị thông báo lỗi: **"Không tìm thấy thành viên"**. Không chuyển trang. | REQ-01 | Decision Table |
| TC-04 | Từ chối — mật khẩu sai (email đúng) | Đang ở trang đăng nhập | 1. Nhập email hợp lệ 2. Nhập mật khẩu sai 3. Nhấn **Đăng nhập** | Email: `ba.nguyen@email.com` Mật khẩu: `wrongpass` | Hệ thống hiển thị thông báo lỗi: **"Mật khẩu không đúng"** — khác với thông báo khi email sai. Không chuyển trang. | REQ-01 | Decision Table |
| TC-05 | Từ chối — bỏ trống cả email lẫn mật khẩu | Đang ở trang đăng nhập | 1. Không nhập gì vào cả 2 ô 2. Nhấn **Đăng nhập** | Email: `""` (rỗng) Mật khẩu: `""` (rỗng) | Hệ thống hiển thị thông báo: **"Vui lòng nhập email và mật khẩu"**. Không chuyển trang. | REQ-01 | Decision Table |

---

### Nhóm 2 — Xem danh sách sách (REQ-02) | 3 TC | Kỹ thuật: EP

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|----------------|----------------|-----------------|------------------|-----|-----------|
| TC-06 | Hiển thị đầy đủ danh sách và thông tin từng sách | Đã đăng nhập với bất kỳ vai trò nào | 1. Đăng nhập 2. Vào tab **Sách** 3. Quan sát danh sách và thông tin từng sách | Tài khoản: `librarian@library.com` / `admin123` | Hiển thị đúng 20 sách từ Seed Data. Mỗi sách có đủ: tên sách, tác giả, thể loại, năm xuất bản, trạng thái. BOOK007 và BOOK020 hiển thị trạng thái **"Thất lạc"**. BOOK003 hiển thị **"Đã mượn"**. | REQ-02 | EP |
| TC-07 | Trạng thái sách cập nhật real-time sau khi mượn | Đã đăng nhập `ba.nguyen`, BOOK001 đang ở trạng thái **"Có sẵn"** | 1. Đăng nhập `ba.nguyen` 2. Vào tab **Sách** — ghi nhận BOOK001 đang "Có sẵn" 3. Nhấn **Mượn** trên BOOK001 4. Quay lại tab **Sách** — quan sát BOOK001 | Tài khoản: `ba.nguyen@email.com` / `password123` Sách mượn: BOOK001 | Sau khi mượn, BOOK001 **lập tức** chuyển sang trạng thái **"Đã mượn"** trên danh sách — không cần refresh trang. | REQ-02 | EP |
| TC-08 | Thành viên cũng xem được đầy đủ danh sách sách | Đã đăng nhập với tài khoản Thành viên | 1. Đăng nhập `ba.nguyen` 2. Vào tab **Sách** 3. Đếm số sách hiển thị | Tài khoản: `ba.nguyen@email.com` / `password123` | Thành viên thấy toàn bộ 20 sách, đủ thông tin như Thủ thư — không có sách nào bị ẩn. | REQ-02 | EP |

---

### Nhóm 3 — Tìm kiếm và lọc sách (REQ-03) | 4 TC | Kỹ thuật: EP

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|----------------|----------------|-----------------|------------------|-----|-----------|
| TC-09 | Tìm kiếm case-insensitive theo tên sách | Đã đăng nhập, đang ở tab **Sách** | 1. Nhập từ khóa vào ô tìm kiếm 2. Quan sát kết quả | Từ khóa: `flutter` (chữ thường) | Hiển thị **BOOK001 — "Lập trình Flutter cơ bản"**. Kết quả giống hệt khi nhập `"Flutter"` hoặc `"FLUTTER"`. | REQ-03 | EP |
| TC-10 | Tìm kiếm theo tên tác giả | Đã đăng nhập, đang ở tab **Sách** | 1. Nhập tên tác giả vào ô tìm kiếm 2. Quan sát kết quả | Từ khóa: `Nguyễn Minh Đức` | Hiển thị **BOOK001** (Lập trình Flutter cơ bản) và **BOOK009** (Nhập môn lập trình Python) — cả 2 cùng tác giả. | REQ-03 | EP |
| TC-11 | Tìm kiếm không có kết quả — hiển thị thông báo phù hợp | Đã đăng nhập, đang ở tab **Sách** | 1. Nhập từ khóa không khớp sách nào 2. Quan sát kết quả | Từ khóa: `xyznotexist999` | Danh sách trống, hiển thị thông báo **"Không tìm thấy sách"**. | REQ-03 | EP |
| TC-12 | Lọc theo thể loại — chỉ hiện đúng sách thuộc thể loại đó | Đã đăng nhập, đang ở tab **Sách** | 1. Chọn thể loại **"Kinh tế"** từ bộ lọc 2. Quan sát danh sách | Bộ lọc: `Kinh tế` | Chỉ hiển thị **3 sách**: BOOK007 (Kinh tế vi mô), BOOK014 (Kinh tế vĩ mô), BOOK015 (Nguyên lý kế toán). Không có sách thể loại khác. | REQ-03 | EP |

---

### Nhóm 4 — Mượn sách (REQ-04) | 6 TC | Kỹ thuật: Decision Table + BVA

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|----------------|----------------|-----------------|------------------|-----|-----------|
| TC-13 | Mượn sách thành công — đủ mọi điều kiện | `ba.nguyen` (MEM002, Hoạt động) đã đăng nhập. BOOK001 đang "Có sẵn". `ba.nguyen` đang mượn < 3 sách. | 1. Đăng nhập `ba.nguyen` 2. Vào tab **Sách** → BOOK001 3. Nhấn **Mượn** 4. Vào tab **Mượn/Trả** — kiểm tra phiếu mới | Tài khoản: `ba.nguyen@email.com` / `password123` Sách: BOOK001 | Phiếu mượn mới được tạo thành công. Ngày hết hạn = **ngày hôm nay + 14 ngày**. Trạng thái phiếu: **"Đang mượn"**. BOOK001 chuyển sang **"Đã mượn"**. | REQ-04 | Decision Table |
| TC-14 | Từ chối mượn — sách đang ở trạng thái "Đã mượn" | `ba.nguyen` đã đăng nhập. BOOK003 đang **"Đã mượn"** (bởi MEM002 — BR001 từ Seed Data). | 1. Đăng nhập `ba.nguyen` 2. Vào tab **Sách** → BOOK003 3. Nhấn **Mượn** | Tài khoản: `ba.nguyen@email.com` / `password123` Sách: BOOK003 | Hệ thống từ chối và hiển thị thông báo lỗi: sách **không có sẵn** / đã được mượn. Không tạo phiếu mới. | REQ-04 | Decision Table |
| TC-15 | Từ chối mượn — tài khoản thành viên bị **Tạm ngưng** | `cu.le` (MEM004, **Tạm ngưng**) đã đăng nhập. BOOK001 đang "Có sẵn". | 1. Đăng nhập `cu.le` 2. Vào tab **Sách** → BOOK001 3. Nhấn **Mượn** | Tài khoản: `cu.le@email.com` / `password123` Sách: BOOK001 | Hệ thống từ chối và hiển thị thông báo lý do **"Tạm ngưng"** — thông báo phải khác với trường hợp "Hết hạn". Không tạo phiếu. | REQ-04 | Decision Table |
| TC-16 | Từ chối mượn — tài khoản thành viên đã **Hết hạn** | `binh.pham` (MEM005, **Hết hạn**) đã đăng nhập. BOOK001 đang "Có sẵn". | 1. Đăng nhập `binh.pham` 2. Vào tab **Sách** → BOOK001 3. Nhấn **Mượn** | Tài khoản: `binh.pham@email.com` / `password123` Sách: BOOK001 | Hệ thống từ chối và hiển thị thông báo lý do **"Hết hạn"** — thông báo phải khác với trường hợp "Tạm ngưng". Không tạo phiếu. | REQ-04 | Decision Table |
| TC-17 | **BVA max−1=2** — Cho phép mượn sách thứ 3 (tại biên hợp lệ) | `ba.nguyen` đã đăng nhập và **đang mượn đúng 2 sách**. Có sách "Có sẵn" để mượn thêm. | 1. Đăng nhập `ba.nguyen` 2. Mượn 1 sách (nếu chưa đủ 2) để đạt 2 sách đang mượn 3. Vào tab **Sách** → chọn sách Có sẵn (ví dụ BOOK002) 4. Nhấn **Mượn** | Tài khoản: `ba.nguyen@email.com` / `password123` Số sách đang mượn trước thao tác: **2** | Mượn **thành công** — phiếu mượn thứ 3 được tạo. Tổng đang mượn = **3**. Không có thông báo lỗi. | REQ-04 | BVA |
| TC-18 | **BVA max=3** — Từ chối mượn sách thứ 4 (vượt giới hạn) | `ba.nguyen` đã đăng nhập và **đang mượn đúng 3 sách** (đạt giới hạn). Có sách "Có sẵn". | 1. Đảm bảo `ba.nguyen` đang mượn đủ 3 sách 2. Vào tab **Sách** → chọn sách Có sẵn 3. Nhấn **Mượn** | Tài khoản: `ba.nguyen@email.com` / `password123` Số sách đang mượn trước thao tác: **3** | Hệ thống từ chối và hiển thị thông báo: **"Đã đạt giới hạn 3 sách"** (hoặc tương đương). Không tạo phiếu mượn thứ 4. | REQ-04 | BVA |

---

### Nhóm 5 — Trả sách (REQ-05) | 3 TC | Kỹ thuật: EP

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|----------------|----------------|-----------------|------------------|-----|-----------|
| TC-19 | Trả sách thành công — phiếu còn trong hạn | `biet.hoang` (MEM006) đã đăng nhập. BR003 (BOOK013, hạn 15/10/2024) đang **"Đang mượn"** và còn trong hạn. | 1. Đăng nhập `biet.hoang` 2. Vào tab **Mượn/Trả** 3. Chọn phiếu BR003 4. Nhấn **Trả** 5. Kiểm tra trạng thái BOOK013 | Tài khoản: `biet.hoang@email.com` / `password123` Phiếu: BR003 — BOOK013 | Trả thành công. BR003 chuyển sang trạng thái **"Đã trả"**. BOOK013 chuyển về **"Có sẵn"** trong danh sách sách. Không có cảnh báo quá hạn. | REQ-05 | EP |
| TC-20 | Trả sách quá hạn — hệ thống hiển thị cảnh báo | `ba.nguyen` (MEM002) đã đăng nhập. BR001 (BOOK003, hạn 15/09/2024) đang **quá hạn thực tế** (nhưng chưa được đánh dấu). | 1. Đăng nhập `librarian` → nhấn **"Kiểm tra quá hạn"** để đánh dấu BR001 2. Đăng xuất → Đăng nhập `ba.nguyen` 3. Vào tab **Mượn/Trả** 4. Chọn BR001 → Nhấn **Trả** | Tài khoản: `ba.nguyen@email.com` / `password123` Phiếu: BR001 — BOOK003 (hạn 15/09/2024) | Hệ thống **hiển thị cảnh báo quá hạn** trước khi xác nhận. Sau khi xác nhận trả: BR001 → **"Đã trả"**, BOOK003 → **"Có sẵn"**. | REQ-05 | EP |
| TC-21 | Trả sách xong — sách có thể được mượn lại ngay | `ba.nguyen` đã trả BOOK003 (BR001) ở TC-20. BOOK003 đang "Có sẵn". | 1. Sau khi trả BOOK003 2. Vào tab **Sách** 3. Thử nhấn **Mượn** trên BOOK003 | Tài khoản: `ba.nguyen@email.com` / `password123` Sách: BOOK003 (vừa được trả) | BOOK003 có thể được mượn lại bình thường — hệ thống tạo phiếu mượn mới thành công. | REQ-05 | EP |

---

### Nhóm 6 — Xử lý quá hạn (REQ-06) | 3 TC | Kỹ thuật: EP

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|----------------|----------------|-----------------|------------------|-----|-----------|
| TC-22 | Thủ thư nhấn "Kiểm tra quá hạn" — phiếu quá hạn được đánh dấu đúng | `librarian` đã đăng nhập. BR001 (hạn 15/09/2024) đang ở trạng thái **"Đang mượn"** (chưa bị đánh dấu quá hạn). | 1. Đăng nhập `librarian` 2. Nhấn nút **"Kiểm tra quá hạn"** 3. Vào tab **Mượn/Trả** — quan sát trạng thái BR001 | Tài khoản: `librarian@library.com` / `admin123` | BR001 (hạn 15/09/2024 ≤ ngày hiện tại) **chuyển sang trạng thái "Quá hạn"**. BR003 (hạn 15/10/2024, nếu còn hạn) giữ nguyên **"Đang mượn"**. | REQ-06 | EP |
| TC-23 | Thành viên không có nút "Kiểm tra quá hạn" | `ba.nguyen` đã đăng nhập (vai trò Thành viên) | 1. Đăng nhập `ba.nguyen` 2. Tìm kiếm nút **"Kiểm tra quá hạn"** trên giao diện (tab Mượn/Trả và các nơi khác) | Tài khoản: `ba.nguyen@email.com` / `password123` | Giao diện **không hiển thị** nút "Kiểm tra quá hạn" ở bất kỳ đâu — chức năng được ẩn hoàn toàn với Thành viên. | REQ-06 | EP |
| TC-24 | Thành viên thấy phiếu quá hạn của mình sau khi Thủ thư kiểm tra | `librarian` đã nhấn "Kiểm tra quá hạn" → BR001 đã được đánh dấu quá hạn. `ba.nguyen` (MEM002 — chủ phiếu BR001) chưa đăng nhập. | 1. (Đã thực hiện kiểm tra quá hạn ở TC-22) 2. Đăng xuất Thủ thư 3. Đăng nhập `ba.nguyen` 4. Vào tab **Mượn/Trả** — quan sát trạng thái BR001 | Tài khoản: `ba.nguyen@email.com` / `password123` | `ba.nguyen` thấy BR001 với trạng thái **"Quá hạn"** trong danh sách phiếu của mình. | REQ-06 | EP |

---

### Nhóm 7 — Quản lý thành viên (REQ-07) | 4 TC | Kỹ thuật: EP + BVA

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|----------------|----------------|-----------------|------------------|-----|-----------|
| TC-25 | Thêm thành viên mới hợp lệ thành công | `librarian` đã đăng nhập. Email chưa tồn tại trong DB. | 1. Đăng nhập `librarian` 2. Vào tab **Thành viên** 3. Nhấn **Thêm mới** 4. Điền đầy đủ thông tin hợp lệ 5. Nhấn **Xác nhận / Tạo** 6. Kiểm tra danh sách thành viên | Họ tên: `Nguyễn Test` Email: `test.new@gmail.com` SĐT: `0912345678` | Thành viên mới **xuất hiện trong danh sách**, trạng thái **"Hoạt động"**. Thông tin hiển thị đúng như đã nhập. | REQ-07 | EP |
| TC-26 | Từ chối — email không có dấu `.` trong phần domain | `librarian` đã đăng nhập, đang mở form thêm thành viên. | 1. Điền form với email thiếu dấu chấm trong domain 2. Các trường khác nhập hợp lệ 3. Nhấn **Tạo** | Họ tên: `Test User` Email: `user@domain` (thiếu dấu `.`) SĐT: `0912345678` | Hệ thống hiển thị thông báo **email không hợp lệ**. Không tạo thành viên mới. (Theo SRS: `user@domain` là KHÔNG hợp lệ — phải có dấu `.` trong domain.) | REQ-07 | EP |
| TC-27 | Từ chối — email đã tồn tại trong hệ thống | `librarian` đã đăng nhập. Email `ba.nguyen@email.com` đã có trong DB (MEM002). | 1. Điền form với email đã tồn tại 2. Các trường khác nhập hợp lệ 3. Nhấn **Tạo** | Họ tên: `Test Trùng` Email: `ba.nguyen@email.com` (đã tồn tại) SĐT: `0922222222` | Hệ thống hiển thị thông báo **email đã tồn tại** (hoặc tương đương). Không tạo thành viên mới. | REQ-07 | EP |
| TC-28 | **BVA SĐT** — Số điện thoại 9 chữ số (dưới biên min=10) bị từ chối | `librarian` đã đăng nhập, đang mở form thêm thành viên. | 1. Điền form với SĐT chỉ 9 số 2. Các trường khác nhập hợp lệ 3. Nhấn **Tạo** | Họ tên: `Test BVA` Email: `testbva@gmail.com` SĐT: `091234567` (9 số — min−1) | Hệ thống hiển thị thông báo lỗi **"Số điện thoại không đúng định dạng"** (hoặc tương đương). Không tạo thành viên. (SRS BR-09: đúng 10 số.) | REQ-07 | BVA |

---

### Nhóm 8 — Tra cứu phiếu mượn (REQ-08) | 3 TC | Kỹ thuật: EP

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|----------------|----------------|-----------------|------------------|-----|-----------|
| TC-29 | Thành viên chỉ thấy phiếu mượn của chính mình | `ba.nguyen` (MEM002) đã đăng nhập. DB có BR001, BR004 (của MEM002) và BR003 (của MEM006). | 1. Đăng nhập `ba.nguyen` 2. Vào tab **Mượn/Trả** 3. Kiểm tra danh sách phiếu | Tài khoản: `ba.nguyen@email.com` / `password123` | Thấy **BR001** (BOOK003) và **BR004** (BOOK005) — đều là phiếu của MEM002. **Không** thấy BR003 (của MEM006 — `biet.hoang`). | REQ-08 | EP |
| TC-30 | Thủ thư xem được tất cả phiếu mượn của mọi thành viên | `librarian` đã đăng nhập. DB có đủ BR001–BR005 từ Seed Data. | 1. Đăng nhập `librarian` 2. Vào tab **Mượn/Trả** 3. Đếm và xác nhận danh sách phiếu | Tài khoản: `librarian@library.com` / `admin123` | Thấy tất cả **5 phiếu** từ Seed Data: BR001, BR002, BR003, BR004, BR005 thuộc nhiều thành viên khác nhau. | REQ-08 | EP |
| TC-31 | Thông tin phiếu mượn hiển thị đầy đủ các trường | `librarian` đã đăng nhập, đang ở tab **Mượn/Trả**. | 1. Đăng nhập `librarian` 2. Vào tab **Mượn/Trả** 3. Chọn phiếu **BR001** — quan sát chi tiết thông tin | Tài khoản: `librarian@library.com` / `admin123` Phiếu kiểm tra: BR001 | BR001 hiển thị đầy đủ: **Mã phiếu** (BR001), **Sách mượn** (Kiểm thử phần mềm nhập môn — BOOK003), **Ngày mượn** (01/09/2024), **Ngày hết hạn** (15/09/2024), **Trạng thái** (Đang mượn hoặc Quá hạn). | REQ-08 | EP |

---

## Phụ lục — Decision Table: REQ-04 Mượn sách

> Bảng này mô hình hóa tất cả tổ hợp điều kiện ảnh hưởng đến quyết định cho phép/từ chối mượn sách.

|  | R1 (TC-13) | R2 (TC-14) | R3 (TC-15) | R4 (TC-16) | R5 (TC-17) | R6 (TC-18) |
|--|--|--|--|--|--|--|
| **ĐIỀU KIỆN** | | | | | | |
| Sách trạng thái "Có sẵn"? | ✓ | ✗ | ✓ | ✓ | ✓ | ✓ |
| Thành viên "Hoạt động"? | ✓ | ✓ | ✗ Tạm ngưng | ✗ Hết hạn | ✓ | ✓ |
| Số sách đang mượn < 3? | ✓ (=0) | ✓ | ✓ | ✓ | ✓ (=2, biên max−1) | ✗ (=3, biên max) |
| **HÀNH ĐỘNG** | | | | | | |
| Tạo phiếu mượn, hạn +14 ngày | ✓ | — | — | — | ✓ | — |
| Thông báo sách không có sẵn | — | ✓ | — | — | — | — |
| Thông báo tài khoản "Tạm ngưng" | — | — | ✓ | — | — | — |
| Thông báo tài khoản "Hết hạn" | — | — | — | ✓ | — | — |
| Thông báo "Đạt giới hạn 3 sách" | — | — | — | — | — | ✓ |
| **Tài khoản Seed Data** | `ba.nguyen` (MEM002) | `ba.nguyen` | `cu.le` (MEM004) | `binh.pham` (MEM005) | `ba.nguyen` — 2 sách | `ba.nguyen` — 3 sách |
| **Sách Seed Data** | BOOK001 | BOOK003 (Đã mượn) | BOOK001 | BOOK001 | BOOK002 | BOOK001 |

---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Loại (Happy/Neg/BVA) | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|----------------------|
| Đăng nhập | 5 | REQ-01 | 2 Happy / 3 Negative | Decision Table |
| Xem danh sách sách | 3 | REQ-02 | 3 Happy | EP |
| Tìm kiếm và lọc | 4 | REQ-03 | 3 Happy / 1 Negative | EP |
| Mượn sách | 6 | REQ-04 | 1 Happy / 3 Negative / 2 BVA | Decision Table + BVA |
| Trả sách | 3 | REQ-05 | 2 Happy / 1 Negative | EP |
| Xử lý quá hạn | 3 | REQ-06 | 2 Happy / 1 Negative | EP |
| Quản lý thành viên | 4 | REQ-07 | 1 Happy / 2 Negative / 1 BVA | EP + BVA |
| Tra cứu phiếu mượn | 3 | REQ-08 | 3 Happy | EP |
| **Tổng** | **31** | **8/8** | **17 Happy / 13 Neg / 3 BVA** | **EP, BVA, Decision Table** |

---

## Khai báo sử dụng AI

> *"Nhóm đã dùng Claude để hỗ trợ gợi ý cấu trúc và rà soát test case còn thiếu; nội dung cuối cùng được nhóm tự đối chiếu SRS và xác nhận với nhau."*