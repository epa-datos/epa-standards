# EPA Digital Skills - Index

Complete set of skills for standardizing EPA Digital development. All skills are ready for import into Claude Team Enterprise.

## 🎯 Skills Overview

| Skill | Purpose | Trigger Keywords |
|-------|---------|------------------|
| **nextjs-scaffold** | Create new NextJS repos | "create NextJS repo", "scaffold Next app" |
| **go-api-scaffold** | Create new Go APIs | "create Go API", "hexagonal scaffold" |
| **go-client-scaffold** | Create Go clients | "create Go client", "new client library" |
| **validate-pr-format** | Validate PRs | "check PR format", "validate PR" |
| **generate-openapi** | Generate OpenAPI specs | "update OpenAPI", "generate Postman" |
| **git-flow-guide** | Git workflow guide | "git workflow", "how do I create a PR" |

## 📁 Directory Structure

```
skills/
├── README.md (this file)
├── nextjs-scaffold/
│   └── SKILL.md
├── go-api-scaffold/
│   └── SKILL.md
├── go-client-scaffold/
│   └── SKILL.md
├── validate-pr-format/
│   └── SKILL.md
├── generate-openapi/
│   └── SKILL.md
└── git-flow-guide/
    └── SKILL.md
```

## 🚀 Quick Start

Each skill folder contains a `SKILL.md` file that can be imported directly into Claude Team Enterprise.

### To Use a Skill:

1. Go to Claude Team Enterprise → Skills (Organization level)
2. Click "New Skill"
3. Paste the contents of `SKILL.md`
4. Save

Or: Create new skill using the file directly.

## 📝 Skill Descriptions

### 1. NextJS Scaffold
**File:** `nextjs-scaffold/SKILL.md`

Creates a complete NextJS repository with EPA Digital standards:
- Directory structure with features, components, lib
- TailwindCSS with OKLCH semantic tokens
- Central API client (apiFetch)
- GitHub workflows for testing and deployment
- CLAUDE.md customized for the project
- Ready to push to GitHub

**Use:** `"Create a new NextJS app for the dashboard"`

---

### 2. Go API Scaffold
**File:** `go-api-scaffold/SKILL.md`

Creates a complete Go API with hexagonal architecture:
- Domain layer (entities)
- Usecases layer (business logic)
- Adapters (HTTP handlers, persistence)
- Dependency injection setup
- Docker support
- OpenAPI/Swagger ready
- GitHub workflows for testing and Cloud Run
- Example code (User domain, handlers, tests)

**Use:** `"Create a new Go API for the user service"`

---

### 3. Go Client Scaffold
**File:** `go-client-scaffold/SKILL.md`

Creates a focused Go client library:
- Simple client structure
- Request/response types
- Error handling
- Retry logic
- Unit tests with httptest
- Example usage
- Ready to publish

**Use:** `"Create a Go client for the analytics service"`

---

### 4. Validate PR Format
**File:** `validate-pr-format/SKILL.md`

Validates PRs before submission:
- Checks title format (feat:, fix:, etc.)
- Validates branch name (feature/, fix/, etc.)
- Verifies checklist completion
- Suggests fixes for issues
- Provides quality checklist

**Use:** `"Check if my PR is ready to submit"`

---

### 5. Generate OpenAPI
**File:** `generate-openapi/SKILL.md`

Generates and maintains API documentation:
- Analyzes Go handlers for OpenAPI comments
- Generates docs/swagger.json
- Exports Postman collections
- Validates spec consistency
- Detects missing documentation
- Optional Postman workspace upload

**Use:** `"Update OpenAPI docs for my Go API"`

---

### 6. Git Flow Guide
**File:** `git-flow-guide/SKILL.md`

Interactive guide for branching and PRs:
- Branch type recommendations (feature/, fix/, docs/, etc.)
- Step-by-step workflow examples
- PR review requirements
- Common mistakes and fixes
- Quick reference commands

**Use:** `"How do I create a PR?" or "What branch should I use?"`

---

## 🔗 Related Documentation

- [CLAUDE.md standards](../docs/CLAUDE.md) - How CLAUDE.md files work
- [Branch protection rules](../docs/BRANCHING-STRATEGY.md) - Git workflow
- [PR template](../../epa-standards/PULL_REQUEST_TEMPLATE.md) - Standard PR format

---

## ✅ Checklist for Implementation

Before rolling out to team:

- [ ] All 6 skills created and tested
- [ ] Each skill imported into Claude Team Enterprise
- [ ] Skills enabled in organization
- [ ] Team trained on skill availability
- [ ] Team given access to skill documentation

---

**Status:** ✅ Ready for import

Last updated: 2026-05-21
