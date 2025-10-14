# Quick Start Guide

Get started with the Prompt Engineering Course in 5 minutes!

## 🚀 Fast Track Setup

### Option 1: Web-Based Learning (No Installation Required)

**Perfect for:** Getting started immediately, exploring concepts

1. **Get ChatGPT Access**

   - Go to https://chat.openai.com
   - Sign up for free account
   - You're ready to start!
2. **Start Learning**

   - Open [Module 1, Lesson 1](modules/module-1-introduction/01-what-is-prompt-engineering.md)
   - Follow along with examples
   - Practice prompts directly in ChatGPT

**First Prompt to Try:**

```
Act as a Senior QA Engineer. Generate 5 test cases for a login page 
with email and password fields. Include positive and negative scenarios.
Format as a table.
```

---

### Option 2: Full Setup (With Code Examples)

**Perfect for:** Hands-on coding, building projects

**Time Required:** 10-15 minutes

#### Step 1: Install Python (if not already installed)

```bash
# Check if Python is installed
python --version

# Should be Python 3.9 or higher
# If not, download from: https://www.python.org/downloads/
```

#### Step 2: Clone the Repository

```bash
git clone https://github.com/yourusername/prompt-engineering-course.git
cd prompt-engineering-course
```

#### Step 3: Set Up Virtual Environment

```bash
# Create virtual environment
python -m venv venv

# Activate it
# On macOS/Linux:
source venv/bin/activate

# On Windows:
venv\Scripts\activate
```

#### Step 4: Install Dependencies

```bash
# Install basic dependencies
pip install openai python-dotenv pytest

# Or install everything
pip install -r requirements.txt
```

#### Step 5: Configure API Key (Optional)

```bash
# Copy example env file
cp .env.example .env

# Edit .env and add your OpenAI API key
# Get API key from: https://platform.openai.com/api-keys
```

#### Step 6: Test Your Setup

```bash
# Run setup verification
python -c "import openai; print('✅ Setup complete!')"
```

---

## 📖 Choose Your Learning Path

### Path 1: Manual QA Engineer (15-20 hours)

```
Day 1: Module 1 + Module 2 (Fundamentals)
Day 2: Module 3 (Manual QA)
Day 3: Practice exercises
Day 4-5: Project 1 (Test Suite Generator)
```

**Start here:** [Module 1 - Introduction](modules/module-1-introduction/01-what-is-prompt-engineering.md)

### Path 2: Automation Engineer (25-30 hours)

```
Day 1: Module 1 + Module 2 (Fundamentals)
Day 2: Module 3 (Manual QA)
Day 3: Module 4 (Automation Testing)
Day 4: Module 7 (AI Agents)
Day 5-7: Projects 1 & 2
```

**Start here:** [Module 1 - Introduction](modules/module-1-introduction/01-what-is-prompt-engineering.md)

### Path 3: Developer (20-25 hours)

```
Day 1: Module 1 + Module 2 (Fundamentals)
Day 2: Module 5 (Developers)
Day 3: Module 7 (AI Agents)
Day 4-5: Projects 3 & 4
```

**Start here:** [Module 1 - Introduction](modules/module-1-introduction/01-what-is-prompt-engineering.md)

---

## 🎯 Quick Wins - Try These First

### Win 1: Generate Test Cases (5 minutes)

**Prompt:**

```
Act as a QA Engineer. Create test cases for a shopping cart feature 
where users can add, remove, and update quantities. Include edge cases.
Format as a table with: Test ID, Description, Steps, Expected Result.
```

**Copy this into ChatGPT and see the results!**

---

### Win 2: Generate Automation Code (10 minutes)

**Prompt:**

```
Act as a Test Automation Engineer expert in Selenium and Python.

Create a Selenium script to:
1. Open https://example.com
2. Find element with id="search"
3. Enter "test" and submit
4. Verify results page loads

Include:
- Imports
- WebDriver setup
- Explicit waits
- Error handling
```

**Try this and see working code generated!**

---

### Win 3: Code Review (5 minutes)

**Prompt:**

```
Act as a Senior Developer. Review this code and suggest improvements:

def login(username, password):
    user = db.query(f"SELECT * FROM users WHERE name='{username}'")
    if user and user.password == password:
        return True
    return False

Focus on security issues.
```

**Learn about SQL injection and security!**

---

## 📚 Essential Resources

### Bookmark These

1. **Course Structure:** [course-structure.md](course-structure.md)
2. **Module Index:** [README.md](README.md)
3. **Practice Prompts:** [exercises/practice-prompts.md](exercises/practice-prompts.md)
4. **Projects:** [projects/](projects/)

### Quick Reference

**5-Component Prompt Template:**

```
Role: Act as a [role with expertise]
Context: [Background information]
Task: [What you want done]
Format: [How to structure output]
Constraints: [Limitations/requirements]
```

**Example:**

```
Role: Act as a Senior QA Engineer
Context: Testing a user registration API endpoint
Task: Generate API test cases including security tests
Format: Table with Test ID, Request, Expected Response
Constraints: Focus on REST API, use JSON format
```

---

## 🎓 Learning Tips

### Do This ✅

1. **Practice Daily:** Spend 30 minutes/day minimum
2. **Experiment:** Try variations of prompts
3. **Take Notes:** Keep a prompt library
4. **Build Projects:** Don't just read, do!
5. **Share Learning:** Teach others what you learn

### Avoid This ❌

1. **Don't Skip Fundamentals:** Module 1 & 2 are essential
2. **Don't Just Copy-Paste:** Understand what you're doing
3. **Don't Rush:** Quality over speed
4. **Don't Skip Practice:** Exercises are crucial
5. **Don't Work in Isolation:** Join the community

---

## 🆘 Troubleshooting

### Issue: "OpenAI API Key Invalid"

**Solution:**

```bash
# Check your .env file
cat .env | grep OPENAI_API_KEY

# Verify key at: https://platform.openai.com/api-keys
# Make sure it starts with 'sk-'
```

### Issue: "Module Not Found"

**Solution:**

```bash
# Ensure virtual environment is activated
# You should see (venv) in terminal

# Reinstall dependencies
pip install -r requirements.txt
```

### Issue: "ChatGPT Not Responding Well"

**Solution:**

- Be more specific in your prompt
- Add context
- Specify output format
- Review Module 2 on prompt structure

### Issue: "Python Not Found"

**Solution:**

```bash
# Install Python 3.9+
# Download from: https://www.python.org/downloads/

# Verify installation
python --version
python3 --version
```

---

## 💬 Get Help

### Community Support

- **Discord:** [Join our community](#)
- **GitHub Issues:** [Report problems](https://github.com/yourusername/prompt-engineering-course/issues)
- **Email:** support@example.com

### Office Hours

- **When:** Weekly on Fridays, 2-3 PM PST
- **Where:** Zoom (link in Discord)
- **What:** Live Q&A and demos

---

## ✅ First Day Checklist

Complete these on Day 1:

- [ ] Set up ChatGPT account
- [ ] Read Module 1, Lesson 1
- [ ] Try your first 5 prompts
- [ ] (Optional) Install Python and dependencies
- [ ] Join the community Discord
- [ ] Bookmark this guide
- [ ] Set learning schedule
- [ ] Complete Module 1 exercises

---

## 🎯 Your First Hour Plan

**0:00 - 0:10** - Setup

- Create ChatGPT account
- Bookmark course repo

**0:10 - 0:25** - Learn

- Read Module 1, Lesson 1
- Understand the basics

**0:25 - 0:45** - Practice

- Try 5 example prompts
- Experiment with variations

**0:45 - 1:00** - Reflect

- Take notes on what works
- Plan tomorrow's learning

---

## 🚀 Ready to Start?

**Option 1 - Start Learning Now (No Setup):**
👉 [Module 1: What is Prompt Engineering?](modules/module-1-introduction/01-what-is-prompt-engineering.md)

**Option 2 - Full Course Access:**
👉 [Complete README](README.md)

**Option 3 - Jump to Practice:**
👉 [Practice Exercises](exercises/practice-prompts.md)

---

## 📈 Track Your Progress

Use this simple tracker:

```markdown
## My Progress

- [ ] Quick Start Complete
- [ ] Module 1 Complete
- [ ] Module 2 Complete
- [ ] First Project Started
- [ ] First Project Complete
- [ ] Course Complete
```

---

**Questions? Issues? Feedback?**

Open an issue or join our Discord!

**Let's get started! 🎉**
