# Module 1: Introduction to Prompt Engineering

## Lesson 3: Setting Up Your Environment

### Prerequisites

Before starting this course, ensure you have:
- Computer with internet connection
- Text editor or IDE (VS Code recommended)
- Basic understanding of testing or programming
- Curiosity and willingness to learn!

### Required Tools Setup

#### 1. **ChatGPT Account**

**Free Account:**
- Visit: https://chat.openai.com
- Sign up with email or Google account
- Access to GPT-3.5 (sufficient for learning)

**ChatGPT Plus ($20/month):**
- Access to GPT-4
- Faster response times
- Priority access
- Advanced features

**Setup Steps:**
```
1. Go to chat.openai.com
2. Click "Sign up"
3. Enter email and create password
4. Verify email
5. Start chatting!
```

#### 2. **API Access (Optional but Recommended)**

**OpenAI API:**
```bash
# Install OpenAI Python library
pip install openai

# Set up API key
export OPENAI_API_KEY='your-api-key-here'
```

**Example Python Script:**
```python
import openai
import os

openai.api_key = os.getenv("OPENAI_API_KEY")

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "system", "content": "You are a helpful QA assistant."},
        {"role": "user", "content": "Generate 5 test cases for a login page"}
    ]
)

print(response.choices[0].message.content)
```

#### 3. **Development Environment**

**VS Code (Recommended):**
```bash
# Download from: https://code.visualstudio.com

# Recommended Extensions:
- Python
- Prettier
- GitLens
- REST Client
- GitHub Copilot (optional, paid)
```

**Python Setup:**
```bash
# Check Python version
python --version

# Should be Python 3.8+
# If not installed, download from python.org

# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# Install testing libraries
pip install pytest selenium requests
```

#### 4. **Testing Tools**

**For Automation Testers:**
```bash
# Selenium
pip install selenium

# Download WebDriver (Chrome)
# Visit: https://chromedriver.chromium.org

# API Testing
pip install requests pytest

# Framework
pip install pytest-html pytest-xdist
```

**For API Testing:**
```bash
# Postman (GUI)
# Download from: https://www.postman.com

# Or use REST Client in VS Code
# Install REST Client extension
```

#### 5. **Version Control**

**Git Setup:**
```bash
# Check if git is installed
git --version

# Configure git
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Create GitHub account
# Visit: https://github.com
```

### Workspace Organization

**Recommended Folder Structure:**
```
prompt-engineering-course/
│
├── exercises/
│   ├── module-1/
│   ├── module-2/
│   └── ...
│
├── projects/
│   ├── test-case-generator/
│   ├── automation-scripts/
│   └── code-review-bot/
│
├── notes/
│   └── my-learnings.md
│
├── prompts/
│   ├── testing-prompts.md
│   └── development-prompts.md
│
└── resources/
    └── useful-links.md
```

**Create Your Workspace:**
```bash
mkdir prompt-engineering-course
cd prompt-engineering-course

mkdir -p exercises/{module-1,module-2,module-3}
mkdir -p projects
mkdir notes prompts resources

# Initialize git
git init

# Create README
echo "# My Prompt Engineering Journey" > README.md
```

### Prompt Template Library

Create a file to store your reusable prompts:

**File: `prompts/template-library.md`**
```markdown
# Prompt Template Library

## Test Case Generation
```
Act as a Senior QA Engineer. Generate test cases for [FEATURE] with these requirements:
[LIST REQUIREMENTS]

Include:
- Positive test cases
- Negative test cases
- Edge cases
- Boundary value analysis

Format as a table with: Test ID, Description, Steps, Expected Result, Priority
```

## Code Review
```
Review this [LANGUAGE] code for:
- Bugs and errors
- Performance issues
- Security vulnerabilities
- Best practices
- Code readability

[CODE HERE]

Provide:
1. Issues found (with severity)
2. Specific recommendations
3. Improved code snippets
```

## Bug Report
```
Create a detailed bug report for the following issue:

**Observed:** [WHAT HAPPENED]
**Expected:** [WHAT SHOULD HAPPEN]
**Steps:** [HOW TO REPRODUCE]
**Environment:** [BROWSER/OS/VERSION]

Include:
- Severity/Priority classification
- Suggested root cause
- Recommended fix
- Related test cases
```
```

### Verification Checklist

Before proceeding, verify:

✅ ChatGPT account is active and working
✅ Text editor or IDE is installed
✅ Python environment is set up (if doing automation)
✅ Git is configured
✅ Workspace folders are created
✅ You can access the course materials
✅ (Optional) API access is configured

### Exercise 3.1: Environment Test

Run this test to verify your setup:

**Step 1: Test ChatGPT**
Open ChatGPT and enter:
```
Hello! I'm setting up for a prompt engineering course. 
Can you confirm you can help me with:
1. Test case generation
2. Code review
3. Automation script creation
```

**Step 2: Test Python (if applicable)**
```python
# test_setup.py
import sys

def test_environment():
    print(f"Python version: {sys.version}")
    
    try:
        import pytest
        print("✅ pytest installed")
    except ImportError:
        print("❌ pytest not installed")
    
    try:
        import selenium
        print("✅ selenium installed")
    except ImportError:
        print("❌ selenium not installed")
    
    try:
        import requests
        print("✅ requests installed")
    except ImportError:
        print("❌ requests not installed")
    
    print("\n🎉 Setup verification complete!")

if __name__ == "__main__":
    test_environment()
```

Run:
```bash
python test_setup.py
```

**Step 3: Test Git**
```bash
git --version
git config --list
```

### Troubleshooting

**Issue: Python not found**
```bash
# Solution: Install Python from python.org
# Verify installation:
python --version
```

**Issue: pip not working**
```bash
# Solution: Use python -m pip
python -m pip install --upgrade pip
```

**Issue: ChatGPT not responding**
```bash
# Solution: 
- Check internet connection
- Try different browser
- Clear browser cache
- Wait a few minutes (might be rate limited)
```

**Issue: API key not working**
```bash
# Solution:
- Verify API key is correct
- Check billing is set up on OpenAI platform
- Ensure key has proper permissions
```

### Additional Resources

📚 **Documentation:**
- OpenAI API Docs: https://platform.openai.com/docs
- Python Testing: https://docs.pytest.org
- Selenium Docs: https://www.selenium.dev/documentation/

🎥 **Video Tutorials:**
- Setting up Python: YouTube search "Python setup [your OS]"
- ChatGPT basics: OpenAI YouTube channel

💬 **Communities:**
- r/ChatGPT on Reddit
- OpenAI Community Forum
- Stack Overflow

### Key Takeaways

✅ Proper setup is crucial for effective learning
✅ You don't need all tools immediately - start simple
✅ API access is optional but recommended for automation
✅ Organized workspace helps maintain progress
✅ Keep a prompt library for reusable templates

### Next Steps
- Verify your environment is working
- Create your workspace folders
- Start a prompt template library
- Move to [Module 2: Fundamentals of Prompt Engineering](../module-2-fundamentals/README.md)
