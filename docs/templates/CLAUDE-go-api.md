# CLAUDE.md Template - Go API

Este es un resumen de la guía completa para Go APIs en EPA Digital.

**Para la guía completa, ve a:** https://github.com/epa-datos/epa-standards-backend/blob/main/CLAUDE.md

## 🏗️ Hexagonal Architecture

```
┌─────────────────────────────┐
│  HTTP Layer (Adapters)      │ ← Incoming requests
│  - Handlers                 │   - Parse input
│  - Middleware               │   - Call usecases
└────────────┬────────────────┘
             │
┌────────────▼────────────────┐
│  Usecases Layer             │ ← Business logic
│  - Orchestration            │   - Validation
│  - Business rules           │   - Decisions
└────────────┬────────────────┘
             │
┌────────────▼────────────────┐
│  Domain Layer               │ ← Entities
│  - User, Order, etc.        │   - Interfaces
│  - Pure logic               │   - No dependencies
└────────────┬────────────────┘
             │
┌────────────▼────────────────┐
│  Adapters (External)        │ ← Outgoing
│  - Database                 │   - Firestore
│  - External APIs            │   - 3rd party
└─────────────────────────────┘
```

## 📁 Directory Structure

```
cmd/api/
  └── main.go           # Entry point, DI setup

internal/
  ├── domain/           # Business entities (no imports)
  │   └── user.go
  ├── usecases/         # Business logic
  │   ├── create_user.go
  │   └── get_user.go
  ├── adapters/
  │   ├── http/
  │   │   ├── handlers/    # HTTP handlers
  │   │   │   └── user_handler.go
  │   │   └── middleware/  # Middleware
  │   │       └── middleware.go
  │   └── persistence/     # Database
  │       └── user_repository.go
  └── shared/
      ├── logger/
      └── errors/

pkg/
  ├── logger/           # Utilities
  └── ...
```

## 💻 Quick Commands

```bash
# Run
go run ./cmd/api

# Test
go test ./...
go test -cover ./...

# Format
go fmt ./...

# Lint
golangci-lint run

# OpenAPI docs
swag init -g cmd/api/main.go
# View: http://localhost:8080/swagger/index.html
```

## 🧪 Testing Pattern - Table-Driven

```go
func TestCreateUser(t *testing.T) {
  tests := []struct {
    name    string
    email   string
    wantErr bool
  }{
    {"valid", "user@example.com", false},
    {"invalid", "invalid", true},
  }
  
  for _, tt := range tests {
    t.Run(tt.name, func(t *testing.T) {
      err := CreateUser(tt.email)
      if (err != nil) != tt.wantErr {
        t.Errorf("got %v, want %v", err, tt.wantErr)
      }
    })
  }
}
```

## 🔐 Environment Variables

```bash
# .env.example
PORT=8080
GCP_PROJECT_ID=my-project
DATABASE_URL=
```

## 🚀 Deployment

```bash
# Docker build
docker build -t my-api:latest .

# Docker run
docker run -p 8080:8080 my-api:latest

# Cloud Run
gcloud run deploy my-api \
  --source . \
  --region us-central1 \
  --allow-unauthenticated
```

---

**Full guide:** https://github.com/epa-datos/epa-standards-backend/blob/main/CLAUDE.md
