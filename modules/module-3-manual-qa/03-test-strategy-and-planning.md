# Module 3: Prompt Engineering for Manual QA

## Lesson 3: Test Strategy and Planning

---

## 🎯 Lesson Overview

**Time Required:** 45-60 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** Lessons 1 and 2 completed

In this lesson, you'll learn:
- How to create comprehensive test plans with AI
- Risk-based test prioritization techniques
- Test coverage analysis methods
- Regression test selection strategies
- Test effort estimation approaches

---

## 📋 Creating Test Plans with AI

### Test Plan Generation Prompt

```
Act as a Senior QA Lead creating a test plan for a new feature.

Feature: [Feature Name]

Feature Description:
[Detailed description]

Requirements:
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

Technical Context:
- Platform: [Web/Mobile/API]
- Tech Stack: [Technologies]
- Integration Points: [Other systems]

Create a comprehensive test plan including:

1. **Test Objectives**
   - What we're testing and why

2. **Scope**
   - In-scope items
   - Out-of-scope items (and why)

3. **Test Types**
   - Functional testing approach
   - Non-functional testing needs (performance, security, etc.)

4. **Test Levels**
   - Unit testing coverage
   - Integration testing approach
   - System testing requirements
   - User acceptance testing plan

5. **Entry Criteria**
   - What must be true before testing starts

6. **Exit Criteria**
   - What defines testing complete

7. **Test Environment**
   - Required environments
   - Test data needs
   - Tool requirements

8. **Risk Assessment**
   - High-risk areas
   - Mitigation strategies

9. **Resource Requirements**
   - Team members needed
   - Time estimates

10. **Deliverables**
    - Test artifacts to produce

11. **Schedule**
    - Testing timeline and milestones

Format as a professional document with clear sections.
```

### Example: Test Plan for Payment Feature

**Input:**
```
Feature: Credit Card Payment Integration

Description:
Adding credit card payment option to checkout. Users can save cards,
use saved cards, and enter new card details.

Requirements:
- Support Visa, Mastercard, Amex
- Save card option (encrypted)
- CVV required for every transaction
- 3D Secure support
- PCI DSS compliance required
```

**AI Output:**
```markdown
# Test Plan: Credit Card Payment Integration

## Document Control
| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | [Date] | QA Team | Initial draft |

---

## 1. Test Objectives

### Primary Objectives
- Verify all supported card types process correctly (Visa, Mastercard, Amex)
- Ensure card saving/retrieval functions securely
- Validate 3D Secure authentication flow
- Confirm PCI DSS compliance requirements are met
- Verify error handling for declined/failed transactions

### Success Criteria
- 100% of acceptance criteria covered
- Zero Critical/High severity bugs in production
- All security tests passed
- Performance within SLA (<3s transaction time)

---

## 2. Scope

### In Scope
| Area | Details |
|------|---------|
| Card Processing | Visa, Mastercard, American Express |
| Card Management | Save, retrieve, delete saved cards |
| Security | 3D Secure, CVV validation, encryption |
| Error Handling | Declined cards, network errors, timeouts |
| UI/UX | Payment form, card selection, feedback |

### Out of Scope
| Area | Reason |
|------|--------|
| Other payment methods | PayPal, crypto (separate project) |
| Recurring payments | Phase 2 feature |
| Multi-currency | Initial release USD only |
| Mobile app | Web only for initial release |

---

## 3. Test Types

### 3.1 Functional Testing
- [ ] Card input validation
- [ ] Transaction processing
- [ ] Card save/retrieve functionality
- [ ] 3D Secure flow
- [ ] Error messages and handling
- [ ] Receipt/confirmation generation

### 3.2 Security Testing
- [ ] PCI DSS compliance checklist
- [ ] SQL injection on card forms
- [ ] XSS on payment pages
- [ ] CVV handling (never stored)
- [ ] Encryption verification
- [ ] Token security

### 3.3 Integration Testing
- [ ] Payment gateway integration
- [ ] Order management system
- [ ] Email notification service
- [ ] Analytics/logging

### 3.4 Performance Testing
- [ ] Transaction response time (<3s)
- [ ] Concurrent transaction handling
- [ ] Gateway timeout handling
- [ ] Load testing (100 concurrent users)

### 3.5 Usability Testing
- [ ] Card input UX
- [ ] Error message clarity
- [ ] Mobile responsiveness
- [ ] Accessibility (WCAG 2.1)

---

## 4. Entry Criteria

| Criterion | Verification |
|-----------|--------------|
| Code complete and deployed to test | Deployment confirmed |
| Unit tests passing (>80% coverage) | CI/CD report |
| Test environment configured | Environment checklist |
| Test data available | Data setup confirmed |
| Payment gateway sandbox access | Credentials received |
| Test plan approved | Sign-off from PM |

---

## 5. Exit Criteria

| Criterion | Target |
|-----------|--------|
| Test case execution | 100% executed |
| Pass rate | >95% |
| Critical bugs | 0 open |
| High bugs | 0 open |
| Medium bugs | <5 open (approved) |
| Security tests | 100% passed |
| Performance | Within SLA |

---

## 6. Test Environment

### Environments Required
| Environment | Purpose | Data |
|-------------|---------|------|
| Dev | Development testing | Synthetic |
| QA | Full testing | Sanitized production |
| Staging | Pre-production validation | Production-like |
| Production | Smoke tests only | Real (careful!) |

### Test Tools
- **Test Management:** Jira/TestRail
- **API Testing:** Postman
- **Security:** OWASP ZAP, Burp Suite
- **Performance:** JMeter
- **Browser Testing:** Chrome, Firefox, Safari, Edge

### Test Cards (Sandbox)
| Card Type | Number | Behavior |
|-----------|--------|----------|
| Visa | 4111111111111111 | Success |
| Mastercard | 5555555555554444 | Success |
| Amex | 378282246310005 | Success |
| Declined | 4000000000000002 | Decline |

---

## 7. Risk Assessment

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| Payment gateway downtime | High | Medium | Retry logic, fallback messaging |
| Security vulnerability | Critical | Low | Security testing, code review |
| Performance degradation | High | Medium | Load testing, monitoring |
| Integration failure | High | Low | Early integration testing |
| Data breach | Critical | Low | PCI compliance, encryption |

---

## 8. Resource Requirements

### Team
| Role | Count | Effort |
|------|-------|--------|
| QA Lead | 1 | 20% |
| QA Engineer | 2 | 100% |
| Security Tester | 1 | 50% |
| Performance Tester | 1 | 25% |

### Time Estimate
| Phase | Duration |
|-------|----------|
| Test planning | 2 days |
| Test case creation | 3 days |
| Test execution (Round 1) | 5 days |
| Bug fixes & retest | 3 days |
| Regression testing | 2 days |
| **Total** | **15 days** |

---

## 9. Deliverables

| Deliverable | Owner | Due |
|-------------|-------|-----|
| Test Plan | QA Lead | Day 2 |
| Test Cases | QA Team | Day 5 |
| Test Data | QA Team | Day 5 |
| Daily Status Reports | QA Lead | Daily |
| Bug Reports | QA Team | Ongoing |
| Test Summary Report | QA Lead | Final day |
| Sign-off Document | QA Lead | Final day |

---

## 10. Approvals

| Role | Name | Signature | Date |
|------|------|-----------|------|
| QA Lead | | | |
| Dev Lead | | | |
| Product Manager | | | |
| Security Lead | | | |
```

---

## ⚖️ Risk-Based Test Prioritization

### Risk Assessment Prompt

```
Act as a QA Risk Analyst performing risk-based testing prioritization.

Feature: [Feature Name]

Components:
1. [Component 1]
2. [Component 2]
3. [Component 3]
[etc.]

For each component, analyze:

1. **Business Impact** (1-5)
   - How critical is this to the business?
   - What's the cost if it fails?

2. **Usage Frequency** (1-5)
   - How often is this used?
   - How many users affected?

3. **Technical Complexity** (1-5)
   - How complex is the implementation?
   - How likely are bugs?

4. **Change Frequency** (1-5)
   - How often is this modified?
   - Recent changes?

Calculate Risk Score = (Business Impact × Usage Frequency × Technical Complexity)

Provide:
1. Risk matrix with all components ranked
2. Testing priority order
3. Recommended test intensity for each
4. Suggested test types per component
```

### Example Risk Matrix

```markdown
## Risk-Based Test Prioritization

### Risk Matrix

| Component | Business | Usage | Complexity | Risk Score | Priority |
|-----------|----------|-------|------------|------------|----------|
| Payment Processing | 5 | 5 | 4 | 100 | P0 |
| User Authentication | 5 | 5 | 3 | 75 | P0 |
| Shopping Cart | 4 | 5 | 3 | 60 | P1 |
| Search | 3 | 5 | 3 | 45 | P1 |
| Product Catalog | 3 | 4 | 2 | 24 | P2 |
| User Profile | 2 | 3 | 2 | 12 | P3 |
| About Page | 1 | 2 | 1 | 2 | P4 |

### Testing Strategy by Priority

**P0 (Risk Score 75+) - Exhaustive Testing**
- 100% requirement coverage
- Full positive, negative, edge case testing
- Security testing
- Performance testing
- Multiple browser/device testing

**P1 (Risk Score 40-74) - Comprehensive Testing**
- High coverage (90%+)
- Key positive and negative scenarios
- Critical edge cases
- Security spot checks

**P2 (Risk Score 15-39) - Standard Testing**
- Moderate coverage (75%+)
- Main flows and key error scenarios
- Basic boundary testing

**P3 (Risk Score 5-14) - Basic Testing**
- Essential flows only
- Happy path validation
- Obvious error handling

**P4 (Risk Score <5) - Minimal Testing**
- Smoke test only
- Visual verification
```

---

## 📊 Test Coverage Analysis

### Coverage Analysis Prompt

```
Act as a QA Coverage Analyst.

I have the following requirements:
[List requirements]

I have the following test cases:
[List test cases]

Analyze the coverage:

1. **Requirements Coverage**
   - Which requirements are covered?
   - Which requirements are NOT covered?
   - Coverage percentage

2. **Test Type Coverage**
   - Positive tests: X%
   - Negative tests: X%
   - Edge cases: X%
   - Security tests: X%

3. **Gap Analysis**
   - Missing test scenarios
   - Undertested areas
   - Recommended additional tests

4. **Coverage Improvement Plan**
   - Prioritized list of tests to add
   - Effort estimate for each

Provide a comprehensive coverage report.
```

---

## 🔄 Regression Test Selection

### Regression Selection Prompt

```
Act as a QA Engineer selecting regression tests.

We made the following code changes:
[Describe changes or paste diff summary]

Available test suite:
- [List available tests or categories]

System architecture:
- [Describe how components connect]

Analyze and recommend:

1. **Direct Impact Areas**
   - What's directly changed?
   - Required tests for changed code

2. **Indirect Impact Areas**
   - What might be affected?
   - Integration points to test

3. **Recommended Regression Tests**
   - Must run (high confidence)
   - Should run (medium confidence)
   - Optional (low confidence)

4. **Estimated Time**
   - Full regression: X hours
   - Recommended subset: X hours

5. **Risk if Tests Skipped**
   - What bugs might we miss?

Provide a prioritized regression test list.
```

---

## ⏱️ Test Effort Estimation

### Estimation Prompt

```
Act as a QA Estimator with experience in test effort estimation.

Feature: [Feature Name]

Scope:
- Requirements: [Number of requirements]
- Complexity: [Low/Medium/High]
- Integration points: [List integrations]
- Team experience: [Familiar/New technology]

Provide detailed estimates for:

1. **Test Planning**
   - Test plan creation
   - Test strategy review
   - Risk assessment

2. **Test Design**
   - Test case creation (per complexity)
   - Test data preparation
   - Environment setup

3. **Test Execution**
   - First pass execution
   - Bug verification
   - Retesting after fixes
   - Regression testing

4. **Test Reporting**
   - Daily reporting
   - Summary reports
   - Metrics collection

Include:
- Effort in person-hours
- Assumptions made
- Risks that might affect estimates
- Buffer recommendations

Use industry standards and past experience.
```

### Estimation Table Example

```markdown
## Test Effort Estimation: User Registration Feature

### Assumptions
- 1 QA engineer available full-time
- Test environment ready
- Requirements are clear and stable
- Medium complexity feature

### Effort Breakdown

| Activity | Effort (Hours) | Notes |
|----------|---------------|-------|
| **Test Planning** | | |
| Test plan creation | 4 | Based on feature scope |
| Risk assessment | 2 | Medium complexity |
| **Test Design** | | |
| Test case writing (15 cases) | 8 | ~30 min/case avg |
| Test data preparation | 3 | User data, edge cases |
| Environment setup | 2 | Config and verification |
| **Test Execution** | | |
| First pass (15 cases) | 6 | ~20-30 min/case |
| Bug reporting | 4 | Estimate 8-10 bugs |
| Retesting | 4 | After bug fixes |
| Regression testing | 3 | Related features |
| **Test Reporting** | | |
| Daily status updates | 2 | 30 min/day × 4 days |
| Final test report | 2 | Summary and metrics |
| **Total Estimate** | **40 hours** | **5 working days** |

### Buffer Recommendation
| Scenario | Buffer | Total |
|----------|--------|-------|
| Best case (stable, no blockers) | 0% | 40 hours |
| Normal case (some issues) | 20% | 48 hours |
| Worst case (many bugs, delays) | 50% | 60 hours |

### Risks
- Requirement changes: +20% effort
- Environment issues: +15% effort
- High bug count: +25% effort
```

---

## 💻 Practical Exercises

### Exercise 3.7: Create a Test Plan

**Scenario:** New social media "Share" button feature
- Share to Facebook, Twitter, LinkedIn
- Copy link option
- Share analytics tracking

Create a test plan using AI:
```
[Write your prompt and resulting plan]
```

### Exercise 3.8: Risk-Based Prioritization

**Scenario:** E-commerce website with these components:
1. Product search
2. Shopping cart
3. Checkout/payment
4. User account
5. Product reviews
6. Wishlist
7. Order history

Create a risk matrix and testing priority:
```
[Write your prompt and resulting analysis]
```

### Exercise 3.9: Estimate Testing Effort

**Scenario:** Password reset feature
- Email verification
- Security questions
- New password validation
- Confirmation email

Estimate the testing effort:
```
[Write your prompt and resulting estimate]
```

---

## 📝 Key Takeaways

1. **Test plans** provide structure and ensure nothing is missed
2. **Risk-based prioritization** focuses effort on what matters most
3. **Coverage analysis** identifies gaps before they become problems
4. **Regression selection** balances thoroughness with efficiency
5. **Effort estimation** helps set realistic expectations
6. **AI accelerates** all these activities significantly

---

## ✅ Module 3 Complete! 🎉

Congratulations! You've mastered AI-powered manual QA techniques!

**You can now:**
- ✅ Generate comprehensive test cases rapidly
- ✅ Create professional bug reports
- ✅ Develop complete test plans
- ✅ Apply risk-based prioritization
- ✅ Estimate testing effort accurately

---

## 🔗 Next Steps

Choose your path:

| Your Interest | Next Module |
|---------------|-------------|
| **Learn automation** | [Module 4: Automation Testing](../module-4-automation-testing/README.md) |
| **Advanced techniques** | [Module 6: Advanced Techniques](../module-6-advanced-techniques/README.md) |
| **Hands-on project** | [Project 1: Test Suite Generator](../../projects/project-1-test-suite-generator/) |

---

## 📖 Quick Reference

```
┌─────────────────────────────────────────────────────────────────┐
│  MODULE 3 QUICK REFERENCE                                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  TEST CASE GENERATION:                                          │
│  "Generate [N] test cases for [feature] including              │
│   positive, negative, and edge cases..."                        │
│                                                                  │
│  BUG REPORTS:                                                    │
│  Include: Title, Severity, Steps, Expected/Actual,             │
│   Environment, Root cause analysis                              │
│                                                                  │
│  TEST PLANNING:                                                  │
│  Include: Scope, Risks, Entry/Exit criteria,                   │
│   Resources, Timeline, Deliverables                             │
│                                                                  │
│  RISK PRIORITIZATION:                                           │
│  Risk = Business Impact × Usage × Complexity                    │
│                                                                  │
│  ESTIMATION FORMULA:                                             │
│  Planning + Design + Execution + Reporting + Buffer             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

**Outstanding work completing Module 3! 🚀**
