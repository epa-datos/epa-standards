---
name: git-flow-guide
description: Interactive guide for EPA Digital Git workflow
keywords:
  - how do I create a PR
  - what branch should I use
  - git workflow
  - staging vs main
  - branch naming
---

# Git Flow Guide - EPA Digital Standard

Interactive guide for EPA Digital's branching strategy and PR workflow.

## What This Skill Does

1. ✅ Asks what you're working on (feature, bug, docs, etc.)
2. ✅ Recommends correct branch name format
3. ✅ Provides step-by-step workflow
4. ✅ Explains PR process and review requirements
5. ✅ Links to relevant documentation

## Workflow Overview

```
main (production)
  ↑
  └─ PR from staging (manual, release process)

staging (pre-production)
  ↑
  └─ PR from feature/, fix/, etc.

feature/xxx, fix/xxx, etc. (development)
  └─ Create from staging, merge back to staging
```

## Branch Types

### 1. Feature Branch (`feature/...`)
**When:** Adding a new feature
**Create from:** `staging`
**Merge to:** `staging` (via PR)
**Naming:** `feature/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b feature/user-authentication
```

**Example names:**
- ✅ `feature/user-auth`
- ✅ `feature/add-billing-page`
- ✅ `feature/google-oauth`
- ❌ `feature/add_user_auth` (use hyphens, not underscores)
- ❌ `feature/Add-User` (lowercase)

### 2. Bug Fix Branch (`fix/...`)
**When:** Fixing a bug
**Create from:** `staging` (or `main` for production hotfixes)
**Merge to:** `staging` (or `main` for hotfixes)
**Naming:** `fix/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b fix/login-timeout
```

**Example names:**
- ✅ `fix/user-not-loading`
- ✅ `fix/payment-duplicate`
- ✅ `fix/session-expiry`

### 3. Documentation Branch (`docs/...`)
**When:** Updating documentation only (no code changes)
**Create from:** `staging`
**Merge to:** `staging`
**Naming:** `docs/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b docs/api-guide
```

**Example names:**
- ✅ `docs/setup-guide`
- ✅ `docs/api-reference`
- ✅ `docs/architecture`

### 4. Refactor Branch (`refactor/...`)
**When:** Improving code without changing functionality
**Create from:** `staging`
**Merge to:** `staging`
**Naming:** `refactor/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b refactor/user-service
```

**Example names:**
- ✅ `refactor/simplify-auth`
- ✅ `refactor/extract-utils`

### 5. Test Branch (`test/...`)
**When:** Adding or improving tests only
**Create from:** `staging`
**Merge to:** `staging`
**Naming:** `test/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b test/add-auth-tests
```

### 6. Hotfix Branch (For Production Issues)
**When:** Critical bug in production
**Create from:** `main`
**Merge to:** `main` AND `staging`
**Naming:** `hotfix/short-description`

```bash
git checkout main
git pull origin main
git checkout -b hotfix/critical-security-issue

# After merge to main:
git checkout staging
git pull origin main
# Merge changes back to staging
```

## Complete Workflow Example

```bash
# 1. Update local staging branch
git checkout staging
git pull origin staging

# 2. Create feature branch
git checkout -b feature/user-authentication

# 3. Make changes and commit
git add src/features/auth/
git commit -m "feat: add user authentication"

# 4. Push to remote
git push -u origin feature/user-authentication

# 5. Create PR on GitHub
# → Open https://github.com/epa-datos/my-service/pulls
# → Click "New Pull Request"
# → Base: staging, Compare: feature/user-authentication
# → Fill PR template with description
# → Submit for review

# 6. Address review comments (if any)
git add .
git commit -m "refactor: simplify auth logic per review"
git push origin feature/user-authentication

# 7. After approval, merge on GitHub
# → Click "Squash and merge" or "Create a merge commit"

# 8. Delete branch locally
git checkout staging
git pull origin staging
git branch -d feature/user-authentication
git push origin --delete feature/user-authentication
```

## PR Checklist

Before creating a PR, ensure:

- [ ] Branch created from correct base (`staging` or `main`)
- [ ] Branch name follows EPA Digital format
- [ ] All changes committed and pushed
- [ ] Tests passing locally: `npm run test:run` or `go test ./...`
- [ ] Linter passing: `npm run lint`
- [ ] No console.logs or debug code
- [ ] TypeScript strict mode passing (if applicable)
- [ ] CLAUDE.md updated (if necessary)

## PR Review Requirements

For **feature**, **fix**, **refactor**, **docs**:
- ✅ 1 approval from another team member
- ✅ All tests passing
- ✅ No merge conflicts
- ⚠️ Branch must be up to date with staging

## Common Mistakes

### ❌ Creating PR from `main`
```bash
# DON'T do this:
git checkout -b feature/my-feature  # Creates from main!

# DO this instead:
git checkout staging
git checkout -b feature/my-feature
```

### ❌ Pushing directly to `staging` or `main`
```bash
# DON'T do this:
git push origin staging

# DO this instead:
# Make a PR, get approval, merge on GitHub
```

### ❌ Wrong branch name format
```bash
# DON'T use:
my-feature, My-Feature, feature_name, featurename

# DO use:
feature/my-feature (lowercase, hyphens)
```

### ❌ Merging without tests
```bash
# DON'T merge if:
git status shows uncommitted changes
tests are failing
linter is complaining

# DO check first:
npm run test:run
npm run lint
npm run typecheck
```

## Need Help?

If you're unsure:

1. **Which branch to create?** Ask in the task/issue what type of work it is
2. **How to name the branch?** Use the format: `{type}/{short-description}`
3. **Can't merge PR?** Check for conflicts: `git pull origin staging`
4. **Accidentally pushed to main?** Let the team know immediately

## Tips

1. **Keep branches short-lived**
   - Create Monday, merge Wednesday
   - Don't keep branches open for weeks

2. **Commit messages matter**
   ```bash
   ✅ git commit -m "feat: add user authentication"
   ✅ git commit -m "fix: resolve login timeout"
   ❌ git commit -m "update stuff"
   ❌ git commit -m "fixed"
   ```

3. **Push regularly**
   - Push at end of day
   - Don't wait until PR is ready
   - Helps prevent conflicts

4. **Always pull before starting**
   ```bash
   git checkout staging
   git pull origin staging
   ```

5. **Review your own PR first**
   - Click "Files changed" on your PR
   - Make sure only intended files changed
   - Check for any debug code

## Quick Reference

```bash
# View branches
git branch -a

# Switch branch
git checkout staging

# Create and switch
git checkout -b feature/my-feature

# View unpushed commits
git log origin/staging..

# Sync with remote
git fetch origin
git pull origin staging

# Delete local branch
git branch -d feature/my-feature

# Delete remote branch
git push origin --delete feature/my-feature

# Check branch tracking
git branch -vv
```

---

When in doubt, ask your team. Better to clarify than to undo a merge! 💪
