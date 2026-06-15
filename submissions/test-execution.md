# Test Execution — Test Execution Results

| Info | |
|---|---|
| **Group** | Group 09 |
| **Execution Date** | 17/05/2026 |
| **Browser** | Chrome 148.0.7778.168, Safari 26.4 |
| **Operating System** | Windows 10/11, macOS 26.4 |

---

## Detailed Results

| TC ID | Feature Group | Expected Result (Summary) | Actual Result | Verdict | Evidence | Bug |
|---|---|---|---|---|---|---|
| TC-01 | Login / Authentication | Dashboard displayed after valid login | Dashboard displayed successfully | **Pass** | N/A | N/A |
| TC-02 | Login / Authentication | Specific "incorrect password" error message shown | System highlighted the **email** field with "Password is not correct" — wrong field flagged | **Fail** | [BUG-01](bug-reports.md#bug-01) | BUG-01 |
| TC-03 | Login / Authentication | Both email and password fields highlighted when empty | Only email field was highlighted; password field showed no error | **Fail** | [BUG-02](bug-reports.md#bug-02) | BUG-02 |
| TC-04 | Login / Authentication | Error message for non-existent account | Appropriate error message displayed | **Pass** | N/A | N/A |
| TC-05 | Login / Authentication | User logged out, session ended, redirected to login | Logout successful, login page shown | **Pass** | N/A | N/A |
| TC-06 | Real-time Status | Book list shows status in real-time after borrow | The status of that book immediately updates from Borrowed to Available on the page in real-time | **Pass** | N/A | N/A |
| TC-07 | Searching and Filtering | Search executes automatically on typing keyword | Search does not execute automatically; pressing Enter key also does nothing, requiring manual button click | **Fail** | [BUG-03](bug-reports.md#bug-03) | BUG-03 |
| TC-08 | Searching and Filtering | Records searching is not case-sensitive | No records returned when member ID typed in lowercase (`mem002`); only exact-case input (`MEM002`) returns results | **Fail** | [BUG-10](bug-reports.md#bug-10) | BUG-10 |
| TC-09 | Searching and Filtering | Search records by clicking the Search button | Matching records are displayed successfully after clicking the button | **Pass** | N/A | N/A |
| TC-10 | Searching and Filtering | Search with no-match keyword shows "No results" message | Message was shown | **Pass** | N/A | N/A |
| TC-11 | Searching and Filtering | Search by author name | Books authored by Nguyễn Minh Đức are displayed in the results | **Pass** | N/A | N/A |
| TC-12 | Searching and Filtering | Filter by genre returns only matching books | Only books belonging to the selected category are displayed | **Pass** | N/A | N/A |
| TC-13 | Borrow Book | Borrow book as an active member | Borrow recorded successfully | **Pass** | N/A | N/A |
| TC-14 | Borrow Book | Borrow rejected for suspended member with suspension-specific message | System showed "Thành viên hết hạn" (expired) instead of a suspension-specific message | **Fail** | [BUG-04](bug-reports.md#bug-04) | BUG-04 |
| TC-15 | Borrow Book | Borrow rejected for expired member with message distinct from suspended | The error message is not distinct (the system shows the same expired message for both suspended and expired members) | **Pass** | N/A | N/A |
| TC-16 | Borrow Book | Borrow allowed when member has 1 book | Borrow allowed; member now holds 2 books | **Pass** | N/A | N/A |
| TC-17 | Borrow Book | Borrow allowed when member has 2 books (one below limit) | Borrow allowed; member now holds 3 books | **Pass** | N/A | N/A |
| TC-18 | Borrow Book | Borrow rejected when member has 3 books (at SRS limit) | Borrow was accepted — member allowed to hold 4 books (SRS limit of 3 not enforced) | **Fail** | [BUG-07](bug-reports.md#bug-07) | BUG-07 |
| TC-19 | Borrow Book | Borrow rejected when member has 4 books (above SRS limit) | Borrow was accepted — member allowed to hold 4 books | **Pass** | N/A | N/A |
| TC-20 | Borrow Book | Borrow rejected for a book already borrowed | System hides the borrow button, indicating the book is unavailable | **Pass** | N/A | N/A |
| TC-21 | Return Book | Book returned on time with no overdue warning | Book returned, no warning shown | **Pass** | N/A | N/A |
| TC-22 | Return Book | Overdue warning displayed on late return | Return processed silently with no overdue warning | **Fail** | [BUG-05](bug-reports.md#bug-05) | BUG-05 |
| TC-23 | Return Book | Member cannot return a book borrowed by another member | Return was accepted despite book belonging to another member | **Fail** | [BUG-09](bug-reports.md#bug-09) | BUG-09 |
| TC-24 | Overdue Check and Access (members) | Access denied when viewing another member's records | MEM002 could view MEM003's records without restriction | **Fail** | [BUG-08](bug-reports.md#bug-08) | BUG-08 |
| TC-25 | Overdue Check and Access (librarian) | Overdue check triggered and records marked | Overdue check is triggered, and record status is marked as "Quá hạn" successfully | **Pass** | N/A | N/A |
| TC-26 | Member Registration | Register new member with valid email | Member created successfully | **Pass** | N/A | N/A |
| TC-27 | Member Registration | Form rejected for email missing dot in domain | Form accepted — member created with invalid email (`@examplecom`); form rejected for valid email with dot in domain | **Fail** | [BUG-06](bug-reports.md#bug-06) | BUG-06 |
| TC-28 | Member Registration | Form rejected for email missing @ symbol | Validation error displayed correctly | **Pass** | N/A | N/A |
| TC-29 | Member Registration | Duplicate email rejected | System rejects with a duplicate email error | **Pass** | N/A | N/A |
| TC-30 | Access Control for Librarian | Librarian can view borrow records of any member | All borrow records are displayed successfully | **Pass** | N/A | N/A |
| TC-31 | Book Information | Verify book information displayed correctly | Book information (title, author, etc.) is displayed correctly on the main page | **Pass** | N/A | N/A |

---

## Results Summary

| Metric | Value |
|---|---|
| Total test cases | 31 |
| Pass | 21 |
| Fail | 10 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass Rate** | **67.7%** |
| **Bugs Found** | **10** |

### Results by Feature Group

| Group | Total TC | Pass | Fail | Pass Rate |
|---|---|---|---|---|
| Login / Authentication | 5 | 3 | 2 | 60% |
| Real-time Status | 1 | 1 | 0 | 100% |
| Searching and Filtering | 6 | 4 | 2 | 66.7% |
| Borrow Book | 8 | 6 | 2 | 75% |
| Return Book | 3 | 1 | 2 | 33.3% |
| Overdue Check and Access | 2 | 1 | 1 | 50% |
| Member Registration | 4 | 3 | 1 | 75% |
| Access Control for Librarian | 1 | 1 | 0 | 100% |
| Book Information | 1 | 1 | 0 | 100% |
| **Total** | **31** | **21** | **10** | **67.7%** |