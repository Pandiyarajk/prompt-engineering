# Project 3: Code Review Bot

## Objective

Build an AI-powered code review assistant that automatically reviews code for quality, security, best practices, and provides constructive feedback.

## Requirements

- Understanding of code review practices
- Knowledge of programming languages and frameworks
- Access to AI model
- Code repository access

## Step-by-Step Guide

### Step 1: Define Review Criteria

Establish code review standards:

```
You are a Senior Software Engineer defining code review criteria.

Create comprehensive review criteria covering:

1. Code Quality:
   - Readability and maintainability
   - Code organization
   - Naming conventions
   - Code complexity

2. Best Practices:
   - Language-specific best practices
   - Design patterns
   - SOLID principles
   - DRY principle

3. Security:
   - OWASP Top 10
   - Input validation
   - Authentication/Authorization
   - Data protection

4. Testing:
   - Test coverage
   - Test quality
   - Edge cases
   - Test maintainability

5. Performance:
   - Efficiency
   - Resource usage
   - Scalability
   - Optimization opportunities

Provide detailed checklist for each category.
```

### Step 2: Code Review Prompt

Create the main review prompt:

```
You are a Senior Code Reviewer with expertise in [language/framework].

Review this code:

```[language]
[Code snippet]
```

Review Criteria:
1. Code Quality and Readability
2. Best Practices and Patterns
3. Security Vulnerabilities
4. Performance Issues
5. Test Coverage
6. Documentation

For each finding, provide:
- Category
- Severity (Critical/High/Medium/Low)
- Location (line numbers)
- Issue Description
- Suggested Fix
- Example (if applicable)

Format as structured review comments.
```

### Step 3: Security-Focused Review

Create security-specific review:

```
You are a Security Expert reviewing code for vulnerabilities.

Code:
[Code snippet]

Focus Areas:
1. SQL Injection
2. XSS (Cross-Site Scripting)
3. Authentication/Authorization flaws
4. Input validation
5. Sensitive data exposure
6. Insecure dependencies

Provide security review with:
- Vulnerability type
- Risk level
- Exploitation scenario
- Fix recommendation
- Secure code example
```

### Step 4: Performance Review

Create performance-focused review:

```
You are a Performance Engineer reviewing code.

Code:
[Code snippet]

Analyze:
1. Time complexity
2. Space complexity
3. Resource usage
4. Database queries
5. API calls
6. Caching opportunities

Provide:
- Performance issues
- Bottlenecks
- Optimization suggestions
- Before/after examples
```

### Step 5: Generate Review Comments

Format review comments for pull requests:

```
Generate code review comments for this pull request:

Changes:
[Diff or code changes]

Generate comments in this format:

**Category**: [Category]
**Severity**: [Level]
**Location**: [File:Line]

**Issue**: [Description]

**Suggestion**: [Fix]

**Example**:
```[language]
[Code example]
```

Generate [number] most important comments.
```

## Prompt Templates

### Template 1: Quick Review

```
Quick review this code:

[Code snippet]

Provide:
- Top 3 issues
- Top 3 suggestions
- Overall quality score (1-10)
```

### Template 2: Comprehensive Review

```
Perform comprehensive code review:

File: [File name]
Language: [Language]
Framework: [Framework]

Code:
[Code]

Review all aspects:
- Quality
- Security
- Performance
- Testing
- Documentation
- Best practices
```

### Template 3: Comparison Review

```
Compare these two implementations:

Implementation A:
[Code A]

Implementation B:
[Code B]

Compare:
- Code quality
- Performance
- Maintainability
- Best practices
- Security

Recommend which is better and why.
```

## Example Output

### Review Comments

```
Code Review Comments
===================

1. **Category**: Security
   **Severity**: High
   **Location**: auth.js:45

   **Issue**: SQL injection vulnerability. User input is directly concatenated into SQL query.

   **Suggestion**: Use parameterized queries or prepared statements.

   **Example**:
   ```javascript
   // Before
   const query = `SELECT * FROM users WHERE id = ${userId}`;
   
   // After
   const query = 'SELECT * FROM users WHERE id = ?';
   db.query(query, [userId]);
   ```

2. **Category**: Code Quality
   **Severity**: Medium
   **Location**: utils.js:12-25

   **Issue**: Function is too long (14 lines) and does multiple things. Violates Single Responsibility Principle.

   **Suggestion**: Split into smaller, focused functions.

   **Example**:
   ```javascript
   // Split into:
   - validateInput()
   - processData()
   - formatOutput()
   ```

3. **Category**: Performance
   **Severity**: Low
   **Location**: api.js:78

   **Issue**: N+1 query problem. Fetching related data in a loop.

   **Suggestion**: Use eager loading or batch queries.

   **Example**:
   ```javascript
   // Use include/join to fetch related data in one query
   ```
```

## Best Practices

1. **Be Constructive**: Focus on improvement, not criticism
2. **Provide Examples**: Show how to fix issues
3. **Prioritize**: Focus on high-severity issues first
4. **Be Specific**: Point to exact locations and lines
5. **Explain Why**: Help developers understand reasoning

## Extensions

- Integrate with GitHub/GitLab APIs
- Create review dashboard
- Build review metrics
- Add automated fixes
- Create review templates

## Next Project

Continue to [Project 4: Test Data Generator](./project-4-test-data-generator.md)
