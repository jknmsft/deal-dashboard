---
name: add-feature
description: Structured workflow for adding new features to the Deal Health Dashboard. Use this skill when implementing new functionality, adding capabilities, or extending the application.
---

# Add Feature Skill

This skill provides a structured workflow for adding new features while maintaining spec-driven development practices.

## When to Use This Skill
- When adding a new feature to the deal health dashboard
- When implementing a new requirement
- When extending existing functionality

## Feature Addition Workflow

### Phase 1: Requirements (Do First!)

Before writing any code:

1. **Check if requirement exists** in `specs/PRD.md`
   - If yes: Note the requirement ID
   - If no: Add the requirement first

2. **Requirement format** (add to PRD.md if missing):
   ```
   R[number]: [Feature name]
       - [What the feature does]
       - How to verify: [specific test steps]
   ```

3. **Update specs/Tasks.md** with new task:
   ```
   [ ] Task: [description]
       Satisfies: R[number]
       Done when: [verification criteria]
   ```

### Phase 2: Implementation

Only after requirements are documented:

1. **Read existing code** in `app/index.html`
2. **Identify where new code should go**
3. **Implement the feature** following:
   - Rules in `.github/copilot-instructions.md`
   - Frontend rules in `.github/instructions/app.instructions.md`
4. **Test in browser** before marking complete

### Phase 3: Verification

After implementation:

1. **Test the feature** using verification steps from PRD
2. **Mark task complete** in `specs/Tasks.md`:
   - Change `[ ]` to `[x]`
   - Add to Progress Log

3. **Update verification report** (if exists)

## Feature Checklist Template

```markdown
## Feature: [Name]

### Requirements
- [ ] Requirement added to specs/PRD.md (R__)
- [ ] Task added to specs/Tasks.md

### Implementation  
- [ ] Code follows project instructions
- [ ] Code follows frontend instructions
- [ ] Feature works in browser

### Verification
- [ ] Tested using PRD verification steps
- [ ] Task marked complete in Tasks.md
- [ ] Progress log updated
```
