# CLAUDE.md Template - Full Stack

Este es un resumen de la guía completa para full stacks en EPA Digital.

**Para la guía completa, ve a:** https://github.com/epa-datos/epa-standards-fullapp/blob/main/CLAUDE.md

## 🏗️ Full Stack Architecture

```
┌──────────────────────┐
│  React Components    │ ← Frontend UI
│  (src/app/)          │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│  API Routes / Hooks  │ ← Integration layer
│  (src/app/api/)      │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│  Server Logic        │ ← Business logic
│  (server/ - Go style)│   - Hexagonal
└──────────┬───────────┘   - Domain-driven
           │
┌──────────▼───────────┐
│  Firestore / APIs    │ ← Data layer
└──────────────────────┘
```

## 📁 Directory Structure

```
src/
├── app/               # NextJS App Router + API routes
│   ├── (dashboard)/   # Protected routes
│   ├── api/           # API endpoints
│   └── layout.tsx
├── components/        # UI components
├── lib/               # Utilities
└── __tests__/         # Tests

server/               # Business logic (hexagonal)
├── domain/            # Entities
│   └── user.ts
├── usecases/          # Use cases
│   └── create-user.ts
└── adapters/
    └── persistence/   # Database
```

## 💻 Quick Commands

```bash
# Run
npm run dev

# Test
npm run test:run
npm run test:e2e

# Quality
npm run lint
npm run typecheck
```

## 🎨 Design

Use semantic Tailwind tokens (OKLCH):

```tsx
<div className="bg-background text-foreground">
  <button className="bg-primary text-primary-foreground">
    Click
  </button>
</div>
```

## 🏛️ Server Logic Pattern

```ts
// 1. Domain (entities)
export interface User {
  id: string
  email: string
}

// 2. Usecase (business logic)
export class CreateUserUseCase {
  async execute(email: string) { ... }
}

// 3. API Route (handler)
export async function POST(req: Request) {
  const useCase = new CreateUserUseCase(repo)
  return useCase.execute(...)
}
```

## 🧪 Testing

```tsx
// Unit test (server logic)
import { CreateUserUseCase } from '@/server/usecases'

it('creates user', async () => {
  const result = await useCase.execute('user@example.com')
  expect(result.email).toBe('user@example.com')
})

// E2E test
test('user signup flow', async ({ page }) => {
  await page.goto('/signup')
  await page.fill('[data-testid=email]', 'new@example.com')
  await page.click('[data-testid=signup]')
  await expect(page).toHaveURL('/dashboard')
})
```

## 🔐 Environment

```bash
# .env.example
NEXT_PUBLIC_API_BASE_URL=http://localhost:3000/api
NEXT_PUBLIC_SITE_URL=http://localhost:3000
```

---

**Full guide:** https://github.com/epa-datos/epa-standards-fullapp/blob/main/CLAUDE.md
