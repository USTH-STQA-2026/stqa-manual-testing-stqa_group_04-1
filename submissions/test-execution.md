# Test Execution — Kết quả thực thi kiểm thử

> **Hướng dẫn**: Chạy từng TC trên hệ thống https://stqa.rbc.vn, ghi lại kết quả thực tế.
> Kết luận: **Pass** (kết quả đúng), **Fail** (kết quả sai → tạo bug report), **Blocked** (không thực hiện được vì lỗi khác chặn), **Not Run** (chưa chạy).

| Thông tin | |
|---|---|
| **Nhóm** | `GROUP_04` |
| **Ngày thực thi** | ` 29/05/2026 ` |
| **Trình duyệt** | Chrome `148.0.7778.217(64 bit)` |
| **Hệ điều hành** | ` Windows ` |

---

## Kết quả chi tiết
### REQ-01/Login
| TC ID | Functional Group | Expected Result (Summary) |  Actual Result  | Actual Result  | Proof  | Bug |
|-------|------------------|---------------------------------------------------------|----------------------------------------------------------------|---------|-----------|----| 
| TC-01 |   Login   |  Navigate to the homepage. AppBar shows username and role "Thủ thư".         |  Navigate to the homepage. AppBar shows "Nguyễn Thủ Thư" and role "Thủ thư".The "Thành viên" tab is visible in the menu.         |**Pass**|-|  -   |
| TC-02 |   Login   |  Navigate to the homepage. AppBar shows username and role "Thành viên".      |  Navigate to the homepage. AppBar shows "Nguyễn Học Bá" and role "Thành viên".The "Thành viên" tab is not visible in the menu.   |**Pass**|-|  -   | 
| TC-03 |   Login   |  The system displays the error message "Không tìm thấy thành viên ".         |  The system displays the error message "Không tìm thấy thành viên".The page does not navigate.                                   |**Pass**|-|  -   | 
| TC-04 |   Login   |  The system displays the error message "Mật khẩu không đúng".                |  The system displays the error message "Mật khẩu không đúng".The page does not navigate.                                         |**Pass**|-|BUG-01|
| TC-05 |   Login   |  Displays "Vui lòng nhập email và mật khẩu".The page does not redirect.      | Displays "Vui lòng nhập email và mật khẩu".The page does not redirect.                                                           |**Pass**|-|  -   | 
### REQ_02/View Book List
| TC ID | Functional Group | Expected Result (Summary) |  Actual Result  | Actual Result  | Proof  | Bug |
|-------|------------------|---------------------------------------------------------|----------------------------------------------------------------|---------|-----------|----| 
| TC-06 |  View Book List  | Displays the list of books from the seed data.Each book shows: title, author, genre, publication year, and status("Có sẵn","Thất lạc","Đang mượn")  | Displays all 20 books with 5 fields (Title, Author, Genre, Publication Year, Status).BOOK007 and BOOK020 display status "Thất lạc". BOOK003 and BOOK013 displays "Đang mượn". |**Pass**|-|  -  |  
| TC-07 |  View Book List  |  Navigate to the homepage. AppBar shows username and role "Librarian".       |  Navigate to the homepage. AppBar shows username and role "Librarian".The Members tab is visible in the menu.  |**Pass**|-|  -  |  
| TC-08 |  View Book List  | The member can view all 20 books with complete information, the same as the librarian — no books are hidden. | The member can view all 20 books with complete information, the same as the librarian — no books are hidden. |**Pass**|-|  -  |  

  
---

## Tổng hợp kết quả

| Chỉ số | Giá trị |
|--------|---------|
| Tổng số test case | `<!-- số -->` |
| Pass | `<!-- số -->` |
| Fail | `<!-- số -->` |
| Blocked | `<!-- số -->` |
| Not Run | `<!-- số -->` |
| **Tỷ lệ Pass** | `<!-- xx% -->` |

### Kết quả theo nhóm chức năng

| Nhóm | Tổng TC | Pass | Fail | Tỷ lệ Pass |
|------|---------|------|------|------------|
| | | | | |
