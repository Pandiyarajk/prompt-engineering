# Module 2: Fundamentals of Prompt Engineering

## Lesson 3: Best Practices and Optimization

---

## 🎯 Lesson Overview

**Time Required:** 60-75 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** Lessons 1 and 2 completed

In this lesson, you'll learn:
- The 10 Commandments of Prompt Engineering
- Advanced optimization techniques
- Common pitfalls and how to avoid them
- How to build an effective prompt library
- Quality metrics for measuring prompt effectiveness

---

## 📜 The 10 Commandments of Prompt Engineering

Master these principles for consistent, high-quality results:

```
┌─────────────────────────────────────────────────────────────────┐
│  THE 10 COMMANDMENTS                                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   1. Be Specific and Clear                                      │
│   2. Provide Sufficient Context                                 │
│   3. Use Examples (Few-Shot Learning)                          │
│   4. Define the Output Format                                   │
│   5. Iterate and Refine                                         │
│   6. Break Complex Tasks Down                                   │
│   7. Set Clear Constraints                                      │
│   8. Always Verify Output                                       │
│   9. Use System Messages Wisely                                 │
│  10. Maintain a Prompt Library                                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 1️⃣ Be Specific and Clear

### The Problem

```
❌ Vague: "Test the app"
   Result: Generic, unusable response
```

### The Solution

```
✅ Specific: "Create 5 functional test cases for a user registration 
   form with email, password, and confirm password fields. Include 
   positive, negative, and boundary value tests. Format as a table 
   with columns: ID, Description, Steps, Expected Result, Priority."
   
   Result: Specific, actionable test cases
```

### Specificity Checklist

- [ ] **What** exactly do you want?
- [ ] **How many** items/cases/examples?
- [ ] **What type** (positive, negative, edge cases)?
- [ ] **Which format** (table, code, list)?
- [ ] **What constraints** (language, framework, style)?

### Examples of Being Specific

| Vague | Specific |
|-------|----------|
| "Write tests" | "Write 5 pytest functions to test the calculate_tax function" |
| "Review code" | "Review this Python function for security vulnerabilities and performance issues" |
| "Create documentation" | "Create a README with installation, usage, and API sections" |
| "Find bugs" | "Identify logical errors, edge cases not handled, and potential exceptions in this function" |

---

## 2️⃣ Provide Sufficient Context

### The Problem

```
❌ No context: "Why isn't my test working?"
   AI thinks: "What test? What application? What framework?"
```

### The Solution

```
✅ With context:
   "I have a Selenium test in Python using pytest. It should click a 
   login button, but I'm getting a NoSuchElementException. 
   
   Here's my code:
   [code snippet]
   
   The button HTML is:
   <button id="login-btn" class="primary">Login</button>
   
   What's wrong and how do I fix it?"
```

### Context Checklist

```
For Test Cases:
- [ ] What feature/function?
- [ ] What are the requirements?
- [ ] What technical stack?
- [ ] Who are the users?
- [ ] What are the constraints?

For Code:
- [ ] What language/version?
- [ ] What framework?
- [ ] What's the purpose?
- [ ] What's already built?
- [ ] What are the requirements?

For Bug Analysis:
- [ ] What happened?
- [ ] What should have happened?
- [ ] What's the environment?
- [ ] What's the error message?
- [ ] What have you tried?
```

---

## 3️⃣ Use Examples (Few-Shot Learning)

### What is Few-Shot Learning?

Providing examples in your prompt helps the AI understand the exact pattern you want.

```
┌─────────────────────────────────────────────────────────────────┐
│  FEW-SHOT LEARNING TYPES                                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Zero-Shot: No examples (just instructions)                     │
│  One-Shot: One example                                          │
│  Few-Shot: 2-5 examples (optimal for most tasks)                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Examples of Few-Shot Learning

**For Test Case Format:**
```
Generate test cases in this exact format:

Example 1:
TC-001 | Valid Login | Enter valid email and password | User is logged in, redirected to dashboard | High

Example 2:
TC-002 | Invalid Email | Enter "invalid-email" without @ | Error message "Invalid email format" displayed | High

Now generate 5 test cases for a password reset feature in the same format.
```

**For Bug Reports:**
```
Write bug reports in this format:

Example:
**BUG-001: Login button unresponsive on mobile Safari**
- Severity: High
- Steps: 1. Open on iPhone, 2. Tap login
- Expected: Button responds
- Actual: No response
- Environment: iPhone 12, iOS 15, Safari

Now write a bug report for: "Search results don't update when changing filters"
```

**For Code Style:**
```
Write functions following this style:

Example:
def validate_email(email: str) -> tuple[bool, str]:
    """
    Validate email format.
    
    Args:
        email: Email string to validate
        
    Returns:
        Tuple of (is_valid, error_message)
    """
    if not email or '@' not in email:
        return False, "Invalid email format"
    return True, ""

Now write a function to validate phone numbers in the same style.
```

### Benefits of Few-Shot Learning

- **Consistent format** every time
- **Reduced iteration** needed
- **Higher quality** first-response
- **AI learns your style**

---

## 4️⃣ Define the Output Format

### Common Formats

**Tables (Best for Test Cases):**
```
Format as a markdown table:
| Column1 | Column2 | Column3 |
|---------|---------|---------|
| data    | data    | data    |
```

**JSON (Best for Structured Data):**
```
Format as JSON:
{
  "key": "value",
  "array": ["item1", "item2"]
}
```

**Numbered Lists (Best for Steps):**
```
Format as numbered steps:
1. First step
   - Sub-detail
2. Second step
```

**Code (Best for Programming):**
```
Format as Python code with:
- Imports at top
- Functions with docstrings
- Type hints
- Comments
```

### Format Specification Examples

```
✅ Good: "Format as a markdown table with columns: 
         ID, Description, Steps, Expected, Priority"

✅ Good: "Provide as Python code with:
         - pytest test functions
         - Docstrings for each test
         - Comments explaining assertions"

✅ Good: "Output as JSON with this structure:
         {
           'test_cases': [
             {'id': '', 'description': '', 'steps': []}
           ]
         }"
```

---

## 5️⃣ Iterate and Refine

### The Iteration Mindset

Expect 2-3 iterations for optimal results. Each iteration builds on the previous.

```
┌─────────────────────────────────────────────────────────────────┐
│  THE ITERATION CYCLE                                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────┐                                                │
│  │   Initial   │                                                │
│  │   Prompt    │                                                │
│  └──────┬──────┘                                                │
│         │                                                        │
│         ▼                                                        │
│  ┌─────────────┐                                                │
│  │  Evaluate   │◄──────────────────────────┐                    │
│  │  Response   │                           │                    │
│  └──────┬──────┘                           │                    │
│         │                                   │                    │
│    Good enough?                            │                    │
│     /        \                             │                    │
│   YES         NO                           │                    │
│    │          │                            │                    │
│    ▼          ▼                            │                    │
│  ┌───────┐  ┌─────────┐                   │                    │
│  │ Done! │  │ Refine  │───────────────────┘                    │
│  └───────┘  │ Prompt  │                                         │
│             └─────────┘                                         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Iteration Strategies

**Strategy 1: Progressive Detail**
```
Prompt 1: "Create test cases for login feature"
         → Get basic structure

Prompt 2: "Add security test cases to these"
         → Enhance with security

Prompt 3: "Now add edge cases for the email validation"
         → Complete coverage
```

**Strategy 2: Refinement Requests**
```
Prompt: [Initial request]
Response: [Initial output]

Follow-up: "This is good, but please also:
- Add test data for each case
- Include negative scenarios
- Format priorities as High/Medium/Low"
```

**Strategy 3: Ask for Improvements**
```
"Review what you just provided and:
1. Identify any gaps in coverage
2. Suggest improvements
3. Add missing edge cases"
```

### What to Do in Each Iteration

| Iteration | Focus |
|-----------|-------|
| 1st | Get basic structure right |
| 2nd | Add missing details/cases |
| 3rd | Refine format and quality |
| 4th+ | Polish and finalize |

---

## 6️⃣ Break Complex Tasks Down

### The Problem

```
❌ Overwhelming: "Create a complete test automation framework with 
   Selenium, pytest, page objects, data-driven tests, reporting, 
   CI/CD integration, parallel execution, and documentation."
   
   Result: Incomplete, disorganized output
```

### The Solution

```
✅ Step by step:

Step 1: "Design the folder structure for a pytest + Selenium framework"
Step 2: "Create a BasePage class with common methods"
Step 3: "Create a LoginPage class extending BasePage"
Step 4: "Create pytest fixtures for WebDriver setup"
Step 5: "Write sample login tests"
Step 6: "Add pytest.ini configuration"
Step 7: "Create requirements.txt"
Step 8: "Write README documentation"
```

### Task Breakdown Guidelines

```
┌─────────────────────────────────────────────────────────────────┐
│  HOW TO BREAK DOWN TASKS                                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. Identify the main goal                                      │
│     "Build a test automation framework"                         │
│                                                                  │
│  2. List all components needed                                  │
│     - Folder structure                                          │
│     - Base classes                                              │
│     - Page objects                                              │
│     - Test files                                                │
│     - Configuration                                             │
│     - Documentation                                             │
│                                                                  │
│  3. Determine logical order                                     │
│     Structure → Base → Pages → Tests → Config → Docs            │
│                                                                  │
│  4. Create one prompt per component                             │
│     Keep each prompt focused on ONE thing                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Benefits of Breaking Down

- ✅ Better quality for each component
- ✅ Easier to review and validate
- ✅ Can iterate on individual parts
- ✅ Less context overload for AI
- ✅ Clearer organization

---

## 7️⃣ Set Clear Constraints

### Types of Constraints

| Type | Examples |
|------|----------|
| **Quantity** | "Exactly 10 test cases", "Limit to 5 functions" |
| **Technical** | "Python 3.9+", "pytest only", "No external libraries" |
| **Scope** | "Only API tests", "Exclude UI", "Focus on security" |
| **Quality** | "Production-ready", "With error handling", "Documented" |
| **Style** | "PEP 8 compliant", "Beginner-friendly", "Concise" |
| **Time** | "Tests should run in under 30 seconds each" |

### Constraint Examples

**For Test Cases:**
```
Constraints:
- Limit to 15 most critical test cases
- Focus on functional testing only (no performance)
- Each test must be independent (no dependencies)
- Include specific test data values
- Prioritize security-related scenarios
```

**For Code:**
```
Constraints:
- Python 3.9+ compatible
- Use only standard library (no external deps)
- Follow PEP 8 style guide
- Include type hints
- Maximum 50 lines per function
- Include unit tests
```

**For Documentation:**
```
Constraints:
- Maximum 500 words
- Use simple language (for junior team members)
- Include at least 3 code examples
- Avoid jargon without explanation
- Must be scannable (use headings/bullets)
```

---

## 8️⃣ Always Verify Output

### Why Verification Matters

AI can:
- ❌ Generate plausible-sounding incorrect information
- ❌ Write code with subtle bugs
- ❌ Miss important edge cases
- ❌ Use outdated practices
- ❌ Hallucinate non-existent features

### Verification Checklist

**For Test Cases:**
```
- [ ] All requirements covered?
- [ ] Steps are clear and reproducible?
- [ ] Expected results defined?
- [ ] Edge cases included?
- [ ] Priorities make sense?
- [ ] Test data provided?
- [ ] No duplicate scenarios?
```

**For Code:**
```
- [ ] Code runs without errors?
- [ ] Logic is correct?
- [ ] All imports work?
- [ ] Error handling present?
- [ ] Edge cases handled?
- [ ] Follows best practices?
- [ ] No security issues?
- [ ] Tests pass?
```

**For Documentation:**
```
- [ ] Information is accurate?
- [ ] Examples work?
- [ ] Instructions are clear?
- [ ] No missing steps?
- [ ] Links work?
```

### Verification Prompt

```
"Review what you just provided and:
1. Check for any errors or inaccuracies
2. Identify gaps or missing scenarios
3. Verify the logic/code is correct
4. Suggest any improvements"
```

---

## 9️⃣ Use System Messages Wisely

### What Are System Messages?

System messages set the AI's behavior and personality at the start of a conversation. They're like giving detailed instructions to a new employee.

### Effective System Messages

**For QA Work:**
```python
system_message = """
You are a Senior QA Engineer with 15 years of experience in:
- Web application testing
- Test automation (Selenium, Playwright)
- API testing
- Security testing

You always:
- Consider edge cases
- Think about security implications
- Provide comprehensive, production-ready outputs
- Follow industry best practices
- Include specific test data examples
"""
```

**For Development:**
```python
system_message = """
You are a Senior Python Developer who specializes in:
- Clean, maintainable code
- Test-driven development
- Design patterns
- Code optimization

You always:
- Write well-documented code
- Include type hints
- Follow PEP 8
- Consider error handling
- Write corresponding tests
"""
```

**For Code Review:**
```python
system_message = """
You are a Code Review Expert who:
- Provides constructive, specific feedback
- Identifies bugs, security issues, and performance problems
- Suggests improvements with code examples
- Explains WHY something is an issue
- Prioritizes feedback by severity
"""
```

### Using System Messages with OpenAI API

```python
response = client.chat.completions.create(
    model="gpt-5.2",
    messages=[
        {
            "role": "system",
            "content": system_message  # Your system message here
        },
        {
            "role": "user",
            "content": "Your actual prompt..."
        }
    ]
)
```

---

## 🔟 Maintain a Prompt Library

### Why Keep a Prompt Library?

- ✅ **Save time** - Don't recreate prompts
- ✅ **Consistency** - Same quality every time
- ✅ **Share** - Help team members
- ✅ **Improve** - Iterate on what works

### Library Structure

```
prompts/
├── testing/
│   ├── test-case-generation.md
│   ├── test-plan.md
│   ├── bug-report.md
│   └── security-testing.md
├── automation/
│   ├── selenium-tests.md
│   ├── api-tests.md
│   └── page-objects.md
├── development/
│   ├── code-generation.md
│   ├── code-review.md
│   └── refactoring.md
└── documentation/
    ├── readme.md
    ├── api-docs.md
    └── test-plan.md
```

### Template Format

```markdown
# [Prompt Name]

## Purpose
[What this prompt is for]

## When to Use
[Situations where this prompt is helpful]

## The Prompt
```
[Your prompt template with [PLACEHOLDERS] for customization]
```

## Example Usage
```
[Filled-in example]
```

## Expected Output
[What kind of output to expect]

## Variations
[Alternative versions for different needs]

## Tips
[Best practices for using this prompt]

## Last Updated
[Date]
```

---

## 🚫 Common Pitfalls to Avoid

### Pitfall 1: Being Too Vague

```
❌ "Help with testing"
✅ "Create 5 functional test cases for user login with email and password"
```

### Pitfall 2: Overloading the Prompt

```
❌ "Create test cases, automation code, test data, documentation, 
    and also review my existing code, and suggest improvements, 
    and create a CI/CD pipeline"
    
✅ Focus on ONE task at a time
```

### Pitfall 3: Not Providing Examples

```
❌ "Format it nicely"
✅ "Format like this example: [provide example]"
```

### Pitfall 4: Assuming AI Knows Your Context

```
❌ "Fix the bug in my code" (without providing code)
✅ "Here's my code [code]. The error is [error]. Please fix it."
```

### Pitfall 5: Not Iterating

```
❌ Accepting first response without review
✅ Review, refine, repeat until quality is good
```

### Pitfall 6: Trusting Without Verification

```
❌ Copy-pasting AI output directly
✅ Review, test, verify before using
```

### Pitfall 7: Ignoring Limitations

```
❌ Expecting real-time information
❌ Expecting access to your private codebase
❌ Expecting 100% accuracy
✅ Understand what AI can and cannot do
```

---

## 📊 Prompt Quality Metrics

### Score Your Prompts (1-5 each)

| Criterion | Score |
|-----------|-------|
| **Clarity:** Easy to understand? | ⭐ |
| **Specificity:** Detailed enough? | ⭐ |
| **Context:** Background provided? | ⭐ |
| **Format:** Structure defined? | ⭐ |
| **Constraints:** Boundaries set? | ⭐ |
| **Examples:** Samples included? | ⭐ |
| **Focus:** One main task? | ⭐ |

### Scoring Guide

| Total Score | Rating | Action |
|-------------|--------|--------|
| 28-35 | Excellent | Use as template |
| 21-27 | Good | Minor improvements |
| 14-20 | Needs work | Revise using guidelines |
| 7-13 | Poor | Rewrite completely |

---

## 💻 Practice Exercises

### Exercise 2.4: Optimize These Prompts

**Poor Prompt 1:**
```
"Write code to test an API"
```

**Your optimized version:**
```
[Apply RCTFC framework]
[Add specifics]
[Include format and constraints]
```

**Poor Prompt 2:**
```
"Review this code and make it better"
```

**Your optimized version:**
```
[Apply all 10 best practices]
```

**Poor Prompt 3:**
```
"Create test cases for shopping cart"
```

**Your optimized version:**
```
[Make it comprehensive and specific]
```

### Exercise 2.5: Build Your Prompt Library

**Task:** Create 3 prompt templates for your most common tasks:

1. **Template 1: [Your Task Name]**
```
[Write your complete template]
```

2. **Template 2: [Your Task Name]**
```
[Write your complete template]
```

3. **Template 3: [Your Task Name]**
```
[Write your complete template]
```

---

## 📝 Key Takeaways

1. **Be specific** - Vague prompts = vague results
2. **Provide context** - AI needs background information
3. **Use examples** - Show the AI what you want
4. **Define format** - Specify exactly how output should look
5. **Iterate** - Expect 2-3 refinements
6. **Break down** - One focused task per prompt
7. **Set constraints** - Define boundaries and requirements
8. **Verify** - Always check AI output
9. **Use system messages** - Set AI behavior upfront
10. **Build library** - Save and reuse effective prompts

---

## ✅ Module 2 Complete! 🎉

Congratulations! You've mastered the fundamentals of prompt engineering!

**You can now:**
- ✅ Use the RCTFC framework
- ✅ Control AI behavior with parameters
- ✅ Apply the 10 best practices
- ✅ Avoid common pitfalls
- ✅ Build and maintain a prompt library

---

## 🔗 Next Steps

Choose your path based on your role:

| Your Role | Next Module |
|-----------|-------------|
| **Manual QA** | [Module 3: Prompt Engineering for Manual QA](../module-3-manual-qa/README.md) |
| **Automation Engineer** | [Module 4: Prompt Engineering for Automation Testing](../module-4-automation-testing/README.md) |
| **Developer** | [Module 5: Prompt Engineering for Developers](../module-5-developers/README.md) |
| **Want to learn all** | Start with Module 3, then 4, then 5 |

---

## 📖 Module 2 Quick Reference Card

```
┌─────────────────────────────────────────────────────────────────┐
│  MODULE 2 QUICK REFERENCE                                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  RCTFC FRAMEWORK:                                                │
│  R - Role     ("Act as a Senior QA Engineer...")                │
│  C - Context  ("I'm testing a login feature...")                │
│  T - Task     ("Generate 10 test cases...")                     │
│  F - Format   ("As a markdown table...")                        │
│  C - Constraints ("Python 3.9+, pytest...")                     │
│                                                                  │
│  TEMPERATURE:                                                    │
│  0.1-0.3: Code, test cases, documentation                       │
│  0.7-1.0: Brainstorming, creative ideas                         │
│                                                                  │
│  10 COMMANDMENTS:                                                │
│  1. Be specific    6. Break down tasks                          │
│  2. Give context   7. Set constraints                           │
│  3. Use examples   8. Verify output                             │
│  4. Define format  9. Use system messages                       │
│  5. Iterate       10. Keep prompt library                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

**Excellent work completing Module 2! You're ready for specialized content! 🚀**
