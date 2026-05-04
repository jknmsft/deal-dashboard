---
name: fix-bug
description: Structured workflow for fixing bugs in the Deal Health Dashboard. Use this skill when something isn't working correctly, when tests fail, or when users report issues.
---

# Bug Fix Skill

This skill provides a structured workflow for identifying, fixing, and verifying bug fixes.

## When to Use This Skill
- When something isn't working as expected
- When a test fails
- When a user reports an issue
- When verification reveals a problem

## Bug Fix Workflow

### Phase 1: Understand the Bug

1. **Document the bug**:
   - What should happen? (expected behavior)
   - What actually happens? (actual behavior)
   - Steps to reproduce

2. **Identify related requirement**:
   - Which requirement in `specs/PRD.md` is affected?
   - What are the acceptance criteria?

3. **Locate the problem**:
   - Which file(s) contain the bug?
   - What code is responsible?

### Phase 2: Fix the Bug

1. **Make the minimal fix**:
   - Change only what's necessary
   - Don't refactor unrelated code
   - Don't add new features

2. **Follow project rules**:
   - Check `.github/copilot-instructions.md`
   - Check path-specific instructions

3. **Test the fix**:
   - Verify the bug is resolved
   - Verify nothing else broke

### Phase 3: Document and Verify

1. **Update Progress Log** in `specs/Tasks.md`:
   ```
   ### Bug Fix: [date]
   - Bug: [description]
   - Related: R[number]
   - Fix: [what was changed]
   - Verified: [how you tested it]
   ```

2. **Verify related features** still work

## Bug Report Template

```markdown
## Bug Report

**Summary**: [one-line description]

**Related Requirement**: R[number]

**Expected Behavior**: 
[what should happen]

**Actual Behavior**:
[what actually happens]

**Steps to Reproduce**:
1. [step 1]
2. [step 2]
3. [step 3]

**Severity**: Low / Medium / High / Critical

**Proposed Fix**:
[suggested solution]
```
