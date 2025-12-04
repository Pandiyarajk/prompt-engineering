# Project 4: Test Data Generator

## Objective

Develop an AI-powered test data generation system that creates realistic, varied, and comprehensive test data for different testing scenarios.

## Requirements

- Understanding of test data requirements
- Knowledge of data types and formats
- Access to AI model
- Testing environment setup

## Step-by-Step Guide

### Step 1: Define Data Requirements

Create a prompt to define data needs:

```
You are a Test Data Manager. Define test data requirements for:

Feature: [Feature name]
Data Model: [Describe data structure]

Generate requirements for:
1. Valid data sets
   - Number of records needed
   - Data variations required
   - Realistic constraints

2. Invalid data sets
   - Error scenarios
   - Boundary values
   - Invalid formats

3. Edge cases
   - Empty values
   - Maximum lengths
   - Special characters
   - Unicode characters

4. Data relationships
   - Foreign keys
   - Dependencies
   - Constraints

Provide comprehensive data requirements document.
```

### Step 2: Generate Valid Test Data

Create prompts for valid data generation:

```
Generate realistic test data for [entity]:

Schema:
- Field 1: [Type] - [Constraints]
- Field 2: [Type] - [Constraints]
- Field 3: [Type] - [Constraints]

Requirements:
- Number of records: [N]
- Data should be realistic and varied
- Follow all constraints
- Include different scenarios

Format: [JSON/CSV/SQL]

Generate [N] records with variations.
```

### Step 3: Generate Invalid Test Data

Create prompts for negative test data:

```
Generate invalid test data for testing validation:

Entity: [Entity name]
Fields: [List fields with validations]

For each field, generate:
1. Empty/null values
2. Values exceeding max length
3. Invalid formats
4. Wrong data types
5. Boundary values (min-1, max+1)
6. Special characters
7. SQL injection attempts
8. XSS attempts

Format: JSON array with test cases
Include expected error messages.
```

### Step 4: Generate Edge Case Data

Create prompts for edge cases:

```
Generate edge case test data for [feature]:

Edge Cases Needed:
1. Boundary values
2. Empty strings vs null
3. Maximum length strings
4. Unicode and special characters
5. Very large numbers
6. Very small numbers
7. Date edge cases (leap years, etc.)
8. Timezone variations

For each edge case, provide:
- Test data value
- Expected behavior
- Why it's an edge case
```

### Step 5: Generate Related Data

Create prompts for data relationships:

```
Generate related test data:

Primary Entity: [Entity 1]
  - Fields: [List]
  - Records needed: [N]

Related Entity: [Entity 2]
  - Fields: [List]
  - Relationship: [One-to-many/Many-to-many]
  - Records needed: [M]

Constraints:
- [Constraint 1]
- [Constraint 2]

Generate data maintaining referential integrity.
Format: [Format]
```

## Prompt Templates

### Template 1: Simple Data Generation

```
Generate [N] test records for:

Entity: [Name]
Fields:
- [Field]: [Type]

Format: JSON
```

### Template 2: Complex Data Generation

```
Generate comprehensive test data:

Schema: [Full schema]
Relationships: [Relationships]
Constraints: [Constraints]

Generate:
- [N] valid records
- [M] invalid records
- [K] edge case records

Include data variations and realistic values.
```

### Template 3: Data for Specific Test Scenario

```
Generate test data for this test scenario:

Scenario: [Description]
Test Case: [Test case]

Required Data:
- [Data requirement 1]
- [Data requirement 2]
- [Data requirement 3]

Generate data that covers:
- Positive case
- Negative case
- Edge cases
```

## Example Output

### Generated Test Data

```json
{
  "validUsers": [
    {
      "id": 1,
      "email": "john.doe@example.com",
      "firstName": "John",
      "lastName": "Doe",
      "age": 28,
      "phone": "+1-555-0123",
      "createdAt": "2024-01-15T10:30:00Z"
    },
    {
      "id": 2,
      "email": "jane.smith@test.com",
      "firstName": "Jane",
      "lastName": "Smith",
      "age": 35,
      "phone": "+1-555-0456",
      "createdAt": "2024-01-16T14:20:00Z"
    }
  ],
  "invalidUsers": [
    {
      "email": "invalid-email",
      "age": -5,
      "phone": "123"
    },
    {
      "email": "",
      "firstName": "A".repeat(1000),
      "age": "not-a-number"
    }
  ],
  "edgeCases": [
    {
      "email": "test@example.com",
      "firstName": "",
      "age": 0
    },
    {
      "email": "a@b.co",
      "firstName": "A",
      "age": 150
    }
  ]
}
```

## Best Practices

1. **Realistic Data**: Generate data that looks real
2. **Variety**: Include diverse data variations
3. **Coverage**: Cover all test scenarios
4. **Maintainability**: Keep data generation scripts
5. **Documentation**: Document data requirements

## Data Types to Consider

- **Personal Information**: Names, emails, phones, addresses
- **Dates and Times**: Various formats, timezones, edge dates
- **Numbers**: Integers, decimals, large numbers, negative
- **Strings**: Short, long, special characters, Unicode
- **Booleans**: True, false, null
- **Files**: Images, documents, various formats
- **JSON/XML**: Nested structures, arrays

## Extensions

- Create data generation API
- Build data management dashboard
- Add data masking for sensitive info
- Create data refresh workflows
- Integrate with test frameworks

## Module Summary

Congratulations! You've completed all four hands-on projects:

1. ✅ Automated Test Suite Generation
2. ✅ Bug Triage Assistant
3. ✅ Code Review Bot
4. ✅ Test Data Generator

## Next Module

Proceed to [Module 9: Best Practices and Ethics](../module-9-best-practices-and-ethics/)
