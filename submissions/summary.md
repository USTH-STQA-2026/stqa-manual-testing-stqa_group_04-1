# Test Summary — Báo cáo tổng hợp kiểm thử

> \\\*\\\*Note\\\*\\\*: This is a \\\*\\\*Quality Assurance\\\*\\\* activity — you are evaluating the overall quality of the software, not just listing bugs.

\---

## 1\. Thông tin nhóm

|Item|Information|
|-|-|
|**Group**|`GROUP_04`|
|**Class**|`252ICT2012.P1`|
|**Report Date**|`29/05/2026`|
|**System Under Test**|https://stqa.rbc.vn — v1.0|

\---

## 2\. Tổng quan kết quả

|Metric|Value|
|-|-|
|Total Test Cases|`35`|
|Pass|`20`|
|Fail|`12`|
|Blocked|`3`|
|Not Run|`0`|
|**Pass Rate**|`57.14%`|
|**Bugs Found**|`13`|

### Phân bổ theo nhóm chức năng

|Functional Group|TC|Pass|Fail|Bugs|Assessment|
|-|-|-|-|-|-|
|REQ-01/Login|5|4|1|1|Good, only 1 minor UI bug|
|REQ-02/View Book List|3|3|0|0|Excellent|
|REQ-03/Search and Filter Books|6|4|2|3|Both category filter and Vietnamese unaccented search are broken|
|REQ-04/Borrow Book|7|3|4|5|Violates the 3-book limit; wrong/duplicate error messages|
|REQ-05/Return Book|4|2|2|2|Security bug (returning on another member's behalf); missing overdue warning|
|REQ-06/Overdue Processing|3|2|1|0\*|Overdue handling is inconsistent (\*no corresponding bug report yet)|
|REQ-07/Member Management|4|0|1|1|Unusable — email validation bug blocks the entire flow|
|REQ-08/Borrow Slip Lookup|3|2|1|1|Security bug — can view another member's borrow slips|

### Phân bổ bug theo mức độ

|Severity|Count|Bug IDs|
|-|-|-|
|High|6|BUG-02, BUG-06, BUG-07, BUG-09, BUG-11, BUG-12|
|Medium|5|BUG-03, BUG-05, BUG-08, BUG-10, BUG-13|
|Low|2|BUG-01, BUG-04|

\---

## 3\. Kỹ thuật thiết kế đã sử dụng

|Technique|Applied to which REQ?|# TCs Used|Explanation of Application|
|-|-|-|-|
|Decision Table|REQ-01, REQ-04|5 TC + 7 TC|Combines conditions (valid email/password; book status + member status + number of books currently borrowed) into different output cases.|
|BVA|REQ-04, REQ-07|3 TC (TC-18, TC-19, TC-31)|Tests boundaries: number of books currently borrowed = 2 and = 3 (borrow limit); phone number with 9 digits (just below the valid 10-digit threshold).|
|EP|REQ-02, REQ-03, REQ-05, REQ-06, REQ-07, REQ-08|23 TC|Splits input into equivalence classes (role, book status, keyword exists/doesn't exist, valid/invalid email, slip within/past due date...), each class tested with one representative value.|

\---

## 4\. Phân tích chất lượng phần mềm

### 4.1. Điểm mạnh

Login (REQ-01) and View Book List (REQ-02) work reliably, role-based permissions (Librarian/Member) are correct, and book status updates in real time. The "happy path" flows for borrowing/returning books (TC-14, TC-17, TC-18, TC-21, TC-23) follow the correct logic, with the due date calculated accurately (+14 days). Search by properly accented keywords, case-insensitive matching (TC-09–TC-11), and the permission scheme for viewing/handling slips and overdue checks for the Librarian (TC-26, TC-33, TC-34) all work as designed.

### 4.2. Điểm yếu

The two most critical security bugs: a member can view (BUG-11) and even return (BUG-12) another member's borrow slip — a serious access-control violation. The 3-book borrow limit is completely ignored (BUG-06). Filtering books by category doesn't work (BUG-02, BUG-03), and search doesn't support unaccented Vietnamese keywords (BUG-08), making REQ-03 nearly unusable. Adding a new member is blocked by an email validation bug (BUG-09), which in turn blocks 3 other TCs — REQ-07 is completely unusable. The rejection messages for "Suspended" vs. "Expired" accounts are duplicated/incorrect (BUG-05, BUG-10), error messages mix Vietnamese and English (BUG-07), and no overdue warning is shown when returning a book (BUG-13). In addition, TC-27 (REQ-06) is logged as Fail but has no corresponding bug report yet — this needs to be added.

\---

## 5\. Đề xuất ưu tiên sửa lỗi

> 💡 This is the \\\*\\\*Quality Assurance\\\*\\\* part: you don't just find bugs — you also \\\*\\\*propose a fix priority order\\\*\\\* and assess impact.
> State the prioritization criteria clearly: based on \\\*\\\*severity\\\*\\\* (technical seriousness) and/or \\\*\\\*priority\\\*\\\* (business priority).

|Order|Bug|Severity|Reason for Priority|
|-|-|-|-|
|1|BUG-09|High|Blocks the entire Add Member flow, causing 3 other TCs to be Blocked|
|2|BUG-12|High|Returning a book on another member's behalf — corrupts data across the whole system|
|3|BUG-11|High|Privacy violation — can view another member's borrow slips|
|4|BUG-06|High|Violates the 3-book borrow-limit business rule|
|5|BUG-02|High|Category filter doesn't work — REQ-03 is unusable|
|6|BUG-07|High|Mixed languages in error messages, hurting UX|
|7|BUG-03|Medium|Same feature as BUG-02 — should be fixed together|
|8|BUG-10|Medium|Wrong message ("Suspended" shown as "Expired"), causing confusion|
|9|BUG-05|Medium|Same root cause as BUG-10 (TC-16)|
|10|BUG-08|Medium|Search doesn't support unaccented Vietnamese keywords|
|11|BUG-13|Medium|Missing overdue warning, doesn't block the main flow|
|12|BUG-01|Low|Minor UI bug, no impact on business logic|
|13|BUG-04|Low|Missing clear message but no logic error|

\---

## 6\. Kết luận

The system is **not ready for release**. The pass rate is only 57.14%, well below the typical release threshold (≥90%). Of the 13 bugs found, 6 are High severity, including 2 security bugs (BUG-11, BUG-12) and 1 core business-rule violation (BUG-06); REQ-07 is essentially unusable (0% Pass). The High-severity bugs should be fixed first, followed by regression testing on REQ-03, REQ-04, REQ-05, REQ-07, and REQ-08, before release is reconsidered.

\---

## 7\. Bài học rút ra (Tùy chọn)

Designing test cases using IDM/BVA/Decision Table before testing helps catch boundary errors and condition combinations that random testing tends to miss (e.g., BUG-06 was found thanks to BVA at the max=3 boundary). Figures should be carefully cross-checked between test-execution and bug-reports to avoid discrepancies (as in the TC-27 case, which is missing a bug report). Severity should reflect both technical and business/security impact, not just the number of failed TCs.

\---

## 8\. Khai báo sử dụng AI (Tùy chọn)

> If your team used any AI tools (ChatGPT, Copilot, Gemini...), state this clearly below. An honest declaration \\\*\\\*does not affect your grade\\\*\\\* — it is a professional transparency skill.

|AI Tool|Used For|How You Verified/Edited It|
|-|-|-|
|Claude|Helped compile figures from test-execution.md and bug-reports.md|The team cross-checked every figure (Pass/Fail/Blocked, severity, Bug ID) against the original test-cases.md, test-execution.md, and bug-reports.md files before finalizing the content|



