# Module 6: Advanced Techniques

## Lesson 2: Few-Shot Learning

---

## 🎯 Lesson Overview

**Time Required:** 45 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** Lesson 1 completed

Few-shot learning is a powerful technique that teaches AI through examples. By showing 2-5 examples of what you want, you can achieve remarkable consistency and quality in outputs.

---

## 📚 What is Few-Shot Learning?

Few-shot learning means providing a few examples (typically 2-5) of the desired input-output pattern **before** asking the AI to generate new content. The AI learns the pattern from your examples and applies it.

### Zero-Shot vs Few-Shot

```
┌─────────────────────────────────────────────────────────────────┐
│  ZERO-SHOT (No Examples)                                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Prompt: "Write a test case for login"                          │
│                                                                  │
│  Result: AI guesses the format you want                         │
│          (May not match your needs)                             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  FEW-SHOT (With Examples)                                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Prompt:                                                         │
│  "Here are example test cases in our format:                    │
│                                                                  │
│   Example 1:                                                     │
│   TC-001: Verify login with valid credentials                   │
│   Steps: 1. Go to login 2. Enter valid email 3. Submit          │
│   Expected: User logged in                                       │
│                                                                  │
│   Example 2:                                                     │
│   TC-002: Verify login fails with wrong password                │
│   Steps: 1. Go to login 2. Enter wrong password 3. Submit       │
│   Expected: Error message shown                                  │
│                                                                  │
│   Now write test cases for password reset in this format."      │
│                                                                  │
│  Result: AI follows your exact format and style!                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔑 Key Benefits

| Benefit | Description |
|---------|-------------|
| **Consistency** | Output always matches your format |
| **Quality** | AI understands exactly what you want |
| **Pattern Matching** | Complex requirements become clear |
| **Less Iteration** | Fewer corrections needed |
| **Team Standards** | Enforce coding/documentation standards |

---

## 📐 The Basic Pattern

```
Here are examples of [what you want]:

Example 1:
Input: [example input]
Output: [example output you want]

Example 2:
Input: [example input]
Output: [example output you want]

Example 3:
Input: [example input]
Output: [example output you want]

Now, using the same format:
Input: [your actual input]
Output:
```

---

## 💡 Practical Examples

### Example 1: Test Case Formatting

**Prompt:**
```
Generate test cases using this exact format:

Example 1:
Feature: User Login
Test ID: TC_LOGIN_001
Title: Verify successful login with valid credentials
Priority: High
Preconditions:
- User has registered account
- User is on login page
Steps:
1. Enter valid email "test@example.com"
2. Enter valid password "Test@1234"
3. Click "Login" button
Expected Result: User redirected to dashboard
Actual Result: [To be filled during testing]

Example 2:
Feature: User Login
Test ID: TC_LOGIN_002
Title: Verify error message for invalid password
Priority: High
Preconditions:
- User has registered account
- User is on login page
Steps:
1. Enter valid email "test@example.com"
2. Enter invalid password "wrongpassword"
3. Click "Login" button
Expected Result: Error message "Invalid credentials" displayed
Actual Result: [To be filled during testing]

---

Now generate test cases for "Password Reset" feature following the EXACT same format.
```

---

### Example 2: Bug Report Formatting

**Prompt:**
```
Format bug reports exactly like this:

Example 1:
---
TITLE: Login button unresponsive on mobile Safari
SEVERITY: High
PRIORITY: P1
ENVIRONMENT: iOS 17.0, Safari, iPhone 15 Pro
STEPS TO REPRODUCE:
1. Open app on Safari mobile browser
2. Navigate to login page
3. Enter credentials
4. Tap "Login" button
EXPECTED: Login form submits, user authenticated
ACTUAL: Button does not respond to tap
WORKAROUND: Use Chrome browser instead
ATTACHMENTS: [video recording]
---

Example 2:
---
TITLE: Form validation error message invisible
SEVERITY: Medium
PRIORITY: P2
ENVIRONMENT: Windows 11, Chrome 120
STEPS TO REPRODUCE:
1. Open registration form
2. Leave required fields empty
3. Click "Submit"
EXPECTED: Red error messages appear below empty fields
ACTUAL: Error messages exist in DOM but have white text on white background
WORKAROUND: Inspect element to read error messages
ATTACHMENTS: [screenshot]
---

Now create a bug report for: "Search returns empty results when query contains apostrophe"
```

---

### Example 3: Code Review Comments

**Prompt:**
```
Write code review comments in this format:

Example 1:
File: auth.py, Line 45
Category: Security
Severity: Critical
Comment: Password is being logged in plain text. This is a security vulnerability.
Suggestion: Remove password logging or hash before logging.
Code: logger.info(f"Login attempt: {username}, {password}")  # ❌
Fixed: logger.info(f"Login attempt: {username}")  # ✅

Example 2:
File: utils.py, Line 23
Category: Performance
Severity: Medium
Comment: List is being searched with O(n) complexity in a loop, making overall O(n²)
Suggestion: Convert list to set for O(1) lookups
Code: if item in large_list:  # ❌
Fixed: if item in large_set:  # ✅ (convert list to set before loop)

---

Now review this code and provide comments in the same format:

def process_users(users):
    result = []
    for user in users:
        if user['email'] in [u['email'] for u in result]:
            continue
        result.append(user)
    return result
```

---

### Example 4: API Documentation

**Prompt:**
```
Document API endpoints in this format:

Example 1:
### GET /api/users/{id}

**Description:** Retrieve a specific user by ID.

**Authentication:** Bearer token required

**Parameters:**
| Name | Type | Location | Required | Description |
|------|------|----------|----------|-------------|
| id | string (UUID) | path | Yes | User's unique identifier |

**Response 200:**
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "email": "user@example.com",
  "name": "John Doe",
  "created_at": "2024-01-15T10:30:00Z"
}
```

**Response 404:**
```json
{
  "error": "User not found",
  "code": "USER_NOT_FOUND"
}
```

---

Now document the endpoint: POST /api/products (creates a new product with name, price, category)
```

---

## 📊 How Many Examples?

| Situation | Recommended # | Why |
|-----------|---------------|-----|
| Simple format | 2 examples | Pattern is clear |
| Complex format | 3-5 examples | Show edge cases |
| Classification | 1-2 per category | Cover all categories |
| Edge cases | Include 1 edge case | Show how to handle them |

---

## 🎯 Common Patterns

### Pattern 1: Input → Output Mapping

```
Convert these inputs to outputs:

Example 1:
Input: "John Smith"
Output: { "firstName": "John", "lastName": "Smith" }

Example 2:
Input: "Jane Mary Doe"
Output: { "firstName": "Jane", "lastName": "Doe", "middleName": "Mary" }

Now convert:
Input: "Robert James Wilson Jr."
Output:
```

### Pattern 2: Before → After (Transformation)

```
Refactor code following these examples:

Before:
if (x == true) { return true; } else { return false; }
After:
return x;

Before:
if (value != null) { if (value != "") { doSomething(); } }
After:
if (value) { doSomething(); }

Now refactor:
Before:
if (list.length > 0) { return list[0]; } else { return null; }
After:
```

### Pattern 3: Category Examples

```
Classify these issues:

Example 1:
Issue: "App crashes when uploading file larger than 10MB"
Category: BUG - FUNCTIONAL

Example 2:
Issue: "Page takes 8 seconds to load"
Category: BUG - PERFORMANCE

Example 3:
Issue: "Add dark mode support"
Category: FEATURE REQUEST

Example 4:
Issue: "Button text is hard to read"
Category: UX IMPROVEMENT

Now classify:
Issue: "SQL error shown to user when special characters in search"
Category:
```

---

## ⚠️ Common Mistakes

| Mistake | Solution |
|---------|----------|
| Too few examples | Add 1-2 more examples |
| Inconsistent examples | Make all examples follow same format |
| Examples too similar | Show variety and edge cases |
| No clear pattern | Make the pattern explicit |
| Examples too complex | Start simple, add complexity |

---

## 💻 Practical Exercises

### Exercise 6.2: Create Few-Shot Prompts

**Task 1:** Create a few-shot prompt for generating Gherkin test scenarios

**Your Prompt:**
```
[Create 2-3 examples of Gherkin scenarios, then ask AI to generate more]
```

---

**Task 2:** Create a few-shot prompt for writing docstrings

**Your Prompt:**
```
[Show 2-3 function docstring examples, then provide a function to document]
```

---

**Task 3:** Create a few-shot prompt for error message generation

**Your Prompt:**
```
[Show 2-3 user-friendly error messages for technical errors]
```

---

## 📝 Key Takeaways

```
┌─────────────────────────────────────────────────────────────────┐
│  FEW-SHOT LEARNING KEY TAKEAWAYS                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ✅ 2-5 examples usually work best                              │
│                                                                  │
│  ✅ Examples must be consistent in format                       │
│                                                                  │
│  ✅ Show the pattern you want AI to follow                      │
│                                                                  │
│  ✅ Include edge cases in examples                              │
│                                                                  │
│  ✅ Use clear separators between examples                       │
│                                                                  │
│  ✅ Explicitly ask AI to follow the same format                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔗 Next Lesson

Continue to [Lesson 3: Role-Based Prompting](./03-role-based-prompting.md) to learn how to create expert AI personas!

---

**You're becoming a prompt engineering expert! 🚀**
