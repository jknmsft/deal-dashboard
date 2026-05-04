---
name: verify-requirements
description: Verify that implementation meets all requirements in specs/PRD.md. Use this skill when checking if features are complete, validating implementations, or creating verification reports.
---

# Requirement Verification Skill

This skill helps verify that the Deal Health Dashboard implementation meets all specified requirements.

## When to Use This Skill
- After implementing new features
- Before marking tasks as complete
- When creating verification reports
- When someone asks "does this meet the requirements?"

## Verification Process

### Step 1: Load Requirements
Read `specs/PRD.md` and extract all numbered requirements (R1, R2, R3, etc.)

### Step 2: Load Implementation
Read `app/index.html` and understand what's actually built

### Step 3: Test Each Requirement
For each requirement:
1. Identify the acceptance criteria
2. Check if the implementation meets it
3. Document evidence of compliance or gaps

### Step 4: Generate Report
Create a verification report with this structure:

## Verification Report Template

| Req | Description | Status | Evidence |
|-----|-------------|--------|----------|
| R1  | [description] | ✅ PASS / ❌ FAIL / ⚠️ PARTIAL | [what you observed] |

### Summary
- Total Requirements: [count]
- Passing: [count]  
- Failing: [count]
- Partial: [count]

### Failing Requirements Detail
For each failing requirement:
- **Requirement**: [ID and description]
- **Expected**: [what should happen]
- **Actual**: [what actually happens]
- **Fix Needed**: [what to change]
