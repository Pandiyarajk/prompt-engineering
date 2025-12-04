# Module 3: Prompt Engineering for Manual QA

## Lesson 2: Bug Report Generation

---

## 🎯 Lesson Overview

**Time Required:** 45-60 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** Lesson 1 completed

In this lesson, you'll learn:
- How to create professional, detailed bug reports with AI
- Techniques for enhancing incomplete bug reports
- Root cause analysis assistance
- Severity and priority classification
- Duplicate detection strategies

---

## 🐛 The Anatomy of a Great Bug Report

A great bug report contains all the information a developer needs to understand and fix the issue.

```
┌─────────────────────────────────────────────────────────────────┐
│  PERFECT BUG REPORT STRUCTURE                                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  📋 TITLE (Clear, specific, searchable)                         │
│     "[Feature] [What's Wrong] [Where/When]"                     │
│                                                                  │
│  📊 CLASSIFICATION                                               │
│     • Severity (Critical/High/Medium/Low)                       │
│     • Priority (P0/P1/P2/P3)                                    │
│     • Type (Functional/UI/Performance/Security)                 │
│     • Component (Login/Cart/Checkout/etc.)                      │
│                                                                  │
│  📝 DESCRIPTION                                                  │
│     Brief summary of the issue                                  │
│                                                                  │
│  👣 STEPS TO REPRODUCE                                           │
│     1. Step one                                                 │
│     2. Step two (be specific!)                                  │
│     3. Step three                                               │
│                                                                  │
│  ✅ EXPECTED RESULT                                              │
│     What should happen                                          │
│                                                                  │
│  ❌ ACTUAL RESULT                                                │
│     What actually happened                                      │
│                                                                  │
│  🖥️ ENVIRONMENT                                                  │
│     Browser, OS, device, app version, etc.                      │
│                                                                  │
│  📎 ATTACHMENTS                                                  │
│     Screenshots, videos, logs                                   │
│                                                                  │
│  💡 ADDITIONAL CONTEXT                                           │
│     Workarounds, related issues, frequency                      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📝 Bug Report Generation Prompts

### Basic Bug Report Template

```
Act as a Senior QA Engineer with expertise in bug reporting.

I found an issue and need help creating a professional bug report.

Issue Details:
- What I was doing: [Your action]
- What happened: [Actual behavior]
- What should have happened: [Expected behavior]
- Environment: [Browser/OS/Device]

Create a detailed bug report with:
1. Clear, specific title (format: [Component] Brief description)
2. Severity classification (Critical/High/Medium/Low) with justification
3. Priority classification (P0/P1/P2/P3) with justification
4. Detailed steps to reproduce (numbered, specific)
5. Expected vs Actual results
6. Environment details
7. Suggested category/component
8. Any potential root cause (if identifiable)
9. Impact assessment
10. Suggested assignee type (Frontend/Backend/DevOps)

Format professionally for a bug tracking system like Jira.
```

### Example Usage

**Input:**
```
Issue Details:
- What I was doing: Trying to log in after resetting my password
- What happened: Got error "Invalid credentials" even with correct new password
- What should have happened: Should log in successfully
- Environment: Chrome 120, Windows 11, Production site
```

**AI Output:**
```markdown
# Bug Report

## BUG-XXXX: [Authentication] Login fails with newly reset password

### Classification
| Attribute | Value | Justification |
|-----------|-------|---------------|
| Severity | **High** | Core functionality blocked, affects all users who reset passwords |
| Priority | **P1** | Impacts user access, needs resolution within 24 hours |
| Type | Functional | Core feature not working as designed |
| Component | Authentication/Login |

### Description
Users cannot log in with their new password immediately after completing the password reset process. The system returns "Invalid credentials" error despite the password reset completing successfully.

### Steps to Reproduce
1. Navigate to https://app.example.com/login
2. Click "Forgot Password" link
3. Enter registered email: test@example.com
4. Receive password reset email
5. Click reset link in email
6. Enter new password: "NewPassword123!"
7. Confirm new password: "NewPassword123!"
8. Click "Reset Password" button
9. See success message: "Password reset successfully"
10. Navigate to login page
11. Enter email: test@example.com
12. Enter new password: "NewPassword123!"
13. Click "Login" button

### Expected Result
- User successfully logs in
- User is redirected to dashboard
- Session is created

### Actual Result
- Error message displayed: "Invalid credentials"
- User cannot log in
- Multiple retry attempts also fail

### Environment
- **Browser:** Chrome 120.0.6099.130
- **OS:** Windows 11 Pro (22H2)
- **Site:** Production (app.example.com)
- **Date/Time:** 2024-01-15 10:30 AM EST
- **User Type:** Registered customer

### Attachments
- [Screenshot of error message]
- [Screenshot of successful password reset]
- [Network console logs]

### Additional Context
- **Frequency:** Reproducible 100% of the time
- **Workaround:** None found; users cannot access their accounts
- **Impact:** All users who attempt password reset are locked out
- **First Reported:** Today
- **Related Issues:** None found

### Potential Root Cause
- Password may not be updating in database despite success message
- Cache issue - old password hash may still be cached
- Password hash algorithm mismatch between reset and login

### Suggested Assignee
Backend/Authentication Team
```

---

## 🔧 Enhancing Incomplete Bug Reports

Often, bug reports are incomplete. AI can help enhance them.

### Enhancement Prompt

```
Act as a Senior QA Engineer improving bug report quality.

Here's an incomplete bug report:
"""
[Paste incomplete bug report]
"""

Please enhance this bug report by:
1. Rewriting the title to be clearer and more searchable
2. Adding missing sections
3. Making steps to reproduce more specific
4. Clarifying expected vs actual results
5. Adding severity/priority with justification
6. Suggesting additional information to gather
7. Improving overall clarity and professionalism

Provide the enhanced bug report and list what was missing.
```

### Example: Enhancing a Poor Report

**Original (Poor):**
```
Title: Search broken
Description: Search doesn't work. Tried searching for something.
```

**Enhanced (After AI):**
```markdown
# Enhanced Bug Report

## [Search] No results returned for valid product queries

### Classification
| Severity | Priority | Type | Component |
|----------|----------|------|-----------|
| High | P1 | Functional | Search |

### Description
The product search feature returns zero results for valid search queries that should match existing products in the catalog.

### Steps to Reproduce
1. Navigate to https://shop.example.com
2. Click on the search bar (top navigation)
3. Enter search term: "blue shirt"
4. Press Enter or click the search icon
5. Observe the results page

### Expected Result
- Search results page displays products matching "blue shirt"
- Results include product images, names, and prices
- Filter options are available

### Actual Result
- "No results found" message displayed
- No products shown despite "blue shirt" products existing in catalog

### Environment
- [MISSING - Please specify browser, OS, and app version]

### Information Needed
- [ ] Browser and version
- [ ] Operating system
- [ ] Were you logged in?
- [ ] Does this happen for all search terms?
- [ ] When did this start happening?
- [ ] Screenshot of the issue

### Suggested Tests
- Try different search terms
- Test with exact product names
- Test in different browsers
- Check if issue is user-specific

---

### What Was Missing in Original Report:
1. ❌ Clear, searchable title
2. ❌ Severity and priority
3. ❌ Specific steps to reproduce
4. ❌ Expected vs actual results
5. ❌ Environment information
6. ❌ Any useful context
```

---

## 🔍 Root Cause Analysis Assistance

AI can help analyze potential root causes of bugs.

### Root Cause Analysis Prompt

```
Act as a Senior Software Engineer and Debugging Expert.

Analyze this bug and provide root cause analysis:

Bug Description:
[Describe the bug]

Technical Context:
- Application: [Type of application]
- Technology: [Tech stack]
- When it happens: [Circumstances]

Error Messages/Logs (if any):
[Paste any error messages or logs]

Provide:
1. Most likely root cause with reasoning
2. Alternative possible causes (ranked by likelihood)
3. Evidence that would confirm each hypothesis
4. Suggested investigation steps
5. Technical questions to ask developers
6. Related areas to check

Be specific and technical in your analysis.
```

### Example: Root Cause Analysis

**Input:**
```
Bug Description:
Shopping cart total shows $0.00 after adding items, but items are visible

Technical Context:
- Application: E-commerce website
- Technology: React frontend, Node.js backend, PostgreSQL
- When it happens: After adding 3+ items to cart

Error Messages/Logs:
Console: "TypeError: Cannot read property 'reduce' of undefined"
```

**AI Analysis:**
```markdown
# Root Cause Analysis

## Bug: Shopping Cart Total Shows $0.00

### Most Likely Root Cause (85% confidence)
**Array handling error in price calculation**

The error message `Cannot read property 'reduce' of undefined` indicates:
- The cart items array is `undefined` when the total calculation runs
- This happens after adding 3+ items, suggesting:
  - A race condition where calculation runs before array is populated
  - Array pagination/chunking issue
  - State update timing problem

**Evidence to confirm:**
- Check if `items` is undefined in React state when calculating
- Add console.log before reduce operation
- Check if issue occurs with exactly 3 items or "more than 3"

### Alternative Causes

**2. State Management Bug (60% confidence)**
- Redux/Context state not updating correctly
- Items added but state reference stale
- Evidence: Check Redux DevTools for state shape

**3. API Response Format Change (40% confidence)**
- Backend returning different format for 3+ items
- Pagination kicking in unexpectedly
- Evidence: Check network tab for API response structure

**4. Price Field Naming Issue (20% confidence)**
- Items using `price` vs `item_price` vs `unitPrice`
- Inconsistent field names
- Evidence: Log item objects

### Investigation Steps

1. **Add logging before reduce:**
   ```javascript
   console.log('Items before reduce:', cartItems);
   console.log('Type:', typeof cartItems, Array.isArray(cartItems));
   ```

2. **Check API response:**
   - GET /api/cart with 2 items
   - GET /api/cart with 3+ items
   - Compare response structures

3. **Review state updates:**
   - Use React DevTools to watch state changes
   - Check for async timing issues

4. **Reproduce with timing:**
   - Add items slowly vs quickly
   - Does delay between adds affect it?

### Questions for Developers

1. Was any cart calculation logic changed recently?
2. Is there pagination for cart items?
3. What's the expected array structure for cart items?
4. Are there any null checks before reduce operations?
5. Did the backend API change?

### Related Areas to Check

- [ ] Cart calculation utility functions
- [ ] Redux cart reducer
- [ ] Cart API endpoint handlers
- [ ] Price formatting components
- [ ] Similar reduce operations elsewhere
```

---

## 📊 Severity and Priority Classification

### Classification Prompt

```
Act as a QA Lead who classifies bug severity and priority.

Bug Description:
[Describe the bug]

Impact Information:
- Who is affected: [Users affected]
- How often: [Frequency]
- Workaround available: [Yes/No]
- Business impact: [Any known impact]

Classify this bug:

1. Severity (based on technical impact):
   - Critical: System down, data loss, security breach
   - High: Major feature broken, no workaround
   - Medium: Feature impaired but workaround exists
   - Low: Minor issue, cosmetic, edge case

2. Priority (based on business impact):
   - P0: Fix immediately (drop everything)
   - P1: Fix within 24 hours
   - P2: Fix within current sprint
   - P3: Fix when convenient

Provide classification with detailed justification.
```

### Severity vs Priority Guide

```
┌─────────────────────────────────────────────────────────────────┐
│  SEVERITY vs PRIORITY                                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  SEVERITY = Technical Impact                                    │
│  "How bad is the technical problem?"                            │
│                                                                  │
│  PRIORITY = Business Impact                                     │
│  "How urgently must we fix it?"                                 │
│                                                                  │
│  ┌───────────────────────────────────────────────────────┐     │
│  │ Example: Typo on About page                            │     │
│  │ Severity: Low (cosmetic issue)                         │     │
│  │ Priority: P3 (fix when convenient)                     │     │
│  └───────────────────────────────────────────────────────┘     │
│                                                                  │
│  ┌───────────────────────────────────────────────────────┐     │
│  │ Example: Checkout broken (can't complete orders)       │     │
│  │ Severity: Critical (core feature broken)               │     │
│  │ Priority: P0 (fix immediately, losing revenue)         │     │
│  └───────────────────────────────────────────────────────┘     │
│                                                                  │
│  ┌───────────────────────────────────────────────────────┐     │
│  │ Example: Admin report page slow                        │     │
│  │ Severity: Medium (still works, just slow)              │     │
│  │ Priority: P3 (only 2 admins affected, workaround exists)│     │
│  └───────────────────────────────────────────────────────┘     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Duplicate Detection

### Duplicate Check Prompt

```
Act as a QA Engineer checking for duplicate bugs.

New Bug Report:
"""
[New bug description]
"""

Existing Bug Reports:
"""
1. BUG-123: [Title and brief description]
2. BUG-456: [Title and brief description]
3. BUG-789: [Title and brief description]
"""

Analyze:
1. Is this new bug a duplicate of any existing bug?
2. Similarity score for each (0-100%)
3. Key differences if not a duplicate
4. Recommendation: Mark as duplicate, link as related, or new bug

Be thorough in your comparison.
```

---

## 💻 Practical Exercises

### Exercise 3.4: Create a Bug Report

**Scenario:** You found this issue:
- Login page shows "500 Internal Server Error" when clicking login with valid credentials
- Happens on Chrome, works fine on Firefox
- Started happening after today's deployment

Create a complete bug report using AI:
```
[Write your prompt and the resulting bug report]
```

### Exercise 3.5: Enhance a Poor Report

**Poor Bug Report:**
```
Title: App crashes
Description: Opened the app and it crashed. This is annoying.
```

Use AI to enhance this report:
```
[Write your prompt and the enhanced report]
```

### Exercise 3.6: Root Cause Analysis

**Bug:** Users report that emails are not being sent from the system, but no errors appear in the UI.

Use AI to analyze potential root causes:
```
[Write your prompt and the analysis]
```

---

## 📝 Key Takeaways

1. **Complete bug reports** save developer time and lead to faster fixes
2. **AI can enhance** incomplete reports to professional quality
3. **Root cause analysis** helps in prioritization and assignment
4. **Severity vs Priority** are different - understand both
5. **Duplicate detection** prevents wasted effort
6. **Always verify** AI-generated content against reality

---

## ✅ Lesson 2 Checklist

Before moving on, ensure you can:

- [ ] Create comprehensive bug reports with AI
- [ ] Enhance incomplete bug reports
- [ ] Use AI for root cause analysis
- [ ] Properly classify severity and priority
- [ ] Check for duplicate bugs
- [ ] Complete all exercises

---

## 🔗 Next Steps

**Continue to:** [Lesson 3: Test Strategy and Planning](./03-test-strategy-and-planning.md)

---

**Excellent work! You're becoming a bug reporting expert! 🐛**
