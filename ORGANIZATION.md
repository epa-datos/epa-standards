# EPA Digital - Estándares de Organización

Principios, nomenclatura y estándares globales para EPA Digital.

## 📋 Principios

### 1. Código es Documentación
- **CLAUDE.md en cada repo** es la fuente de verdad
- Ejemplos en el código, no en wikis
- Si cambias arquitectura, actualiza CLAUDE.md

### 2. Simplicidad sobre Elegancia
- Prefiere soluciones simples y mantenibles
- No sobre-ingeniería para casos teóricos
- Si no lo entienden en 5 min, es complejo

### 3. Automatización
- Tests automáticos en cada PR
- Deploy automático a staging
- Scripts para tareas repetitivas

### 4. Seguridad desde el Inicio
- Nunca hardcodear secrets
- Usar .env.example como template
- Branch protection rules en main

---

## 🏗️ Tipos de Repositorios

### Backend API (Go - Hexagonal)
**Cuándo usarlo:** Servicios REST, microservicios, APIs públicas

**Stack:**
- Go 1.22+
- Hexagonal architecture (domain → usecases → adapters)
- OpenAPI/Swagger
- Tests con table-driven pattern
- Docker for deployment

**Repositorio template:** https://github.com/epa-datos/epa-standards-backend

**Ver:** [CLAUDE-go-api.md](./docs/templates/CLAUDE-go-api.md)

---

### Frontend SPA (NextJS - React)
**Cuándo usarlo:** Web apps, dashboards, customer portals

**Stack:**
- NextJS 15+
- React 19 (Server Components)
- Tailwind CSS con tokens OKLCH
- Central API client (apiFetch)
- Vitest + Playwright
- BFF pattern (frontend calls API, not direct DB)

**Repositorio template:** https://github.com/epa-datos/epa-standards-frontend

**Ver:** [CLAUDE-nextjs.md](./docs/templates/CLAUDE-nextjs.md)

---

### Full Stack (NextJS + Go)
**Cuándo usarlo:** Aplicaciones pequeñas a medianas con lógica compartida

**Stack:**
- NextJS 15+ (frontend + API routes)
- Go-style business logic en `server/` (hexagonal)
- Tailwind CSS
- Tests integrados
- Single Docker image

**Repositorio template:** https://github.com/epa-datos/epa-standards-fullapp

**Ver:** [CLAUDE-fullapp.md](./docs/templates/CLAUDE-fullapp.md)

---

### Internal Client (Go)
**Cuándo usarlo:** SDKs, librerías internas para consumir APIs

**Stack:**
- Go 1.22+
- Estructura simple (client.go, types.go)
- Unit tests con httptest
- Publicable en GitHub Packages

**Repositorio template:** Usa `go-client-scaffold` skill

---

## 📝 Nomenclatura

### Repositorios
```
Format: {tipo}-{nombre}
Ejemplos:
  epa-standards
  epa-standards-backend
  api-users
  web-dashboard
  client-analytics
```

**Convención:**
- Lowercase
- Hyphens (no underscores)
- Descriptivo pero conciso

---

### Ramas
```
Prefixes:
  feature/xxx     - Nueva feature
  fix/xxx         - Bug fix
  refactor/xxx    - Mejora sin cambio funcional
  docs/xxx        - Documentación
  test/xxx        - Tests
  chore/xxx       - Actualización de deps, config

Ejemplos:
  feature/user-authentication
  fix/login-timeout
  refactor/user-service
  docs/api-guide
```

**Convención:**
- Lowercase
- Hyphens para separar palabras
- Descriptivo en 3-4 palabras max

---

### Commits
```
Prefixes:
  feat:     - Nueva feature
  fix:      - Bug fix
  docs:     - Documentación
  refactor: - Refactor sin cambio funcional
  test:     - Tests
  chore:    - Deps, config
  perf:     - Performance

Ejemplos:
  feat: add user authentication
  fix: resolve login timeout
  docs: update API documentation
  refactor: simplify user service
```

**Convención:**
- Lowercase
- Imperativo ("add", no "adds", "added")
- Primera línea <50 chars
- Detalle en líneas adicionales si es necesario

---

### Archivos y Directorios

#### Go (Backend)
```
cmd/api/              - Entry point
internal/domain/      - Business entities
internal/usecases/    - Business logic
internal/adapters/    - HTTP, DB, externos
  /http/handlers/     - HTTP handlers
  /http/middleware/   - HTTP middleware
  /persistence/       - Database
pkg/                  - Utilities compartidas
docs/                 - OpenAPI, diagramas
tests/                - Integration tests
migrations/           - Database migrations
```

**Convención:**
- Lowercase
- Hyphens en multi-word filenames
- `user_handler.go`, `user_test.go`

---

#### TypeScript/NextJS (Frontend)
```
src/
  app/                - Next.js App Router
    (public)/         - Marketing pages
    (auth)/           - Auth pages
    (dashboard)/      - Protected routes
    api/              - API routes
  components/         - UI components
  features/           - Feature modules
  lib/                - Utilities
  __tests__/          - Tests
```

**Convención:**
- Kebab-case para componentes: `login-form.tsx`
- Kebab-case para hooks: `use-auth.ts`
- Kebab-case para directorios: `auth-feature/`
- Lowercase para files

---

## 🔐 Secrets & Environment

### No Hardcodear Secrets

❌ NUNCA:
```
API_KEY=sk-123456789
DATABASE_PASSWORD=admin123
```

✅ SIEMPRE:
```env
# .env.example (commit to repo)
API_KEY=                    # Add in GitHub Secrets
DATABASE_PASSWORD=          # Add in GitHub Secrets

# .env.local (gitignored)
API_KEY=sk-123456789
DATABASE_PASSWORD=admin123
```

### GitHub Secrets
Configurar en: **Settings → Secrets and variables → Actions**

**Backend (Go):**
- GCP_PROJECT_ID
- GCP_REGION
- DOCKER_REGISTRY

**Frontend (NextJS):**
- STAGING_API_BASE_URL
- PRODUCTION_API_BASE_URL
- GCP_PROJECT_ID (if needed)

---

## 📊 Quality Standards

### Code Coverage
- **Goal:** 80%+ unit test coverage
- **Critical paths:** 100% (auth, payments)
- Tools: vitest (frontend), go test (backend)

### Performance
- **Frontend:** Lighthouse score 90+
- **Backend:** <200ms p95 response time
- **Database queries:** <100ms

### TypeScript
- **strict mode:** true
- **noImplicitAny:** true
- **noUnusedLocals:** true

### Go
- **fmt:** All code must be formatted (`gofmt`)
- **vet:** Run `go vet ./...` before PR
- **linter:** `golangci-lint run`

---

## 📦 Dependencies

### Update Policy
- Security updates: Apply immediately
- Minor/patch: Weekly review
- Major: Plan 1-2 sprints ahead

### Allowed Frameworks
**Backend (Go):**
- Frameworks: None (standard library preferred)
- Web: Not needed (http.HandleFunc is enough)
- Database: Firestore client

**Frontend (TypeScript):**
- Framework: React 19+
- Router: Next.js App Router
- Styling: Tailwind CSS
- State: TanStack Query (React Query)
- Testing: Vitest + Playwright

### No External Dependencies For:
- Logging (use stdlib)
- Error handling (use stdlib + custom)
- Basic utilities (write custom or use stdlib)

---

## 🔄 Release Process

### Versioning
Semantic Versioning: MAJOR.MINOR.PATCH

- **MAJOR:** Breaking changes
- **MINOR:** Backward-compatible features
- **PATCH:** Backward-compatible fixes

Example: `v1.2.3`

### Release Workflow
1. Create `release/v1.2.3` branch from `main`
2. Update version numbers
3. Update CHANGELOG
4. Merge to `main`
5. Tag: `git tag v1.2.3`
6. Create GitHub Release
7. Auto-deploy

---

## 📞 Soporte

¿Preguntas sobre estándares?
- Revisa [CONTRIBUTING.md](./CONTRIBUTING.md)
- Abre issue en [epa-standards](https://github.com/epa-datos/epa-standards)
- Contacta a Eddye (manager)

---

**Última actualización:** 2026-05-21
