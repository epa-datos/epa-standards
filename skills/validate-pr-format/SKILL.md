---
name: validate-pr-format
description: Validate that a PR follows EPA Digital standards
keywords:
  - check PR format
  - validate PR
  - PR review checklist
  - PR validation
---

# Validate PR Format - EPA Digital Standard

Validate that a PR follows EPA Digital branching, naming, and format standards.

## What This Skill Does

1. ✅ Validates PR title format (feat:, fix:, docs:, etc.)
2. ✅ Validates branch name format (feature/, fix/, refactor/, etc.)
3. ✅ Checks PR description completeness
4. ✅ Verifies required checklist items are completed
5. ✅ Validates type of change is selected
6. ✅ Suggests fixes for issues found

## Inputs

Ask the user for:

- **PR Title** (e.g., "feat: add user authentication")
- **Branch name** (e.g., "feature/user-auth")
- **PR description** (paste the full PR body)
  - Or: PR URL to fetch from GitHub

Alternatively:
- **GitHub PR URL** (e.g., "https://github.com/epa-datos/my-service/pull/42")

## Validation Rules

### PR Title Format
```
✅ VALID:
  feat: add user authentication
  fix: resolve login timeout issue
  docs: update API documentation
  refactor: simplify user service
  test: add unit tests for auth
  chore: update dependencies

❌ INVALID:
  Add user auth (no prefix)
  FEAT: add user auth (uppercase)
  feature: add user auth (should be "feat:")
```

**Valid prefixes:** `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`, `ci`

### Branch Name Format
```
✅ VALID:
  feature/user-auth
  feature/add-user-authentication
  fix/login-timeout
  fix/session-expiry-bug
  refactor/user-service
  docs/api-guide

❌ INVALID:
  user-auth (missing prefix)
  Feature/user-auth (uppercase)
  feature/User-Auth (uppercase word)
  feature/add_user_auth (underscores, use hyphens)
```

**Valid prefixes:** `feature/`, `fix/`, `refactor/`, `docs/`, `test/`, `chore/`

### PR Description Requirements

Check for:
- [ ] Clear description of changes
- [ ] Link to issue (if applicable): "Closes #123"
- [ ] Type of change selected (checkbox):
  - [ ] Bug fix
  - [ ] New feature
  - [ ] Breaking change
  - [ ] Documentation update
- [ ] Testing checklist:
  - [ ] Unit tests added/updated
  - [ ] E2E tests added/updated (if applicable)
  - [ ] All tests passing
- [ ] Code quality:
  - [ ] No console.logs or debug code
  - [ ] Code follows project conventions
  - [ ] TypeScript/Go strict mode passing
- [ ] Documentation:
  - [ ] CLAUDE.md updated (if necessary)
  - [ ] Comments added for complex logic

## Validation Output

```markdown
## ✅ PR Validation Report

### Title
✅ Format is valid: "feat: add user authentication"

### Branch
✅ Format is valid: "feature/user-auth"

### Description
⚠️ Issues found:
  - Missing issue reference (e.g., "Closes #42")
  - Type of change not clearly selected
  
✅ Strengths:
  - Clear description of changes
  - Testing checklist present

### Checklist Completion
⚠️ Incomplete:
  - [ ] Type of change (missing)
  - [ ] Unit tests added

✅ Complete:
  - [x] Bug fix identified
  - [x] Testing checklist present

## Suggestions

1. Add issue reference to description:
   \`\`\`
   Closes #42
   \`\`\`

2. Ensure all checklist items are marked complete before requesting review

3. Verify tests are passing: \`npm run test:run\`

## Summary
❌ Not ready to merge
→ Please address the issues above before requesting review
```

## GitHub PR Template Reference

The standard template EPA Digital uses:

```markdown
## Description
Brief description of changes...

## Closes
Closes #issue_number

## Type of Change
- [ ] Bug fix (non-breaking change fixing an issue)
- [ ] New feature (non-breaking change adding functionality)
- [ ] Breaking change (fix or feature causing existing functionality to change)
- [ ] Documentation update

## Testing Checklist
- [ ] Unit tests added/updated
- [ ] E2E tests added/updated (if applicable)
- [ ] All tests passing locally
- [ ] Manual testing completed

## Code Quality Checklist
- [ ] No console.logs or debug code
- [ ] Code follows project conventions
- [ ] TypeScript passes strict mode (`npm run typecheck`)
- [ ] Linter passes (`npm run lint`)

## Documentation
- [ ] CLAUDE.md updated (if necessary)
- [ ] Comments added for complex logic
- [ ] README updated (if necessary)
```

## Tips for Perfect PRs

1. **Title:** Be specific and use correct prefix
   - ✅ `feat: add login with Google OAuth`
   - ❌ `add auth stuff`

2. **Branch:** Match the PR title
   - ✅ Title: `feat: add login`, Branch: `feature/google-oauth`
   - ❌ Title: `feat: add login`, Branch: `my-branch`

3. **Description:** Explain *why* and *what*, not just code changes
   - Include issue number: `Closes #123`
   - List checklist items completed

4. **Tests:** Always include tests
   - Unit tests for business logic
   - E2E tests for critical flows
   - Run tests before submitting: `npm run test:run`

5. **Keep PRs small**
   - Easier to review
   - Faster to merge
   - Easier to revert if needed

---

Use this skill to validate before submitting PR to team.
