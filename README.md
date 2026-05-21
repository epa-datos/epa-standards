# EPA Digital - Estándares de Ingeniería

Documentación centralizada y repositorios estándares para EPA Digital.

## 🚀 Comienza Aquí

Eres nuevo en EPA Digital? Clona uno de nuestros repos estándares:

### 1. Backend API (Go - Hexagonal)
```bash
git clone https://github.com/epa-datos/epa-standards-backend.git my-api
cd my-api
# Abre CLAUDE.md y comienza a desarrollar
```

### 2. Frontend Web App (NextJS - Tailwind)
```bash
git clone https://github.com/epa-datos/epa-standards-frontend.git my-app
cd my-app
npm install
npm run dev
```

### 3. Full Stack App (NextJS + API integrada)
```bash
git clone https://github.com/epa-datos/epa-standards-fullapp.git my-fullapp
cd my-fullapp
npm install
npm run dev
```

---

## 📚 Documentación

### Visión General
- **[ORGANIZATION.md](./ORGANIZATION.md)** - Estándares globales, nomenclatura, principios

### Estrategia y Procesos
- **[BRANCHING-STRATEGY.md](./docs/BRANCHING-STRATEGY.md)** - Git workflow, branch naming, PR process
- **[CONTRIBUTING.md](./CONTRIBUTING.md)** - Cómo contribuir, tests, commits

### Por Tipo de Proyecto
- **[CLAUDE-go-api.md](./docs/templates/CLAUDE-go-api.md)** - Guía para APIs Go
- **[CLAUDE-nextjs.md](./docs/templates/CLAUDE-nextjs.md)** - Guía para frontends NextJS
- **[CLAUDE-fullapp.md](./docs/templates/CLAUDE-fullapp.md)** - Guía para full stacks

### GitHub & CI/CD
- **[GitHub Workflows](./docs/github-workflows.md)** - Workflows test/deploy
- **[Secrets & Configuration](./docs/secrets.md)** - Variables de entorno y secrets

---

## 🏗️ Repositorios Estándares

### epa-standards-backend
**Para APIs Go con arquitectura hexagonal**

```
epa-standards-backend/
├── cmd/api/               # Entry point
├── internal/
│   ├── domain/           # Business logic (sin deps externas)
│   ├── usecases/         # Orchestración
│   └── adapters/         # HTTP, DB, externos
├── pkg/                  # Utilidades compartidas
├── docs/                 # OpenAPI, diagramas
├── .github/workflows/    # CI/CD
├── CLAUDE.md            # Guía completa
└── Dockerfile           # Multi-stage
```

**Quick start:**
```bash
git clone https://github.com/epa-datos/epa-standards-backend.git
cd epa-standards-backend
go mod download
go run ./cmd/api
```

### epa-standards-frontend
**Para apps NextJS con Tailwind OKLCH**

```
epa-standards-frontend/
├── src/
│   ├── app/             # Next.js app router
│   ├── components/      # Shared components
│   ├── features/        # Feature modules (domain-driven)
│   └── lib/             # Utilities (api-client, query-client, etc.)
├── __tests__/           # Tests (unit, E2E)
├── public/              # Static assets
├── .github/workflows/   # CI/CD
├── CLAUDE.md           # Guía completa
└── tailwind.config.ts  # Colores OKLCH
```

**Quick start:**
```bash
git clone https://github.com/epa-datos/epa-standards-frontend.git
cd epa-standards-frontend
npm install
npm run dev          # http://localhost:3000
npm run test         # Tests en watch mode
```

### epa-standards-fullapp
**Para full stacks: NextJS + API integrada**

```
epa-standards-fullapp/
├── src/
│   ├── app/             # Next.js routes + RPC endpoints
│   ├── components/      # UI components
│   ├── features/        # Feature modules
│   ├── lib/             # Client utilities
│   └── api/             # Server actions
├── server/              # Backend logic (hexagonal)
│   ├── domain/
│   ├── usecases/
│   └── adapters/
├── __tests__/           # Tests
├── .github/workflows/   # CI/CD
├── CLAUDE.md           # Guía completa
└── Dockerfile          # Full stack image
```

**Quick start:**
```bash
git clone https://github.com/epa-datos/epa-standards-fullapp.git
cd epa-standards-fullapp
npm install
npm run dev          # http://localhost:3000
```

---

## 🔄 Git Workflow

```
feature/xxx          staging          main
   ↓                   ↓                ↓
  [PR]→ [merge] → [tests & deploy] → [tests & deploy to prod]
                       ↓                ↓
                   staging env       production env
```

**Resumen:**
1. Crea rama: `feature/`, `fix/`, `refactor/`
2. Abre PR a `staging`
3. Espera tests + approval
4. Merge a `staging` (auto-deploys)
5. Cuando esté listo: PR `staging` → `main`
6. Release y deploy a prod

Ver [BRANCHING-STRATEGY.md](./docs/BRANCHING-STRATEGY.md) para detalles.

---

## 📊 Tipos de Repos

| Tipo | Stack | Deploy | Repo Estándar |
|------|-------|--------|---|
| **Backend API** | Go 1.22+ | Cloud Run | epa-standards-backend |
| **Frontend SPA** | NextJS 15+ | Cloud Run | epa-standards-frontend |
| **Full Stack** | NextJS + Go | Cloud Run | epa-standards-fullapp |
| **Internal Client** | Go 1.22+ | GitHub Packages | N/A (simple) |

---

## 🛠️ Herramientas Recomendadas

### IDE
- **VS Code** + extensions: Go, ES7+, Prettier, ESLint

### CLI Tools
- `gh` - GitHub CLI
- `gcloud` - Google Cloud SDK
- `docker` - Docker for containers

### Language Tools
- **Go:** `golangci-lint`, `swag` (OpenAPI)
- **Node:** `pnpm` (preferido) o `npm`

---

## 🚀 Para el Equipo Existente

Si ya estás en EPA Digital, **no clones** los repos estándares. En su lugar:

1. Revisa [BRANCHING-STRATEGY.md](./docs/BRANCHING-STRATEGY.md)
2. Revisa el [CONTRIBUTING.md](./CONTRIBUTING.md) del repo
3. Sigue [CLAUDE.md](./CLAUDE.md) en cada repo

---

## 🤖 Skills Disponibles

**6 Skills creados** para estandarizar desarrollo en EPA Digital:

### Para Scaffolding
- **`nextjs-scaffold`** - Crea nuevo repo NextJS con estándares
- **`go-api-scaffold`** - Crea nuevo Go API con hexagonal architecture
- **`go-client-scaffold`** - Crea librería cliente Go reutilizable

### Para Validación y Workflow
- **`validate-pr-format`** - Valida PRs antes de submitear
- **`git-flow-guide`** - Guía interactiva de branching strategy
- **`generate-openapi`** - Genera/actualiza docs OpenAPI para Go APIs

**Ubicación:** [./skills](./skills) | **Detalle:** [skills/README.md](./skills/README.md)

### Para Importar a Claude Team Enterprise:
1. Ve a Organization → Skills
2. Crea nuevo skill
3. Copia contenido de `./skills/{skill}/SKILL.md`
4. Guarda

---

## 🔌 MCPs (Próximamente)

Estamos creando:
- **epa-standards MCP:** Proporciona estándares a Claude automáticamente
- **epa-github MCP:** Integración con GitHub workflows y validation

Especificación: [SKILLS-MCPS-GUIDE.md](./SKILLS-MCPS-GUIDE.md)

---

## 📞 Soporte

¿Preguntas?
- Revisa [CONTRIBUTING.md](./CONTRIBUTING.md)
- Abre issue en [epa-standards](https://github.com/epa-datos/epa-standards)
- Contacta a Eddye (manager)

---

## 📄 Licencia

Interno - EPA Digital

---

**Última actualización:** 2026-05-21
