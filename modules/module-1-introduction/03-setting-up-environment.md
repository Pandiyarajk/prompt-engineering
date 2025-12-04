# Module 1: Introduction to Prompt Engineering

## Lesson 3: Setting Up Your Environment

---

## 🎯 Lesson Overview

**Time Required:** 30-60 minutes  
**Difficulty:** Beginner  
**Prerequisites:** Lessons 1 and 2 completed

In this lesson, you'll:
- Set up your ChatGPT account
- Configure your development environment (optional but recommended)
- Learn how to use ChatGPT effectively
- Create your first prompt library
- Verify everything is working

---

## 📋 What You'll Need

### Minimum Requirements (Web-Based Learning)
- ✅ Computer with internet access
- ✅ Modern web browser (Chrome, Firefox, Safari, Edge)
- ✅ Email address for account registration

### Recommended (For Hands-On Projects)
- ✅ Text editor or IDE (VS Code recommended)
- ✅ Python 3.9+ (for coding exercises)
- ✅ Git (for version control)

### Optional (For Advanced Use)
- OpenAI API key (for programmatic access)
- GitHub Copilot subscription

---

## 🚀 Setup Option 1: Web-Based (Quick Start)

**Perfect for:** Beginners, learning the basics, trying things out

### Step 1: Create ChatGPT Account

1. **Go to** [https://chat.openai.com](https://chat.openai.com)

2. **Click "Sign up"**

3. **Choose sign-up method:**
   - Email and password
   - Google account
   - Microsoft account
   - Apple account

4. **Verify your email** (if using email signup)

5. **Complete profile setup**

**That's it! You're ready to use ChatGPT! 🎉**

### Step 2: Navigate the ChatGPT Interface

```
┌─────────────────────────────────────────────────────────────────┐
│  CHATGPT INTERFACE OVERVIEW                                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────┐   ┌────────────────────────────────────────────┐ │
│  │          │   │                                            │ │
│  │  Sidebar │   │                                            │ │
│  │          │   │           Chat Area                        │ │
│  │ • New    │   │                                            │ │
│  │   Chat   │   │   Your conversations appear here          │ │
│  │          │   │                                            │ │
│  │ • Recent │   │                                            │ │
│  │   Chats  │   │                                            │ │
│  │          │   │                                            │ │
│  │          │   ├────────────────────────────────────────────┤ │
│  │          │   │  Message input box                        │ │
│  │          │   │  [Type your prompt here...]         [→]   │ │
│  └──────────┘   └────────────────────────────────────────────┘ │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Key areas:**
- **Sidebar:** Create new chats, access history
- **Chat Area:** Where conversations happen
- **Input Box:** Where you type prompts
- **Model Selector:** Choose between models (if available)

### Step 3: Your First Chat

**Try this now:**

1. Click "New Chat"
2. Type this prompt:

```
Hello! I'm learning prompt engineering for software testing 
and development. Can you help me practice?
```

3. Press Enter or click the send button
4. Read the response

**Congratulations!** You've just had your first AI conversation!

---

## 💻 Setup Option 2: Full Development Environment

**Perfect for:** Hands-on projects, automation, coding exercises

### Step 1: Install Python

**Check if Python is installed:**

```bash
python --version
# or
python3 --version
```

**Expected output:** `Python 3.9.x` or higher

**If not installed:**
- **Windows/Mac:** Download from [python.org](https://www.python.org/downloads/)
- **Linux:** `sudo apt install python3 python3-pip`

### Step 2: Install VS Code

1. Download from [code.visualstudio.com](https://code.visualstudio.com/)
2. Install with default settings
3. Open VS Code

**Recommended Extensions:**
- Python (Microsoft)
- Prettier - Code Formatter
- GitLens
- REST Client

**To install extensions:**
1. Click Extensions icon (left sidebar)
2. Search for extension name
3. Click "Install"

### Step 3: Set Up Project Folder

**Create your workspace:**

```bash
# Create main folder
mkdir prompt-engineering-course
cd prompt-engineering-course

# Create subfolders
mkdir -p exercises/{module-1,module-2,module-3,module-4,module-5}
mkdir -p projects/{test-suite-generator,bug-triage-assistant}
mkdir notes
mkdir prompts

# Create initial files
touch README.md
touch notes/my-learnings.md
touch prompts/my-templates.md
```

**Your folder structure:**

```
prompt-engineering-course/
├── README.md
├── exercises/
│   ├── module-1/
│   ├── module-2/
│   ├── module-3/
│   ├── module-4/
│   └── module-5/
├── projects/
│   ├── test-suite-generator/
│   └── bug-triage-assistant/
├── notes/
│   └── my-learnings.md
└── prompts/
    └── my-templates.md
```

### Step 4: Set Up Python Virtual Environment

```bash
# Navigate to your project folder
cd prompt-engineering-course

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On macOS/Linux:
source venv/bin/activate

# On Windows:
venv\Scripts\activate
```

**You'll see `(venv)` in your terminal when activated.**

### Step 5: Install Python Packages

```bash
# Install essential packages
pip install openai python-dotenv pytest requests

# Install testing packages
pip install selenium playwright pytest-html

# Save requirements
pip freeze > requirements.txt
```

**Alternatively, create `requirements.txt` with:**

```
openai>=1.0.0
python-dotenv>=1.0.0
pytest>=7.0.0
requests>=2.31.0
selenium>=4.0.0
pytest-html>=4.0.0
```

Then install with:
```bash
pip install -r requirements.txt
```

---

## 🔑 Optional: Set Up OpenAI API Access

**Why get API access?**
- Programmatic access to AI
- Build automation tools
- Create custom applications
- Run bulk operations

### Step 1: Get API Key

1. Go to [platform.openai.com](https://platform.openai.com)
2. Sign in (or create account)
3. Navigate to API Keys section
4. Click "Create new secret key"
5. **Copy and save the key immediately** (shown only once!)

### Step 2: Configure API Key

**Create `.env` file:**

```bash
# In your project folder
touch .env
```

**Add your key to `.env`:**

```
OPENAI_API_KEY=sk-your-key-here
```

**⚠️ IMPORTANT SECURITY RULES:**
- ❌ Never commit `.env` to Git
- ❌ Never share your API key
- ❌ Never put keys in code
- ✅ Add `.env` to `.gitignore`

**Create `.gitignore`:**

```
# .gitignore
.env
venv/
__pycache__/
*.pyc
.DS_Store
```

### Step 3: Test API Connection

**Create `test_api.py`:**

```python
#!/usr/bin/env python3
"""Test OpenAI API connection."""

import os
from dotenv import load_dotenv
from openai import OpenAI

# Load environment variables
load_dotenv()

def test_connection():
    """Test API connection with a simple prompt."""
    
    # Initialize client
    client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
    
    try:
        # Make a simple request
        response = client.chat.completions.create(
            model="gpt-3.5-turbo",
            messages=[
                {"role": "system", "content": "You are a helpful assistant."},
                {"role": "user", "content": "Say 'API connection successful!' if you can read this."}
            ],
            max_tokens=50
        )
        
        # Print response
        print("✅ " + response.choices[0].message.content)
        print("\n✅ API connection test passed!")
        return True
        
    except Exception as e:
        print(f"❌ Connection failed: {e}")
        return False

if __name__ == "__main__":
    test_connection()
```

**Run the test:**

```bash
python test_api.py
```

**Expected output:**
```
✅ API connection successful!

✅ API connection test passed!
```

---

## 📝 Create Your Prompt Library

A prompt library is essential for efficiency. Start building yours now!

### Create `prompts/my-templates.md`:

```markdown
# My Prompt Engineering Library

## 📋 Table of Contents
1. [Test Case Generation](#test-case-generation)
2. [Code Generation](#code-generation)
3. [Code Review](#code-review)
4. [Bug Analysis](#bug-analysis)

---

## Test Case Generation

### Template 1: Basic Test Cases

**Use when:** You need functional test cases for any feature

```
Act as a Senior QA Engineer with expertise in [DOMAIN].

Feature: [FEATURE NAME]

Requirements:
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

Generate test cases including:
- Positive scenarios (happy path)
- Negative scenarios (error handling)
- Edge cases (boundary conditions)

Format as a table with columns:
| Test ID | Description | Steps | Expected Result | Priority |

Constraints:
- [Any specific constraints]
```

### Template 2: BDD/Gherkin Format

**Use when:** You need test cases in Given-When-Then format

```
Act as a BDD Testing Expert.

Convert these requirements into Gherkin format:
[REQUIREMENTS]

Create scenarios for:
- Happy path
- Error scenarios
- Edge cases

Format each scenario as:
Feature: [Feature Name]
  Scenario: [Scenario Name]
    Given [precondition]
    When [action]
    Then [expected result]
```

---

## Code Generation

### Template 3: Function Generation

**Use when:** You need to create a new function

```
Act as a Senior [LANGUAGE] Developer.

Create a function to: [PURPOSE]

Requirements:
- [Requirement 1]
- [Requirement 2]

Include:
- Type hints/annotations
- Docstring with examples
- Error handling
- Unit tests

Follow [CODING STANDARD] standards.
```

### Template 4: Test Automation Code

**Use when:** You need automation test scripts

```
Act as a Test Automation Engineer expert in [FRAMEWORK] with [LANGUAGE].

Create an automation script for: [TEST SCENARIO]

Page elements:
- [Element 1]: [Locator 1]
- [Element 2]: [Locator 2]

Include:
- Page Object Model pattern
- Explicit waits
- Error handling
- Assertions
- Comments

Follow best practices for [FRAMEWORK].
```

---

## Code Review

### Template 5: Code Review Request

**Use when:** You need code reviewed

```
Act as a Senior Software Engineer and Code Reviewer.

Review this [LANGUAGE] code for:
1. Code quality and readability
2. Best practices
3. Security vulnerabilities
4. Performance issues
5. Error handling

Code:
[PASTE CODE HERE]

For each issue found, provide:
- Severity (Critical/High/Medium/Low)
- Location
- Problem description
- Recommended fix
- Example code
```

---

## Bug Analysis

### Template 6: Bug Analysis

**Use when:** You need help analyzing an error

```
Act as a Debugging Expert.

I'm getting this error:
[ERROR MESSAGE]

Stack trace:
[STACK TRACE]

Context:
- Language/Framework: [TECH]
- What I was doing: [ACTION]

Please:
1. Explain what the error means
2. Identify likely causes
3. Suggest how to fix it
4. Provide example code
```

---

## My Custom Prompts

### [Add your own prompts here as you learn!]

```
[Your custom prompt]
```

---

*Last updated: [DATE]*
```

---

## ✅ Verification Checklist

### Web-Based Setup (Required)

Run through this checklist:

- [ ] ChatGPT account created
- [ ] Can log in successfully
- [ ] Can create new chat
- [ ] Successfully sent first message
- [ ] Received response from ChatGPT

### Development Environment (Optional)

- [ ] Python 3.9+ installed
- [ ] VS Code installed
- [ ] Project folder created
- [ ] Virtual environment working
- [ ] Required packages installed
- [ ] (If using API) API key configured
- [ ] (If using API) Test connection successful

---

## 🧪 Final Exercise: Environment Verification

### Exercise 3.1: ChatGPT Functionality Test

**Try these prompts to verify your setup:**

**Prompt 1: Simple Test**
```
What is 2 + 2?
```
*Expected: ChatGPT answers "4"*

**Prompt 2: QA Role Test**
```
Act as a QA Engineer. List 3 important things to test for a login form.
```
*Expected: ChatGPT provides relevant testing points*

**Prompt 3: Code Generation Test**
```
Act as a Python developer. Write a function that checks if a string is a valid email address. Include a brief docstring.
```
*Expected: ChatGPT generates Python code with documentation*

**Prompt 4: Format Test**
```
Create a comparison table of manual testing vs automated testing with columns: Aspect, Manual, Automated
```
*Expected: ChatGPT creates a formatted markdown table*

### Exercise 3.2: Create Your First Prompt Template

**Task:** Create a custom prompt template for your most common task.

1. Think of a task you do frequently (e.g., writing test cases, reviewing code)
2. Use the 4-element formula (Role + Context + Task + Format)
3. Add it to your `prompts/my-templates.md` file

**Template:**
```
Prompt Name: [Name]
Use When: [When to use this prompt]

---
[Your prompt here]
---

Example Usage:
[Show an example of using this prompt]
```

---

## 🔧 Troubleshooting Common Issues

### Problem: "ChatGPT not responding"

**Solutions:**
1. Check internet connection
2. Refresh the page
3. Try a different browser
4. Clear browser cache
5. Wait a few minutes (might be at capacity)

### Problem: "Python not found"

**Solutions:**
```bash
# Try different commands
python --version
python3 --version
py --version

# If still not found, reinstall Python
# Make sure to check "Add Python to PATH" during installation
```

### Problem: "pip command not found"

**Solutions:**
```bash
# Try these alternatives
pip3 install package_name
python -m pip install package_name
python3 -m pip install package_name
```

### Problem: "Module not found" error

**Solutions:**
```bash
# Make sure virtual environment is activated
# You should see (venv) in your terminal

# Reinstall the package
pip install package_name

# Or reinstall all packages
pip install -r requirements.txt
```

### Problem: "API key invalid"

**Solutions:**
1. Check key starts with `sk-`
2. Verify key in `.env` file has no extra spaces
3. Regenerate key in OpenAI dashboard
4. Check you have API credits

---

## 📚 Additional Resources

### Official Documentation
- [OpenAI Documentation](https://platform.openai.com/docs)
- [ChatGPT Best Practices](https://help.openai.com)
- [Python Documentation](https://docs.python.org)

### Learning Resources
- [VS Code Tutorial](https://code.visualstudio.com/docs)
- [Python Beginner Guide](https://www.python.org/about/gettingstarted/)
- [Git Basics](https://git-scm.com/book/en/v2)

### Community
- [OpenAI Community Forum](https://community.openai.com)
- [r/ChatGPT on Reddit](https://reddit.com/r/ChatGPT)
- [Stack Overflow](https://stackoverflow.com)

---

## 📝 Key Takeaways

1. **ChatGPT account** is essential - create it first!
2. **Development environment** is optional but recommended for projects
3. **Organize your work** with a proper folder structure
4. **Keep a prompt library** - saves time and improves consistency
5. **API access** is optional but useful for automation
6. **Security** - never share or commit API keys
7. **Practice** - use the verification exercises to confirm setup

---

## ✅ Module 1 Complete! 🎉

Congratulations! You've finished Module 1. You now:

- ✅ Understand what prompt engineering is
- ✅ Know how AI models like ChatGPT work
- ✅ Have your environment set up and ready
- ✅ Can write basic prompts
- ✅ Have started your prompt library

---

## 🔗 Next Steps

You're ready for Module 2! 

**Continue to:** [Module 2: Fundamentals of Prompt Engineering](../module-2-fundamentals/README.md)

In Module 2, you'll learn:
- The 5-component prompt structure
- Temperature and other parameters
- Best practices for optimal results
- Advanced prompt patterns

---

## 📖 Quick Reference Card

```
┌─────────────────────────────────────────────────────────────────┐
│  MODULE 1 QUICK REFERENCE                                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  PROMPT FORMULA:                                                 │
│  Role + Context + Task + Format = Effective Prompt               │
│                                                                  │
│  CHATGPT URL: https://chat.openai.com                           │
│                                                                  │
│  KEY SHORTCUTS:                                                  │
│  • New Chat: Ctrl/Cmd + Shift + O                               │
│  • Send Message: Enter                                          │
│  • New Line: Shift + Enter                                      │
│                                                                  │
│  BEGINNER TIP:                                                   │
│  Start simple, iterate based on responses                       │
│                                                                  │
│  COMMON MISTAKE:                                                 │
│  Being too vague - always provide context!                      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

**Excellent work completing Module 1! See you in Module 2! 🚀**
