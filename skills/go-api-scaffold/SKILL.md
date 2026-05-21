---
name: go-api-scaffold
description: Create a new Go API with hexagonal architecture
keywords:
  - create Go API
  - new Go project
  - hexagonal scaffold
  - Go API template
---

# Go API Scaffold - EPA Digital Standard

Create a new Go API with hexagonal architecture, Docker support, and GitHub workflows.

## What This Skill Does

1. ✅ Creates hexagonal directory structure
2. ✅ Generates main.go with dependency injection
3. ✅ Adds Dockerfile and docker-compose.yml
4. ✅ Customizes CLAUDE.md for the project
5. ✅ Sets up GitHub workflows for testing and Cloud Run
6. ✅ Creates OpenAPI/Swagger setup
7. ✅ Generates example domain, usecase, and handler

## Inputs

Ask the user for:

- **Project name** (e.g., "user-service", "billing-api")
  - Validate: kebab-case, 2-50 characters
- **GitHub organization** (e.g., "epa-datos")
- **GCP project ID** (for Cloud Run)
- **Primary domain/feature** (e.g., "users", "analytics", "billing")

## Workflow

```
1. Validate inputs
   └─ Project name format
   └─ GCP project exists

2. Create directory structure
   ├─ cmd/api/main.go
   ├─ internal/domain/
   ├─ internal/usecases/
   ├─ internal/adapters/
   │  ├─ http/handlers/
   │  ├─ http/middleware/
   │  └─ persistence/
   ├─ pkg/logger/
   ├─ pkg/errors/
   ├─ docs/
   ├─ migrations/
   └─ tests/

3. Generate boilerplate
   ├─ go.mod / go.sum
   ├─ Dockerfile (multi-stage)
   ├─ docker-compose.yml
   ├─ .env.example
   ├─ CLAUDE.md (customized)
   └─ .gitignore

4. Create example code
   ├─ internal/domain/{feature}.go (entity + interfaces)
   ├─ internal/usecases/create_{feature}.go (usecase with logic)
   ├─ internal/adapters/http/handlers/{feature}_handler.go (HTTP handler)
   ├─ internal/adapters/persistence/{feature}_repository.go (in-memory repo)
   └─ internal/usecases/{feature}_test.go (table-driven tests)

5. Set up workflows
   ├─ .github/workflows/test.yml
   ├─ .github/workflows/build.yml
   ├─ .github/workflows/deploy-staging.yml
   └─ .github/workflows/deploy-production.yml

6. Initialize Git
   ├─ git init
   ├─ git add .
   ├─ git commit -m "initial: Go API template"
   └─ git branch -M main

7. Provide checklist
   └─ OpenAPI setup
   └─ GitHub configuration
   └─ GCP Cloud Run setup
```

## Example Outputs

The skill generates actual, runnable code:

```go
// internal/domain/user.go
package domain

type User struct {
    ID        string
    Email     string
    CreatedAt time.Time
}

type UserRepository interface {
    Create(ctx context.Context, user *User) error
    GetByID(ctx context.Context, id string) (*User, error)
}
```

```go
// internal/usecases/create_user.go
package usecases

type CreateUserInput struct {
    Email string
}

type CreateUserOutput struct {
    ID string
}

func NewCreateUser(repo domain.UserRepository) *CreateUser {
    return &CreateUser{repo: repo}
}

func (u *CreateUser) Execute(ctx context.Context, input CreateUserInput) (*CreateUserOutput, error) {
    // Validate email
    // Create user
    // Save to repo
    // Return output
}
```

```go
// internal/adapters/http/handlers/user_handler.go
// @Summary Create a new user
// @Description Create user with email
// @Tags users
// @Accept json
// @Produce json
// @Param body body CreateUserRequest true "User data"
// @Success 201 {object} UserResponse
// @Router /users [post]
func (h *UserHandler) Create(w http.ResponseWriter, r *http.Request) {
    // Handler implementation
}
```

## GitHub Setup Checklist

```markdown
## ✅ GitHub Setup Checklist

- [ ] Create GitHub repo: https://github.com/{org}/{project-name}
- [ ] Push your local repo:
  \`\`\`bash
  git remote add origin https://github.com/{org}/{project-name}.git
  git push -u origin main
  \`\`\`

- [ ] Configure Secrets:
  Go to Settings → Secrets and variables → Actions
  - [ ] GCP_PROJECT_ID = {gcp-project}
  - [ ] GCP_REGION = us-central1
  - [ ] DOCKER_REGISTRY = gcr.io/{gcp-project}

- [ ] Enable Branch Protection:
  Go to Settings → Branches → Add rule
  - Branch name pattern: \`main\`
  - [x] Require pull request reviews (1 approval)
  - [x] Require status checks to pass

- [ ] Generate OpenAPI:
  \`\`\`bash
  go install github.com/swaggo/swag/cmd/swag@latest
  swag init -g cmd/api/main.go
  \`\`\`

- [ ] Set up Cloud Run:
  \`\`\`bash
  gcloud run deploy {project-name} \\
    --source . \\
    --region us-central1 \\
    --allow-unauthenticated
  \`\`\`
```

## Success Output

After completion:

```
{project-name}/
├── cmd/api/
│   └── main.go
├── internal/
│   ├── domain/
│   ├── usecases/
│   ├── adapters/
│   │   ├── http/
│   │   │   ├── handlers/
│   │   │   └── middleware/
│   │   └── persistence/
│   └── shared/
├── pkg/
│   ├── logger/
│   └── errors/
├── docs/
├── migrations/
├── tests/
├── .github/workflows/
├── Dockerfile
├── docker-compose.yml
├── go.mod
├── CLAUDE.md (customized)
├── .env.example
└── .git (initialized)
```

## Next Steps

1. Push to GitHub (use checklist)
2. Generate OpenAPI: `swag init`
3. Run tests: `go test ./...`
4. Start with: `docker-compose up`
5. Access API at: http://localhost:8080

---

For detailed architecture, see CLAUDE.md in the generated project.
