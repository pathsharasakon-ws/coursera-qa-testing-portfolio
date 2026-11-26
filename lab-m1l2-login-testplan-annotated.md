# Login System Test Plan - Annotated

## Overview
This is a sample test plan for the Login System (used by Students, Admins, and Faculty). It includes test cases designed to validate whether the system meets the functional requirement:

* **REQ-001:** "The system must allow valid users to log in using their email and password. Invalid login attempts must be rejected with a clear error message."

---

## Test Cases (Annotated & Expanded)

| Test Case ID | Requirement ID | Test Title | Objective | Preconditions | Test Steps | Expected Result | Priority | Owner |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-001** | REQ-001 | Valid login | Verify login succeeds for valid user credentials | User has valid account<br><br>*(Annotation: Clarify user roles—Student, Admin, or Faculty—and confirm account status is active/verified.)* | 1. Enter valid email<br>2. Enter valid password<br>3. Click login | User is successfully logged in<br><br>*(Annotation: Too vague. Specify exact post-login behavior, e.g., "User is redirected to the role-specific dashboard and a session token is generated.")* | High | QA Team |
| **TC-002** | REQ-001 | Invalid password | Verify login fails with incorrect password | User has valid account | 1. Enter valid email<br>2. Enter wrong password<br>3. Click login<br><br>*(Annotation: Missing edge case for consecutive failed attempts. Account lockout policy after 3–5 failed tries should also be tested.)* | Error: “Invalid username or password” | High | QA Team |
| **TC-003** | REQ-001 | Invalid email format | Verify login fails for improperly formatted email | None<br><br>*(Annotation: Should state "User is on the login page" instead of "None" to establish a clear starting state.)* | 1. Enter malformed email<br>2. Enter any password<br>3. Click login<br><br>*(Annotation: "Malformed email" is ambiguous. Provide concrete test data examples such as `userdomain.com`, `user@.com`, or leading/trailing spaces.)* | Error: “Invalid email format” | Medium | QA Team |
| **TC-004** | REQ-001 | Blank fields | Verify login fails when email and/or password fields are blank | None<br><br>*(Annotation: Specify starting state: "User is on the login page.")* | 1. Leave email and/or password blank<br>2. Click login<br><br>*(Annotation: Using "and/or" combines multiple scenarios into one test case. Split into 3 atomic cases: both blank, only email blank, and only password blank.)* | Error: “Email and password cannot be blank” | High | QA Team |
| **TC-005** | REQ-001 | Case sensitivity | Verify password field is case-sensitive | User has known password with mixed case (e.g., `PassWord123`) | 1. Enter valid email<br>2. Enter password in wrong case (e.g., `password123`)<br>3. Click login | Error: “Invalid username or password” | Medium | QA Team |
| **TC-006** *(New)* | REQ-001 | Reject login with expired password | Verify that the system blocks users with an expired password and prompts them to reset it | User account exists, is active, and has an expired password; user is on the login page | 1. Enter valid email<br>2. Enter expired password<br>3. Click login | Error: “Password expired. Please reset your password.” | Medium | QA Team |
| **TC-007** *(New)* | REQ-001 | Prevent SQL injection in login fields | Verify that the system sanitizes inputs and rejects SQL injection payloads without exposing database errors | User is on the login page | 1. Enter `' OR '1'='1` in the email field<br>2. Enter `' OR '1'='1` in the password field<br>3. Click login | Login is rejected with a generic error: “Invalid username or password” (no system crash or database error exposed) | High | QA Team |

> **General Annotation on Missing Coverage:** Critical edge cases and security scenarios were missing from the initial draft, including expired passwords, SQL injection attempts, unregistered email addresses, and account lockouts after multiple failed login attempts.

---

## Traceability Matrix

| Requirement ID | Requirement Description | Test Case ID | Test Case Description | Status |
| :--- | :--- | :--- | :--- | :--- |
| REQ-001 | Login and error handling logic | TC-001 | Valid login | Not started |
| REQ-001 | Login and error handling logic | TC-002 | Invalid password | Not started |
| REQ-001 | Login and error handling logic | TC-003 | Invalid email format | Not started |
| REQ-001 | Login and error handling logic | TC-004 | Blank fields | Not started |
| REQ-001 | Login and error handling logic | TC-005 | Case sensitivity | Not started |
| REQ-001 | Login and error handling logic | TC-006 | Reject login with expired password | Not started |
| REQ-001 | Login and error handling logic | TC-007 | Prevent SQL injection in login fields | Not started |

---

## System Professional Review Summary

### 1. What I Improved
* Added inline review annotations (`*(Annotation: ...)*`) directly within the test case table to clarify vague expected results (`TC-001`), define explicit starting states (`TC-003`, `TC-004`), and request concrete test data.
* Created two new test cases (`TC-006` and `TC-007`) to cover expired password handling and SQL injection prevention.
* Updated the Traceability Matrix to link the new test cases to `REQ-001`, ensuring full visibility of requirement coverage.

### 2. Gaps Discovered
* **Ambiguous Definitions:** Missing preconditions (listed as `None`), unspecific test data (e.g., "malformed email"), and unclear post-login outcomes.
* **Non-Atomic Test Design:** `TC-004` combined multiple blank-field conditions (`and/or`) into a single test execution, making failure tracking harder.
* **Missing Edge & Security Cases:** Omitted role-based access considerations (Student, Admin, Faculty), account lockout thresholds after repeated failed attempts, expired credentials, and basic input sanitization/security checks.

### 3. Top Two Recommendations to Strengthen QA Readiness
1. **Enforce Atomic Test Cases & Explicit Test Data:** Split combined scenarios (such as blank email vs. blank password) into separate test cases and require concrete input examples and exact expected system states (e.g., redirect URLs and error messages) before test execution begins.
2. **Expand Security & Account Lifecycle Coverage:** Add negative security tests (SQL injection, brute-force account lockout) and account state validations (unverified, locked, or expired accounts) into the standard baseline test suite.