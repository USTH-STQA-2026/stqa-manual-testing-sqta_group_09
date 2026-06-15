# Test Summary Report

## 1. Group Information

| Field | Info |
|---|---|
| **Group** | Group 09 |
| **Class** | ICT - Class 1 |
| **Report Date** | 17/05/2026 |
| **System Under Test** | https://stqa.rbc.vn — v1.0 |

---

## 2. Results Overview

| Metric | Value |
|---|---|
| Total test cases | 31 |
| Pass | 21 |
| Fail | 10 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass Rate** | **67.7%** |
| **Bugs Found** | **10** |

### Distribution by Feature Group

| Feature Group | Total TCs | Pass | Fail | Bug | Assessment |
|---|---|---|---|---|---|
| Login / Authentication | 5 | 3 | 2 | BUG-01, BUG-02 | Partially functional — main login flow works but error handling is wrong and incomplete |
| Real-time Status | 1 | 1 | 0 | N/A | Fully functional — book status updates in real-time after borrow/return transactions |
| Searching and Filtering | 6 | 4 | 2 | BUG-03, BUG-10 | Core search works via button click, but case-insensitivity fails and Enter key shortcut is missing |
| Borrow Book | 8 | 6 | 2 | BUG-04, BUG-07 | Significant defects: wrong error message for expired/suspended members, and the 3-book borrow limit is not enforced (allows 4) |
| Return Book | 3 | 1 | 2 | BUG-05, BUG-09 | Critical failures: no overdue warning on late return, and any member can return another member's book |
| Overdue Check and Access | 2 | 1 | 1 | BUG-08 | Complete failure on access control: members can read each other's borrow records, though overdue check works |
| Member Registration | 4 | 3 | 1 | BUG-06 | Partial failure: email validation accepts malformed addresses (missing dot in domain) |
| Access Control for Librarian | 1 | 1 | 0 | N/A | Fully functional — librarians have unrestricted access to all borrow records |
| Book Information | 1 | 1 | 0 | N/A | Fully functional — Book information is correctly displayed. |
| **Total** | **31** | **21** | **10** | **10 bugs** | |

### Bug Distribution by Severity

| Severity | Count | Bug IDs |
|---|---|---|
| High | 4 | BUG-05, BUG-07, BUG-08, BUG-09 |
| Medium | 4 | BUG-01, BUG-02, BUG-04, BUG-06 |
| Low | 2 | BUG-03, BUG-10 |

---

## 3. Design Techniques Used

| Technique | Applied to REQ | # of TCs | How It Was Applied |
|---|---|---|---|
| Equivalence Partitioning (EP) | REQ-01, 02, 03, 04, 05, 06, 07, 08 | 27 | Inputs were divided into valid/invalid partitions. For each partition (e.g. active / suspended / expired member; valid / malformed email), one representative value was chosen and tested. This efficiently covered all distinct behaviour groups without testing every possible value. |
| Boundary Value Analysis (BVA) | REQ-04 | 4 | Applied to the 3-book borrow limit: tested at 1 book, 2 books (one below limit), 3 books (at limit), and 4 books (above limit). This revealed that the system incorrectly allows borrowing at 4 books (BUG-07). |
| Decision Table Testing (DT) | REQ-04, REQ-05 | 9 | Created a decision table for borrow and return flows, mapping combinations of conditions (e.g., member status, book availability, return timing) to expected outcomes. This systematic approach identified gaps such as the missing overdue warning (BUG-05) and the lack of member-specific return validation (BUG-09). |

---

## 4. Software Quality Analysis

### 4.1. Strengths

- **Basic login flow**: Logging in with valid credentials works correctly and redirects to the dashboard.
- **Suspended member rejection**: The system correctly blocks borrow requests for suspended members and shows a rejection message.
- **Logout**: Session termination works correctly and redirects to the login page.
- **Basic email validation**: Completely malformed emails (missing @) are correctly rejected during member registration.

### 4.2. Weaknesses

- **Wrong error message on wrong password (BUG-01)**: The error shown when entering an incorrect password is unrelated to the actual failure (e.g. showing a suspension message). This is misleading and erodes user trust.
- **Incomplete empty-form validation (BUG-02)**: Only the email field is highlighted when both fields are empty. The password field is silently ignored during validation.
- **Enter key does not trigger search (BUG-03)**: A fundamental UX convention is broken — users must click the button manually.
- **Wrong status message for expired members (BUG-04)**: The system shows "Member is suspended" when the actual status is "Expired". This causes librarians to give wrong guidance to members.
- **No overdue warning on late return (BUG-05)**: Overdue books are accepted back without any alert. Late return policy cannot be enforced, which is a core operational requirement.
- **Invalid email accepted in registration (BUG-06)**: Emails without a dot in the domain are stored in the database, making those accounts unreachable.
- **Borrow limit allows 4 books instead of 3 (BUG-07)**: The SRS-defined limit of 3 books is not correctly enforced — the system allows one extra book.
- **Members can view each other's records (BUG-08)**: There is no access control on borrow history. Any member can read another member's loan data — a serious privacy defect.
- **Members can return books belonging to others (BUG-09)**: A member can process returns for books checked out under a different member's account, which can be exploited to clear loan records fraudulently.

---

## 5. Bug Fix Priority Recommendations

| Order | Bug | Severity | Reason for Priority |
|---|---|---|---|
| 1 | BUG-08 | High | Privacy violation — any member can access another member's personal borrow records. Must be fixed immediately before any public release. |
| 2 | BUG-09 | High | Data integrity — any member can return a book belonging to someone else, creating fraudulent records and potentially clearing other members' overdue loans. |
| 3 | BUG-05 | High | Policy enforcement — overdue returns are not flagged at all. Core library operational rule is completely unimplemented. |
| 4 | BUG-07 | High | Business rule violation — the 3-book borrow limit is not enforced; members can hold 4 books. Reduces availability for others. |
| 5 | BUG-01 | Medium | Trust and UX — wrong error message on login (e.g. showing "suspended") actively misleads users and damages confidence in the system. |
| 6 | BUG-04 | Medium | Operational accuracy — expired members see the "suspended" error, leading to wrong librarian advice and incorrect member actions. |
| 7 | BUG-06 | Medium | Data quality — invalid emails (missing dot in domain) are stored, making member accounts unreachable via email. |
| 8 | BUG-02 | Medium | Usability — empty password field is not highlighted, making form validation inconsistent and confusing. |
| 9 | BUG-03 | Low | Usability — Enter key does not trigger search. Functional workaround exists (clicking the button) but breaks standard UX expectations. |
| 10 | BUG-10 | Low | Usability — Member ID cannot be searched in all lowercase. |

---

## 6. Conclusion

The system at https://stqa.rbc.vn is **not ready for release** in its current state.

Out of 31 test cases executed, only 21 passed (67.7%), with 10 bugs found across 6 functional areas (out of 8 defined feature groups). Four bugs are rated High severity. Most critically, BUG-08 and BUG-09 represent serious security and data integrity failures: members can view each other's private records, and can fraudulently return books that do not belong to them. These alone are sufficient grounds to block any release.

Additionally, BUG-05 (no overdue warning) means the library cannot enforce its return policy at all, and BUG-07 (4-book limit instead of 3) means a core business rule is silently violated.

**Recommendation**: Fix all 4 High severity bugs as release blockers. After fixes, a full re-test cycle covering all 12 failed TCs is mandatory before re-evaluating release readiness.

---

## 7. Lessons Learned

- **Negative test cases catch the most critical bugs**: The most severe defects (BUG-08, BUG-09) were found by testing what the system should *reject* or *deny*, not what it should allow. Testing only happy paths would have missed all security vulnerabilities.
- **Boundary Value Analysis is efficient and effective**: Testing at 1, 2, 3, and 4 books efficiently revealed that the borrow limit is off by one (BUG-07) with 4 targeted test cases.
- **Access control must be tested explicitly**: It is easy to overlook authorisation testing. Without a dedicated TC for "can member A see member B's data?", BUG-08 and BUG-09 would have gone undetected.
- **Error messages are testable requirements**: BUG-01 and BUG-04 show that the *content* of error messages must be verified, not just whether an error appears. Writing expected results with the exact message text helps catch these bugs.

---

## 8. AI Usage Declaration

| AI Tool | Used For | How We Reviewed/Edited |
|---|---|---|
| Claude | Assisted in structuring the bug reports, test cases, execution log, and summary document based on bugs we identified through guidance from GitHub.| All bug descriptions, steps to reproduce, and severity ratings were written based on actual observed behaviour during our testing session. The content was reviewed and corrected to match what we truly found — e.g. the exact Vietnamese error message "Thành viên bị tạm ngưng" in BUG-04, the Enter key issue in BUG-03, and the specific access control failures in BUG-08 and BUG-09. |
