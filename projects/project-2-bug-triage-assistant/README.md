# Project 2: AI-Powered Bug Triage Assistant

## Overview

Build an intelligent system that automatically analyzes bug reports, classifies them, assigns priority, and suggests solutions.

## Learning Objectives

- Natural language processing with AI
- Bug classification and analysis
- Building practical QA tools
- Working with structured and unstructured data
- Implementing AI decision-making logic

## Project Requirements

### Core Features

1. **Bug Report Analysis**
   - Parse bug reports (text, JSON, GitHub issues)
   - Extract key information (steps, expected/actual, environment)
   - Identify missing information

2. **Classification**
   - Categorize by type (bug, enhancement, question)
   - Classify severity (critical, high, medium, low)
   - Assign priority based on impact
   - Detect duplicates

3. **Root Cause Suggestion**
   - Analyze error logs
   - Search knowledge base
   - Suggest potential root causes
   - Recommend investigation steps

4. **Assignment Recommendation**
   - Suggest team/person to assign
   - Based on expertise and past issues
   - Consider workload

5. **Reporting**
   - Generate triage report
   - Create enhanced bug report
   - Export to bug tracking system

### Bonus Features

- Integration with GitHub Issues/Jira
- Sentiment analysis of bug reports
- Trend analysis
- Automated responses
- Slack/Teams notifications

## Technical Stack

- **Language:** Python 3.9+
- **AI:** OpenAI GPT-4 API
- **Web Framework:** Flask or FastAPI
- **Database:** SQLite or PostgreSQL
- **Testing:** pytest

## Project Structure

```
bug-triage-assistant/
├── src/
│   ├── __init__.py
│   ├── main.py
│   ├── bug_analyzer.py
│   ├── classifier.py
│   ├── duplicate_detector.py
│   ├── root_cause_analyzer.py
│   ├── assignment_suggester.py
│   ├── report_generator.py
│   └── integrations/
│       ├── github_integration.py
│       └── jira_integration.py
├── api/
│   ├── app.py
│   └── routes.py
├── web/
│   ├── static/
│   └── templates/
├── data/
│   ├── knowledge_base.json
│   └── historical_bugs.json
├── tests/
│   ├── test_analyzer.py
│   ├── test_classifier.py
│   └── test_detector.py
├── .env.example
├── requirements.txt
└── README.md
```

## Implementation Steps

### Step 1: Bug Analyzer

**Prompt for AI:**
```
Act as a Senior QA Engineer and Python developer.

Create a BugAnalyzer class that analyzes bug reports.

Methods:
- analyze_report(bug_text) → dict with extracted info
- extract_steps_to_reproduce() → list
- extract_expected_actual() → tuple
- extract_environment() → dict
- identify_missing_info() → list
- enhance_report() → enhanced bug report

The analyzer should:
- Use OpenAI to parse unstructured text
- Handle various bug report formats
- Extract structured data
- Identify incomplete information

Include:
- Complete class implementation
- Prompts for AI
- Error handling
- Type hints and docstrings
- Unit tests
```

**Example Input:**
```
Title: Login button not working

Description:
I tried to login but nothing happened when I clicked the button.
Using Chrome on Windows 10.

Steps:
1. Go to login page
2. Enter username and password  
3. Click login
```

**Expected Output:**
```json
{
  "title": "Login button not working",
  "severity": "high",
  "priority": "high",
  "type": "bug",
  "steps": [
    "Navigate to login page",
    "Enter valid username and password",
    "Click login button"
  ],
  "expected": "User should be logged in and redirected to dashboard",
  "actual": "Login button does not respond to clicks",
  "environment": {
    "browser": "Chrome",
    "os": "Windows 10"
  },
  "missing_info": [
    "Chrome version",
    "Error messages in console",
    "Network requests status",
    "Reproducibility rate"
  ]
}
```

### Step 2: Classifier

**Prompt for AI:**
```
Act as a Senior QA Engineer and ML expert.

Create a BugClassifier that classifies bugs.

Classification Categories:
1. Type: bug, enhancement, question, documentation
2. Severity: critical, high, medium, low
3. Priority: p0, p1, p2, p3
4. Category: UI, Backend, Database, API, Performance, Security

Methods:
- classify_type(bug_report)
- determine_severity(bug_report)
- determine_priority(bug_report, severity)
- categorize(bug_report)
- classify_all(bug_report) → complete classification

Use AI to:
- Analyze bug impact
- Consider user experience
- Evaluate business impact
- Apply classification rules

Include:
- Classification logic
- Confidence scores
- Explanation of classification
- Tests with examples
```

### Step 3: Duplicate Detector

**Prompt for AI:**
```
Act as a Python developer expert in similarity algorithms.

Create a DuplicateDetector that finds duplicate bug reports.

Methods:
- find_duplicates(new_bug, historical_bugs) → list of similar bugs
- calculate_similarity(bug1, bug2) → similarity score
- extract_features(bug) → feature vector

Techniques:
- Text similarity (TF-IDF, cosine similarity)
- AI-based semantic similarity
- Error message matching
- Stack trace comparison

Include:
- Complete implementation
- Multiple similarity metrics
- Configurable threshold
- Detailed comparison report
- Unit tests
```

### Step 4: Root Cause Analyzer

**Prompt for AI:**
```
Act as a Senior Software Engineer and debugging expert.

Create a RootCauseAnalyzer that suggests root causes.

Methods:
- analyze_error(error_message, stack_trace)
- suggest_root_cause(bug_report)
- find_similar_issues(bug_report)
- recommend_investigation_steps()

The analyzer should:
- Parse error messages and stack traces
- Search knowledge base for similar issues
- Use AI to suggest likely causes
- Provide investigation checklist
- Link to documentation

Include:
- Analysis logic
- Knowledge base integration
- AI prompts
- Investigation recommendations
- Tests
```

### Step 5: Web API

**Prompt for AI:**
```
Act as a Backend Developer expert in FastAPI.

Create a REST API for the bug triage assistant.

Endpoints:
- POST /api/bugs/analyze - Analyze bug report
- POST /api/bugs/classify - Classify bug
- GET /api/bugs/duplicates/{bug_id} - Find duplicates
- POST /api/bugs/triage - Complete triage
- GET /api/bugs/stats - Triage statistics

Features:
- Request validation (Pydantic)
- Error handling
- Rate limiting
- API documentation (OpenAPI)
- CORS support

Include:
- Complete API implementation
- Request/response models
- Error handlers
- Documentation
- Example requests
```

### Step 6: Web Interface

**Prompt for AI:**
```
Act as a Full-Stack Developer.

Create a web interface for the bug triage assistant.

Pages:
1. Bug Submission Form
2. Triage Results Display
3. Duplicate Detection Results
4. Historical Bug Search
5. Statistics Dashboard

Features:
- Responsive design
- Real-time analysis
- Interactive results
- Export functionality
- Dark mode

Technologies:
- HTML/CSS/JavaScript
- Bootstrap or Tailwind CSS
- Chart.js for visualizations
- Fetch API for backend calls

Include:
- Complete HTML/CSS/JS
- Modern, clean UI
- Accessibility features
- Mobile responsive
```

## Example Usage

### CLI Usage

```bash
# Analyze single bug
python src/main.py analyze --input bug_report.txt

# Batch process
python src/main.py batch --input bugs/ --output results/

# Find duplicates
python src/main.py duplicates --bug-id 123 --threshold 0.8
```

### API Usage

```bash
# Analyze bug via API
curl -X POST http://localhost:8000/api/bugs/analyze \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Login button not working",
    "description": "User cannot login...",
    "steps": ["Navigate to login", "Click button"]
  }'

# Get triage report
curl -X POST http://localhost:8000/api/bugs/triage \
  -H "Content-Type: application/json" \
  -d @bug_report.json
```

### Python Library Usage

```python
from bug_triage import BugTriageAssistant

assistant = BugTriageAssistant(api_key="your_key")

# Analyze bug
result = assistant.analyze_bug(bug_report)

print(f"Severity: {result['severity']}")
print(f"Priority: {result['priority']}")
print(f"Suggested Owner: {result['suggested_owner']}")
print(f"Root Cause: {result['root_cause_suggestions']}")
print(f"Duplicates: {result['potential_duplicates']}")
```

## Sample Bug Reports

**File: `data/sample_bugs.json`**

```json
[
  {
    "id": 1,
    "title": "Application crashes on startup",
    "description": "The app crashes immediately after opening. Getting error code 0x80004005.",
    "reporter": "user@example.com",
    "date": "2024-01-15",
    "environment": {
      "os": "Windows 11",
      "version": "2.1.0"
    }
  },
  {
    "id": 2,
    "title": "Cannot upload files larger than 5MB",
    "description": "When trying to upload files over 5MB, getting timeout error. Works fine with smaller files.",
    "steps": [
      "Go to upload page",
      "Select file > 5MB",
      "Click upload",
      "Wait 2 minutes",
      "Get timeout error"
    ]
  }
]
```

## Testing

```bash
# Run all tests
pytest tests/ -v

# Test with sample data
python src/main.py analyze --input data/sample_bugs.json

# Run web server
python api/app.py

# Access at http://localhost:8000
```

## Evaluation Criteria

### Functionality (40%)
- [ ] Accurate bug analysis
- [ ] Correct classification
- [ ] Effective duplicate detection
- [ ] Useful root cause suggestions
- [ ] Working web interface

### AI Integration (25%)
- [ ] Effective prompts
- [ ] Good AI responses
- [ ] Appropriate use of AI
- [ ] Fallback for AI failures

### Code Quality (20%)
- [ ] Clean, maintainable code
- [ ] Error handling
- [ ] Documentation
- [ ] Type hints

### User Experience (15%)
- [ ] Intuitive interface
- [ ] Clear results
- [ ] Helpful outputs
- [ ] Good performance

## Advanced Challenges

1. **Machine Learning:** Train custom classifier on historical bugs
2. **Integration:** Connect to Jira/GitHub/GitLab
3. **Automation:** Auto-triage and assign bugs
4. **Analytics:** Bug trends and insights dashboard
5. **Notifications:** Slack/Teams integration
6. **Multi-language:** Support bug reports in multiple languages

## Submission

Submit:
1. Complete source code
2. README with setup and usage
3. Sample data and outputs
4. API documentation
5. Demo video or screenshots

## Resources

- [OpenAI API Docs](https://platform.openai.com/docs)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [scikit-learn for ML](https://scikit-learn.org/)

## Next Steps

- Move to Project 3: Code Review Bot
- Deploy your application
- Share with your QA team!
