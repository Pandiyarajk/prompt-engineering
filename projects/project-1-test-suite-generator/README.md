# Project 1: Automated Test Suite Generator

## Overview

Build an AI-powered tool that generates complete test suites from requirements documents.

## Learning Objectives

- Apply prompt engineering for test generation
- Create reusable prompt templates
- Build a CLI tool
- Integrate OpenAI API
- Generate multiple test artifacts

## Project Requirements

### Core Features

1. **Input Processing**
   - Accept requirements in multiple formats (text, markdown, JSON)
   - Parse and extract key features
   - Identify user workflows

2. **Test Case Generation**
   - Generate functional test cases
   - Generate negative test cases
   - Generate edge case scenarios
   - Generate security test cases

3. **Multiple Output Formats**
   - Markdown tables
   - CSV files
   - JSON
   - Gherkin (BDD)

4. **Coverage Analysis**
   - Map test cases to requirements
   - Identify coverage gaps
   - Suggest additional test scenarios

### Bonus Features

- Generate automation code (Selenium/Playwright)
- Create test data
- Generate API test collections (Postman/Insomnia)
- Export to test management tools

## Technical Stack

- **Language:** Python 3.9+
- **AI:** OpenAI GPT-4 API
- **CLI:** Click or argparse
- **File Handling:** CSV, JSON, Markdown
- **Testing:** pytest

## Project Structure

```
test-suite-generator/
├── src/
│   ├── __init__.py
│   ├── main.py                 # CLI entry point
│   ├── requirement_parser.py   # Parse requirements
│   ├── test_generator.py       # Generate test cases
│   ├── coverage_analyzer.py    # Analyze coverage
│   ├── output_formatter.py     # Format outputs
│   └── prompts/
│       ├── test_case_prompts.py
│       └── templates.py
├── tests/
│   ├── test_parser.py
│   ├── test_generator.py
│   └── test_formatter.py
├── examples/
│   ├── sample_requirements.md
│   ├── sample_output.csv
│   └── sample_output.json
├── .env.example
├── requirements.txt
├── README.md
└── setup.py
```

## Implementation Steps

### Step 1: Setup Project

```bash
mkdir test-suite-generator
cd test-suite-generator
python -m venv venv
source venv/bin/activate  # or venv\Scripts\activate on Windows
pip install openai click python-dotenv pytest
```

Create `.env`:
```
OPENAI_API_KEY=your_api_key_here
```

### Step 2: Create Requirement Parser

**Prompt for AI:**
```
Act as a Python developer.

Create a RequirementParser class that:
- Reads requirements from text/markdown/JSON files
- Extracts features and user stories
- Identifies acceptance criteria
- Returns structured data

Include:
- Class implementation
- Error handling
- Type hints
- Docstrings
- Unit tests
```

**File: `src/requirement_parser.py`**

### Step 3: Create Test Generator

**Prompt for AI:**
```
Act as a Senior QA Engineer and Python developer.

Create a TestGenerator class that uses OpenAI API to generate test cases.

Methods:
- generate_functional_tests(feature)
- generate_negative_tests(feature)
- generate_edge_cases(feature)
- generate_security_tests(feature)

Each method should:
- Use appropriate prompts for test type
- Call OpenAI API
- Parse and structure response
- Return list of test case objects

Include:
- Class implementation
- Prompt templates
- Error handling
- Rate limiting
- Unit tests with mocked API
```

**File: `src/test_generator.py`**

### Step 4: Create Output Formatter

**Prompt for AI:**
```
Act as a Python developer.

Create an OutputFormatter class that formats test cases to:
- Markdown table
- CSV file
- JSON file
- Gherkin/BDD format

Include:
- All format methods
- File writing capability
- Pretty formatting
- Validation
- Unit tests
```

**File: `src/output_formatter.py`**

### Step 5: Create CLI

**Prompt for AI:**
```
Act as a Python developer expert in Click library.

Create a CLI tool that:

Commands:
- generate: Generate test suite from requirements
- analyze: Analyze coverage
- export: Export to different formats

Options:
- --input: Input requirements file
- --output: Output directory
- --format: Output format (md/csv/json/gherkin)
- --test-types: Types of tests to generate
- --api-key: OpenAI API key (or from .env)

Include:
- Complete CLI implementation
- Help messages
- Error handling
- Progress indicators
- Examples in README
```

**File: `src/main.py`**

### Step 6: Create Coverage Analyzer

**Prompt for AI:**
```
Act as a QA Engineer and Python developer.

Create a CoverageAnalyzer class that:
- Maps test cases to requirements
- Identifies untested requirements
- Calculates coverage percentage
- Suggests missing test scenarios
- Generates coverage report

Include:
- Class implementation
- Coverage calculation logic
- Report generation
- Visualization (text-based)
- Unit tests
```

**File: `src/coverage_analyzer.py`**

## Example Usage

```bash
# Generate test suite
python src/main.py generate \
  --input examples/requirements.md \
  --output output/ \
  --format json \
  --test-types functional,negative,edge

# Analyze coverage
python src/main.py analyze \
  --requirements examples/requirements.md \
  --test-suite output/test_suite.json

# Export to different format
python src/main.py export \
  --input output/test_suite.json \
  --output output/test_suite.csv \
  --format csv
```

## Sample Requirements File

**File: `examples/sample_requirements.md`**

```markdown
# User Registration Feature

## Overview
Users can create a new account on the platform.

## Requirements

### REQ-001: Registration Form
- User must provide: email, password, first name, last name
- All fields are required
- Email must be unique in the system

### REQ-002: Email Validation
- Email must be valid format (user@domain.com)
- Check for common typos (gmial → gmail)
- Prevent disposable email addresses

### REQ-003: Password Requirements
- Minimum 8 characters
- At least 1 uppercase letter
- At least 1 number
- At least 1 special character
- Cannot be same as email

### REQ-004: Verification
- Send verification email upon registration
- User must verify email within 24 hours
- Unverified users cannot login
- Resend verification option available

### REQ-005: Error Handling
- Display clear error messages
- Highlight invalid fields
- Prevent duplicate submissions
- Handle server errors gracefully
```

## Testing the Project

```bash
# Run all tests
pytest tests/ -v

# Run specific test
pytest tests/test_generator.py -v

# Run with coverage
pytest tests/ --cov=src --cov-report=html
```

## Evaluation Criteria

### Functionality (40%)
- [ ] Correctly parses requirements
- [ ] Generates comprehensive test cases
- [ ] Multiple output formats work
- [ ] Coverage analysis accurate

### Code Quality (30%)
- [ ] Clean, readable code
- [ ] Proper error handling
- [ ] Type hints used
- [ ] Well-documented

### Prompt Engineering (20%)
- [ ] Effective prompts
- [ ] Appropriate temperature settings
- [ ] Good result quality
- [ ] Handles edge cases

### Testing (10%)
- [ ] Unit tests present
- [ ] Good test coverage
- [ ] Tests pass

## Advanced Challenges

1. **Add GUI:** Create a web interface using Flask/FastAPI
2. **Integration:** Export to Jira/TestRail
3. **Automation Code:** Generate Selenium/Playwright code
4. **AI Refinement:** Iteratively improve test cases based on feedback
5. **Collaboration:** Multi-user support with version control

## Submission

Submit:
1. Complete source code
2. README with setup instructions
3. Example inputs and outputs
4. Test coverage report
5. Demo video (optional)

## Resources

- [OpenAI API Docs](https://platform.openai.com/docs)
- [Click Documentation](https://click.palletsprojects.com/)
- [pytest Documentation](https://docs.pytest.org/)

## Next Steps

After completing this project:
- Move to Project 2: Bug Triage Assistant
- Enhance with more advanced features
- Share with the community!
