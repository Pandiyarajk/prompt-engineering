# Module 2: Fundamentals of Prompt Engineering

## Lesson 1: Basic Prompt Structure

### Anatomy of an Effective Prompt

A well-structured prompt typically contains:

```
[ROLE] + [CONTEXT] + [TASK] + [FORMAT] + [CONSTRAINTS]
```

### The 5 Components of Great Prompts

#### 1. **ROLE** - Who should the AI be?

**Purpose:** Establishes the perspective and expertise level

**Examples:**
```
❌ "Write test cases"
✅ "Act as a Senior QA Engineer with 10 years of experience"
✅ "You are an automation testing expert specializing in Selenium"
✅ "As a manual tester focused on mobile applications"
```

#### 2. **CONTEXT** - What's the background?

**Purpose:** Provides necessary information for accurate responses

**Examples:**
```
❌ "Test the login"
✅ "We have a web application login page with username, password, 
    remember me checkbox, and OAuth options (Google, Facebook)"
✅ "Testing a REST API endpoint POST /api/auth/login that accepts 
    JSON with email and password fields"
```

#### 3. **TASK** - What do you want?

**Purpose:** Clearly states the desired outcome

**Examples:**
```
❌ "Help with testing"
✅ "Create a comprehensive test plan including functional, security, 
    and performance test cases"
✅ "Generate Selenium WebDriver code in Python to automate the login flow"
✅ "Review this test script and suggest improvements for maintainability"
```

#### 4. **FORMAT** - How should the output look?

**Purpose:** Specifies the desired structure of the response

**Examples:**
```
❌ "List the test cases"
✅ "Format as a markdown table with columns: ID, Test Case, Steps, 
    Expected Result, Priority"
✅ "Provide output as Python code with comments and docstrings"
✅ "Structure as: 1) Summary, 2) Detailed steps, 3) Code example"
```

#### 5. **CONSTRAINTS** - What are the limitations?

**Purpose:** Sets boundaries and requirements

**Examples:**
```
✅ "Use Python 3.9+ syntax"
✅ "Limit to 10 most critical test cases"
✅ "Follow PEP 8 coding standards"
✅ "Focus only on API testing, not UI"
✅ "Response should be under 500 words"
```

### The Prompt Formula

**Basic Formula:**
```
Act as a [ROLE].

Context: [BACKGROUND INFORMATION]

Task: [WHAT YOU WANT]

Format: [HOW TO PRESENT IT]

Constraints: [LIMITATIONS/REQUIREMENTS]
```

### Real-World Examples

#### Example 1: Test Case Generation

**Poor Prompt:**
```
Create test cases for shopping cart
```

**Good Prompt:**
```
Act as a Senior QA Engineer specializing in e-commerce applications.

Context: I'm testing a shopping cart feature that allows users to:
- Add items to cart
- Update quantities
- Remove items
- Apply discount codes
- Proceed to checkout

Task: Generate comprehensive test cases covering positive, negative, 
and edge cases.

Format: Create a table with columns:
- Test ID
- Test Scenario
- Test Steps
- Test Data
- Expected Result
- Priority (High/Medium/Low)

Constraints:
- Focus on functional testing only
- Include at least 15 test cases
- Cover all user workflows
- Consider multi-currency scenarios
```

#### Example 2: Automation Code Generation

**Poor Prompt:**
```
Write selenium code for login
```

**Good Prompt:**
```
Act as a Test Automation Engineer expert in Selenium with Python.

Context: I need to automate a login test for a web application at 
https://example.com/login. The page has:
- Email input field (id="email")
- Password input field (id="password")
- Login button (id="login-btn")
- Error message div (class="error-msg")

Task: Create a complete Selenium WebDriver script that:
1. Opens the login page
2. Enters valid credentials
3. Clicks login button
4. Verifies successful login (checks for dashboard element)
5. Handles potential errors with try-except

Format: 
- Complete Python script with imports
- Use Page Object Model pattern
- Include detailed comments
- Add logging statements

Constraints:
- Python 3.9+
- Selenium 4.x
- Use pytest framework
- Follow PEP 8 standards
- Include WebDriverWait for synchronization
```

#### Example 3: Code Review

**Poor Prompt:**
```
Review this code
```

**Good Prompt:**
```
Act as a Senior Software Engineer and Code Review Expert.

Context: This is a test automation script written by a junior QA engineer.
The code works but needs improvement for maintainability and best practices.

Task: Review the following code and provide detailed feedback on:
1. Code quality and readability
2. Error handling
3. Best practices violations
4. Performance issues
5. Security concerns
6. Maintainability improvements

Format:
- List issues by category (Critical, Major, Minor)
- For each issue: explain the problem and provide corrected code
- Suggest at least 3 specific improvements
- Provide a refactored version of the most problematic section

Constraints:
- Focus on actionable feedback
- Explain the "why" behind each suggestion
- Keep explanations concise but clear
- Prioritize critical issues

[CODE HERE]
```

### Prompt Patterns for Testing

#### Pattern 1: Test Design Pattern
```
As a [TESTER TYPE], create [TEST ARTIFACT] for [FEATURE] that [REQUIREMENTS].
Include [CATEGORIES] formatted as [FORMAT].
```

#### Pattern 2: Automation Pattern
```
Generate [LANGUAGE] code using [FRAMEWORK] to test [SCENARIO].
The code should [ACTIONS] and handle [EDGE CASES].
Follow [STANDARDS] and include [DOCUMENTATION].
```

#### Pattern 3: Analysis Pattern
```
Analyze [ARTIFACT] for [ISSUES].
Identify [PROBLEMS] and suggest [SOLUTIONS].
Prioritize by [CRITERIA] and format as [FORMAT].
```

### Common Mistakes to Avoid

❌ **Too Vague:**
```
"Test the application"
```

❌ **Too Many Tasks:**
```
"Create test cases, write automation code, generate test data, 
create documentation, and review the entire codebase"
```

❌ **No Context:**
```
"Why isn't this working?" [without providing code or error]
```

❌ **Unclear Format:**
```
"Give me the test cases" [doesn't specify how]
```

✅ **Just Right:**
```
"Act as a QA Engineer. Create 5 boundary value test cases for an 
age input field (valid range: 18-65) formatted as a table."
```

### Progressive Prompting Technique

**Step 1: Start Broad**
```
"I need to test a user registration feature. What should I consider?"
```

**Step 2: Get Specific**
```
"Create test cases for email validation in the registration form"
```

**Step 3: Refine Output**
```
"Add negative test cases for invalid email formats (missing @, 
multiple @, special characters)"
```

**Step 4: Iterate**
```
"Now generate Selenium code for these email validation tests"
```

### Exercise 2.1: Prompt Construction

**Task:** Construct effective prompts for these scenarios:

1. **Scenario 1:** Test a password reset feature
   - Write a poor prompt
   - Write a good prompt using all 5 components

2. **Scenario 2:** Generate API test automation
   - Write a poor prompt
   - Write a good prompt using all 5 components

3. **Scenario 3:** Review a test strategy document
   - Write a poor prompt
   - Write a good prompt using all 5 components

**Answers Template:**
```markdown
## Scenario 1

### Poor Prompt:
[Your answer]

### Good Prompt:
Role: [Your answer]
Context: [Your answer]
Task: [Your answer]
Format: [Your answer]
Constraints: [Your answer]

[Repeat for scenarios 2 and 3]
```

### Key Takeaways

✅ Every great prompt has 5 components: Role, Context, Task, Format, Constraints
✅ More specific prompts yield better results
✅ Context is crucial for accurate responses
✅ Format specification ensures usable output
✅ One complex task is better than multiple simple tasks
✅ Iterate and refine based on responses

### Prompt Checklist

Before submitting a prompt, ask:
- [ ] Is the role clearly defined?
- [ ] Have I provided enough context?
- [ ] Is the task specific and actionable?
- [ ] Did I specify the output format?
- [ ] Are constraints and requirements clear?
- [ ] Is this one focused task (not multiple)?

### Next Steps
- Move to [Lesson 2: Parameters and Temperature Settings](02-parameters-and-settings.md)
