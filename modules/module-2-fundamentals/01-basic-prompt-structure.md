# Module 2: Fundamentals of Prompt Engineering

## Lesson 1: Basic Prompt Structure

---

## 🎯 Lesson Overview

**Time Required:** 60-90 minutes  
**Difficulty:** Beginner to Intermediate  
**Prerequisites:** Module 1 completed

In this lesson, you'll master:
- The 5-component RCTFC framework
- How to write each component effectively
- Real-world examples for QA and development
- Common patterns and templates
- Hands-on practice exercises

---

## 🧱 The 5-Component Framework: RCTFC

Every great prompt has five key components:

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                  │
│     R  ───►  ROLE        Who should the AI be?                   │
│     │                                                            │
│     C  ───►  CONTEXT     What's the background?                  │
│     │                                                            │
│     T  ───►  TASK        What do you want done?                  │
│     │                                                            │
│     F  ───►  FORMAT      How should output look?                 │
│     │                                                            │
│     C  ───►  CONSTRAINTS What are the limits?                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Memory Aid:** **R**eally **C**lever **T**esters **F**ind **C**ritical bugs

---

## 1️⃣ ROLE - Who Should the AI Be?

### What It Does

The Role component tells the AI what expertise, perspective, and approach to use. It's like hiring a specialist for a specific job.

### Why It Matters

```
Without Role:
  "Write test cases" → Generic, textbook-style response

With Role:
  "Act as a Senior QA Engineer with 10 years experience..." 
  → Expert-level, practical, industry-standard response
```

### How to Write Effective Roles

**Basic Formula:**
```
Act as a [seniority level] [job title] with expertise in [specialization]
```

### Examples for Different Needs

#### For QA Professionals

| Role Statement | Best For |
|----------------|----------|
| "Act as a Senior QA Engineer" | General testing tasks |
| "Act as a Manual Testing Expert" | Test case creation |
| "Act as a Test Automation Architect" | Framework design |
| "Act as a Security Testing Specialist" | Security test cases |
| "Act as a Performance Testing Expert" | Load/stress testing |
| "Act as an API Testing Specialist" | API test design |
| "Act as a Mobile Testing Expert" | iOS/Android testing |

#### For Developers

| Role Statement | Best For |
|----------------|----------|
| "Act as a Senior Python Developer" | Python code |
| "Act as a Full-Stack Developer" | Web applications |
| "Act as a DevOps Engineer" | CI/CD, infrastructure |
| "Act as a Software Architect" | Design patterns |
| "Act as a Code Review Expert" | Code analysis |
| "Act as a Database Administrator" | SQL, optimization |

#### For Test Automation

| Role Statement | Best For |
|----------------|----------|
| "Act as a Selenium Expert using Python" | Web automation |
| "Act as an API Testing Expert using Python requests" | API automation |
| "Act as a Playwright Automation Specialist" | Modern web testing |
| "Act as a pytest Framework Expert" | Test framework setup |

### Advanced Role Techniques

**Combining Roles:**
```
Act as a Senior QA Engineer who is also a Security Expert
```

**Adding Personality:**
```
Act as a meticulous QA Engineer who is thorough and pays attention to edge cases
```

**Setting Experience Level:**
```
Act as a QA Engineer with 15 years of experience in e-commerce testing
```

### 🔴 Common Mistakes

❌ **Too Vague:**
```
"You are an expert"
```

❌ **Wrong Role for Task:**
```
"Act as a Chef" (for a testing task)
```

✅ **Correct:**
```
"Act as a Senior QA Engineer specializing in web application testing"
```

---

## 2️⃣ CONTEXT - What's the Background?

### What It Does

Context provides the background information the AI needs to understand your specific situation. Without context, the AI makes generic assumptions.

### Why It Matters

```
Without Context:
  "Test the login" → Could be any login, anywhere

With Context:
  "Test the login feature of our e-commerce website that uses 
   email and password authentication, supports OAuth with Google, 
   and locks accounts after 5 failed attempts"
  → Specific, relevant test cases
```

### What to Include in Context

```
┌─────────────────────────────────────────────────────────────────┐
│  CONTEXT CHECKLIST                                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✅ Application/Feature Description                             │
│     What are you working on?                                    │
│                                                                  │
│  ✅ Technical Stack                                              │
│     What technologies are involved?                             │
│                                                                  │
│  ✅ Requirements/Specifications                                  │
│     What should it do?                                          │
│                                                                  │
│  ✅ User Types                                                   │
│     Who uses this feature?                                      │
│                                                                  │
│  ✅ Business Context                                             │
│     Why does this matter?                                       │
│                                                                  │
│  ✅ Constraints/Limitations                                      │
│     What can't change?                                          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Examples of Good Context

**For Test Case Generation:**
```
Context:
I'm testing a user registration feature for an e-commerce website.

Feature Details:
- Fields: Email, Password, Confirm Password, First Name, Last Name
- Email must be unique in the system
- Password requirements: 8+ characters, 1 uppercase, 1 number, 1 special char
- Terms and conditions checkbox (required)
- CAPTCHA verification
- Sends verification email after registration

Technical Stack:
- Frontend: React
- Backend: Node.js with Express
- Database: PostgreSQL
- Authentication: JWT tokens
```

**For Code Generation:**
```
Context:
I'm building test automation for our company's REST API.

API Details:
- Base URL: https://api.example.com/v1
- Authentication: Bearer token in header
- Endpoints:
  - POST /users - Create user
  - GET /users/{id} - Get user
  - PUT /users/{id} - Update user
  - DELETE /users/{id} - Delete user

Response Format: JSON
Error Format: {"error": "message", "code": number}

Current Framework: pytest with requests library
```

**For Code Review:**
```
Context:
This code is from a test automation framework used by a team of 5 QA engineers.
- Written by a junior engineer
- Should follow PEP 8 style guide
- Used in CI/CD pipeline (runs on every commit)
- Must be maintainable by the whole team
```

### 🔴 Common Mistakes

❌ **No Context:**
```
"Generate test cases"
```

❌ **Irrelevant Context:**
```
"I started working here 3 years ago and my favorite color is blue.
Generate test cases for login."
```

✅ **Correct:**
```
"Context: Testing a login page with email/password fields, 
'Remember Me' checkbox, and 'Forgot Password' link. 
The system locks accounts after 5 failed attempts."
```

---

## 3️⃣ TASK - What Do You Want?

### What It Does

The Task component clearly states what you want the AI to do. Be specific and action-oriented.

### Why It Matters

```
Vague Task:
  "Help with testing" → AI doesn't know what to produce

Clear Task:
  "Generate 10 functional test cases" → Specific, countable output
```

### Action Words for Tasks

| Category | Action Words |
|----------|--------------|
| **Create** | Generate, Create, Write, Build, Design, Develop |
| **Analyze** | Review, Analyze, Evaluate, Assess, Check, Examine |
| **Transform** | Convert, Translate, Refactor, Reformat, Optimize |
| **Explain** | Explain, Describe, Summarize, Clarify, Define |
| **Find** | Identify, Find, Locate, Discover, Detect |

### Examples of Good Tasks

**For Test Cases:**
```
Task: Generate 15 test cases for the checkout process including:
- 5 positive test cases (happy path)
- 5 negative test cases (error handling)
- 5 edge cases (boundary conditions)
```

**For Code:**
```
Task: Create a Python function that:
1. Accepts a list of test results (pass/fail)
2. Calculates pass rate percentage
3. Returns a summary dictionary
4. Handles empty lists gracefully
```

**For Analysis:**
```
Task: Review this code and identify:
1. Potential bugs
2. Security vulnerabilities
3. Performance issues
4. Best practice violations
Provide specific line numbers and fixes for each issue.
```

**For Documentation:**
```
Task: Create a README.md file that explains:
1. What this test framework does
2. How to install it
3. How to run tests
4. How to add new tests
5. Troubleshooting common issues
```

### 🔴 Common Mistakes

❌ **Too Vague:**
```
"Help with this"
```

❌ **Multiple Unrelated Tasks:**
```
"Create test cases, also write a README, and fix my code, 
and explain Docker"
```

❌ **No Clear Outcome:**
```
"Think about testing"
```

✅ **Correct:**
```
"Generate 10 test cases covering positive, negative, and edge case scenarios"
```

---

## 4️⃣ FORMAT - How Should Output Look?

### What It Does

Format specification tells the AI exactly how to structure the response. This makes outputs immediately usable.

### Why It Matters

```
Without Format:
  Output is a wall of text → Requires manual reformatting

With Format:
  Output is a structured table → Ready to copy into test management tool
```

### Common Formats

#### Tables (Great for Test Cases)

```
Format the output as a markdown table with columns:
| Test ID | Description | Steps | Expected Result | Priority |
```

**Result:**
| Test ID | Description | Steps | Expected Result | Priority |
|---------|-------------|-------|-----------------|----------|
| TC001 | Valid login | 1. Enter email... | User logged in | High |

#### Bullet Lists

```
Format as a bullet list with:
- Category name (bold)
- Sub-items with details
```

**Result:**
- **Positive Tests**
  - Valid credentials login
  - Remember me functionality
- **Negative Tests**
  - Invalid email format
  - Wrong password

#### Numbered Steps

```
Format as numbered steps:
1. Step description
   - Expected result
```

#### JSON

```
Format as JSON:
{
  "test_cases": [
    {
      "id": "string",
      "description": "string",
      "steps": ["string"],
      "expected": "string"
    }
  ]
}
```

#### Code

```
Format as Python code with:
- Imports at the top
- Functions with docstrings
- Comments for complex logic
- Example usage at the bottom
```

#### Gherkin/BDD

```
Format in Gherkin syntax:
Feature: [Feature name]
  Scenario: [Scenario name]
    Given [precondition]
    When [action]
    Then [expected result]
```

### Detailed Format Specifications

**For Comprehensive Test Cases:**
```
Format each test case as:

## TC-XXX: [Title]

**Priority:** [High/Medium/Low]
**Type:** [Positive/Negative/Edge]

**Preconditions:**
- [Precondition 1]

**Test Steps:**
1. [Step 1]
2. [Step 2]

**Test Data:**
- [Data needed]

**Expected Result:**
- [What should happen]

**Notes:**
- [Any additional information]
```

### 🔴 Common Mistakes

❌ **No Format Specified:**
```
"Give me test cases"
```

❌ **Conflicting Formats:**
```
"Format as JSON and also as a table and also as bullet points"
```

✅ **Correct:**
```
"Format as a markdown table with columns: ID, Description, Steps, Expected, Priority"
```

---

## 5️⃣ CONSTRAINTS - What Are the Limits?

### What It Does

Constraints set boundaries and requirements. They narrow the focus and ensure the output meets your specific needs.

### Types of Constraints

```
┌─────────────────────────────────────────────────────────────────┐
│  CONSTRAINT CATEGORIES                                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  📊 QUANTITY                                                     │
│     "Limit to 10 test cases"                                    │
│     "Provide exactly 5 examples"                                │
│                                                                  │
│  🔧 TECHNICAL                                                    │
│     "Use Python 3.9+"                                           │
│     "Follow PEP 8 standards"                                    │
│     "Use pytest framework"                                      │
│                                                                  │
│  🎯 SCOPE                                                        │
│     "Focus only on API testing"                                 │
│     "Exclude UI tests"                                          │
│     "Only security-related scenarios"                           │
│                                                                  │
│  ⭐ QUALITY                                                      │
│     "Include error handling"                                    │
│     "Add comments and docstrings"                               │
│     "Make it production-ready"                                  │
│                                                                  │
│  📝 STYLE                                                        │
│     "Keep explanations simple"                                  │
│     "Use professional tone"                                     │
│     "Beginner-friendly language"                                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Examples of Good Constraints

**For Test Cases:**
```
Constraints:
- Limit to 15 most critical test cases
- Focus on functional testing only (no performance)
- Prioritize security-related scenarios
- Each test case should be executable in under 2 minutes
- Include test data for each case
```

**For Code:**
```
Constraints:
- Python 3.9+ compatible
- Follow PEP 8 style guide
- Include type hints
- Use pytest for test functions
- No external dependencies beyond requests and pytest
- Include docstrings for all functions
- Handle exceptions gracefully
```

**For Documentation:**
```
Constraints:
- Keep language simple (for junior team members)
- Maximum 500 words
- Include code examples
- Use markdown formatting
- Avoid jargon without explanation
```

### 🔴 Common Mistakes

❌ **Contradictory Constraints:**
```
"Be comprehensive" AND "Limit to 3 test cases"
```

❌ **Impossible Constraints:**
```
"Write 100 detailed test cases in JSON format in under 50 words"
```

✅ **Correct:**
```
"Limit to 10 test cases, prioritize security tests, use Python 3.9+"
```

---

## 🎯 Putting It All Together

### Complete Prompt Template

```
[ROLE]
Act as a [seniority] [job title] with expertise in [specialization].

[CONTEXT]
Context:
I'm working on [what you're working on].

Details:
- [Specific detail 1]
- [Specific detail 2]
- [Specific detail 3]

Technical Stack: [technologies involved]

[TASK]
Task:
[Action verb] [specific deliverable] that includes:
1. [Requirement 1]
2. [Requirement 2]
3. [Requirement 3]

[FORMAT]
Format:
[Specify exact output structure]

[CONSTRAINTS]
Constraints:
- [Constraint 1]
- [Constraint 2]
- [Constraint 3]
```

### Example 1: Complete Test Case Prompt

```
Act as a Senior QA Engineer with 10 years of experience in e-commerce testing.

Context:
I'm testing the shopping cart feature of our online store.

Feature Details:
- Users can add items to cart
- Update quantities (1-99 range)
- Remove items
- Apply discount codes
- View cart total with tax
- Proceed to checkout

Technical Stack: React frontend, Node.js backend, PostgreSQL database

Task:
Generate 15 test cases including:
- 5 positive test cases (happy path scenarios)
- 5 negative test cases (error handling scenarios)
- 5 edge cases (boundary conditions)

Format as a markdown table with columns:
| Test ID | Category | Description | Preconditions | Steps | Expected Result | Priority |

Constraints:
- Focus on functional testing only
- Each test should be independent
- Include specific test data values
- Prioritize high-risk scenarios
- Consider multi-currency support
```

### Example 2: Complete Code Generation Prompt

```
Act as a Senior Test Automation Engineer expert in Selenium with Python.

Context:
I'm building an automated test suite for our web application's login functionality.

Page Details:
- URL: https://app.example.com/login
- Email field: id="email-input"
- Password field: id="password-input"
- Login button: id="login-btn"
- Error message: class="error-message"
- Success indicator: Dashboard page loads with class="user-dashboard"

Task:
Create a complete Selenium test file that:
1. Tests successful login with valid credentials
2. Tests login failure with invalid password
3. Tests login failure with invalid email format
4. Tests account lockout after 5 failed attempts
5. Tests "Remember Me" functionality

Format as a complete Python file with:
- Imports at the top
- Page Object class for login page
- Test class with pytest
- Fixtures in conftest.py (separate code block)
- Clear comments explaining each section

Constraints:
- Python 3.9+
- Selenium 4.x
- pytest framework
- Use explicit waits (WebDriverWait)
- Follow PEP 8 standards
- Include error handling
- Add docstrings to all functions
- Tests should be independent (no dependencies between tests)
```

### Example 3: Complete Code Review Prompt

```
Act as a Senior Software Engineer and Code Review Expert.

Context:
This code is part of a test automation framework used by a team of 5 QA engineers.
- Written by a junior engineer
- Will be maintained by the whole team
- Runs in CI/CD pipeline
- Should follow company coding standards

Code to review:
```python
def login(driver, username, password):
    driver.get("http://example.com/login")
    driver.find_element("id", "user").send_keys(username)
    driver.find_element("id", "pass").send_keys(password)
    driver.find_element("id", "submit").click()
    time.sleep(5)
    return driver.find_element("id", "welcome").text
```

Task:
Review this code and identify:
1. Code quality issues
2. Best practices violations
3. Error handling gaps
4. Maintainability concerns
5. Performance problems

For each issue:
- Explain why it's a problem
- Show the corrected code
- Rate severity (Critical/High/Medium/Low)

Format:
## Issue [Number]: [Title]
**Severity:** [Level]
**Problem:** [Description]
**Solution:** [Fix with code example]

Constraints:
- Focus on actionable feedback
- Prioritize critical issues first
- Provide working code examples
- Keep explanations concise but clear
- Consider team maintenance perspective
```

---

## 💡 Prompt Patterns

### Pattern 1: The Standard Pattern

```
[Role] + [Context] + [Task] + [Format] + [Constraints]
```

### Pattern 2: The Problem-Solution Pattern

```
I have this problem: [problem description]
I need: [what you need]
Please: [specific request]
Format: [output format]
```

### Pattern 3: The Progressive Pattern

```
Step 1: [Initial broad request]
Step 2: "Based on that, now [more specific request]"
Step 3: "Refine this by [refinement]"
```

### Pattern 4: The Example-Based Pattern

```
Here's an example of what I want:
[Example 1]

Now create similar [output type] for:
[Your actual request]
```

---

## 💻 Practice Exercises

### Exercise 2.1: Build Complete Prompts

**Task:** Create complete prompts for these scenarios:

**Scenario A: Password Reset Testing**

Requirements:
- User enters email
- System sends reset link
- Link expires in 24 hours
- Password must be 8+ chars with uppercase and number

Create your prompt:
```
Role: ___________________________

Context: ___________________________

Task: ___________________________

Format: ___________________________

Constraints: ___________________________
```

**Scenario B: API Test Automation**

Requirements:
- REST API endpoint: POST /api/orders
- Creates a new order
- Accepts JSON body with items and customer info
- Returns order ID and total

Create your prompt:
```
Role: ___________________________

Context: ___________________________

Task: ___________________________

Format: ___________________________

Constraints: ___________________________
```

### Exercise 2.2: Fix These Prompts

**Fix each poor prompt using the RCTFC framework:**

**Poor Prompt 1:**
```
"Write code for testing"
```

Your improved version:
```
[Write your complete prompt here]
```

**Poor Prompt 2:**
```
"I need test cases for my app"
```

Your improved version:
```
[Write your complete prompt here]
```

**Poor Prompt 3:**
```
"Review my code and make it better"
```

Your improved version:
```
[Write your complete prompt here]
```

### Exercise 2.3: Try and Compare

**Test this sequence in ChatGPT:**

**Version 1 (Basic):**
```
Write test cases for a search feature
```

**Version 2 (With Role + Task):**
```
Act as a QA Engineer. Write 5 test cases for a product search feature.
```

**Version 3 (Complete RCTFC):**
```
Act as a Senior QA Engineer with e-commerce experience.

Context:
Testing product search on an online store:
- Text search box
- Auto-complete suggestions
- Category filter
- Price range filter
- Sort options (price, popularity, newest)
- Shows "no results" for empty matches

Task:
Generate 10 test cases covering:
- 4 positive scenarios
- 4 negative scenarios
- 2 edge cases

Format as a table:
| ID | Type | Description | Steps | Expected | Priority |

Constraints:
- Focus on functional testing
- Include specific test data
- Consider mobile responsiveness
```

**Document your observations:**
- How did the outputs differ?
- Which was most useful?
- How much more effort was Version 3?
- Was the extra effort worth it?

---

## 📋 Prompt Quality Checklist

Use this before submitting any prompt:

- [ ] **Role:** Clear expertise defined?
- [ ] **Context:** Enough background provided?
- [ ] **Task:** Specific action requested?
- [ ] **Format:** Output structure specified?
- [ ] **Constraints:** Boundaries set?
- [ ] **Clarity:** Is it easy to understand?
- [ ] **Focused:** One main task (not multiple)?
- [ ] **Specific:** Numbers, names, details included?

---

## 📝 Key Takeaways

1. **RCTFC = Role + Context + Task + Format + Constraints**
2. **Role** sets the expertise level and perspective
3. **Context** provides essential background information
4. **Task** clearly states what you want done
5. **Format** ensures usable, structured output
6. **Constraints** set boundaries and requirements
7. **More specific = better results**
8. **Iterate** based on responses

---

## ✅ Lesson 1 Checklist

Before moving on, ensure you can:

- [ ] Explain the RCTFC framework
- [ ] Write effective Role statements
- [ ] Provide comprehensive Context
- [ ] Specify clear Tasks
- [ ] Define output Formats
- [ ] Set appropriate Constraints
- [ ] Complete all practice exercises
- [ ] Build a complete prompt from scratch

---

## 🔗 Next Steps

You've mastered the prompt structure! 🎉

**Continue to:** [Lesson 2: Parameters and Temperature Settings](./02-parameters-and-settings.md)

---

## 📖 Quick Reference Card

```
┌─────────────────────────────────────────────────────────────────┐
│  RCTFC QUICK REFERENCE                                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  R - ROLE                                                        │
│      "Act as a [Senior/Expert] [Job Title] with expertise in..." │
│                                                                  │
│  C - CONTEXT                                                     │
│      "I'm working on [feature] with [details]..."               │
│                                                                  │
│  T - TASK                                                        │
│      "[Action verb] [specific number] [deliverable]..."         │
│                                                                  │
│  F - FORMAT                                                      │
│      "Format as [table/code/list/JSON] with [specifics]..."     │
│                                                                  │
│  C - CONSTRAINTS                                                 │
│      "Limit to... Focus on... Use [technology]..."              │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

**Excellent work! On to parameters and settings! 🚀**
