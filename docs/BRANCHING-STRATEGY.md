# Git Branching Strategy

EPA Digital's standardized branching strategy and workflow.

## 📊 Overview

```
main (production)
  ↑
  └─ PR from staging (release process)

staging (pre-production / testing)
  ↑
  └─ PR from feature/, fix/, etc. (development)

feature/xxx, fix/xxx, etc. (development branches)
  └─ Create from staging
```

---

## 🌳 Branch Types

### `main` (Production)
**Purpose:** Production-ready code  
**Protection:**
- ✅ Require pull request reviews (1 approval minimum)
- ✅ Require status checks to pass (tests)
- ✅ Require branches to be up to date
- ❌ DO NOT push directly

**Who can merge:**
- Pull requests from `staging` only
- Manual approval required (for releases)

**Usage:**
```bash
# Releasing to production
# 1. Create PR: staging → main
# 2. Merge after approval
# 3. Auto-deploy to production
```

---

### `staging` (Pre-Production)
**Purpose:** Pre-release testing and integration  
**Protection:**
- ✅ Require pull request reviews (1 approval)
- ✅ Require status checks to pass
- ✅ Require branches to be up to date
- ❌ DO NOT push directly

**Who can merge:**
- Pull requests from `feature/`, `fix/`, `refactor/`, etc.
- Auto-deploy after merge

**Usage:**
```bash
# Feature development workflow
# 1. Create branch: feature/xxx (from staging)
# 2. Create PR to staging
# 3. Merge after approval
# 4. Auto-deploy to staging
```

---

### Development Branches

#### `feature/xxx` - New Features
**Create from:** `staging`  
**Merge to:** `staging`  
**Naming:** `feature/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b feature/user-authentication
```

**Examples:**
- ✅ `feature/user-auth`
- ✅ `feature/add-billing-page`
- ✅ `feature/google-oauth`
- ❌ `feature/add_user_auth` (use hyphens)
- ❌ `feature/Add-User` (lowercase)

---

#### `fix/xxx` - Bug Fixes
**Create from:** `staging` (or `main` for production hotfixes)  
**Merge to:** `staging` (or `main` for hotfixes)  
**Naming:** `fix/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b fix/login-timeout
```

**Examples:**
- ✅ `fix/user-not-loading`
- ✅ `fix/payment-duplicate`
- ✅ `fix/session-expiry`

---

#### `refactor/xxx` - Code Improvements
**Create from:** `staging`  
**Merge to:** `staging`  
**Naming:** `refactor/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b refactor/user-service
```

**Examples:**
- ✅ `refactor/simplify-auth`
- ✅ `refactor/extract-utils`

---

#### `docs/xxx` - Documentation
**Create from:** `staging`  
**Merge to:** `staging`  
**Naming:** `docs/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b docs/api-guide
```

**Examples:**
- ✅ `docs/setup-guide`
- ✅ `docs/api-reference`

---

#### `test/xxx` - Testing
**Create from:** `staging`  
**Merge to:** `staging`  
**Naming:** `test/short-description`

```bash
git checkout staging
git pull origin staging
git checkout -b test/add-auth-tests
```

---

#### `hotfix/xxx` - Production Hotfixes
**Create from:** `main`  
**Merge to:** `main` AND `staging`  
**Naming:** `hotfix/short-description`

```bash
git checkout main
git pull origin main
git checkout -b hotfix/critical-bug

# After merge to main:
git checkout staging
git pull origin main
# Merge changes back to staging
```

---

## 🔄 Complete Workflow Example

### Scenario: Add User Authentication

```bash
# 1️⃣ Update local staging
git checkout staging
git pull origin staging

# 2️⃣ Create feature branch
git checkout -b feature/user-authentication

# 3️⃣ Make changes
# (Edit files, add code)

# 4️⃣ Test locally
npm run test:run      # frontend
go test ./...         # backend

# 5️⃣ Commit changes
git add .
git commit -m "feat: add user authentication with Google OAuth"

# 6️⃣ Push to remote
git push -u origin feature/user-authentication

# 7️⃣ Create PR on GitHub
# → Go to https://github.com/epa-datos/my-service/pulls
# → Click "New Pull Request"
# → Base: staging, Compare: feature/user-authentication
# → Fill PR template
# → Submit

# 8️⃣ Address review comments (if any)
git add src/features/auth/
git commit -m "refactor: simplify auth logic per review"
git push origin feature/user-authentication

# 9️⃣ After approval, merge on GitHub
# → Click "Squash and merge"
# → Confirm

# 🔟 Delete local branch
git checkout staging
git pull origin staging
git branch -d feature/user-authentication
git push origin --delete feature/user-authentication
```

---

## 📋 PR Requirements

### Before Creating PR
- [ ] Branch created from correct base (`staging` or `main`)
- [ ] Branch name follows EPA Digital format
- [ ] All changes committed and pushed
- [ ] Tests passing locally
- [ ] Linter passing
- [ ] No console.logs or debug code
- [ ] TypeScript/Go strict mode passing

### PR Requirements
- ✅ **1 approval minimum** from another team member
- ✅ **All tests passing** (CI checks)
- ✅ **No merge conflicts**
- ✅ **Branch up to date** with base branch

### PR Title Format
```
Format: {type}: {description}

Types: feat, fix, docs, refactor, test, chore, perf, ci

Examples:
  feat: add user authentication
  fix: resolve login timeout
  docs: update API guide
```

---

## 🚫 Common Mistakes

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
# Create a PR, get approval, merge on GitHub
```

### ❌ Wrong branch name format
```bash
# DON'T use:
my-feature
My-Feature
feature_name
featurename

# DO use:
feature/my-feature (lowercase, hyphens)
```

### ❌ Merging without tests
```bash
# DON'T merge if:
npm run test:run       # Has failures
npm run lint           # Has errors
git status             # Has uncommitted changes

# DO check first:
npm run test:run
npm run lint
npm run typecheck
```

### ❌ Force pushing
```bash
# NEVER do this:
git push -f origin feature/xxx

# Use instead if you need to redo:
git reset --soft HEAD~1    # Undo last commit
git reset --hard origin/xx # Reset to remote
```

---

## 📊 Status Checks

### Backend (Go)
Checks required before merge:
- [ ] `go test ./...` - All tests pass
- [ ] `golangci-lint run` - Linting
- [ ] `go vet ./...` - Go vet checks

### Frontend (NextJS)
Checks required before merge:
- [ ] `npm run test:run` - Unit tests
- [ ] `npm run lint` - ESLint
- [ ] `npm run typecheck` - TypeScript

### Both
- [ ] PR template filled
- [ ] 1 approval from team
- [ ] No merge conflicts

---

## 🔔 Notifications

### Auto-Deployment
- **Merge to `staging`** → Auto-deploy to staging environment
- **Merge to `main`** → Auto-deploy to production

### Slack Notifications
- PR created → Slack notification
- PR approved → Slack notification
- Deploy started → Slack notification
- Deploy completed → Slack notification

---

## 📖 Quick Reference

```bash
# Update staging
git checkout staging
git pull origin staging

# Create branch
git checkout -b feature/my-feature

# Check status
git status
git log --oneline -5

# Push
git push -u origin feature/my-feature

# Delete branch
git branch -d feature/my-feature
git push origin --delete feature/my-feature

# Sync with staging
git fetch origin
git rebase origin/staging

# See branches
git branch -a

# See tracking
git branch -vv
```

---

## 🆘 Troubleshooting

### "fatal: not a git repository"
```bash
# You're not in a git repo
cd /path/to/repo
git status
```

### "error: Your local changes to X would be overwritten by merge"
```bash
# You have uncommitted changes
git status
git add .
git commit -m "work in progress"
git pull origin staging
```

### "error: permission denied"
```bash
# You don't have permission or need to authenticate
gh auth login
# Or use SSH instead of HTTPS
```

### "The branch is N commits behind main"
```bash
# Sync your branch
git fetch origin
git rebase origin/staging
# Or merge
git merge origin/staging
```

---

## 📞 Questions?

- Read this guide (you're here)
- Use `git-flow-guide` skill: `"What branch should I create?"`
- Ask the team
- Check [CONTRIBUTING.md](../CONTRIBUTING.md)

---

**Last updated:** 2026-05-21
