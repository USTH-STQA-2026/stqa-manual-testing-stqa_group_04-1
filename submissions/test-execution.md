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
| TC-04 |   Login   |  The system displays the error message "Mật khẩu không đúng" in the **Password** field                |  The system displays the error message "Mật khẩu không đúng" in the **Email** field.The page does not navigate.                                         |**Fail**|-|BUG|
| TC-05 |   Login   |  Displays "Vui lòng nhập email và mật khẩu".The page does not redirect.      | Displays "Vui lòng nhập email và mật khẩu".The page does not redirect.                                                           |**Pass**|-|  -   | 
### REQ-02/View Book List
| TC ID | Functional Group | Expected Result (Summary) |  Actual Result  | Actual Result  | Proof  | Bug |
|-------|------------------|---------------------------------------------------------|----------------------------------------------------------------|---------|-----------|----| 
| TC-06 |  View Book List  | Displays the list of books from the seed data.Each book shows: title, author, genre, publication year, and status("Có sẵn","Thất lạc","Đang mượn")  | Displays all 20 books with 5 fields (Title, Author, Genre, Publication Year, Status).BOOK007 and BOOK020 display status "Thất lạc". BOOK003 and BOOK013 displays "Đang mượn". |**Pass**|-|  -  |  
| TC-07 |  View Book List  |  Navigate to the homepage. AppBar shows username and role "Librarian".       |  Navigate to the homepage. AppBar shows username and role "Librarian".The Members tab is visible in the menu.  |**Pass**|-|  -  |  
| TC-08 |  View Book List  | The member can view all 20 books with complete information, the same as the librarian — no books are hidden. | The member can view all 20 books with complete information, the same as the librarian — no books are hidden. |**Pass**|-|  -  |  
### REQ-03/Search and Filter Books 
| TC ID | Functional Group | Expected Result (Summary) |  Actual Result  | Actual Result  | Proof  | Bug |
|-------|------------------|---------------------------------------------------------|----------------------------------------------------------------|---------|-----------|----| 
| TC-09 |  Search and Filter Books   |  Shows BOOK001 — "Lập trình Flutter cơ bản".Result is the same as entering `"Flutter"` or `"FLUTTER"`.  | Only displays BOOK001 "Lập trình Flutter cơ bản". Other books are hidden.Result is the same as entering `"Flutter"` or `"FLUTTER"` |**Pass**|-|  -   |
| TC-10 |  Search and Filter Books   |  Shows BOOK001 and BOOK009 — both by the same author when searching "Nguyễn Minh Đức"  |  Displays BOOK001 and BOOK009. Books by other authors are hidden.  |**Pass**|-|  -   | 
| TC-11 |  Search and Filter Books   |  List is empty, shows message "No books found".         |   List is empty, shows message "No books found".  |**Pass**|-|  -   | 
| TC-12 |  Search and Filter Books   |  Shows only 3 books: BOOK007 (Kinh tế vi mô), BOOK014 (Kinh tế vĩ mô), BOOK015 (Nguyên lý kế toán). No books from other categories.| Shows only **3 books**: BOOK007, BOOK014, BOOK015. |**Pass**|-|  -   | 
| TC-33 |  Search and Filter Books   |  Shows only 3 books: BOOK007 (Kinh tế vi mô), BOOK014 (Kinh tế vĩ mô), BOOK015 (Nguyên lý kế toán). No books from other categories.|  List is empty, shows message "No books found". |-|**Fail**|BUG |   
### REQ-04/Borrow Book
| TC ID | Functional Group | Expected Result (Summary) |  Actual Result  | Actual Result  | Proof  | Bug |
|-------|------------------|---------------------------------------------------------|----------------------------------------------------------------|---------|-----------|----| 
| TC-13 |  Borrow Book   |   New borrow slip created successfully. Due date = **today + 14 days**. Slip status: **"Borrowed"**. BOOK001 changes to **"Borrowed"**. |New borrow slip created successfully. Due date = **today + 14 days**. Slip status: **"Borrowed"**. BOOK001 changes to **"Borrowed"**. |**Pass**|-|  -   |
| TC-14 |  Borrow Book   |  System rejects and shows error message: book is **unavailable** / already borrowed. No new slip created. | BOOK003 does not have the (+) button,the borrow action cannot be performed.No error message is displayed. |**Fail**|-|  BUG   | 
| TC-15 |  Borrow Book   |  System rejects and shows reason **"Suspended"** — message must be different from the "Expired" case. No slip created.| System rejects and shows error message:"Thành viên đã hết hạn. Không thể mượn sách."-the same message as the "Expired" case. |**Fail**|-|  BUG   | 
| TC-16 |  Borrow Book   |  System rejects and shows reason **"Expired"**. No slip created.| System rejects and shows error message:"Thành viên đã hết hạn. Không thể mượn sách." |**Pass**|-|-|
| TC-17 |  Borrow Book   |  Borrow **successful** — 3rd slip created. Total currently borrowed = **3**. No error message.|  Borrow **successful** — 3rd slip created. Total currently borrowed = **3**. No error message. |**Pass**|-|  -   | 
| TC-18 |  Borrow Book   |  System rejects and shows message: **"3-book limit reached"** (or equivalent). No 4th slip created.|System rejects and shows message: **"3-book limit reached"** (or equivalent). No 4th slip created. |-|**Pass**|- |
| TC-34 |  Borrow Book   |  Shows reason "Thành viên đã hết hạn. Không thể mượn sách." in VI or "The member account has expired. Borrowing books is not allowed." in EN|Shows reason "Thành viên đã hết hạn. Không thể mượn sách." in VI and EN |**Fail**|-|BUG |
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
