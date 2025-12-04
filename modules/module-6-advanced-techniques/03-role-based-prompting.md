# Role-Based Prompting

Role-based prompting involves assigning a specific role or persona to the AI, which helps it adopt the appropriate perspective, knowledge, and communication style for the task.

## What is Role-Based Prompting?

Role-based prompting means telling the AI to act as a specific expert, professional, or character. This helps the AI:
- Access relevant knowledge
- Use appropriate terminology
- Follow professional standards
- Consider relevant factors

## Why Use Role-Based Prompting?

1. **Expertise**: AI adopts domain-specific knowledge
2. **Consistency**: Maintains character and perspective
3. **Quality**: Better outputs for specialized tasks
4. **Context**: Considers relevant factors automatically

## Basic Role-Based Pattern

```
You are a [role] with [years] years of experience in [domain].

Your expertise includes:
- [Skill 1]
- [Skill 2]
- [Skill 3]

Task: [Your task]

Consider:
- [Factor 1]
- [Factor 2]
- [Factor 3]
```

## Examples

### Example 1: Senior QA Engineer

**Prompt:**
```
You are a Senior QA Engineer with 10 years of experience in web application testing.

Your expertise includes:
- Test strategy and planning
- Risk-based testing
- Test automation frameworks
- API and UI testing

Task: Review this test plan for a new e-commerce checkout feature.

Consider:
- Edge cases and boundary conditions
- Security vulnerabilities
- Performance implications
- User experience issues

Provide a comprehensive review with recommendations.
```

### Example 2: Security Expert

**Prompt:**
```
You are a Security Expert specializing in application security testing.

Your knowledge includes:
- OWASP Top 10 vulnerabilities
- Authentication and authorization flaws
- Data protection best practices
- Security testing methodologies

Task: Generate security test cases for a user authentication system.

Focus on:
- SQL injection vulnerabilities
- XSS attacks
- Session management
- Password security
- Rate limiting

Provide detailed test cases with attack vectors.
```

### Example 3: Test Automation Architect

**Prompt:**
```
You are a Test Automation Architect with expertise in designing scalable test frameworks.

Your skills include:
- Framework design patterns
- Test data management
- CI/CD integration
- Maintainable test code

Task: Design a test automation framework for a microservices-based application.

Consider:
- Framework architecture
- Test organization
- Reporting mechanisms
- Integration with CI/CD
- Maintainability and scalability

Provide a detailed framework design document.
```

## Common Roles for QA and Development

### QA Roles
- Senior QA Engineer
- Test Automation Engineer
- Security Tester
- Performance Tester
- Test Architect

### Development Roles
- Senior Software Engineer
- Code Reviewer
- DevOps Engineer
- Security Developer
- Technical Architect

## Best Practices

1. **Be Specific**: Define the role clearly with relevant experience
2. **List Expertise**: Explicitly state relevant skills and knowledge
3. **Set Context**: Provide domain and project context
4. **Specify Considerations**: List what factors to consider
5. **Maintain Role**: Remind AI of role if conversation gets long

## Combining Roles

You can combine roles for complex tasks:

```
You are a Senior QA Engineer AND a Security Expert. 

As a QA Engineer, you focus on:
- Test coverage and quality

As a Security Expert, you focus on:
- Security vulnerabilities and threats

Task: Create a comprehensive security test plan...
```

## Exercises

1. Create a role-based prompt for a Performance Tester analyzing load test results
2. Design a prompt where AI acts as a Code Reviewer
3. Write a prompt for a Test Data Manager role

## Next Lesson

Continue to [Context Management](./04-context-management.md)
