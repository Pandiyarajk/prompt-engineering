# Module 3: Prompt Engineering for Manual QA

## Lesson 1: Test Case Generation with AI

---

## 🎯 Lesson Overview

**Time Required:** 60-90 minutes  
**Difficulty:** Beginner to Intermediate  
**Prerequisites:** Modules 1 and 2 completed

In this lesson, you'll learn:
- How to generate comprehensive test cases using AI
- Multiple testing techniques (persona-based, journey-based, risk-based)
- Domain-specific test case generation (Web, Mobile, API)
- Output formatting for different tools and formats
- How to review and refine AI-generated test cases

---

## 📚 Introduction

As a manual QA engineer, one of your most time-consuming tasks is creating comprehensive test cases. AI can accelerate this process dramatically while improving coverage and quality.

### Before AI vs. After AI

```
┌─────────────────────────────────────────────────────────────────┐
│                   TRADITIONAL APPROACH                          │
│                                                                  │
│  1. Read requirements            (30 min)                       │
│  2. Identify test scenarios      (60 min)                       │
│  3. Write detailed test cases    (2-4 hours)                    │
│  4. Review and refine           (1 hour)                        │
│  5. Format for test management   (30 min)                       │
│                                                                  │
│  TOTAL: 5-7 hours for ~20 test cases                           │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                    AI-ASSISTED APPROACH                          │
│                                                                  │
│  1. Write AI prompt with requirements    (10 min)               │
│  2. Generate initial test cases          (1 min)                │
│  3. Review and request additions         (15 min)               │
│  4. Refine and validate                  (30 min)               │
│  5. Final adjustments                    (15 min)               │
│                                                                  │
│  TOTAL: 60-90 minutes for ~30+ test cases                       │
└─────────────────────────────────────────────────────────────────┘

                   TIME SAVED: 70-80%! 🚀
```

---

## 🔧 The Test Case Generation Framework

Understanding the workflow is crucial for success:

```
┌─────────────┐      ┌──────────────┐      ┌─────────────────┐
│   INPUT     │      │   AI PROMPT  │      │     OUTPUT      │
│             │ ──→  │              │ ──→  │                 │
│ Requirements│      │  Structured  │      │  Comprehensive  │
│ User Stories│      │   Request    │      │  Test Cases     │
│ Tech Context│      │              │      │                 │
└─────────────┘      └──────────────┘      └─────────────────┘
                                                    │
                                                    ▼
                     ┌──────────────┐      ┌─────────────────┐
                     │    ITERATE   │      │   HUMAN REVIEW  │
                     │              │ ←──  │                 │
                     │ Add more     │      │ Validate        │
                     │ scenarios    │      │ Refine          │
                     │              │      │ Approve         │
                     └──────────────┘      └─────────────────┘
```

### The 5-Step Process

| Step | Action | Time |
|------|--------|------|
| 1️⃣ | Gather requirements and context | 5-10 min |
| 2️⃣ | Craft your AI prompt | 5-10 min |
| 3️⃣ | Generate initial test cases | 1 min |
| 4️⃣ | Review and request additions | 10-20 min |
| 5️⃣ | Validate and finalize | 15-30 min |

---

## 📝 Basic Test Case Generation

### The Core Prompt Template

This is your foundation for generating test cases:

```
Act as a Senior QA Engineer with expertise in [domain].

Feature: [Feature name]

Requirements:
[List requirements or user story]

Generate test cases including:
- Positive test scenarios (happy path)
- Negative test scenarios (error handling)
- Edge cases (unusual but valid scenarios)
- Boundary value analysis (min/max limits)

Format as a table with columns:
| Test ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result | Priority |
```

### Understanding Each Section

```
┌─────────────────────────────────────────────────────────────────┐
│  PROMPT BREAKDOWN                                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  "Act as a Senior QA Engineer..."                               │
│   └─ ROLE: Sets the expertise level and mindset                │
│                                                                  │
│  "Feature: [Feature name]"                                      │
│   └─ CONTEXT: What we're testing                                │
│                                                                  │
│  "Requirements: [List...]"                                      │
│   └─ CONTEXT: Detailed specifications                           │
│                                                                  │
│  "Generate test cases including..."                             │
│   └─ TASK: What types of tests we need                          │
│                                                                  │
│  "Format as a table..."                                         │
│   └─ FORMAT: How we want the output                             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 💡 Example 1: Login Feature (Complete Walkthrough)

### Step 1: Gather Your Requirements

First, collect all the information about the feature:

```
Feature: User Login

Requirements:
- User can login with email and password
- Email must be valid format
- Password minimum 8 characters
- Account locks after 5 failed attempts
- "Remember me" option available
- "Forgot password" link present
```

### Step 2: Craft Your Prompt

```
Act as a Senior QA Engineer with expertise in web application testing.

Feature: User Login

Requirements:
- User can login with email and password
- Email must be valid format
- Password minimum 8 characters
- Account locks after 5 failed attempts
- "Remember me" option available
- "Forgot password" link present

Generate comprehensive test cases including:
- Positive test scenarios
- Negative test scenarios
- Edge cases
- Boundary value analysis
- Security considerations

Format as a markdown table with columns:
Test ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result | Priority
```

### Step 3: What AI Generates

**Sample Output:**

| Test ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result | Priority |
|---------|---------------|---------------|------------|-----------|-----------------|----------|
| TC-001 | Valid login with correct credentials | User account exists | 1. Navigate to login page<br>2. Enter valid email<br>3. Enter valid password<br>4. Click Login | Email: test@example.com<br>Password: Test@1234 | User successfully logged in and redirected to dashboard | High |
| TC-002 | Login with invalid email format | User is on login page | 1. Enter invalid email format<br>2. Try to proceed | Email: testexample.com | Error message: "Please enter a valid email" | High |
| TC-003 | Login with wrong password | User account exists | 1. Enter valid email<br>2. Enter wrong password<br>3. Click Login | Email: test@example.com<br>Password: wrongpass | Error message: "Invalid email or password" | High |
| TC-004 | Account lockout after 5 failed attempts | User account exists | 1. Enter valid email<br>2. Enter wrong password<br>3. Repeat 5 times | Email: test@example.com<br>Password: wrong1-5 | Account locked message after 5th attempt | High |
| TC-005 | Login with "Remember me" checked | User account exists | 1. Enter valid credentials<br>2. Check "Remember me"<br>3. Login<br>4. Close and reopen browser | Valid credentials | User remains logged in after browser restart | Medium |
| TC-006 | Password at minimum length (8 chars) | User is on login page | 1. Enter email<br>2. Enter 8-character password<br>3. Click Login | Password: Abcd@123 | Login successful (boundary test) | Medium |
| TC-007 | Password below minimum length | User is on login page | 1. Enter email<br>2. Enter 7-character password | Password: Abc@123 | Error: "Password must be at least 8 characters" | High |
| TC-008 | SQL injection in email field | User is on login page | 1. Enter SQL injection in email<br>2. Click Login | Email: ' OR '1'='1 | Error handling, no SQL error exposed | Critical |
| TC-009 | XSS attack in email field | User is on login page | 1. Enter XSS script in email | Email: &lt;script&gt;alert('xss')&lt;/script&gt; | Input sanitized, no script execution | Critical |
| TC-010 | Empty email and password | User is on login page | 1. Leave fields empty<br>2. Click Login | Empty fields | Error: "Email and password are required" | High |

### Step 4: Review and Request Additions

After reviewing, you might notice missing scenarios:

```
Add test cases for:
- Session timeout behavior
- Multiple concurrent logins
- Login after password reset
- Case sensitivity in email
- Special characters in password
- Login from different browsers
- Login on mobile devices
```

### Step 5: Quality Check

✅ **Expected Output Quality:**
- 15-25 test cases
- All requirement areas covered
- Mix of positive/negative tests
- Security test cases included
- Clear, executable steps
- Specific test data

---

## 🎭 Advanced Testing Techniques

### Technique 1: Persona-Based Testing

Test from different user perspectives for better coverage:

```
Act as a QA Engineer specializing in user experience testing.

Feature: E-commerce Product Search

Generate test cases from these user personas:

1. Tech-Savvy User (Sarah, 28):
   - Expects advanced filters
   - Uses keyboard shortcuts
   - Wants instant results

2. Senior User (Robert, 65):
   - Needs large, clear text
   - Simple interface
   - Forgiving search (handles typos)

3. Mobile User (Alex, 22):
   - Uses phone on-the-go
   - Voice search
   - Touch gestures

4. Visual Impairment User (Maria, 40):
   - Uses screen reader
   - Keyboard navigation only
   - High contrast mode

For each persona, create 5 test scenarios that validate their specific needs.
Format as grouped test cases by persona.
```

**Why Persona-Based Testing Works:**

```
┌─────────────────────────────────────────────────────────────────┐
│  PERSONA-BASED TESTING BENEFITS                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✓ Discovers usability issues missed by functional testing     │
│  ✓ Ensures accessibility compliance                            │
│  ✓ Validates user experience across demographics               │
│  ✓ Identifies edge cases for specific user types              │
│  ✓ Improves test coverage comprehensively                      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

### Technique 2: Journey-Based Testing (End-to-End)

Test complete user journeys, not just individual features:

```
Act as a QA Engineer expert in user journey testing.

Create test cases for the complete user registration journey:

Journey Steps:
1. User lands on homepage
2. Clicks "Sign Up"
3. Fills registration form
4. Receives verification email
5. Clicks verification link
6. Completes profile setup
7. Receives welcome email

For each step, generate test cases covering:
- Happy path (everything works)
- Error scenarios (what can go wrong)
- Navigation flows (back/forward, refresh)
- Data validation (invalid inputs)
- Email delivery (delays, spam folder)

Include end-to-end test scenarios that span multiple steps.
```

**Journey Mapping:**

```
┌──────────────────────────────────────────────────────────────────────────┐
│  USER REGISTRATION JOURNEY                                                │
│                                                                          │
│  ┌─────────┐    ┌─────────┐    ┌──────────┐    ┌───────────┐           │
│  │Homepage │ →  │ Sign Up │ →  │Fill Form │ →  │Verify Email│           │
│  │         │    │  Page   │    │          │    │           │           │
│  └────┬────┘    └────┬────┘    └────┬─────┘    └─────┬─────┘           │
│       │              │              │                │                   │
│  [Tests:        [Tests:        [Tests:          [Tests:                 │
│   Landing,       Button,        Validation,      Email sent,            │
│   Navigation]    Redirect]      Submit]          Link works]            │
│                                                      │                   │
│                                                      ▼                   │
│            ┌────────────┐    ┌────────────┐    ┌──────────┐            │
│            │  Welcome!  │ ←  │  Profile   │ ←  │Confirmed │            │
│            │            │    │   Setup    │    │          │            │
│            └────────────┘    └────────────┘    └──────────┘            │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

---

### Technique 3: Risk-Based Testing

Prioritize testing based on risk levels:

```
Act as a Senior QA Engineer with expertise in risk-based testing.

Feature: Payment Processing

Risk Assessment:
- High Risk: Payment failures, security breaches, data loss
- Medium Risk: Performance issues, incorrect calculations
- Low Risk: UI glitches, minor UX issues

Generate test cases prioritized by risk level:
1. Critical/High-risk scenarios (must test)
2. Medium-risk scenarios (should test)
3. Low-risk scenarios (nice to test)

For each test case, include:
- Risk level
- Impact if bug found (what happens to user/business)
- Likelihood of occurrence
- Testing priority

Format: Group by risk level, highest risk first.
```

**Risk Priority Matrix:**

| Risk Level | Impact | Likelihood | Action |
|------------|--------|------------|--------|
| 🔴 Critical | High | Any | Test first, most coverage |
| 🟠 High | Medium | High | Test thoroughly |
| 🟡 Medium | Medium | Medium | Good coverage |
| 🟢 Low | Low | Any | Basic testing |

---

## 🌐 Domain-Specific Test Cases

### Web Application Testing

```
Act as a Web Application QA Engineer.

Feature: User Profile Update

Technical Context:
- SPA built with React
- REST API backend
- Form validation on client and server
- Real-time preview
- Auto-save draft feature

Generate test cases covering:

1. Functional Testing:
   - Form field validation (all fields)
   - Submit/cancel actions
   - Data persistence (saves correctly)

2. UI/UX Testing:
   - Responsive design (mobile, tablet, desktop)
   - Browser compatibility (Chrome, Firefox, Safari, Edge)
   - Accessibility (WCAG 2.1)

3. Integration Testing:
   - API interactions (success, errors)
   - Error handling (network failures)
   - Session management (timeout, logout)

4. Performance Testing:
   - Page load time (<3 seconds)
   - Auto-save intervals (debounce)
   - Large file uploads (profile picture)

Format: Organized by test type, with priority for each.
```

---

### Mobile Application Testing

```
Act as a Mobile QA Engineer with expertise in iOS and Android testing.

Feature: Mobile Photo Upload

Technical Context:
- Native mobile app (iOS + Android)
- Supports camera and gallery
- Image compression before upload
- Works offline (queues uploads)

Generate test cases for:

1. Device-Specific:
   - Different screen sizes (iPhone SE to iPad Pro)
   - Different Android versions (10.0 to 14.0)
   - Camera permissions handling
   - Storage permissions handling

2. Functional:
   - Take photo vs select from gallery
   - Multiple photo selection
   - Image editing before upload
   - Offline mode behavior

3. Non-Functional:
   - Battery consumption during upload
   - Memory usage with large images
   - Network handling (2G/3G/4G/5G/WiFi)
   - Large file handling (>10MB)

4. Edge Cases:
   - Camera not available (no permission)
   - Storage full
   - App interrupted during upload (call, notification)
   - Network switches during upload (WiFi → cellular)

Organize by platform (iOS/Android/Both) and priority.
```

---

### API Testing

```
Act as an API Testing Expert.

API Endpoint: POST /api/v1/users

Request Body:
{
  "email": "string (required)",
  "password": "string (required, 8+ chars)",
  "firstName": "string (required)",
  "lastName": "string (optional)",
  "age": "integer (optional, 13-120)"
}

Response Codes:
- 201: User created successfully
- 400: Validation error
- 409: User already exists
- 500: Server error

Generate API test cases covering:

1. Positive Scenarios:
   - Valid request with all fields
   - Valid request with required fields only
   - Valid request with optional fields

2. Negative Scenarios:
   - Missing required fields (each one)
   - Invalid data types (string for age)
   - Invalid formats (bad email format)
   - SQL injection attempts
   - XSS attempts

3. Boundary Testing:
   - Email at max length (255 chars)
   - Password at min (8) and max (128) length
   - Age at boundaries (13, 120, 12, 121)
   - Empty strings vs null

4. Performance:
   - Response time expectations (<500ms)
   - Concurrent requests handling
   - Rate limiting verification

Format as: Request Body → Expected Status → Expected Response
```

---

## 📊 Test Case Output Formats

### Format 1: Gherkin (BDD)

Perfect for teams using Cucumber or Behave:

```
Generate test cases in Gherkin syntax:

Feature: User Login

  Scenario: Successful login with valid credentials
    Given I am on the login page
    And I have a registered account with email "test@example.com"
    When I enter email "test@example.com"
    And I enter password "Test@1234"
    And I click the "Login" button
    Then I should be redirected to the dashboard
    And I should see a welcome message

  Scenario: Login fails with invalid password
    Given I am on the login page
    When I enter email "test@example.com"
    And I enter password "wrongpassword"
    And I click the "Login" button
    Then I should see an error message "Invalid email or password"
    And I should remain on the login page
```

### Format 2: Detailed Test Steps

Perfect for Jira, TestRail, or detailed documentation:

```
Test Case: TC-001 - Valid Login

Summary: Verify user can login with correct credentials

Pre-conditions:
1. User has a registered account
2. User is on the login page
3. Account is not locked

Test Steps:
| Step # | Action | Test Data | Expected Result |
|--------|--------|-----------|-----------------|
| 1 | Navigate to login page | URL: /login | Login page loads, form visible |
| 2 | Enter email | test@example.com | Email field accepts input |
| 3 | Enter password | Test@1234 | Password is masked with dots |
| 4 | Click "Login" button | N/A | Button is clickable |
| 5 | Wait for redirect | N/A | Page redirects to /dashboard |

Post-conditions:
- User is logged in
- Session is active
- User sees dashboard

Priority: High
Linked Requirement: REQ-LOGIN-001
```

### Format 3: Quick Checklist

Perfect for quick testing or exploratory testing:

```
Generate a testing checklist for: User Login

Login Feature Testing Checklist:

Positive Tests:
□ Valid email + valid password → Login success
□ Remember me → Session persists
□ Forgot password → Reset link sent

Negative Tests:
□ Invalid email format → Validation error
□ Wrong password → Error message
□ Empty fields → Required field error
□ Account locked → Lockout message

Edge Cases:
□ Email with special chars → Handled correctly
□ Password at exactly 8 chars → Works
□ Multiple spaces in email → Trimmed or error
□ Very long email (255+ chars) → Handled

Security Tests:
□ SQL injection in email → No SQL error
□ XSS in email field → Sanitized
□ Brute force protection → Account locks
□ HTTPS → Connection secure

Browser/Device Tests:
□ Chrome (latest) → Works
□ Firefox (latest) → Works  
□ Safari (latest) → Works
□ Mobile Chrome → Works
□ Mobile Safari → Works
```

---

## 🔄 The Review and Refinement Process

### Workflow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                    REVIEW WORKFLOW                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. GENERATE                    2. REVIEW                        │
│     ┌───────────┐                  ┌───────────┐                │
│     │ Initial   │    ─────→       │ Complete? │                │
│     │ Test Cases│                  │ Accurate? │                │
│     └───────────┘                  └─────┬─────┘                │
│                                          │                       │
│                            ┌─────────────┴─────────────┐        │
│                            │                           │        │
│                            ▼                           ▼        │
│                      ┌─────────┐              ┌─────────────┐   │
│  3. ITERATE          │   NO    │              │    YES      │   │
│     ┌───────────┐    │ Missing │              │ Approved!   │   │
│     │ Add More  │ ←──│ Cases   │              │             │   │
│     │ Scenarios │    └─────────┘              └─────────────┘   │
│     └───────────┘                                     │         │
│           │                                           │         │
│           └──────────────────────────────────────────▶│         │
│                                                       │         │
│                                                       ▼         │
│                                              ┌───────────────┐  │
│  4. FINALIZE                                 │ Export to     │  │
│     ┌───────────┐                            │ Test Tool     │  │
│     │ Add IDs,  │                            │ (Jira/TestRail)│  │
│     │ Priority  │ ──────────────────────────▶│               │  │
│     └───────────┘                            └───────────────┘  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Review Checklist

Use this checklist to validate AI-generated test cases:

```
┌─────────────────────────────────────────────────────────────────┐
│  ✅ TEST CASE REVIEW CHECKLIST                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  COVERAGE                                                        │
│  [ ] All requirements covered?                                  │
│  [ ] Positive scenarios included?                               │
│  [ ] Negative scenarios included?                               │
│  [ ] Edge cases identified?                                     │
│  [ ] Boundary values tested?                                    │
│  [ ] Security scenarios present?                                │
│                                                                  │
│  QUALITY                                                         │
│  [ ] Test steps are clear and specific?                         │
│  [ ] Test data is defined?                                      │
│  [ ] Expected results are measurable?                           │
│  [ ] Preconditions are stated?                                  │
│  [ ] Steps are reproducible?                                    │
│                                                                  │
│  ORGANIZATION                                                    │
│  [ ] Priority assigned?                                         │
│  [ ] Test IDs are unique?                                       │
│  [ ] Linked to requirements?                                    │
│  [ ] Grouped logically?                                         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ⚠️ Common Mistakes to Avoid

### Mistake 1: Vague Requirements

```
❌ BAD PROMPT:
"Generate test cases for a login page"

✅ GOOD PROMPT:
"Generate test cases for a login page with these requirements:
- Email and password authentication
- Email format validation
- Password: 8+ chars, 1 uppercase, 1 number
- Account lockout after 5 failures
- Remember me option
- Social login (Google, Facebook)"
```

### Mistake 2: Missing Context

```
❌ BAD: No technical context

✅ GOOD: Include technical details:
"Technical Context:
- React SPA
- REST API backend
- JWT authentication
- Session timeout: 30 minutes
- Runs on: Chrome, Firefox, Safari, Edge
- Mobile responsive design"
```

### Mistake 3: Not Specifying Output Format

```
❌ BAD: "Generate test cases"
(AI might give inconsistent formats)

✅ GOOD: "Format as a markdown table with:
| Test ID | Scenario | Steps | Expected | Priority |"
```

### Mistake 4: Accepting Without Review

```
❌ BAD: Copy-paste AI output directly into test tool

✅ GOOD: Review, refine, add missing scenarios, validate
```

### Mistake 5: One-Shot Approach

```
❌ BAD: Single prompt, done

✅ GOOD: Iterative approach:
1. Initial generation
2. Review and identify gaps
3. Request specific additions
4. Refine and finalize
```

---

## 💻 Practical Exercises

### Exercise 3.1: Basic Test Case Generation

**Feature: Password Reset**

```
Requirements:
- User clicks "Forgot Password"
- Enters registered email
- Receives reset link via email
- Link expires in 24 hours
- User creates new password (8+ chars, 1 uppercase, 1 number)
- Old password cannot be reused
```

**Your Task:**
1. Write a prompt to generate test cases
2. Generate at least 15 test cases
3. Review for missing scenarios
4. Add security test cases

**Write your prompt here:**
```
[Your prompt]
```

---

### Exercise 3.2: Journey-Based Testing

**Feature: Online Shopping Cart**

```
User Journey:
1. Browse products
2. Add item to cart
3. Update quantity
4. Apply discount code
5. Proceed to checkout
6. Enter shipping info
7. Select payment method
8. Complete order
9. Receive confirmation
```

**Your Task:**
1. Write a journey-based test prompt
2. Generate tests for each step
3. Include end-to-end scenarios

**Write your prompt here:**
```
[Your prompt]
```

---

### Exercise 3.3: Domain-Specific Testing

**Feature: Mobile App File Upload**

```
Requirements:
- Support PDF, DOC, JPG, PNG
- Max file size: 10MB
- Drag and drop or browse
- Progress indicator
- Multiple file upload
- Validate file type server-side
```

**Your Task:**
1. Write a mobile-specific test prompt
2. Include device-specific tests
3. Add network condition tests
4. Cover permission scenarios

**Write your prompt here:**
```
[Your prompt]
```

---

### Exercise 3.4: API Testing

**Endpoint: POST /api/orders**

```json
Request:
{
  "productId": "string (required)",
  "quantity": "integer (required, 1-100)",
  "customerId": "string (required)",
  "shippingAddress": {
    "street": "string",
    "city": "string",
    "zipCode": "string",
    "country": "string"
  }
}
```

**Your Task:**
1. Generate comprehensive API test cases
2. Include boundary testing
3. Add security tests
4. Cover error scenarios

**Write your prompt here:**
```
[Your prompt]
```

---

## 🌟 Real-World Example: Complete Workflow

**Scenario:** You need test cases for a product search feature

### Step 1: Initial Generation

```
Act as a Senior QA Engineer.

Generate test cases for a product search feature in an e-commerce app.

Features:
- Text search box
- Filters (category, price range, rating, brand)
- Sort options (price low/high, popularity, newest, rating)
- Autocomplete suggestions
- "No results" handling
- Search history

Include positive, negative, and edge cases.
Format as a table with: Test ID, Scenario, Steps, Expected Result, Priority.
```

### Step 2: Add Specific Scenarios

```
The search feature also includes:
- Voice search capability (mobile)
- Barcode scanner
- Image search
- "Did you mean..." suggestions for typos

Add test cases for these additional features.
```

### Step 3: Security Focus

```
Add security test cases for the search feature covering:
- SQL injection in search field
- XSS attacks in search input
- Parameter tampering in filters
- Excessive requests (DoS protection)
- Input length limits
```

### Step 4: Performance Tests

```
Add performance test cases:
- Search response time (<2 seconds)
- Autocomplete latency (<200ms)
- Large result sets (1000+ products)
- Concurrent search requests
- Filter combination performance
```

### Step 5: Consolidate and Finalize

Combine all generated tests, remove duplicates, assign priorities, add test IDs, and export to your test management tool.

---

## 📝 Key Takeaways

```
┌─────────────────────────────────────────────────────────────────┐
│  LESSON 1 KEY TAKEAWAYS                                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✅ AI can generate comprehensive test cases 70-80% faster     │
│                                                                  │
│  ✅ Provide detailed requirements for better results            │
│                                                                  │
│  ✅ Use specific prompts for different test types:             │
│     - Persona-based for UX coverage                             │
│     - Journey-based for E2E scenarios                           │
│     - Risk-based for prioritization                             │
│                                                                  │
│  ✅ Always review and refine AI-generated test cases           │
│                                                                  │
│  ✅ Iterate to add missing scenarios                            │
│                                                                  │
│  ✅ Include technical context for better domain coverage       │
│                                                                  │
│  ✅ Specify output format for consistency                       │
│                                                                  │
│  ✅ Combine AI generation with human expertise                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ Lesson 1 Checklist

Before moving on, ensure you can:

- [ ] Write an effective prompt for test case generation
- [ ] Apply persona-based testing technique
- [ ] Apply journey-based testing technique
- [ ] Apply risk-based testing technique
- [ ] Generate tests for Web, Mobile, and API
- [ ] Use different output formats (table, Gherkin, checklist)
- [ ] Review and refine AI-generated test cases
- [ ] Iterate to add missing scenarios

---

## 🔗 Next Steps

Continue to:
- [Lesson 2: Bug Report Generation](./02-bug-report-generation.md)
- [Lesson 3: Test Strategy and Planning](./03-test-strategy-and-planning.md)

---

## 📖 Glossary

| Term | Definition |
|------|------------|
| **Positive Test** | Test that verifies expected behavior with valid input |
| **Negative Test** | Test that verifies proper error handling with invalid input |
| **Edge Case** | Unusual but valid scenario at the boundary of expected behavior |
| **Boundary Value** | Testing at the exact limits (min/max) of valid input ranges |
| **Happy Path** | The ideal scenario where everything works as expected |
| **Precondition** | State that must be true before test execution |
| **Expected Result** | The outcome that should occur if the test passes |
| **Test Data** | Specific values used to execute the test |
| **Persona** | Fictional character representing a user type |
| **User Journey** | Complete end-to-end flow a user takes |

---

## 📚 Additional Resources

- [ISTQB Test Design Techniques](https://www.istqb.org/)
- [Testing Heuristics Cheat Sheet](https://www.ministryoftesting.com/)
- [Boundary Value Analysis Guide](https://www.guru99.com/boundary-value-analysis.html)
- [Equivalence Partitioning Tutorial](https://www.softwaretestinghelp.com/equivalence-partitioning/)

---

**Great progress! Continue to Lesson 2 to learn about Bug Report Generation! 🚀**
