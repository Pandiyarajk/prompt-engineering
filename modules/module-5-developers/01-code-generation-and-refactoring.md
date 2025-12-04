# Module 5: Prompt Engineering for Developers

## Lesson 1: Code Generation and Refactoring

### Introduction

AI can accelerate development by generating code, refactoring existing code, and suggesting improvements. This lesson teaches developers how to use prompt engineering effectively.

### Code Generation Framework

```
REQUIREMENTS → DETAILED PROMPT → CODE → REVIEW → TEST → REFINE
```

### Basic Code Generation

#### Prompt Template:
```
Act as a Senior [Language] Developer with expertise in [domain].

Task: Create [component/function/class] for [purpose]

Requirements:
- [Functional requirement 1]
- [Functional requirement 2]

Technical Specifications:
- Language: [Language + version]
- Framework: [Framework if applicable]
- Design patterns: [Patterns to use]
- Standards: [Coding standards]

Include:
- Complete working code
- Error handling
- Input validation
- Documentation (docstrings/comments)
- Type hints/annotations
- Unit tests
- Example usage

Optimize for:
- [Performance/Readability/Maintainability]
```

### Function Generation

#### Example 1: Utility Function

**Prompt:**
```
Act as a Senior Python Developer.

Create a utility function to validate email addresses.

Requirements:
- Check basic format (user@domain.com)
- Verify domain has valid TLD
- Check for common typos (gmial.com → gmail.com)
- Support international domains
- Return validation result and error message

Technical Specs:
- Python 3.9+
- Use regex for pattern matching
- Type hints required
- Handle edge cases (empty, None, whitespace)

Include:
- Main validation function
- Helper functions if needed
- Comprehensive docstring
- Unit tests using pytest
- Example usage

Follow PEP 8 and best practices.
```

#### Example 2: API Function

**Prompt:**
```
Act as a Backend Developer expert in Python FastAPI.

Create an API endpoint for user registration.

Requirements:
- Endpoint: POST /api/register
- Accept: email, password, firstName, lastName
- Validate input data
- Hash password (bcrypt)
- Save to database (SQLAlchemy)
- Return user object (exclude password)
- Handle duplicate email
- Send verification email (async)

Technical Specs:
- Python 3.9+
- FastAPI framework
- Pydantic for validation
- SQLAlchemy ORM
- Async/await patterns
- HTTP status codes (201, 400, 409)

Include:
- Pydantic models (request/response)
- Database model
- Endpoint function
- Password hashing utility
- Error handling
- Docstring
- Example request/response

Follow REST API best practices.
```

### Class Generation

#### Example 1: Service Class

**Prompt:**
```
Act as a Senior Software Engineer specializing in clean architecture.

Create a UserService class for user management.

Methods Needed:
- create_user(data) → User
- get_user(user_id) → User
- update_user(user_id, data) → User
- delete_user(user_id) → bool
- list_users(page, limit, filters) → List[User]
- authenticate(email, password) → Token

Requirements:
- Dependency injection for repository
- Input validation
- Business logic separation
- Error handling (custom exceptions)
- Logging
- Transaction management

Technical Specs:
- Python 3.9+
- Type hints throughout
- Async methods
- Follow SOLID principles
- Comprehensive docstrings

Include:
- UserService class
- Custom exception classes
- User model/schema
- Repository interface
- Example usage
- Unit tests (mock repository)

Optimize for testability and maintainability.
```

#### Example 2: Design Pattern Implementation

**Prompt:**
```
Act as a Senior Software Architect.

Implement the Repository Pattern for a Task management system.

Requirements:
- Abstract base repository
- Concrete implementation for PostgreSQL
- Support for SQLAlchemy
- Generic CRUD operations
- Custom query methods
- Transaction support
- Connection pooling

Operations:
- add(entity)
- get(id)
- get_all(filters, pagination)
- update(entity)
- delete(id)
- find_by_criteria(criteria)

Technical Specs:
- Python 3.9+
- SQLAlchemy 2.0+
- Abstract base classes
- Type hints with generics
- Async operations
- Context managers

Include:
- Abstract Repository class
- TaskRepository implementation
- Task model
- Usage examples
- Unit tests
- Integration test setup

Follow repository pattern best practices.
```

### Code Refactoring

#### Refactoring Template:
```
Act as a Senior Developer expert in code refactoring.

Refactor the following code to improve [aspect].

Current Code:
[Paste code here]

Issues Identified:
- [Issue 1]
- [Issue 2]

Refactoring Goals:
- Improve readability
- Reduce complexity
- Follow SOLID principles
- Add type hints
- Improve error handling
- Add documentation

Provide:
1. Refactored code
2. Explanation of changes
3. Before/after comparison
4. Performance impact (if any)
```

#### Example 1: Simplify Complex Function

**Prompt:**
```
Act as a Senior Python Developer specializing in clean code.

Refactor this complex function to improve readability and maintainability.

Current Code:
```python
def process_order(order_data):
    if order_data:
        if 'items' in order_data and len(order_data['items']) > 0:
            total = 0
            for item in order_data['items']:
                if 'price' in item and 'quantity' in item:
                    total += item['price'] * item['quantity']
            if total > 100:
                discount = total * 0.1
                total = total - discount
            if 'shipping' in order_data:
                total += order_data['shipping']
            return {'total': total, 'status': 'success'}
        else:
            return {'error': 'No items in order'}
    else:
        return {'error': 'Invalid order data'}
```

Refactoring Goals:
- Extract helper functions
- Add early returns/guard clauses
- Improve variable names
- Add type hints
- Add error handling
- Add docstrings
- Reduce nesting

Provide:
- Refactored code
- Explanation of changes
- Unit tests
```

#### Example 2: Apply Design Patterns

**Prompt:**
```
Act as a Software Architect.

Refactor this code to use appropriate design patterns.

Current Code:
```python
class DataProcessor:
    def process(self, data, output_format):
        if output_format == 'json':
            # JSON processing logic
            result = json.dumps(data)
        elif output_format == 'xml':
            # XML processing logic  
            result = dict_to_xml(data)
        elif output_format == 'csv':
            # CSV processing logic
            result = dict_to_csv(data)
        else:
            raise ValueError("Unsupported format")
        return result
```

Refactoring Goals:
- Apply Strategy Pattern
- Remove if-else chains
- Make it extensible
- Follow Open/Closed Principle

Provide:
- Refactored code with Strategy Pattern
- UML diagram (text-based)
- Explanation of benefits
- Example of adding new format
```

### Algorithm Implementation

**Prompt:**
```
Act as an Algorithms Expert.

Implement [Algorithm Name] in [Language].

Requirements:
- Efficient implementation
- Handle edge cases
- Clear variable names
- Step-by-step comments
- Time complexity: O(?)
- Space complexity: O(?)

Include:
- Main algorithm function
- Helper functions if needed
- Comprehensive docstring with complexity analysis
- Unit tests with various inputs
- Performance comparison with naive approach
- Example usage with explanation

Example:
```
Act as an Algorithms Expert.

Implement Binary Search algorithm in Python.

Requirements:
- Iterative and recursive versions
- Handle edge cases (empty array, single element, not found)
- Generic (works with any comparable type)
- Optimized for performance

Include:
- Both implementations
- Time/space complexity analysis
- Unit tests
- Performance benchmarks
- When to use binary search vs linear search
```

### Database Queries

**Prompt:**
```
Act as a Database Expert specializing in [database type].

Create database queries for [purpose].

Requirements:
[List requirements]

Technical Context:
- Database: [PostgreSQL/MySQL/MongoDB]
- ORM: [SQLAlchemy/Django ORM/Mongoose]
- Tables/Collections: [Schema info]

Provide:
- Raw SQL queries
- ORM equivalent
- Explanation of query
- Index recommendations
- Performance considerations
- Example results

Optimize for:
- Query performance
- Readability
- Maintainability
```

#### Example:

**Prompt:**
```
Act as a Database Expert specializing in PostgreSQL and SQLAlchemy.

Create queries for an e-commerce analytics dashboard.

Queries Needed:
1. Top 10 selling products (last 30 days)
2. Revenue by category (this month vs last month)
3. Customer lifetime value (top 100 customers)
4. Cart abandonment rate
5. Average order value trend (last 6 months)

Schema:
- orders (id, user_id, total, status, created_at)
- order_items (id, order_id, product_id, quantity, price)
- products (id, name, category_id, price)
- users (id, email, created_at)

For each query provide:
1. Raw SQL (optimized)
2. SQLAlchemy ORM version
3. Expected execution time
4. Index recommendations
5. Explanation

Use CTEs, window functions where appropriate.
```

### API Development

**Prompt:**
```
Act as a Backend Developer expert in RESTful API design.

Create a complete REST API for [resource].

Endpoints:
- POST /[resource] - Create
- GET /[resource] - List (with pagination, filtering, sorting)
- GET /[resource]/{id} - Retrieve
- PUT /[resource]/{id} - Update
- PATCH /[resource]/{id} - Partial update
- DELETE /[resource]/{id} - Delete

Requirements:
- Input validation (Pydantic/JSON Schema)
- Authentication/Authorization
- Error handling
- Rate limiting
- CORS configuration
- API documentation (OpenAPI)
- Response pagination
- Filtering and search

Technical Stack:
- [FastAPI/Express/Django REST]
- [Database]
- [Authentication method]

Include:
- All endpoint implementations
- Request/response models
- Authentication middleware
- Error handlers
- OpenAPI documentation
- Example requests (curl)
- Unit and integration tests

Follow REST best practices.
```

### Frontend Components

**Prompt:**
```
Act as a Senior Frontend Developer expert in [React/Vue/Angular].

Create a [component name] component.

Requirements:
- [Functional requirement 1]
- [Functional requirement 2]

Features:
- Props/inputs
- State management
- Event handlers
- Validation
- Error handling
- Loading states
- Accessibility (ARIA)

Technical Specs:
- [Framework] + [version]
- TypeScript
- [State management library]
- [CSS framework]

Include:
- Component code
- PropTypes/Interface definitions
- Storybook stories
- Unit tests (Jest/React Testing Library)
- Usage example
- Documentation

Follow [framework] best practices and hooks patterns.
```

#### Example:

**Prompt:**
```
Act as a Senior React Developer.

Create a reusable Form Input component.

Requirements:
- Support text, email, password, number types
- Label and placeholder
- Validation (required, min/max length, pattern)
- Error message display
- Success state
- Disabled state
- Icon support (prefix/suffix)

Features:
- Controlled component
- TypeScript interfaces
- Custom validation rules
- Accessible (ARIA labels)
- Styled with Tailwind CSS
- Debounced validation for async checks

Include:
- FormInput.tsx component
- FormInput.types.ts interfaces
- FormInput.test.tsx tests
- FormInput.stories.tsx Storybook stories
- Usage examples
- Documentation

Follow React best practices and hooks.
```

### Performance Optimization

**Prompt:**
```
Act as a Performance Optimization Expert.

Optimize the following code for performance.

Current Code:
[Paste code]

Performance Issues:
- [Issue 1]
- [Issue 2]

Optimization Goals:
- Reduce time complexity
- Reduce memory usage
- Improve I/O operations
- Add caching where appropriate
- Use async operations

Provide:
1. Optimized code
2. Benchmark comparison (before/after)
3. Explanation of optimizations
4. Trade-offs (if any)
5. Profiling results

Include performance measurements and explain each optimization.
```

### Exercise 5.1: Code Generation and Refactoring

**Task 1: Generate a Function**
```
Create a function that:
- Validates credit card numbers (Luhn algorithm)
- Identifies card type (Visa, Mastercard, Amex)
- Checks expiry date
- Returns validation result with details
```

Your prompt:
```
[Write your prompt here]
```

**Task 2: Refactor Code**
```
Refactor this code to be more maintainable:

def calc(x, y, op):
    if op == '+':
        return x + y
    elif op == '-':
        return x - y
    elif op == '*':
        return x * y
    elif op == '/':
        return x / y if y != 0 else None
    else:
        return None
```

Your prompt:
```
[Write your prompt here]
```

**Task 3: Design Pattern**
```
Implement Observer Pattern for a notification system where:
- Multiple subscribers can register
- Publisher notifies all on event
- Subscribers can unsubscribe
- Support different event types
```

Your prompt:
```
[Write your prompt here]
```

### Best Practices

✅ **DO:**
- Specify language and version
- Request type hints/annotations
- Ask for error handling
- Request unit tests
- Specify coding standards
- Ask for documentation
- Request performance considerations

❌ **DON'T:**
- Use generated code without review
- Skip security considerations
- Ignore error handling
- Deploy without testing
- Overlook edge cases

### Code Review Checklist

- [ ] Code runs correctly?
- [ ] All edge cases handled?
- [ ] Error handling present?
- [ ] Security vulnerabilities?
- [ ] Performance acceptable?
- [ ] Well-documented?
- [ ] Tests included and passing?
- [ ] Follows best practices?
- [ ] Maintainable?
- [ ] No code smells?

### Key Takeaways

✅ AI accelerates development significantly
✅ Be specific about requirements and tech stack
✅ Always review and test generated code
✅ Use AI for boilerplate, customize for specifics
✅ Request tests and documentation
✅ Refactoring improves existing code
✅ Apply design patterns appropriately
✅ Consider performance and security

### Next Steps
- Move to [Module 7: AI Agents and Automation](../module-7-ai-agents/README.md) (or continue with Module 5 when Lesson 2 is available)
