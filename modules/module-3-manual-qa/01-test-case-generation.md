# Module 3: Prompt Engineering for Manual QA

## Lesson 1: Test Case Generation with AI

### Introduction

As a manual QA, one of your most time-consuming tasks is creating comprehensive test cases. AI can accelerate this process while improving coverage and quality.

### The Test Case Generation Framework

```
INPUT: Requirements/User Story
   ↓
AI PROMPT: Structured request
   ↓
OUTPUT: Comprehensive test cases
   ↓
REVIEW: Human validation & refinement
```

### Basic Test Case Generation

#### Prompt Template:
```
Act as a Senior QA Engineer with expertise in [domain].

Feature: [Feature name]

Requirements:
[List requirements or user story]

Generate test cases including:
- Positive test scenarios
- Negative test scenarios
- Edge cases
- Boundary value analysis

Format as a table with columns:
| Test ID | Test Scenario | Preconditions | Test Steps | Test Data | Expected Result | Priority |
```

#### Example 1: Login Feature

**Prompt:**
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

**Expected Output Quality:**
- 15-25 test cases
- All requirement areas covered
- Mix of positive/negative tests
- Security test cases included
- Clear, executable steps

### Advanced Test Case Generation

#### Technique 1: Persona-Based Testing

**Prompt:**
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
   - Forgiving search (typos)

3. Mobile User (Alex, 22):
   - Uses on-the-go
   - Voice search
   - Touch gestures

For each persona, create 5 test scenarios that validate their specific needs.
Format as grouped test cases by persona.
```

#### Technique 2: Journey-Based Testing

**Prompt:**
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
- Happy path
- Error scenarios
- Navigation flows
- Data validation
- Email delivery

Include end-to-end test scenarios that span multiple steps.
```

#### Technique 3: Risk-Based Testing

**Prompt:**
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
- Impact if bug found
- Likelihood of occurrence
- Testing priority
```

### Domain-Specific Test Cases

#### Web Application Testing

**Prompt:**
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
   - Form field validation
   - Submit/cancel actions
   - Data persistence

2. UI/UX Testing:
   - Responsive design
   - Browser compatibility
   - Accessibility (WCAG 2.1)

3. Integration Testing:
   - API interactions
   - Error handling
   - Session management

4. Performance:
   - Load time
   - Auto-save intervals
   - Large file uploads

Format: Organized by test type, with priority for each.
```

#### Mobile Application Testing

**Prompt:**
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
   - Different Android versions (9.0 to 13.0)
   - Camera permissions
   - Storage permissions

2. Functional:
   - Take photo vs select from gallery
   - Multiple photo selection
   - Image editing before upload
   - Offline mode

3. Non-Functional:
   - Battery consumption
   - Memory usage
   - Network handling (2G/3G/4G/5G/WiFi)
   - Large file handling

4. Edge Cases:
   - Camera not available
   - Storage full
   - App interrupted during upload
   - Network switches during upload

Organize by platform (iOS/Android/Both) and priority.
```

#### API Testing

**Prompt:**
```
Act as an API Testing Expert.

API Endpoint: POST /api/v1/users

Request Body:
{
  "email": "string",
  "password": "string",
  "firstName": "string",
  "lastName": "string",
  "age": integer
}

Response Codes:
- 201: User created
- 400: Validation error
- 409: User already exists
- 500: Server error

Generate API test cases covering:

1. Positive Scenarios:
   - Valid request body
   - All optional fields
   - Minimal required fields

2. Negative Scenarios:
   - Missing required fields
   - Invalid data types
   - Invalid formats
   - SQL injection attempts
   - XSS attempts

3. Boundary Testing:
   - Email length limits
   - Password length (min/max)
   - Age range validation
   - Special characters

4. Performance:
   - Response time expectations
   - Concurrent requests
   - Rate limiting

Format as: Request → Expected Status → Expected Response Body
```

### Test Case Categories

#### 1. Functional Test Cases

**Prompt:**
```
Generate functional test cases for [feature] that verify:
- Core functionality works as specified
- All features behave according to requirements
- Business rules are enforced
- Data flows correctly through the system
```

#### 2. UI/UX Test Cases

**Prompt:**
```
Generate UI/UX test cases for [feature] covering:
- Layout and alignment
- Color scheme and branding
- Typography and readability
- Navigation and flow
- Responsive design
- Accessibility (screen readers, keyboard navigation)
- Cross-browser compatibility
```

#### 3. Negative Test Cases

**Prompt:**
```
Generate negative test cases for [feature] including:
- Invalid inputs (wrong format, type, range)
- Missing required data
- Boundary violations
- Unauthorized access
- Concurrent operations
- System overload scenarios
- Network failures
```

#### 4. Edge Case Test Cases

**Prompt:**
```
Think creatively and generate edge case test cases for [feature]:
- Unusual but valid inputs
- Maximum/minimum values
- Empty states
- First-time user vs returning user
- Multiple browser tabs
- Slow network conditions
- Browser back button usage
- Session timeout scenarios
```

### Optimizing Test Case Output

#### Getting Specific Formats

**As Gherkin (BDD):**
```
Generate test cases in Gherkin syntax:

Feature: [Feature name]

Scenario: [Scenario name]
  Given [precondition]
  When [action]
  Then [expected result]
  And [additional assertion]
```

**As Test Steps:**
```
Generate detailed test steps:

Test Case: TC-001 - Valid Login

Pre-conditions:
1. User is on the login page
2. User has valid credentials

Test Steps:
1. Step: Enter valid email "test@example.com"
   Expected: Email field accepts input
2. Step: Enter valid password "Test@1234"
   Expected: Password is masked
3. Step: Click "Login" button
   Expected: User redirected to dashboard

Post-conditions:
- User is logged in
- Session is active
```

**As Checklist:**
```
Generate test cases as a checklist:

Login Feature Testing Checklist:

□ Valid credentials login
□ Invalid email format
□ Wrong password
□ Account lockout after 5 attempts
□ Remember me functionality
□ Forgot password link
□ Password visibility toggle
□ XSS in email field
□ SQL injection in password
□ Session timeout handling
```

### Combining AI-Generated with Manual Review

**Workflow:**

```
1. Generate initial test cases with AI
   ↓
2. Review for completeness
   ↓
3. Add missing scenarios (prompt AI)
   ↓
4. Review for accuracy
   ↓
5. Refine test data
   ↓
6. Add test case IDs
   ↓
7. Link to requirements
   ↓
8. Final review and approval
```

**Review Checklist:**
- [ ] All requirements covered?
- [ ] Positive and negative cases?
- [ ] Edge cases included?
- [ ] Security scenarios present?
- [ ] Test steps are clear?
- [ ] Expected results defined?
- [ ] Test data specified?
- [ ] Priority assigned?
- [ ] Prerequisites listed?

### Exercise 3.1: Generate Test Cases

**Task:** Use AI to generate test cases for these features:

**Feature 1: Password Reset**
```
Requirements:
- User clicks "Forgot Password"
- Enters registered email
- Receives reset link via email
- Link expires in 24 hours
- User creates new password (8+ chars, 1 uppercase, 1 number)
- Old password cannot be reused
```

Your prompt:
```
[Write your prompt here]
```

**Feature 2: Shopping Cart**
```
Requirements:
- Add/remove items
- Update quantities
- Apply discount code
- Calculate tax
- Show shipping options
- Persist cart across sessions
```

Your prompt:
```
[Write your prompt here]
```

**Feature 3: File Upload**
```
Requirements:
- Support PDF, DOC, JPG, PNG
- Max file size: 10MB
- Drag and drop or browse
- Progress indicator
- Multiple file upload
- Validate file type server-side
```

Your prompt:
```
[Write your prompt here]
```

### Real-World Example

**Scenario:** You need test cases for a search feature

**Step-by-Step Approach:**

**Prompt 1: Initial Generation**
```
Act as a QA Engineer.

Generate test cases for a product search feature in an e-commerce app.

Features:
- Text search
- Filters (category, price range, rating)
- Sort options (price, popularity, newest)
- Autocomplete suggestions
- "No results" handling

Include positive, negative, and edge cases.
Format as a table with: Test ID, Scenario, Steps, Expected Result, Priority.
```

**Prompt 2: Add More Detail**
```
The search feature also includes:
- Voice search capability
- Search history
- Recent searches
- Barcode scanner

Add test cases for these features.
```

**Prompt 3: Security Focus**
```
Add security test cases for the search feature covering:
- SQL injection
- XSS attacks
- Parameter tampering
- Excessive requests (DoS)
```

**Prompt 4: Performance**
```
Add performance test cases:
- Search response time
- Autocomplete latency
- Large result sets
- Concurrent searches
```

### Key Takeaways

✅ AI can generate comprehensive test cases quickly
✅ Provide detailed requirements for better results
✅ Use specific prompts for different test types
✅ Always review and refine AI-generated test cases
✅ Combine AI generation with human expertise
✅ Iterate to add missing scenarios
✅ Organize test cases by type, priority, or risk
✅ Use domain-specific context for better results

### Next Steps
- Move to [Module 4: Prompt Engineering for Automation Testing](../module-4-automation-testing/README.md) (or continue with Module 3 when Lesson 2 is available)
