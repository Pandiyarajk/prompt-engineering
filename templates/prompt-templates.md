# Prompt Templates Library

A comprehensive collection of reusable prompt templates for testing and development.

---

## 📋 Table of Contents

1. [Test Case Generation](#test-case-generation)
2. [Test Automation](#test-automation)
3. [API Testing](#api-testing)
4. [Bug Reports](#bug-reports)
5. [Code Generation](#code-generation)
6. [Code Review](#code-review)
7. [Documentation](#documentation)
8. [Debugging](#debugging)
9. [Performance](#performance)
10. [Security](#security)

---

## Test Case Generation

### Template 1: Basic Test Cases

```
Act as a {role} with expertise in {domain} testing.

Feature: {feature_name}

Requirements:
{list_requirements}

Generate test cases including:
- Positive scenarios
- Negative scenarios
- Edge cases
- Boundary value tests

Format as a table with columns:
| Test ID | Description | Steps | Expected Result | Priority |

Constraints:
- {constraint_1}
- {constraint_2}
```

**Example Usage:**

```
Act as a Senior QA Engineer with expertise in web application testing.

Feature: User Login

Requirements:
- Email and password authentication
- Remember me checkbox
- Forgot password link
- Account lockout after 5 failed attempts

Generate test cases including:
- Positive scenarios
- Negative scenarios
- Edge cases
- Boundary value tests

Format as a table with columns:
| Test ID | Description | Steps | Expected Result | Priority |

Constraints:
- Focus on functional testing only
- Include accessibility considerations
```

---

### Template 2: BDD/Gherkin Format

```
Act as a BDD expert and QA Engineer.

Feature: {feature_name}

Convert the following requirements into Gherkin format:
{requirements}

Create scenarios for:
- Happy path
- Error scenarios
- Edge cases

Format:
Feature: {feature_name}
  {feature_description}

Scenario: {scenario_name}
  Given {precondition}
  When {action}
  Then {expected_result}
  And {additional_assertion}
```

---

### Template 3: Risk-Based Test Cases

```
Act as a Senior QA Engineer specializing in risk-based testing.

Feature: {feature_name}
Requirements: {requirements}

Analyze and categorize test cases by risk:

High Risk (Critical Impact):
- {What could cause major failures?}

Medium Risk (Significant Impact):
- {What could cause moderate issues?}

Low Risk (Minor Impact):
- {What could cause minor issues?}

For each risk level, generate:
- Test scenarios
- Priority
- Impact if bug found
- Recommended test approach

Format as structured list with risk assessment.
```

---

## Test Automation

### Template 4: Selenium Test Generation

```
Act as a Test Automation Engineer expert in {framework} with {language}.

Test Scenario: {description}

Page Details:
- URL: {url}
- Elements:
  - {element_1}: {locator_1}
  - {element_2}: {locator_2}

Generate a complete test script that:
1. {action_1}
2. {action_2}
3. {action_3}

Technical Requirements:
- Language: {language} {version}
- Framework: {test_framework}
- Design Pattern: {pattern}
- Browser: {browser}

Include:
- Complete working code
- Setup and teardown
- Explicit waits
- Error handling
- Assertions
- Comments
- Example execution

Follow {coding_standard} standards.
```

**Example:**

```
Act as a Test Automation Engineer expert in Selenium with Python.

Test Scenario: User login and logout flow

Page Details:
- URL: https://example.com/login
- Elements:
  - Email field: id="email"
  - Password field: id="password"
  - Login button: id="login-btn"
  - Logout button: class="logout"

Generate a complete test script that:
1. Navigates to login page
2. Enters credentials
3. Clicks login
4. Verifies dashboard
5. Logs out

Technical Requirements:
- Language: Python 3.9+
- Framework: pytest
- Design Pattern: Page Object Model
- Browser: Chrome

Include:
- Complete working code
- Setup and teardown
- Explicit waits
- Error handling
- Assertions
- Comments
- Example execution

Follow PEP 8 standards.
```

---

### Template 5: Page Object Model

```
Act as a Test Automation Architect expert in Page Object Model.

Create a Page Object for: {page_name}

Page elements:
- {element_1}: {locator_1}
- {element_2}: {locator_2}

User actions on this page:
- {action_1}
- {action_2}

Technical specs:
- Language: {language}
- Framework: {framework}
- Locator strategy: {strategy}

Include:
1. Page class with:
   - Element locators (as class variables)
   - Action methods
   - Verification methods
   - Wait strategies
2. Base page class (if needed)
3. Usage example
4. Documentation

Follow best practices:
- Single Responsibility Principle
- Clear method names
- Proper encapsulation
- Type hints
```

---

## API Testing

### Template 6: API Test Cases

```
Act as an API Testing Expert.

API Endpoint: {method} {endpoint}

Request:
- Headers: {headers}
- Body: {request_body}
- Query params: {params}

Expected Responses:
- Success ({status_code}): {success_response}
- Error scenarios: {error_responses}

Generate comprehensive API test cases for:
1. Positive scenarios (valid inputs)
2. Negative scenarios (invalid inputs)
3. Boundary tests
4. Security tests (injection, XSS)
5. Performance expectations

For each test case include:
- Test description
- Request details (method, headers, body)
- Expected status code
- Expected response body
- Response time threshold

Format as structured test cases.
```

---

### Template 7: API Automation Script

```
Act as an API Test Automation Engineer expert in {language}.

Generate API test automation for: {api_description}

Endpoints to test:
- {endpoint_1}
- {endpoint_2}

Requirements:
- Test all CRUD operations
- Validate response schemas
- Test authentication
- Handle error scenarios
- Test data setup/cleanup

Technical stack:
- Language: {language}
- HTTP library: {library}
- Test framework: {framework}
- Assertions: {assertion_library}

Include:
1. Complete test suite
2. Helper functions
3. Test fixtures
4. Schema validation
5. Configuration
6. Example execution
7. Requirements.txt (if Python)

Follow REST API testing best practices.
```

---

## Bug Reports

### Template 8: Bug Report Generation

```
Act as a Senior QA Engineer.

I encountered an issue and need a professional bug report.

Issue description: {brief_description}

What I observed: {actual_behavior}

What I expected: {expected_behavior}

Steps I took: {steps}

Environment: {environment_details}

Create a detailed bug report with:
1. Clear, specific title
2. Severity classification (Critical/High/Medium/Low)
3. Priority classification (P0/P1/P2/P3)
4. Detailed steps to reproduce
5. Expected vs Actual results
6. Environment information
7. Impact assessment
8. Suggested category (UI/Backend/API/etc)
9. Potential root cause (if identifiable)
10. Recommended assignee (if applicable)

Format professionally for bug tracking system.
```

---

### Template 9: Bug Analysis

```
Act as a Software Engineer and Debugging Expert.

Analyze this bug report:
{bug_report}

Error logs/stack trace:
{logs}

Provide:
1. Root cause analysis
   - Most likely cause
   - Alternative possibilities
   - Evidence/reasoning

2. Impact assessment
   - User impact
   - System impact
   - Business impact

3. Investigation steps
   - What to check
   - How to reproduce
   - Debugging approach

4. Recommended fix
   - Approach
   - Code changes needed
   - Testing strategy

5. Prevention
   - Why it wasn't caught
   - How to prevent similar issues
   - Test improvements needed
```

---

## Code Generation

### Template 10: Function Generation

```
Act as a {language} Developer expert in {domain}.

Create a function to: {purpose}

Signature: {function_signature}

Requirements:
- {requirement_1}
- {requirement_2}

Input:
- {input_1}: {type} - {description}
- {input_2}: {type} - {description}

Output:
- Returns: {return_type} - {description}

Error handling:
- {error_condition_1} → {error_response_1}
- {error_condition_2} → {error_response_2}

Include:
- Complete function implementation
- Type hints/annotations
- Docstring (with examples)
- Input validation
- Error handling
- Unit tests
- Usage examples

Optimize for: {optimization_goal}
Follow: {coding_standard}
```

---

### Template 11: Class Generation

```
Act as a Software Architect expert in {language}.

Create a {class_name} class for: {purpose}

Methods needed:
- {method_1}({params}) → {return_type}
- {method_2}({params}) → {return_type}

Attributes:
- {attribute_1}: {type} - {description}
- {attribute_2}: {type} - {description}

Requirements:
- {requirement_1}
- {requirement_2}

Design patterns: {patterns}

Include:
- Complete class implementation
- Constructor/initialization
- All methods with documentation
- Properties/getters/setters
- Error handling
- Type hints
- Unit tests
- Usage examples

Follow: {design_principles}
```

---

## Code Review

### Template 12: Code Review

```
Act as a Senior Software Engineer and Code Reviewer.

Review this {language} code for:
1. Code quality and readability
2. Best practices adherence
3. Security vulnerabilities
4. Performance issues
5. Error handling
6. Maintainability
7. Testing gaps

Code to review:
{code}

Context: {context}

Provide:
1. Summary assessment (Good/Needs Work/Poor)
2. Issues found:
   - Critical (must fix)
   - Major (should fix)
   - Minor (nice to fix)
3. For each issue:
   - Location
   - Problem description
   - Why it's a problem
   - Recommended fix with code example
4. Positive points
5. Refactored version (for major issues)
6. Testing recommendations

Be constructive and specific.
```

---

### Template 13: Refactoring Suggestions

```
Act as a Refactoring Expert.

Analyze and refactor this code:
{code}

Current issues:
- {issue_1}
- {issue_2}

Refactoring goals:
- Improve readability
- Reduce complexity
- Apply {design_pattern}
- Follow {principles}
- Enhance maintainability

Provide:
1. Analysis of current code
   - Complexity metrics
   - Code smells identified
   - Violation of principles

2. Refactoring plan
   - Steps to refactor
   - Patterns to apply
   - Changes needed

3. Refactored code
   - Complete implementation
   - Explanatory comments
   - Before/after comparison

4. Benefits
   - Improvements gained
   - Trade-offs (if any)

5. Testing strategy
   - How to verify refactoring
   - Test cases needed
```

---

## Documentation

### Template 14: README Generation

```
Act as a Technical Writer.

Create a comprehensive README.md for: {project_name}

Project details:
- Purpose: {purpose}
- Technology: {tech_stack}
- Key features: {features}

Include these sections:
1. Project title and badges
2. Description
3. Features
4. Installation
5. Usage (with examples)
6. Configuration
7. API documentation (if applicable)
8. Testing
9. Contributing
10. License
11. Contact/Support

Make it:
- Professional
- Clear and concise
- Beginner-friendly
- Well-formatted (markdown)
- Include code examples
- Add emojis for readability
```

---

### Template 15: API Documentation

```
Act as an API Documentation Expert.

Create API documentation for: {endpoint}

Endpoint: {method} {path}

Details:
- Purpose: {purpose}
- Authentication: {auth_type}
- Request: {request_details}
- Response: {response_details}

Generate documentation with:
1. Overview
2. Endpoint details
3. Request format
   - Headers
   - Parameters
   - Body schema
4. Response format
   - Success responses
   - Error responses
   - Status codes
5. Examples
   - curl commands
   - Request/response examples
6. Error handling
7. Rate limiting
8. Notes/warnings

Format: {OpenAPI/Markdown/HTML}
```

---

## Debugging

### Template 16: Error Analysis

```
Act as a Debugging Expert.

I'm getting this error:
{error_message}

Stack trace:
{stack_trace}

Context:
- Language/Framework: {tech}
- What I was doing: {action}
- Environment: {environment}

Analyze and provide:
1. Error explanation
   - What the error means
   - Why it occurred

2. Common causes
   - Most likely cause
   - Other possibilities

3. Debugging steps
   - What to check first
   - How to investigate
   - Debug commands/tools

4. Solutions
   - Quick fix
   - Proper fix
   - Prevention

5. Code examples
   - Problematic code pattern
   - Corrected code
```

---

### Template 17: Performance Optimization

```
Act as a Performance Optimization Expert.

Analyze and optimize this code:
{code}

Performance issues:
- {issue_1}
- {issue_2}

Constraints:
- {constraint_1}
- {constraint_2}

Provide:
1. Performance analysis
   - Bottlenecks identified
   - Time/space complexity
   - Resource usage

2. Optimization recommendations
   - Algorithm improvements
   - Data structure changes
   - Caching strategies
   - Database query optimization

3. Optimized code
   - Refactored implementation
   - Explanation of changes
   - Performance gains expected

4. Benchmark
   - Before/after comparison
   - Metrics to measure
   - Testing approach

5. Trade-offs
   - What was sacrificed (if anything)
   - When to use optimization
```

---

## Security

### Template 18: Security Review

```
Act as a Security Expert.

Review this code for security vulnerabilities:
{code}

Context: {application_context}

Check for:
1. Injection attacks (SQL, XSS, Command)
2. Authentication/Authorization issues
3. Sensitive data exposure
4. Security misconfiguration
5. Insecure dependencies
6. Insufficient logging
7. CSRF vulnerabilities
8. Insecure deserialization

For each vulnerability:
- Severity (Critical/High/Medium/Low)
- Location in code
- Exploitation scenario
- Impact
- Remediation
- Secure code example

Provide OWASP Top 10 compliance assessment.
```

---

### Template 19: Security Test Cases

```
Act as a Security Testing Expert.

Feature: {feature_name}
Type: {web/api/mobile}

Generate security test cases for:
1. Authentication
2. Authorization
3. Input validation
4. Session management
5. Cryptography
6. Error handling
7. Data protection

Include tests for:
- OWASP Top 10 vulnerabilities
- Common attack vectors
- Security misconfigurations
- Sensitive data exposure

Format each test with:
- Test ID
- Vulnerability type
- Attack scenario
- Test steps
- Expected security behavior
- Tools needed
- Severity
```

---

## Usage Tips

### How to Use These Templates

1. **Copy the template** that fits your need
2. **Replace placeholders** (text in {braces}) with your specific details
3. **Customize** as needed for your context
4. **Paste into ChatGPT** or your AI tool
5. **Review and iterate** on the results

### Template Customization

**Add specificity:**

```
{language} → Python 3.9+
{framework} → pytest with fixtures
{role} → Senior QA Engineer with 10 years experience
```

**Add constraints:**

```
Constraints:
- Must work on Python 3.9+
- Follow PEP 8
- Include type hints
- Maximum 100 lines
- No external dependencies
```

**Add examples:**

```
Example input: {"name": "test", "age": 25}
Example output: User object with validated data
```

---

## Contributing Templates

Have a great prompt template? Share it!

1. Fork the repository
2. Add your template to this file
3. Include example usage
4. Submit a pull request

---

## Template Categories

- ✅ Test Case Generation: 3 templates
- ✅ Test Automation: 2 templates
- ✅ API Testing: 2 templates
- ✅ Bug Reports: 2 templates
- ✅ Code Generation: 2 templates
- ✅ Code Review: 2 templates
- ✅ Documentation: 2 templates
- ✅ Debugging: 2 templates
- ✅ Security: 2 templates

**Total: 19 templates**
