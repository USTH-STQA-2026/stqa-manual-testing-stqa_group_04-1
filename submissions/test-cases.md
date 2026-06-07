# Test Cases — Test Case Table

> **Instructions**: Write a minimum of **20 TCs** covering all main features (REQ-01 → REQ-08).  
> Source: SRS v1.0 — ABC Library Management System  
> Techniques applied: EP (Equivalence Partitioning) / BVA (Boundary Value Analysis) / Decision Table

| Information | |
|---|---|
| **Group** | `STQA-Group-04` |
| **Date created** | `15/05/2026` |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

> 📖 **Textbook:** Chapter 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Before writing Test Cases**, the group **must** analyze the input domain using the IDM table below.  
> Each feature must identify: **Characteristic**, **Block/Partition**, and **Representative Value**.

---

### IDM — Login (REQ-01)

| Characteristic | Block (Partition) | Representative Value | Expected Result |
|---|---|---|---|
| Does the email exist in the DB? | Yes | `librarian@library.com` | Login successful |
| | No | `noexist@test.com` | "Member not found" |
| Is the password correct? | Correct | `admin123` | Login successful |
| | Incorrect | `wrongpass` | "Incorrect password" |
| Is the input field empty? | Not empty | (any value) | Processed normally |
| | Empty (both fields) | `""` / `""` | "Please enter email and password" |
| Role after login | Librarian | `librarian@library.com` | AppBar shows name + "Librarian"; Members tab visible |
| | Member | `ba.nguyen@email.com` | AppBar shows name + "Member"; no Members tab |

---

### IDM — View Book List (REQ-02)

| Characteristic | Block (Partition) | Representative Value | Expected Result |
|---|---|---|---|
| Viewer role | Librarian | `librarian@library.com` | Can view all 20 books |
| | Member | `ba.nguyen@email.com` | Can view all 20 books |
| Displayed book status | Available | BOOK001 | Shows "Available" |
| | Borrowed | BOOK003 | Shows "Borrowed" |
| | Lost | BOOK007, BOOK020 | Shows "Lost" |
| Real-time update | After borrowing | BOOK001 just borrowed | Status immediately changes to "Borrowed" |
| | After returning | BOOK003 just returned | Status immediately changes to "Available" |

---

### IDM — Search and Filter Books (REQ-03)

| Characteristic | Block (Partition) | Representative Value | Expected Result |
|---|---|---|---|
| Does the keyword exist in the DB? | Yes (book title) | `"Flutter"` | Shows BOOK001 |
| | Yes (author name) | `"Nguyen Minh Duc"` | Shows BOOK001, BOOK009 |
| | No | `"xyznotexist999"` | "No books found" |
| Case-sensitive? | Lowercase | `"flutter"` | Same result as `"Flutter"` |
| | Uppercase | `"FLUTTER"` | Same result as `"Flutter"` |
| | Mixed case | `"fLuTtEr"` | Same result as `"Flutter"` |
| Filter by category | Category has books | `"Economics"` | Shows only BOOK007, BOOK014, BOOK015 |
| | Combined search + filter | keyword=`"Ly"` + filter=`"Technology"` | Shows only BOOK008, BOOK011 |

---

### IDM — Borrow Book (REQ-04)

| Characteristic | Block (Partition) | Representative Value | Expected Result |
|---|---|---|---|
| Book status | Available | BOOK001 | Borrowing allowed |
| | Borrowed | BOOK003 | Rejected — book unavailable error message |
| | Lost | BOOK007 | Rejected — book unavailable error message |
| Member status | Active | MEM002 (`ba.nguyen`) | Borrowing allowed |
| | Suspended | MEM004 (`cu.le`) | Rejected — "Suspended" message (not "Expired") |
| | Expired | MEM005 (`binh.pham`) | Rejected — "Expired" message (not "Suspended") |
| Number of books currently borrowed (BVA) | 0 (min) | MEM002 — 0 books borrowed | Borrowing allowed |
| | 1 | MEM002 — 1 book borrowed | Borrowing allowed |
| | 2 (max−1) | MEM002 — 2 books borrowed | Borrowing allowed — this is the 3rd book |
| | 3 (max = boundary) | MEM with 3 books borrowed | Rejected — "3-book limit reached" |

---

### IDM — Return Book (REQ-05)

| Characteristic | Block (Partition) | Representative Value | Expected Result |
|---|---|---|---|
| Member returning the correct slip? | Correct | MEM002 returns BR001 | Return successful, book back to "Available" |
| Slip status at return | Within due date | BR003 (due 15/10/2024) | Return successful, no warning |
| | Overdue | BR001 (due 15/09/2024) | Return successful but overdue warning shown |

---

### IDM — Overdue Processing (REQ-06)

| Characteristic | Block (Partition) | Representative Value | Expected Result |
|---|---|---|---|
| Who triggers "Check Overdue"? | Librarian | `librarian@library.com` | Button visible and functional |
| | Member | `ba.nguyen@email.com` | Button not visible |
| Is the slip overdue? | Overdue (dueDate ≤ today) | BR001 (due 15/09/2024) | Status changes to "Overdue" |
| | Not overdue (dueDate > today) | BR003 (due 15/10/2024, if still valid) | Remains "Borrowed" |

---

### IDM — Member Management (REQ-07)

| Characteristic | Block (Partition) | Representative Value | Expected Result |
|---|---|---|---|
| Full name | Valid | `"Nguyen Test"` | Accepted |
| | Empty | `""` | Missing information error message |
| Email | Valid | `test.new@gmail.com` | Accepted |
| | Missing `.` in domain | `user@domain` | Invalid email message |
| | Missing `@` | `userdomain.com` | Invalid email message |
| | Already exists in DB | `ba.nguyen@email.com` | "Email already exists" |
| Phone number (BVA, exactly 10 digits) | 9 digits (min−1) | `091234567` | Invalid format message |
| | 10 digits (exact boundary) | `0912345678` | Accepted |
| | 11 digits (max+1) | `09123456789` | Invalid format message |
| Form access rights | Librarian | `librarian@library.com` | Members tab + Add New button visible |
| | Member | `ba.nguyen@email.com` | Members tab not visible |

---

### IDM — Borrow Slip Lookup (REQ-08)

| Characteristic | Block (Partition) | Representative Value | Expected Result |
|---|---|---|---|
| Lookup role | Librarian | `librarian@library.com` | Sees all slips from all members (BR001–BR005) |
| | Member | `ba.nguyen@email.com` (MEM002) | Sees only BR001, BR004 — not BR003 (MEM006) |
| Slip information displayed | Complete | BR001 | Slip ID, book borrowed, borrow date, due date, status |

---

## Step 2: Test Cases

> 💡 Each TC maps to at least 1 IDM row from Step 1. Test data is taken from **Seed Data SRS Section 3** — no fabricated data.

---

### Group 1 — Login (REQ-01) | 5 TCs | Technique: Decision Table

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-01 | Successful login with Librarian role | System loaded, on login page | 1. Open https://stqa.rbc.vn 2. Enter email 3. Enter password 4. Click **Login** | Email: `librarian@library.com` Password: `admin123` | Redirects to homepage. AppBar shows username + role **"Librarian"**. **Members** tab is visible in menu. | REQ-01 | Decision Table |
| TC-02 | Successful login with Member role | System loaded, on login page | 1. Open https://stqa.rbc.vn 2. Enter email 3. Enter password 4. Click **Login** | Email: `ba.nguyen@email.com` Password: `password123` | Redirects to homepage. AppBar shows name + role **"Member"**. **No** Members tab in menu. | REQ-01 | Decision Table |
| TC-03 | Reject — email does not exist in the system | On login page | 1. Enter email not in DB 2. Enter any password 3. Click **Login** | Email: `noexist@test.com` Password: `abc123` | System shows error message: **"Member not found"**. Page does not change. | REQ-01 | Decision Table |
| TC-04 | Reject — wrong password (correct email) | On login page | 1. Enter valid email 2. Enter wrong password 3. Click **Login** | Email: `ba.nguyen@email.com` Password: `wrongpass` | System shows error message: **"Incorrect password"** in the **Password** field  — different from the message for wrong email. Page does not change. | REQ-01 | Decision Table |
| TC-05 | Reject — both email and password left empty | On login page | 1. Leave both fields blank 2. Click **Login** | Email: `""` (empty) Password: `""` (empty) | System shows message: **"Please enter email and password"**. Page does not change. | REQ-01 | Decision Table |

---

### Group 2 — View Book List (REQ-02) | 3 TCs | Technique: EP

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-06 | Display full book list with each book's details | Logged in with any role | 1. Log in 2. Go to **Books** tab 3. Observe the list and book details | Account: `librarian@library.com` / `admin123` | Displays exactly 20 books from Seed Data. Each book shows: title, author, category, publication year, status. BOOK007 and BOOK020 show status **"Lost"**. BOOK003 shows **"Borrowed"**. | REQ-02 | EP |
| TC-07 | Book status updates in real-time after borrowing | Logged in as `ba.nguyen`, BOOK001 currently **"Available"** | 1. Log in as `ba.nguyen` 2. Go to **Books** tab — note BOOK001 is "Available" 3. Click **Borrow** on BOOK001 4. Return to **Books** tab — observe BOOK001 | Account: `ba.nguyen@email.com` / `password123` Book borrowed: BOOK001 | After borrowing, BOOK001 **immediately** changes status to **"Borrowed"** in the list — no page refresh required. | REQ-02 | EP |
| TC-08 | Member can also view the full book list | Logged in with Member account | 1. Log in as `ba.nguyen` 2. Go to **Books** tab 3. Count number of books displayed | Account: `ba.nguyen@email.com` / `password123` | Member sees all 20 books with the same information as the Librarian — no books are hidden. | REQ-02 | EP |

---

### Group 3 — Search and Filter Books (REQ-03) | 5 TCs | Technique: EP

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-09 | Case-insensitive search by book title | Logged in, on **Books** tab | 1. Enter keyword in search box 2. Observe results | Keyword: `flutter` (lowercase) | Shows **BOOK001 — "Basic Flutter Programming"**. Result is the same as entering `"Flutter"` or `"FLUTTER"`. | REQ-03 | EP |
| TC-10 | Search by author name | Logged in, on **Books** tab | 1. Enter author name in search box 2. Observe results | Keyword: `Nguyen Minh Duc` | Shows **BOOK001** (Basic Flutter Programming) and **BOOK009** (Introduction to Python Programming) — both by the same author. | REQ-03 | EP |
| TC-11 | Search with no results — appropriate message displayed | Logged in, on **Books** tab | 1. Enter keyword that matches no books 2. Observe results | Keyword: `xyznotexist999` | List is empty, shows message **"No books found"**. | REQ-03 | EP |
| TC-12 | Filter by category — shows only books in that category | Logged in, on **Books** tab | 1. Select category **"Economics"** from the filter 2. Observe the list | Filter: `Economics` | Shows only **3 books**: BOOK007 (Microeconomics), BOOK014 (Macroeconomics), BOOK015 (Principles of Accounting). No books from other categories. | REQ-03 | EP |
| TC-13 | Filter by category, case-insensitive.| Logged in, on **Books** tab | 1. Select category **"kinh tế"** or **"KInH tẾ"** from the filter 2. Observe the list | Filter: `Economics` | Shows only **3 books**: BOOK007 (Microeconomics), BOOK014 (Macroeconomics), BOOK015 (Principles of Accounting). No books from other categories. | REQ-03 | EP |
---

### Group 4 — Borrow Book (REQ-04) | 6 TCs | Technique: Decision Table + BVA

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-14 | Borrow book successfully — all conditions met | `ba.nguyen` (MEM002, Active) is logged in. BOOK001 is "Available". `ba.nguyen` is borrowing < 3 books. | 1. Log in as `ba.nguyen` 2. Go to **Books** tab → BOOK001 3. Click **Borrow** 4. Go to **Borrow/Return** tab — verify new slip | Account: `ba.nguyen@email.com` / `password123` Book: BOOK001 | New borrow slip created successfully. Due date = **today + 14 days**. Slip status: **"Borrowed"**. BOOK001 changes to **"Borrowed"**. | REQ-04 | Decision Table |
| TC-15 | Reject borrow — book is "Borrowed" | `ba.nguyen` is logged in. BOOK003 is **"Borrowed"** (by MEM002 — BR001 from Seed Data). | 1. Log in as `ba.nguyen` 2. Go to **Books** tab → BOOK003 3. Click **Borrow** | Account: `ba.nguyen@email.com` / `password123` Book: BOOK003 | System rejects and shows error message: book is **unavailable** / already borrowed. No new slip created. | REQ-04 | Decision Table |
| TC-16 | Reject borrow — member account is **Suspended** | `cu.le` (MEM004, **Suspended**) is logged in. BOOK001 is "Available". | 1. Log in as `cu.le` 2. Go to **Books** tab → BOOK001 3. Click **Borrow** | Account: `cu.le@email.com` / `password123` Book: BOOK001 | System rejects and shows reason **"Suspended"** — message must be different from the "Expired" case. No slip created. | REQ-04 | Decision Table |
| TC-17 | Reject borrow — member account has **Expired** | `binh.pham` (MEM005, **Expired**) is logged in. BOOK001 is "Available". | 1. Log in as `binh.pham` 2. Go to **Books** tab → BOOK001 3. Click **Borrow** | Account: `binh.pham@email.com` / `password123` Book: BOOK001 | System rejects and shows reason **"Expired"**. No slip created. | REQ-04 | Decision Table |
| TC-18 | **BVA max−1=2** — Allow borrowing the 3rd book (at valid boundary) | `ba.nguyen` is logged in and **currently borrowing exactly 2 books**. An "Available" book exists to borrow. | 1. Log in as `ba.nguyen` 2. Borrow 1 book (if not yet at 2) to reach 2 books borrowed 3. Go to **Books** tab → select an Available book (e.g. BOOK002) 4. Click **Borrow** | Account: `ba.nguyen@email.com` / `password123` Books borrowed before action: **2** | Borrow **successful** — 3rd slip created. Total currently borrowed = **3**. No error message. | REQ-04 | BVA |
| TC-19 | **BVA max=3** — Reject borrowing 4th book (exceeds limit) | `ba.nguyen` is logged in and **currently borrowing exactly 3 books** (limit reached). An "Available" book exists. | 1. Ensure `ba.nguyen` is borrowing exactly 3 books 2. Go to **Books** tab → select an Available book 3. Click **Borrow** | Account: `ba.nguyen@email.com` / `password123` Books borrowed before action: **3** | System rejects and shows message: **"3-book limit reached"** (or equivalent). No 4th slip created. | REQ-04 | BVA |
| TC-20 | System rejects and shows reason in Vietnamese and English | `binh.pham` (MEM005, **Expired**) is logged in. BOOK001 is "Available". | 1. Log in as `binh.pham` 2. Go to **Books** tab → BOOK001 3. Click **Borrow** | Account: `binh.pham@email.com` / `password123` Book: BOOK001 | Shows reason "Thành viên đã hết hạn. Không thể mượn sách." in VI or "The member account has expired. Borrowing books is not allowed." in EN | REQ-04 | Decision Table |
---

### Group 5 — Return Book (REQ-05) | 4 TCs | Technique: EP

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-21 | Return book successfully — slip still within due date | `biet.hoang` (MEM006) is logged in. BR003 (BOOK013, due 15/10/2024) is **"Borrowed"** and still within due date. | 1. Log in as `biet.hoang` 2. Go to **Borrow/Return** tab 3. Select slip BR003 4. Click **Return** 5. Check status of BOOK013 | Account: `biet.hoang@email.com` / `password123` Slip: BR003 — BOOK013 | Return successful. BR003 changes to **"Returned"**. BOOK013 changes to **"Available"** in the book list. No overdue warning. | REQ-05 | EP |
| TC-22 | Return overdue book — system shows overdue warning | `ba.nguyen` (MEM002) is logged in. BR001 (BOOK003, due 15/09/2024) is **actually overdue** (but not yet flagged). | 1. Log in as `librarian` → click **"Check Overdue"** to flag BR001 2. Log out → Log in as `ba.nguyen` 3. Go to **Borrow/Return** tab 4. Select BR001 → Click **Return** | Account: `ba.nguyen@email.com` / `password123` Slip: BR001 — BOOK003 (due 15/09/2024) | System **shows overdue warning** before confirmation. After confirming return: BR001 → **"Returned"**, BOOK003 → **"Available"**. | REQ-05 | EP |
| TC-23 | After returning — book can be borrowed again immediately | `ba.nguyen` has returned BOOK003 (BR001) in TC-20. BOOK003 is "Available". | 1. After returning BOOK003 2. Go to **Books** tab 3. Try clicking **Borrow** on BOOK003 | Account: `ba.nguyen@email.com` / `password123` Book: BOOK003 (just returned) | BOOK003 can be borrowed normally — system successfully creates a new borrow slip. | REQ-05 | EP |
| TC-24 | **[Security] Reject — member attempts to return a book borrowed by another member** | `ba.nguyen` (MEM002) is logged in. BR003 (BOOK013) belongs to `biet.hoang` (MEM006) and is **"Borrowed"**. MEM002 has no borrowing relationship with BR003. | 1. Log in as `ba.nguyen@email.com` / `password123` 2. Go to **Borrow/Return** tab 3. Attempt to locate or interact with slip BR003 (or BOOK013) 4. If a **Return** button is visible for BR003, click it 5. Observe system response | Account: `ba.nguyen@email.com` / `password123` Slip targeted: BR003 — BOOK013 (belongs to MEM006, not MEM002) | System rejects the action. BR003 must either be **invisible** to MEM002, or if visible, the Return button must be **disabled / absent**. No return is processed. BOOK013 remains **"Borrowed"** by MEM006. *(SRS REQ-05: "Only return books the member is currently borrowing"; REQ-08: "Member can only view their own slips.")* | REQ-05, REQ-08 | EP (Negative) |

---

### Group 6 — Overdue Processing (REQ-06) | 3 TCs | Technique: EP

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-25 | Librarian clicks "Check Overdue" — overdue slips flagged correctly | `librarian` is logged in. BR001 (due 15/09/2024) is **"Borrowed"** (not yet flagged as overdue). | 1. Log in as `librarian` 2. Click **"Check Overdue"** button 3. Go to **Borrow/Return** tab — observe BR001 status | Account: `librarian@library.com` / `admin123` | BR001 (due 15/09/2024 ≤ today) **changes to "Overdue"**. BR003 (due 15/10/2024, if still valid) remains **"Borrowed"**. | REQ-06 | EP |
| TC-26 | Member does not have the "Check Overdue" button | `ba.nguyen` is logged in (Member role) | 1. Log in as `ba.nguyen` 2. Look for **"Check Overdue"** button in the interface (Borrow/Return tab and elsewhere) | Account: `ba.nguyen@email.com` / `password123` | Interface **does not show** the "Check Overdue" button anywhere — the feature is completely hidden from Members. | REQ-06 | EP |
| TC-27 | Member sees their overdue slip after Librarian runs check | `librarian` has clicked "Check Overdue" → BR001 has been flagged as overdue. `ba.nguyen` (MEM002 — owner of BR001) is not yet logged in. | 1. (Overdue check already performed in TC-22) 2. Log out as Librarian 3. Log in as `ba.nguyen` 4. Go to **Borrow/Return** tab — observe BR001 status | Account: `ba.nguyen@email.com` / `password123` | `ba.nguyen` sees BR001 with status **"Overdue"** in their slip list. | REQ-06 | EP |

---

### Group 7 — Member Management (REQ-07) | 4 TCs | Technique: EP + BVA

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-28 | Add new valid member successfully | `librarian` is logged in. Email does not yet exist in DB. | 1. Log in as `librarian` 2. Go to **Members** tab 3. Click **Add New** 4. Fill in all valid information 5. Click **Confirm / Create** 6. Check member list | Full name: `Nguyen Test` Email: `test.new@gmail.com` Phone: `0912345678` | New member **appears in the list**, status **"Active"**. Information displays correctly as entered. | REQ-07 | EP |
| TC-29 | Reject — email missing `.` in domain part | `librarian` is logged in, add member form is open. | 1. Fill form with email missing a dot in the domain 2. Enter valid data in other fields 3. Click **Create** | Full name: `Test User` Email: `user@domain` (missing `.`) Phone: `0912345678` | System shows **invalid email** message. No new member created. (Per SRS: `user@domain` is INVALID — must have `.` in domain.) | REQ-07 | EP |
| TC-30 | Reject — email already exists in the system | `librarian` is logged in. Email `ba.nguyen@email.com` already in DB (MEM002). | 1. Fill form with existing email 2. Enter valid data in other fields 3. Click **Create** | Full name: `Test Duplicate` Email: `ba.nguyen@email.com` (already exists) Phone: `0922222222` | System shows **email already exists** message (or equivalent). No new member created. | REQ-07 | EP |
| TC-31 | **BVA Phone** — 9-digit phone number (below min=10) is rejected | `librarian` is logged in, add member form is open. | 1. Fill form with only 9-digit phone number 2. Enter valid data in other fields 3. Click **Create** | Full name: `Test BVA` Email: `testbva@gmail.com` Phone: `091234567` (9 digits — min−1) | System shows error **"Phone number format is invalid"** (or equivalent). No member created. (SRS BR-09: exactly 10 digits required.) | REQ-07 | BVA |

---

### Group 8 — Borrow Slip Lookup (REQ-08) | 3 TCs | Technique: EP

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | REQ | Technique |
|-------|----------------|--------------|-------|------------|-----------------|-----|-----------|
| TC-32 | Member sees only their own borrow slips | `ba.nguyen` (MEM002) is logged in. DB has BR001, BR004 (belonging to MEM002) and BR003 (belonging to MEM006). | 1. Log in as `ba.nguyen` 2. Go to **Borrow/Return** tab 3. Check the slip list | Account: `ba.nguyen@email.com` / `password123` | Sees **BR001** (BOOK003) and **BR004** (BOOK005) — both are MEM002's slips. Does **not** see BR003 (belonging to MEM006 — `biet.hoang`). | REQ-08 | EP |
| TC-33 | Librarian can view all borrow slips from all members | `librarian` is logged in. DB has BR001–BR005 from Seed Data. | 1. Log in as `librarian` 2. Go to **Borrow/Return** tab 3. Count and verify the slip list | Account: `librarian@library.com` / `admin123` | Sees all **5 slips** from Seed Data: BR001, BR002, BR003, BR004, BR005 belonging to multiple different members. | REQ-08 | EP |
| TC-34 | Borrow slip displays all required fields | `librarian` is logged in, on the **Borrow/Return** tab. | 1. Log in as `librarian` 2. Go to **Borrow/Return** tab 3. Select slip **BR001** — observe the details | Account: `librarian@library.com` / `admin123` Slip to check: BR001 | BR001 shows all fields: **Slip ID** (BR001), **Book Borrowed** (Intro to Software Testing — BOOK003), **Borrow Date** (01/09/2024), **Due Date** (15/09/2024), **Status** (Borrowed or Overdue). | REQ-08 | EP |

---

## Appendix — Decision Table: REQ-04 Borrow Book

> This table models all condition combinations that affect the allow/deny decision for borrowing a book.

|  | R1 (TC-13) | R2 (TC-14) | R3 (TC-15) | R4 (TC-16) | R5 (TC-17) | R6 (TC-18) |
|--|--|--|--|--|--|--|
| **CONDITIONS** | | | | | | |
| Book status is "Available"? | ✓ | ✗ | ✓ | ✓ | ✓ | ✓ |
| Member is "Active"? | ✓ | ✓ | ✗ Suspended | ✗ Expired | ✓ | ✓ |
| Books currently borrowed < 3? | ✓ (=0) | ✓ | ✓ | ✓ | ✓ (=2, max−1 boundary) | ✗ (=3, max boundary) |
| **ACTIONS** | | | | | | |
| Create borrow slip, due date +14 days | ✓ | — | — | — | ✓ | — |
| Book unavailable message | — | ✓ | — | — | — | — |
| Account "Suspended" message | — | — | ✓ | — | — | — |
| Account "Expired" message | — | — | — | ✓ | — | — |
| "3-book limit reached" message | — | — | — | — | — | ✓ |
| **Seed Data Account** | `ba.nguyen` (MEM002) | `ba.nguyen` | `cu.le` (MEM004) | `binh.pham` (MEM005) | `ba.nguyen` — 2 books | `ba.nguyen` — 3 books |
| **Seed Data Book** | BOOK001 | BOOK003 (Borrowed) | BOOK001 | BOOK001 | BOOK002 | BOOK001 |

---

## Summary

| Feature Group | # TCs | REQ Coverage | Type (Happy/Neg/BVA) | IDM Technique Applied |
|----------------|-------|---------|----------------------|----------------------|
| Login | 5 | REQ-01 | 2 Happy / 3 Negative | Decision Table |
| View Book List | 3 | REQ-02 | 3 Happy | EP |
| Search and Filter | 5 | REQ-03 | 4 Happy / 1 Negative | EP |
| Borrow Book | 6 | REQ-04 | 1 Happy / 4 Negative / 2 BVA | Decision Table + BVA |
| Return Book | 4 | REQ-05, REQ-08 | 2 Happy / 2 Negative | EP |
| Overdue Processing | 3 | REQ-06 | 2 Happy / 1 Negative | EP |
| Member Management | 4 | REQ-07 | 1 Happy / 2 Negative / 1 BVA | EP + BVA |
| Borrow Slip Lookup | 3 | REQ-08 | 3 Happy | EP |
| **Total** | **34** | **8/8** | **18 Happy / 13 Neg / 3 BVA** | **EP, BVA, Decision Table** |

---

## AI Usage Declaration

> *"The group used Claude to assist with suggesting structure and reviewing for missing test cases; the final content was cross-referenced against the SRS and confirmed by the group."*
