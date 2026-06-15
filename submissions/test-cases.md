# Test Cases — Test Case Table

| Info | |
|---|---|
| **Group** | Group 09 |
| **Date Created** | 18/05/2026 |
| **System** | https://stqa.rbc.vn |
| **Reference** | SRS v1.0 |

---

## Step 1: Input Domain Modeling (IDM)

### IDM — Login (REQ-01)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Are credentials valid? | Valid email + valid password | `librarian@library.com` / `admin123` | Login successful, redirected to dashboard |
| | Valid email + wrong password | `librarian@library.com` / `wrongpass` | Specific error: "Incorrect password" |
| | Non-existent email | `nobody@x.com` / (any password) | Error: account not found |
| Are fields empty? | Both filled | (any valid value) | Form processed normally |
| | Both fields empty | `""` / `""` | Both fields highlighted with validation error |

### IDM — Book Search (REQ-03)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Does keyword match a record? | Yes | `"Flutter"` | Matching books displayed |
| | No | `"XYZ999"` | "No results found" message displayed |
| Can user trigger search by pressing Enter? | Yes — Enter key works | Type keyword, press Enter | Search executes immediately |
| | No — must click button manually | Type keyword, press Enter | Nothing happens (Bug: requires manual button click) |

### IDM & Decision Table - Borrow Book (REQ-04)

| Conditions / Actions | R1 | R2 | R3 | R4 | R5 | R6 |
|----------------------|----|----|----|----|----|----|
| **Conditions** | | | | | | |
| C1: Book status is "Available"? | True | False (Borrowed) | False (Lost) | True | True | True |
| C2: Account status is "Active"? | True | True | True | False (Suspended) | False (Expired) | True |
| C3: Number of books borrowed < 3? | True | - | - | - | - | False |
| **Actions** | | | | | | |
| A1: Allow borrow successfully | X | | | | | |
| A2: Reject — book already borrowed | | X | | | | |
| A3: Reject — book lost | | | X | | | |
| A4: Reject — suspended member | | | | X | | |
| A5: Reject — expired member | | | | | X | |
| A6: Reject — limit exceeded | | | | | | X |

**For other attributes**

| Characteristic | Block | Representative Value | Expected Result |
|---------------|--------|---------------------|----------------|
| Book status | Available | `BOOK001` | Allow borrow |
|  | Borrowed | `BOOK003` | Reject, show book unavailable error |
|  | Lost | `BOOK007` | Reject, show book lost error |
| Member status | Active | `MEM002` | Allow borrow |
|  | Suspended | `MEM004` | Reject, show suspended account error |
|  | Expired | `MEM005` | Reject, show expired account error |
| Number of books borrowed (BVA) | < 3 (BVA: 0, 1, 2) | `MEM003` (0 books) / `MEM002` (1 book) | Allow borrow |
|  | = 3 (BVA: limit) | `MEM002` after borrowing 2 more books | Reject, show limit exceeded message |
|  | > 3 (BVA: above limit) | Attempting a 4th borrow | Reject, show limit exceeded message |

### IDM & Decision Table - Return Book (REQ-05)

| Conditions / Actions | R1 | R2 | R3 | R4 |
|----------------------|----|----|----|----|
| **Conditions** | | | | |
| C1: Book Belonging is Borrower? | True | True | True | False (non-borrower) |
| C2: returnDate < dueDate? | True | False (returnDate = dueDate) | False (returnDate > dueDate) | - |
| **Actions** | | | | |
| A1: Accept (no warning) | X | | | |
| A2: Accept (warning) | | X (return late) | X (return late) | |
| A3: Reject (reason) | | | | X (not borrow by this member) |

### IDM — Member Data Access (REQ-06)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Whose records does the member view? | Own records | Logged-in member's ID | Records displayed |
| | Another member's records | Different member's ID | Access denied |

### IDM — Member Registration (REQ-07)

| Characteristic | Block | Representative Value | Expected Result |
|---|---|---|---|
| Email format valid? | Valid | `user@example.com` | Accepted |
| | Missing dot in domain | `user@examplecom` | Rejected: "Invalid email format" |
| | Missing @ symbol | `userexample.com` | Rejected: "Invalid email format" |

---

## Step 2: Test Cases

## REQ-01: Authentication

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-01 | Login with valid credentials | Login page open at https://stqa.rbc.vn | 1. Open the login page.<br>2. Enter valid email and password.<br>3. Click Login. | Email: `librarian@library.com`<br>Password: `admin123` | Dashboard is displayed; login successful. | EP |
| TC-02 | Login with incorrect password shows correct error message | Login page open | 1. Enter a valid registered email.<br>2. Enter an incorrect password.<br>3. Click Login. | Email: `librarian@library.com`<br>Password: `wrongpass` | A specific error message is displayed indicating the password is wrong. | EP |
| TC-03 | Login with both fields empty | Login page open, no data entered | 1. Leave both email and password fields blank.<br>2. Click Login. | Email: `""`<br>Password: `""` | Both fields are highlighted with validation errors. | EP |
| TC-04 | Login with non-existent account | Login page open | 1. Enter an unregistered email.<br>2. Enter any password.<br>3. Click Login. | Email: `nobody@test.com`<br>Password: `admin123` | Error message displayed. | EP |
| TC-05 | Logout from the system | Logged in as librarian | 1. Click Logout. | — | Session ends and user is redirected to login page. | EP |

## REQ-02: Book Information & Status

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-06 | Book list shows status in real-time after borrow | Logged in as librarian | 1. Navigate to Borrow / Return book.<br>2. Return any book.<br>3. Navigate back to book list. | Book: BOOK001<br>Member: MEM002 | Status updates immediately without page refresh. | EP |
| TC-31 | Verify book information displayed correctly | Logged in as librarian. Main page open. | 1. Verify book information. | Book ID: `BOOK001` | Book information (title, author, etc.) is displayed correctly. | EP |

## REQ-03: Search & Filter Books

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-07 | Search book's name on main page | Logged in. Main page open. | 1. Click search field.<br>2. Type keyword. | `Flutter` | Search executes automatically and displays results. | EP |
| TC-08 | Records searching is not case-sensitive | Logged in. Borrow / Return page open. | 1. Click search field.<br>2. Type Member ID. | `mem002` | Matching records are displayed. | EP |
| TC-09 | Search records by clicking Search button | Logged in. Main page open. | 1. Enter keyword.<br>2. Click Search. | `Flutter` | Matching records are displayed. | EP |
| TC-10 | Search with no-match keyword shows "No results" message | Logged in. | 1. Enter keyword.<br>2. Click Search. | `XYZ999` | "No results found" message is displayed. | EP |
| TC-11 | Search by author name | Logged in. Main page open. | 1. Enter author name.<br>2. Search. | `Nguyễn Minh Đức` | Books by that author are displayed. | EP |
| TC-12 | Filter by genre returns only matching books | Logged in. Main page open. | 1. Select genre filter. | `Kinh tế` | Only books in the selected genre are displayed. | EP |

## REQ-04: Borrow Book

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-13 | Borrow book as an active member | MEM003 active, BOOK001 available | 1. Navigate to Borrow / Return.<br>2. Borrow BOOK001.<br>3. Confirm. | BOOK001 / MEM003 | Borrow recorded successfully. | EP, DT |
| TC-14 | Borrow rejected for suspended member | MEM004 suspended | 1. Borrow available book for MEM004.<br>2. Confirm. | MEM004 | Borrow rejected with suspension message. | EP, DT |
| TC-15 | Borrow rejected for expired member | MEM005 expired | 1. Borrow available book for MEM005.<br>2. Confirm. | MEM005 | Borrow rejected with expiry message. | EP |
| TC-16 | BVA: borrow allowed when member has 1 book | Member currently has 1 book | 1. Borrow another book.<br>2. Confirm. | 1 borrowed book | Borrow allowed. Member now has 2 books. | BVA |
| TC-17 | Borrow allowed when member has 2 books | Member currently has 2 books | 1. Borrow another book.<br>2. Confirm. | 2 borrowed books | Borrow allowed. Member now has 3 books. | BVA |
| TC-18 | Borrow rejected when member has 3 books | Member currently has 3 books | 1. Borrow another book.<br>2. Confirm. | 3 borrowed books | Borrow rejected. Limit reached. | BVA, DT |
| TC-19 | Borrow rejected when member has 4 books | Member currently has 4 books | 1. Borrow another book.<br>2. Confirm. | 4 borrowed books | Borrow rejected. | BVA, DT |
| TC-20 | Borrow rejected for unavailable book | BOOK003 already borrowed | 1. Search BOOK003. | BOOK003 | Borrow button hidden. | EP, DT |

## REQ-05: Return Book

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-21 | Return book on time | Due date not passed | 1. Return book.<br>2. Confirm. | On-time return | Book returned successfully. No overdue warning. | EP, DT |
| TC-22 | Return book late | Due date passed | 1. Return book.<br>2. Confirm. | Late return | Overdue warning displayed. | EP, DT |
| TC-23 | Member cannot return another member's book | BOOK005 borrowed by MEM003 | 1. Attempt return as MEM002.<br>2. Confirm. | BOOK005 | Return rejected. | EP, DT |

## REQ-06: Borrow Records & Access Control

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-24 | Member cannot view another member's borrow records | Logged in as MEM002 | 1. Attempt to access MEM003 records. | MEM003 | Access denied. | EP |
| TC-25 | Librarian can trigger overdue check | Logged in as librarian | 1. Open borrow records.<br>2. Click "Kiểm tra quá hạn". | BR001 | Record marked as overdue. | EP |

## REQ-07: Member Registration

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-26 | Register new member with valid email | Add New Member page open | 1. Fill form.<br>2. Submit. | `newmember@example.com` | Member created successfully. | EP |
| TC-27 | Register new member – invalid email (missing dot) | Add New Member page open | 1. Fill form.<br>2. Submit. | `newmember@examplecom` | Validation error displayed. | EP |
| TC-28 | Register new member – invalid email (missing @) | Add New Member page open | 1. Fill form.<br>2. Submit. | `newmemberexample.com` | Validation error displayed. | EP |
| TC-29 | Duplicate email rejected | Add New Member page open | 1. Fill form.<br>2. Submit. | `ba.nguyen@email.com` | Duplicate email error displayed. | EP |

## REQ-08: Librarian Access

| TC ID | Test Objective | Precondition | Steps | Input Data | Expected Result | Technique |
|---|---|---|---|---|---|---|
| TC-30 | Librarian can view borrow records of any member | Logged in as librarian | 1. Navigate to Borrow / Return.<br>2. Search member ID. | MEM002 | Borrow records are displayed. | EP |


## Summary

| Feature Group | # of TCs | REQ Coverage | IDM Technique Applied |
|---|---|---|---|
| Login / Authentication | 5 (TC-01 to TC-05) | REQ-01 | EP |
| Book List / Real-time Status | 1 (TC-06) | REQ-02 | EP |
| Book Information | 1 (TC-31) | REQ-02 | EP |
| Searching & Filtering | 6 (TC-07 to TC-12) | REQ-03 | EP |
| Borrow Book | 8 (TC-13 to TC-20) | REQ-04 | EP, BVA, DT |
| Return Book | 3 (TC-21 to TC-23) | REQ-05 | EP, DT |
| Overdue Check & Access | 2 (TC-24, TC-25) | REQ-06 | EP |
| Member Registration | 4 (TC-26 to TC-29) | REQ-07 | EP |
| Access Control | 1 (TC-30) | REQ-08 | EP |
| **Total**: **31** | REQ-01, REQ-02, REQ-03, REQ-04, REQ-05, REQ-06, REQ-07, REQ-08 | EP + BVA + DT |
