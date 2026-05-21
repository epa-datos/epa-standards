# CLAUDE.md Template - NextJS Frontend

Este es un resumen de la guía completa para frontends NextJS en EPA Digital.

**Para la guía completa, ve a:** https://github.com/epa-datos/epa-standards-frontend/blob/main/CLAUDE.md

## 🏗️ BFF Architecture

```
┌──────────────────────┐
│  React Components    │ ← User interface
│  (UI + State)        │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│  Hooks               │ ← State management
│  (useAuth, etc.)     │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│  Services            │ ← Business logic
│  (getUser, etc.)     │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│  API Client          │ ← HTTP calls
│  (apiFetch)          │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│  Go API Backend      │ ← Data layer
│  (http://api:8080)   │
└──────────────────────┘
```

## 📁 Directory Structure

```
src/
├── app/               # Next.js App Router
│   ├── (public)/      # Marketing pages
│   ├── (auth)/        # Auth pages
│   ├── (dashboard)/   # Protected routes
│   └── layout.tsx
├── components/        # UI components
├── features/          # Feature modules
│   ├── auth/
│   │   ├── hooks/     # Feature hooks
│   │   ├── services/  # Feature services
│   │   └── types.ts
│   └── ...
├── lib/               # Utilities
│   ├── api-client.ts  # HTTP client (CENTRAL!)
│   └── env.ts         # Environment validation
└── __tests__/         # Tests
```

## 💻 Quick Commands

```bash
# Run
npm run dev

# Test
npm run test          # Watch mode
npm run test:run      # One-shot (CI)
npm run test:e2e      # E2E tests

# Quality
npm run lint
npm run typecheck
npm run format
```

## 🎨 Design - OKLCH Tokens

```tsx
// ❌ DON'T hardcode colors
<div className="bg-gray-900 text-white">

// ✅ DO use semantic tokens
<div className="bg-background text-foreground">
```

**Available tokens:** `background`, `foreground`, `card`, `muted`, `destructive`, `primary`, `secondary`

## 🔌 API Integration Pattern

**Rule:** Components NEVER call `fetch()` directly.

```tsx
// Step 1: Service (calls api-client)
export async function getUser(id: string) {
  return apiFetch<User>(`/users/${id}`)
}

// Step 2: Hook (calls service)
export function useUser(id: string) {
  return useQuery({ queryKey: ['user', id], queryFn: () => getUser(id) })
}

// Step 3: Component (calls hook)
export function UserProfile({ id }: { id: string }) {
  const { data: user } = useUser(id)
  return <div>{user?.name}</div>
}
```

## 🧪 Testing Pattern

**Unit tests (Vitest):**
```tsx
import { render, screen } from '@testing-library/react'
import { Button } from './button'

describe('Button', () => {
  it('renders', () => {
    render(<Button>Click me</Button>)
    expect(screen.getByText('Click me')).toBeInTheDocument()
  })
})
```

**E2E tests (Playwright):**
```ts
test('user can login', async ({ page }) => {
  await page.goto('/login')
  await page.fill('[data-testid=email]', 'user@example.com')
  await page.click('[data-testid=submit]')
  await expect(page).toHaveURL('/dashboard')
})
```

## 🔐 Environment

```bash
# .env.example
NEXT_PUBLIC_API_BASE_URL=http://localhost:8080/api/v1
NEXT_PUBLIC_SITE_URL=http://localhost:3000
```

---

**Full guide:** https://github.com/epa-datos/epa-standards-frontend/blob/main/CLAUDE.md
