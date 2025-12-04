# Few-Shot Learning

Few-shot learning involves providing examples in your prompt to help the AI understand the desired pattern, format, or approach. This technique is highly effective for achieving consistent, high-quality outputs.

## What is Few-Shot Learning?

Few-shot learning means providing a few examples (typically 2-5) of the desired input-output pattern before asking the AI to generate new content. The AI learns from these examples and applies the pattern to new requests.

## Why Use Few-Shot Learning?

1. **Consistency**: Examples ensure consistent output format
2. **Quality**: Shows the AI exactly what you want
3. **Pattern Matching**: Helps AI understand complex requirements
4. **Reduced Iteration**: Fewer back-and-forth corrections needed

## Basic Few-Shot Pattern

```
Example 1:
Input: [Example input]
Output: [Example output]

Example 2:
Input: [Example input]
Output: [Example output]

Now generate:
Input: [Your new input]
Output:
```

## Examples

### Example 1: Test Case Format

**Prompt:**
```
Generate test cases in the following format:

Example 1:
Feature: User Login
Test Case: TC_LOGIN_001
Description: Verify successful login with valid credentials
Steps:
1. Navigate to login page
2. Enter valid username
3. Enter valid password
4. Click login button
Expected Result: User is redirected to dashboard

Example 2:
Feature: User Login
Test Case: TC_LOGIN_002
Description: Verify error message for invalid credentials
Steps:
1. Navigate to login page
2. Enter invalid username
3. Enter invalid password
4. Click login button
Expected Result: Error message "Invalid credentials" is displayed

Now generate test cases for "Password Reset" feature following the same format.
```

### Example 2: Bug Report Format

**Prompt:**
```
Format bug reports like this:

Example 1:
Title: Login button not responding on mobile devices
Severity: High
Priority: High
Steps to Reproduce:
1. Open app on mobile device
2. Navigate to login page
3. Tap login button
Expected: User should be logged in
Actual: Button does not respond to tap
Environment: iOS 15.0, iPhone 12

Example 2:
Title: Form validation error message not visible
Severity: Medium
Priority: Medium
Steps to Reproduce:
1. Open registration form
2. Leave required fields empty
3. Submit form
Expected: Error messages should be visible
Actual: Error messages are present but not visible (white text on white background)
Environment: Chrome 120, Windows 11

Now create a bug report for: "Search functionality returns no results when query contains special characters"
```

## Best Practices

1. **Quality Examples**: Use clear, well-structured examples
2. **Consistent Format**: Keep example format consistent
3. **Relevant Examples**: Choose examples similar to your use case
4. **Right Number**: 2-5 examples usually work best
5. **Clear Instructions**: Explicitly state to follow the pattern

## Common Patterns

### Pattern 1: Input-Output Mapping
```
Input → Output examples
```

### Pattern 2: Before-After
```
Before: [Bad example]
After: [Good example]
Now improve: [Your content]
```

### Pattern 3: Category Examples
```
Category A: [Example]
Category B: [Example]
Categorize: [Your item]
```

## Exercises

1. Create a few-shot prompt for generating API test cases
2. Design a prompt with examples for code review comments
3. Write a few-shot prompt for test data generation

## Next Lesson

Continue to [Role-Based Prompting](./03-role-based-prompting.md)
