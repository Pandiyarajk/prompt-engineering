# Module 2: Fundamentals of Prompt Engineering

## Lesson 3: Best Practices and Prompt Optimization

### The 10 Commandments of Prompt Engineering

1. **Be Specific and Clear**
2. **Provide Context**
3. **Use Examples (Few-Shot Learning)**
4. **Define the Output Format**
5. **Iterate and Refine**
6. **Break Complex Tasks Down**
7. **Set Constraints and Boundaries**
8. **Verify and Validate Output**
9. **Use System Messages Effectively**
10. **Keep a Prompt Library**

### Best Practice #1: Be Specific and Clear

**Bad:**
```
Test the app
```

**Good:**
```
Create functional test cases for the user registration form with fields:
email, password, confirm password, and terms checkbox. Include positive,
negative, and boundary value test cases.
```

**Impact:** 10x better results with specific prompts

### Best Practice #2: Provide Context

**Without Context:**
```
Write a test script
```

**With Context:**
```
I'm testing a Flask API endpoint (POST /api/users) that creates new users.
The endpoint accepts JSON with email and password fields, returns 201 on
success with user ID, or 400 for validation errors. Write a pytest script
to test this endpoint.
```

**Context Checklist:**
- [ ] What are you testing?
- [ ] What technology/framework?
- [ ] What are the requirements?
- [ ] What's the expected behavior?
- [ ] Any specific constraints?

### Best Practice #3: Use Examples (Few-Shot Learning)

**Zero-Shot (No Examples):**
```
Generate test cases in the standard format
```

**Few-Shot (With Examples):**
```
Generate test cases in this format:

Example 1:
TC-001 | Valid login | Enter valid username and password | User logged in successfully | High

Example 2:
TC-002 | Invalid password | Enter valid username, wrong password | Error message displayed | High

Now generate 5 more test cases for the password reset feature.
```

**One-Shot Example:**
```
Create a bug report like this example:

**Example:**
Title: Login button unresponsive on mobile Safari
Severity: High
Steps:
1. Open app on iPhone 12, iOS 15, Safari
2. Navigate to login page
3. Enter credentials
4. Tap login button
Expected: User logs in
Actual: Button doesn't respond
Environment: iPhone 12, iOS 15.0, Safari 15.0

Now create a bug report for the issue I'm facing with the search feature...
```

### Best Practice #4: Define Output Format

**Formats You Can Request:**

#### Table Format
```
Format as a table:
| Test ID | Description | Steps | Expected | Priority |
|---------|-------------|-------|----------|----------|
```

#### JSON Format
```
Provide output as JSON:
{
  "test_cases": [
    {
      "id": "TC-001",
      "description": "...",
      "steps": [...],
      "expected": "...",
      "priority": "high"
    }
  ]
}
```

#### Code Format
```
Provide as executable Python code with:
- imports at the top
- proper function definitions
- docstrings
- comments for complex logic
- example usage at the bottom
```

#### Markdown Format
```
Format as markdown with:
# Main heading
## Subheadings
- Bullet points
**Bold for important items**
```code blocks```
```

### Best Practice #5: Iterate and Refine

**First Iteration:**
```
Prompt: "Create test cases for login"
Result: Basic test cases
```

**Second Iteration:**
```
Prompt: "Add negative test cases for SQL injection, XSS, and brute force"
Result: Security test cases added
```

**Third Iteration:**
```
Prompt: "Now add performance test cases for concurrent logins"
Result: Complete test suite
```

**Refinement Strategy:**
1. Start broad → Get general output
2. Identify gaps → Ask for specific additions
3. Improve quality → Request enhancements
4. Verify completeness → Final review

### Best Practice #6: Break Complex Tasks Down

**Complex (Overwhelming):**
```
Create a complete test automation framework with Selenium, pytest,
page objects, data-driven tests, reporting, CI/CD integration,
parallel execution, and documentation.
```

**Broken Down (Manageable):**
```
Step 1: "Design the folder structure for a pytest+Selenium framework"
Step 2: "Create a base page class with common methods"
Step 3: "Create a page object for the login page"
Step 4: "Create pytest fixtures for WebDriver setup"
Step 5: "Create a sample test using the page object"
[Continue...]
```

### Best Practice #7: Set Constraints and Boundaries

**Useful Constraints:**

```
Technology Constraints:
- "Use Python 3.9+"
- "Use pytest, not unittest"
- "Follow PEP 8 standards"

Scope Constraints:
- "Focus only on API testing, not UI"
- "Limit to 10 critical test cases"
- "Cover only happy path scenarios"

Quality Constraints:
- "Include error handling"
- "Add docstrings to all functions"
- "Use type hints"
- "Follow SOLID principles"

Output Constraints:
- "Keep response under 500 words"
- "Provide only code, no explanation"
- "Explain in simple terms for beginners"
```

### Best Practice #8: Verify and Validate Output

**Validation Checklist:**

For Test Cases:
- [ ] Are all scenarios covered?
- [ ] Are steps clear and reproducible?
- [ ] Are expected results defined?
- [ ] Are edge cases included?
- [ ] Is priority assigned?

For Code:
- [ ] Does the code run without errors?
- [ ] Are all imports available?
- [ ] Is error handling present?
- [ ] Are there any security issues?
- [ ] Is the code maintainable?

For Bug Reports:
- [ ] Is the issue clearly described?
- [ ] Are steps reproducible?
- [ ] Is environment specified?
- [ ] Is severity appropriate?

**Verification Prompt:**
```
Review the test cases you just generated and:
1. Identify any gaps in coverage
2. Check for duplicate scenarios
3. Verify priority assignments
4. Suggest improvements
```

### Best Practice #9: Use System Messages Effectively

**System Message = Sets the AI's behavior/personality**

```python
# Effective system messages for testing
system_messages = {
    "qa_engineer": "You are a meticulous Senior QA Engineer with 10 years of experience in test automation, specializing in Python and Selenium. You write comprehensive, maintainable test code following best practices.",
    
    "security_tester": "You are a security testing expert who thinks like an attacker. You identify vulnerabilities, edge cases, and security risks that others might miss.",
    
    "code_reviewer": "You are an experienced code reviewer focused on test automation. You provide constructive, specific feedback on code quality, maintainability, and best practices.",
    
    "api_tester": "You are an API testing specialist experienced with REST APIs, GraphQL, and microservices. You create thorough API test strategies and automation.",
}

# Usage
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": system_messages["qa_engineer"]},
        {"role": "user", "content": "Create a Selenium test for login"}
    ]
)
```

### Best Practice #10: Keep a Prompt Library

**Create reusable prompt templates:**

```markdown
# My Prompt Library

## Test Case Generation Template
```
Act as a {role} with expertise in {domain}.

Create test cases for {feature} with these requirements:
{requirements}

Include:
- {test_type_1}
- {test_type_2}
- {test_type_3}

Format: {format_specification}

Constraints:
- {constraint_1}
- {constraint_2}
```

## Code Generation Template
```
Act as a {role} expert in {technology}.

Generate {code_type} for: {description}

Requirements:
- {req_1}
- {req_2}

Technical specs:
- Language: {language}
- Framework: {framework}
- Standards: {standards}

Include:
- Complete working code
- Error handling
- Documentation
- Example usage
```
```

### Advanced Optimization Techniques

#### Technique 1: Chain of Thought

**Instead of:**
```
Find bugs in this code
```

**Use:**
```
Analyze this code step-by-step:
1. First, identify the purpose of each function
2. Then, check for logical errors
3. Next, look for edge cases not handled
4. Finally, suggest specific improvements

[Code here]
```

#### Technique 2: Role Playing with Expertise Levels

```
Act as a Junior QA → simpler explanations
Act as a Senior QA → advanced techniques
Act as a QA Manager → strategic planning
Act as a Security Expert → security focus
```

#### Technique 3: Negative Prompting

**Tell it what NOT to do:**
```
Create test cases for the payment gateway.

DO NOT:
- Include test cases for the UI (focus on API only)
- Suggest using outdated testing tools
- Provide generic examples without context
- Ignore security testing

DO:
- Focus on integration testing
- Include security test scenarios
- Provide specific test data examples
- Consider error handling paths
```

#### Technique 4: Comparative Analysis

```
Compare these two approaches to API testing and recommend which is better
for our use case:

Approach 1: [Description]
Approach 2: [Description]

Our context: [Requirements, constraints, team skills]

Provide:
- Pros and cons of each
- Recommendation with reasoning
- Implementation steps for recommended approach
```

### Common Pitfalls and Solutions

**Pitfall 1: Prompt Too Long**
```
Problem: Prompt is 1000+ words
Solution: Break into multiple prompts or summarize context
```

**Pitfall 2: Ambiguous Language**
```
Bad: "Make it better"
Good: "Improve error handling by adding try-except blocks and logging"
```

**Pitfall 3: Assuming Knowledge**
```
Bad: "Use the standard approach"
Good: "Use the Page Object Model pattern with Selenium"
```

**Pitfall 4: No Quality Criteria**
```
Bad: "Write tests"
Good: "Write tests with >80% code coverage, following AAA pattern"
```

**Pitfall 5: Ignoring AI Limitations**
```
Don't expect:
- Perfect code on first try
- Real-time/current information
- Access to your private codebase
- Understanding of your specific business logic without context
```

### Optimization Workflow

```
1. Write initial prompt
   ↓
2. Get response
   ↓
3. Evaluate quality
   ↓
4. Identify gaps ← ─┐
   ↓                │
5. Refine prompt    │
   ↓                │
6. Get new response │
   ↓                │
7. Still gaps? ─────┘
   ↓
8. Final output
```

### Exercise 2.3: Prompt Optimization

**Task:** Optimize these prompts

**Prompt 1 (Poor):**
```
Write code to test an API
```

Your optimized version:
```
[Write your optimized prompt here]
```

**Prompt 2 (Mediocre):**
```
Create test cases for a shopping cart feature
```

Your optimized version:
```
[Write your optimized prompt here]
```

**Prompt 3 (Needs Work):**
```
Review this test code and make it better:
[code]
```

Your optimized version:
```
[Write your optimized prompt here]
```

### Quality Metrics for Prompts

**Score your prompts (1-5):**

- [ ] Clarity: Is it easy to understand?
- [ ] Specificity: Is it detailed enough?
- [ ] Context: Is background provided?
- [ ] Format: Is output structure defined?
- [ ] Constraints: Are boundaries set?
- [ ] Examples: Are samples provided?
- [ ] Action-oriented: Is it clear what to do?

**Total Score:**
- 28-35: Excellent prompt
- 21-27: Good prompt
- 14-20: Needs improvement
- 7-13: Poor prompt, rewrite

### Key Takeaways

✅ Specific prompts yield better results than vague ones
✅ Context is critical for accurate responses
✅ Examples (few-shot) dramatically improve output quality
✅ Always define the desired output format
✅ Iterate and refine rather than expecting perfection
✅ Break complex tasks into smaller steps
✅ Set clear constraints and boundaries
✅ Always verify and validate AI output
✅ Use system messages to set AI behavior
✅ Maintain a library of effective prompts

### Prompt Engineering Cheat Sheet

```
1. ROLE: "Act as a [specific role with expertise]"
2. CONTEXT: "I'm working on [background info]"
3. TASK: "Create/Generate/Review/Analyze [specific action]"
4. REQUIREMENTS: 
   - [Requirement 1]
   - [Requirement 2]
5. FORMAT: "Structure output as [table/code/json/markdown]"
6. CONSTRAINTS:
   - Technology: [specific tech/version]
   - Scope: [what to include/exclude]
   - Quality: [standards to follow]
7. EXAMPLES: [Provide 1-3 examples if possible]
```

### Next Steps
- Practice optimizing prompts daily
- Build your personal prompt library
- Move to [Module 3: Prompt Engineering for Manual QA](../module-3-manual-qa/README.md)
