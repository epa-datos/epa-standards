# Contributing to EPA Digital

Guía para contribuir a proyectos de EPA Digital.

## 📋 Antes de Empezar

1. Lee [ORGANIZATION.md](./ORGANIZATION.md) - Estándares globales
2. Lee [BRANCHING-STRATEGY.md](./docs/BRANCHING-STRATEGY.md) - Git workflow
3. Lee `CLAUDE.md` en el repo donde vas a contribuir
4. Asegúrate de tener las herramientas instaladas

## 🛠️ Setup Local

### Backend (Go)
```bash
# Clone
git clone https://github.com/epa-datos/epa-standards-backend.git
cd epa-standards-backend

# Dependencies
go mod download

# Run
go run ./cmd/api

# Test
go test ./...

# Lint
golangci-lint run
```

### Frontend (NextJS)
```bash
# Clone
git clone https://github.com/epa-datos/epa-standards-frontend.git
cd epa-standards-frontend

# Dependencies
npm install

# Run
npm run dev

# Test
npm run test:run

# Lint
npm run lint
```

### Full Stack
```bash
# Clone
git clone https://github.com/epa-datos/epa-standards-fullapp.git
cd epa-standards-fullapp

# Dependencies
npm install

# Run
npm run dev

# Test
npm run test:run
```

---

## 🔄 Git Workflow

### 1. Create Branch
```bash
git checkout staging
git pull origin staging
git checkout -b feature/your-feature-name
```

### 2. Make Changes
Follow the conventions in [ORGANIZATION.md](./ORGANIZATION.md):
- **Commits:** Use conventional format (feat:, fix:, etc.)
- **Code:** Follow style guide (see below)
- **Tests:** Write tests for your changes

### 3. Commit & Push
```bash
git add .
git commit -m "feat: describe your change"
git push -u origin feature/your-feature-name
```

### 4. Create PR
- Go to GitHub
- Click "New Pull Request"
- Base: `staging`, Compare: your branch
- Fill PR template with description
- Submit for review

### 5. Address Feedback
```bash
# Make changes
git add .
git commit -m "refactor: address review feedback"
git push origin feature/your-feature-name
```

### 6. Merge
- After 1 approval and tests passing
- Click "Squash and merge" (recommended)
- Delete branch

---

## ✍️ Code Style

### Commits
```
Format: {type}: {description}

Types:
  feat      - New feature
  fix       - Bug fix
  docs      - Documentation
  refactor  - Code refactor (no functional change)
  test      - Tests
  chore     - Dependencies, config
  perf      - Performance improvement

Examples:
  feat: add user authentication
  fix: resolve login timeout issue
  docs: update API documentation
  refactor: simplify user service
  test: add unit tests for auth
```

**Rules:**
- Lowercase
- Imperativo ("add", not "adds", "added")
- Primera línea < 50 chars
- Si necesitas contexto adicional, usa múltiples líneas

---

### Go Code
```bash
# Format (automatic)
go fmt ./...

# Lint
golangci-lint run

# Tests (before push)
go test ./...

# Coverage
go test -cover ./...
```

**Conventions:**
- Use `interfaces` for abstractions
- Package names lowercase
- Function names PascalCase (exported) or camelCase (private)
- Comments for exported functions
- Table-driven tests for parametric cases

**Example:**
```go
// User is a domain entity
type User struct {
    ID    string
    Email string
}

// GetUser retrieves user by ID
func GetUser(ctx context.Context, id string) (*User, error) {
    // Implementation
}
```

---

### TypeScript/React Code
```bash
# Format
npm run format

# Lint
npm run lint

# Type check
npm run typecheck

# Tests
npm run test:run
```

**Conventions:**
- Use `type` over `interface` (unless extending)
- Component names PascalCase: `LoginForm`
- Hook names start with `use`: `useAuth`
- Prefer Server Components
- Use `'use client'` only when necessary

**Example:**
```tsx
// ❌ DON'T
export function login_form() {
  // ...
}

// ✅ DO
export function LoginForm() {
  const { isLoading } = useAuth()
  return <form>{/* ... */}</form>
}
```

---

## 🧪 Testing

### Backend (Go)
**Table-driven tests:**
```go
func TestCreateUser(t *testing.T) {
  tests := []struct {
    name    string
    email   string
    wantErr bool
  }{
    {"valid email", "user@example.com", false},
    {"invalid email", "invalid", true},
  }
  
  for _, tt := range tests {
    t.Run(tt.name, func(t *testing.T) {
      err := CreateUser(tt.email)
      if (err != nil) != tt.wantErr {
        t.Errorf("CreateUser() error = %v, wantErr %v", err, tt.wantErr)
      }
    })
  }
}
```

**Run tests:**
```bash
go test ./...           # All tests
go test -run TestName   # Specific test
go test -cover ./...    # With coverage
```

---

### Frontend (TypeScript/React)
**Unit tests (Vitest):**
```tsx
import { render, screen } from '@testing-library/react'
import { LoginForm } from './login-form'

describe('LoginForm', () => {
  it('renders login form', () => {
    render(<LoginForm />)
    expect(screen.getByText(/login/i)).toBeInTheDocument()
  })
})
```

**E2E tests (Playwright):**
```ts
import { test, expect } from '@playwright/test'

test('user can login', async ({ page }) => {
  await page.goto('/login')
  await page.fill('[data-testid=email]', 'user@example.com')
  await page.fill('[data-testid=password]', 'password')
  await page.click('[data-testid=login-button]')
  await expect(page).toHaveURL('/dashboard')
})
```

**Run tests:**
```bash
npm run test            # Watch mode
npm run test:run        # One-shot (CI)
npm run test:e2e        # E2E tests
npm run test:coverage   # Coverage report
```

---

## ✅ PR Checklist

Before submitting a PR, ensure:

- [ ] Branch created from correct base (`staging`)
- [ ] Branch name follows format: `feature/xxx`, `fix/xxx`, etc.
- [ ] All tests passing: `npm run test:run` or `go test ./...`
- [ ] Code formatted: `npm run format` or `go fmt ./...`
- [ ] Linter passing: `npm run lint` or `golangci-lint run`
- [ ] No console.logs or debug code
- [ ] TypeScript strict mode passing (if applicable)
- [ ] CLAUDE.md updated (if necessary)
- [ ] PR description is clear and references issue
- [ ] No unnecessary files committed (check `.gitignore`)

---

## 📝 PR Description Template

```markdown
## Description
Brief description of what this PR does.

## Closes
Closes #issue_number

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Unit tests added/updated
- [ ] E2E tests added/updated (if applicable)
- [ ] All tests passing

## Checklist
- [ ] Code follows style guide
- [ ] No console.logs or debug code
- [ ] CLAUDE.md updated (if necessary)
- [ ] Changes documented
```

---

## 🤖 Using Skills

EPA Digital has 6 skills to help you:

### For Creating Repos
- **`nextjs-scaffold`** - Create new NextJS app
- **`go-api-scaffold`** - Create new Go API
- **`go-client-scaffold`** - Create Go client

### For Development
- **`validate-pr-format`** - Check PR before submitting
- **`git-flow-guide`** - Help with branching
- **`generate-openapi`** - Update API docs

Example:
```
Claude: "Create a new NextJS app for the dashboard"
Claude: "Validate my PR format"
Claude: "What branch should I create?"
```

---

## 🐛 Reporting Bugs

When reporting a bug:

1. **Title:** Be specific
   - ❌ "It doesn't work"
   - ✅ "Login form doesn't validate email on blur"

2. **Description:** Include
   - What you tried to do
   - What happened
   - What you expected
   - Steps to reproduce

3. **Environment:**
   - OS (macOS, Linux, Windows)
   - Node version (if applicable)
   - Go version (if applicable)

4. **Example:**
   ```markdown
   ## Bug: Login form doesn't validate email on blur
   
   ### Steps to Reproduce
   1. Go to /login
   2. Click email input
   3. Type invalid email (e.g., "invalid")
   4. Click outside (blur)
   
   ### Expected
   Email validation error appears
   
   ### Actual
   No error shown
   
   ### Environment
   - macOS 14
   - Node 20
   - Chrome 125
   ```

---

## 💡 Requesting Features

When requesting a feature:

1. **Title:** Clear and concise
   - ❌ "Improve user experience"
   - ✅ "Add dark mode toggle to settings"

2. **Description:**
   - Why is this feature needed?
   - How would it work?
   - What would benefit?

3. **Example:**
   ```markdown
   ## Feature: Add dark mode toggle
   
   ### Problem
   App is bright at night, strains eyes
   
   ### Solution
   Add dark mode toggle in user settings
   
   ### Benefit
   - Better UX for evening usage
   - Easier on the eyes
   
   ### Implementation
   - Add `theme` to user preferences
   - Toggle in settings page
   - Persist in localStorage
   ```

---

## 📞 Getting Help

- **Code questions:** Ask in PR comments
- **Architecture questions:** Read CLAUDE.md in the repo
- **General questions:** Open discussion in GitHub
- **Team questions:** Slack or email Eddye

---

## 🎓 Learning Resources

- [Go Documentation](https://golang.org/doc/)
- [Next.js Documentation](https://nextjs.org/docs)
- [React Documentation](https://react.dev)
- [Tailwind CSS](https://tailwindcss.com/docs)
- [TypeScript](https://www.typescriptlang.org/docs/)

---

**Thank you for contributing to EPA Digital!** 🚀
