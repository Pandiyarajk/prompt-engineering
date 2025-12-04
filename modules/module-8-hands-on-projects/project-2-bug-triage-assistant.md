# Project 2: Bug Triage Assistant

## Objective

Create an AI-powered system that helps triage bugs by automatically classifying, prioritizing, and assigning bugs based on their descriptions and context.

## Requirements

- Understanding of bug tracking systems
- Knowledge of bug classification and prioritization
- Access to AI model
- Bug tracking tool (Jira, GitHub Issues, etc.)

## Step-by-Step Guide

### Step 1: Define Triage Criteria

Create a prompt to establish triage rules:

```
You are a QA Lead defining bug triage criteria.

Create a comprehensive bug triage system with:

1. Severity Levels:
   - Critical
   - High
   - Medium
   - Low

2. Priority Levels:
   - P0 (Immediate)
   - P1 (High)
   - P2 (Medium)
   - P3 (Low)

3. Categories:
   - Functional
   - UI/UX
   - Performance
   - Security
   - Compatibility

4. Assignment Rules:
   - Based on component
   - Based on expertise
   - Based on workload

Provide detailed criteria for each classification.
```

### Step 2: Bug Classification Prompt

Create a prompt template for bug classification:

```
You are a Bug Triage Specialist. Classify this bug report:

Bug Description: [Bug description]
Steps to Reproduce: [Steps]
Environment: [Environment]
Screenshots/Logs: [If available]

Classify:
1. Severity: [Critical/High/Medium/Low]
   Reasoning: [Why]

2. Priority: [P0/P1/P2/P3]
   Reasoning: [Why]

3. Category: [Category]
   Reasoning: [Why]

4. Suggested Assignee: [Team/Role]
   Reasoning: [Why]

5. Estimated Effort: [Hours/Days]
   Reasoning: [Why]

6. Related Issues: [Suggestions]
```

### Step 3: Bug Report Enhancement

Enhance incomplete bug reports:

```
You are a QA Engineer. Enhance this bug report:

Original Report:
[Incomplete bug report]

Enhance by:
1. Adding missing information
2. Clarifying steps to reproduce
3. Adding expected vs actual results
4. Suggesting test data
5. Adding environment details
6. Improving title and description

Provide the enhanced bug report.
```

### Step 4: Duplicate Detection

Create a prompt to detect duplicate bugs:

```
You are analyzing bug reports for duplicates.

New Bug Report:
[New bug description]

Existing Bug Reports:
1. [Bug 1 description]
2. [Bug 2 description]
3. [Bug 3 description]

Analyze:
- Is this a duplicate?
- Which existing bug(s) does it match?
- Similarity score (0-100%)
- Key differences if not duplicate

Provide analysis and recommendation.
```

### Step 5: Triage Workflow Automation

Create an automated triage workflow:

```
Bug Triage Workflow:

Step 1: Analyze bug description
Step 2: Classify severity and priority
Step 3: Categorize bug
Step 4: Check for duplicates
Step 5: Suggest assignee
Step 6: Generate triage summary

Bug Report: [Report]

Execute this workflow and provide results.
```

## Prompt Templates

### Template 1: Quick Triage

```
Quick triage this bug:

Title: [Title]
Description: [Description]

Provide:
- Severity (one word)
- Priority (P0-P3)
- Category (one word)
- One-sentence summary
```

### Template 2: Detailed Analysis

```
Perform detailed bug analysis:

[Full bug report]

Analyze:
1. Impact assessment
2. User impact
3. Business impact
4. Technical complexity
5. Risk assessment
6. Recommendations
```

### Template 3: Batch Triage

```
Triage these bugs in batch:

Bug 1: [Description]
Bug 2: [Description]
Bug 3: [Description]

For each bug, provide:
- Severity
- Priority
- Category
- Assignee suggestion

Format as a table.
```

## Example Output

### Triage Result

```
Bug Triage Analysis
==================

Bug ID: BUG-1234
Title: Login button not responding

Classification:
- Severity: High
- Priority: P1
- Category: Functional
- Component: Authentication

Reasoning:
- Blocks core user flow
- Affects all users
- Easy to reproduce
- High user impact

Assignment:
- Suggested Team: Frontend Team
- Suggested Assignee: UI Developer
- Estimated Effort: 4 hours

Related Issues:
- BUG-1200 (Similar issue in registration)
- BUG-1150 (Button responsiveness)

Next Steps:
1. Verify reproduction
2. Check browser compatibility
3. Review recent changes
```

## Best Practices

1. **Consistent Criteria**: Use same criteria across all bugs
2. **Review AI Output**: Always review AI classifications
3. **Learn Patterns**: Refine prompts based on results
4. **Update Rules**: Keep triage criteria updated
5. **Human Oversight**: Use AI as assistant, not replacement

## Extensions

- Integrate with bug tracking APIs
- Create triage dashboard
- Build trend analysis
- Add SLA tracking
- Create triage reports

## Next Project

Continue to [Project 3: Code Review Bot](./project-3-code-review-bot.md)
