# Practice Prompts

## Overview

This file contains practice exercises for each module. Complete these to master prompt engineering.

---

## Module 1: Introduction

### Exercise 1.1: First Prompts
Compare the quality of these prompts:

**Task:** Get help testing a login feature

**Prompt 1 (Beginner):**
```
Help me test login
```

**Prompt 2 (Intermediate):**
```
Create test cases for a login feature with username and password
```

**Prompt 3 (Advanced):**
```
Act as a Senior QA Engineer. Create comprehensive test cases for a web application login feature with the following requirements:
- Username (email format) and password fields
- "Remember me" checkbox
- "Forgot password" link
- Max 5 login attempts before lockout

Include positive, negative, boundary, and security test cases.
Format as a table with: Test ID, Description, Steps, Expected Result, Priority
```

**Your Task:**
- Try all three prompts
- Compare the results
- Document what you learned

---

## Module 2: Fundamentals

### Exercise 2.1: Prompt Structure

**Task:** Write prompts using the 5-component framework

**Scenario:** You need test cases for a shopping cart

**Template:**
```
Role: [Define the AI's role]
Context: [Provide background]
Task: [Specify what you want]
Format: [Define output structure]
Constraints: [Set boundaries]
```

**Your Prompt:**
```
[Write your complete prompt here]
```

**Expected Improvements:**
- More comprehensive test cases
- Better organized output
- Covers more scenarios

---

### Exercise 2.2: Temperature Experiments

**Task:** Test the same prompt with different temperatures

**Prompt:**
```
Generate 5 creative edge cases for a file upload feature
```

**Test Matrix:**

| Temperature | Creativity | Consistency | Best For |
|-------------|------------|-------------|----------|
| 0.2 | Low | High | [Your observation] |
| 0.7 | Medium | Medium | [Your observation] |
| 1.2 | High | Low | [Your observation] |

**Your Task:**
- Run the prompt with each temperature
- Document differences
- Identify optimal temperature for different tasks

---

### Exercise 2.3: Output Format Specification

**Task:** Get the same content in different formats

**Base Prompt:**
```
Create test cases for password validation
```

**Format 1: Table**
```
[Modify prompt to get markdown table]
```

**Format 2: JSON**
```
[Modify prompt to get JSON]
```

**Format 3: Gherkin**
```
[Modify prompt to get Gherkin/BDD format]
```

**Format 4: Checklist**
```
[Modify prompt to get simple checklist]
```

---

## Module 3: Manual QA

### Exercise 3.1: Test Case Generation

**Scenario 1: E-commerce Checkout**

**Requirements:**
```
User can complete purchase:
- Review cart items
- Enter shipping address
- Select shipping method
- Enter payment information
- Review order
- Confirm purchase
- Receive confirmation email
```

**Your Task:**
Write a prompt to generate:
- [ ] Happy path test cases
- [ ] Error scenarios
- [ ] Edge cases
- [ ] Security test cases

**Your Prompt:**
```
[Write your prompt here]
```

---

**Scenario 2: Mobile App Notification Settings**

**Requirements:**
```
Settings screen allows:
- Enable/disable push notifications
- Set quiet hours (start/end time)
- Choose notification sound
- Enable vibration
- Select notification categories
```

**Your Prompt:**
```
[Write your prompt here]
```

---

### Exercise 3.2: Bug Report Enhancement

**Poor Bug Report:**
```
Title: Search doesn't work
Description: I searched for something and got wrong results
```

**Your Task:**
Write a prompt to enhance this bug report with:
- Clear title
- Detailed steps to reproduce
- Expected vs actual results
- Environment information
- Severity/priority classification

**Your Prompt:**
```
[Write your prompt here]
```

---

### Exercise 3.3: Test Scenario Creation

**Feature:** Real-time Chat Application

**Your Task:**
Generate test scenarios for:
1. One-on-one chat
2. Group chat
3. File sharing
4. Message reactions
5. Online/offline status

**Your Prompt:**
```
[Write your prompt here]
```

---

## Module 4: Automation Testing

### Exercise 4.1: Selenium Code Generation

**Scenario:** Login Test Automation

**Requirements:**
- Page: https://example.com/login
- Elements: email (id="email"), password (id="pwd"), button (id="login-btn")
- Validate: success → dashboard appears, failure → error message

**Your Task:**
Write a prompt to generate:
- [ ] Page Object Model class
- [ ] Test cases using pytest
- [ ] Fixtures for setup/teardown
- [ ] Proper waits and error handling

**Your Prompt:**
```
[Write your prompt here]
```

---

### Exercise 4.2: API Test Generation

**API Endpoint:** GET /api/users/{id}

**Specifications:**
```
Success Response (200):
{
  "id": "string",
  "name": "string",
  "email": "string",
  "role": "string"
}

Error Responses:
- 404: User not found
- 401: Unauthorized
- 400: Invalid ID format
```

**Your Task:**
Generate API tests using pytest and requests library

**Your Prompt:**
```
[Write your prompt here]
```

---

### Exercise 4.3: Test Data Generation

**Scenario:** User Registration Testing

**Your Task:**
Generate Python code to create test data for:
- Valid users (various countries, ages, names)
- Invalid emails (wrong format, special characters)
- Invalid passwords (too short, no uppercase, etc.)
- Boundary test data

**Your Prompt:**
```
[Write your prompt here]
```

---

## Module 5: Developers

### Exercise 5.1: Function Implementation

**Task:** Implement a rate limiter

**Requirements:**
```
Function: is_allowed(user_id, action)
- Allow max 100 requests per hour per user
- Track request timestamps
- Clean up old timestamps
- Return True/False
- Thread-safe
```

**Your Prompt:**
```
[Write your prompt here]
```

---

### Exercise 5.2: Code Refactoring

**Code to Refactor:**
```python
def calculate_price(items, user_type, coupon):
    total = 0
    for item in items:
        total = total + item['price'] * item['qty']
    
    if user_type == 'premium':
        total = total * 0.9
    elif user_type == 'vip':
        total = total * 0.8
    
    if coupon == 'SAVE10':
        total = total * 0.9
    elif coupon == 'SAVE20':
        total = total * 0.8
    elif coupon == 'SAVE30':
        total = total * 0.7
    
    if total > 100:
        shipping = 0
    else:
        shipping = 10
    
    total = total + shipping
    tax = total * 0.1
    total = total + tax
    
    return total
```

**Your Task:**
Write a prompt to refactor this for:
- Better readability
- Strategy pattern for discounts
- Separation of concerns
- Type hints
- Documentation

**Your Prompt:**
```
[Write your prompt here]
```

---

### Exercise 5.3: API Endpoint Creation

**Task:** Create a RESTful API for blog posts

**Requirements:**
```
Endpoints:
- POST /posts - Create post
- GET /posts - List posts (paginated)
- GET /posts/{id} - Get single post
- PUT /posts/{id} - Update post
- DELETE /posts/{id} - Delete post

Features:
- Authentication required
- Input validation
- Error handling
- OpenAPI documentation
```

**Your Task:**
Generate FastAPI code

**Your Prompt:**
```
[Write your prompt here]
```

---

## Module 7: AI Agents

### Exercise 7.1: Simple Agent

**Task:** Create a test case validation agent

**Agent Goal:**
```
Review test cases and:
1. Check completeness (steps, expected results)
2. Identify ambiguous language
3. Suggest improvements
4. Verify requirement coverage
```

**Your Task:**
Write code for this agent using OpenAI API

**Starter Code:**
```python
class TestCaseValidatorAgent:
    def __init__(self, api_key):
        # Your implementation
        pass
    
    def validate(self, test_cases):
        # Your implementation
        pass
```

---

### Exercise 7.2: Multi-Step Agent

**Task:** Create an agent that:
1. Reads requirements
2. Generates test cases
3. Creates automation code
4. Generates test data
5. Creates documentation

**Your Implementation:**
```python
[Write your agent code here]
```

---

## Challenge Exercises

### Challenge 1: Comprehensive Test Strategy

**Scenario:** New Mobile Banking App

**Features:**
- Account overview
- Transfer money
- Bill payments
- Transaction history
- Fingerprint/Face ID login
- Push notifications

**Your Task:**
Create a complete test strategy including:
- Test approach for each feature
- Test case examples
- Automation recommendations
- Performance test scenarios
- Security test cases
- Mobile-specific considerations

**Your Prompt:**
```
[Write your prompt here]
```

---

### Challenge 2: Complete Test Framework

**Task:** Generate a complete test automation framework

**Requirements:**
- Page Object Model
- Data-driven tests
- Parallel execution
- HTML reporting
- CI/CD integration
- Docker support

**Your Prompt:**
```
[Write your prompt here]
```

---

### Challenge 3: AI-Powered Test Analyzer

**Task:** Build a tool that analyzes test results and:
- Identifies flaky tests
- Suggests root causes for failures
- Recommends fixes
- Generates report

**Your Implementation Plan:**
```
[Write your plan and prompts here]
```

---

## Prompt Templates Library

### Template 1: Test Case Generation
```
Act as a {role} with expertise in {domain}.

Feature: {feature_name}

Requirements:
{requirements_list}

Generate test cases including:
- {test_type_1}
- {test_type_2}
- {test_type_3}

Format: {output_format}

Constraints:
- {constraint_1}
- {constraint_2}
```

### Template 2: Code Generation
```
Act as a {role} developer expert in {technology}.

Create {artifact_type} for: {description}

Technical Specs:
- Language: {language}
- Framework: {framework}
- Patterns: {patterns}

Requirements:
- {req_1}
- {req_2}

Include:
- Complete working code
- Error handling
- Documentation
- Tests
- Example usage
```

### Template 3: Code Review
```
Act as a Senior Developer and Code Reviewer.

Review this {language} code for:
- Code quality
- Best practices
- Security issues
- Performance
- Maintainability

Code:
{code_here}

Provide:
1. Issues found (with severity)
2. Specific recommendations
3. Refactored code examples
```

---

## Answer Key

[Note: This would contain sample answers for instructors]

---

## Progress Tracker

Track your progress:

- [ ] Module 1 Exercises Complete
- [ ] Module 2 Exercises Complete
- [ ] Module 3 Exercises Complete
- [ ] Module 4 Exercises Complete
- [ ] Module 5 Exercises Complete
- [ ] Module 7 Exercises Complete
- [ ] Challenge 1 Complete
- [ ] Challenge 2 Complete
- [ ] Challenge 3 Complete

---

## Next Steps

1. Complete all exercises in order
2. Compare your prompts with peers
3. Build your personal prompt library
4. Move on to course projects
5. Apply learnings to real work

Happy learning! 🚀
