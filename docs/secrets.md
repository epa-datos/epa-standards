# Secrets & Environment Configuration

EPA Digital's guide for managing environment variables and secrets.

## 🔐 Golden Rule

**NEVER hardcode secrets. EVER.**

- ❌ API keys in code
- ❌ Database passwords in config
- ❌ OAuth tokens in files
- ❌ Private keys in repo

---

## 📝 Environment Variables

### Public Variables (Safe to Commit)

Use `NEXT_PUBLIC_*` (frontend only) or in `.env.example`:

```bash
# .env.example (COMMIT THIS)
NEXT_PUBLIC_API_BASE_URL=http://localhost:8080/api/v1
NEXT_PUBLIC_SITE_URL=http://localhost:3000
NODE_ENV=development
LOG_LEVEL=debug
```

**Rule:** Only non-sensitive config here.

---

### Secret Variables (Never Commit)

Create `.env.local` (gitignored):

```bash
# .env.local (DON'T COMMIT)
API_KEY=sk-123456789abcdef
DATABASE_PASSWORD=secure_password_here
OAUTH_SECRET=secret_oauth_key
```

**Always in `.gitignore`:**
```
.env.local
.env.*.local
.env.development.local
.env.test.local
.env.production.local
```

---

## 🔑 GitHub Secrets

### Setup

1. Go to repository
2. Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `SECRET_NAME`
5. Value: (paste secret value)
6. Click "Add secret"

---

### Types of Secrets

#### Google Cloud (GCP)

```
GCP_PROJECT_ID
  Value: my-project-123456

GCP_REGION
  Value: us-central1

GCP_SA_KEY
  Value: (JSON service account key)

DOCKER_REGISTRY
  Value: gcr.io/my-project-123456
```

**How to get GCP_SA_KEY:**
1. Go to https://console.cloud.google.com/iam-admin/serviceaccounts
2. Click service account
3. Keys tab → Add Key → Create new
4. Download JSON
5. Copy entire JSON content to GitHub Secret

---

#### API Configuration

```
STAGING_API_BASE_URL
  Value: https://api-staging.example.com

PRODUCTION_API_BASE_URL
  Value: https://api.example.com
```

---

#### OAuth / Third-party

```
GITHUB_TOKEN
  Value: ghp_xxxx... (GitHub personal access token)

GOOGLE_CLIENT_ID
  Value: xxxxxx.apps.googleusercontent.com

GOOGLE_CLIENT_SECRET
  Value: GOCSPX-xxxxxx
```

---

### Using Secrets in Workflows

```yaml
- name: Deploy
  env:
    GCP_PROJECT_ID: ${{ secrets.GCP_PROJECT_ID }}
    GCP_SA_KEY: ${{ secrets.GCP_SA_KEY }}
  run: |
    echo $GCP_SA_KEY > /tmp/gcp-key.json
    gcloud auth activate-service-account \
      --key-file=/tmp/gcp-key.json
```

---

### Using Secrets in Code

**NextJS Frontend:**
```ts
// This is available in the browser
const apiUrl = process.env.NEXT_PUBLIC_API_BASE_URL

// This is server-only
const apiSecret = process.env.API_SECRET  // Only in API routes, not in components
```

**Go Backend:**
```go
apiKey := os.Getenv("API_KEY")
if apiKey == "" {
  log.Fatal("API_KEY not set")
}
```

---

## 🌍 Environment-Specific Variables

### Development (Local)

File: `.env.local` (gitignored)

```bash
NODE_ENV=development
NEXT_PUBLIC_API_BASE_URL=http://localhost:8080/api/v1
NEXT_PUBLIC_SITE_URL=http://localhost:3000
API_SECRET=dev-secret-only
```

---

### Staging (Pre-production)

File: GitHub Secrets (accessed in workflow)

```
GCP_PROJECT_ID = staging-project
GCP_REGION = us-central1
STAGING_API_BASE_URL = https://api-staging.example.com
DATABASE_URL = (staging database)
```

---

### Production

File: GitHub Secrets (accessed in workflow)

```
GCP_PROJECT_ID = production-project
GCP_REGION = us-central1
PRODUCTION_API_BASE_URL = https://api.example.com
DATABASE_URL = (production database)
```

---

## 🔒 Best Practices

### DO ✅

- Use environment variables for config
- Use GitHub Secrets for sensitive data
- Rotate secrets quarterly
- Use unique secrets per environment
- Log secret name (not value) when debugging
- Validate secrets exist on startup

### DON'T ❌

- Log secrets (print, console.log, etc.)
- Commit secrets to git
- Share secrets in chat/email
- Use same secret across environments
- Hardcode fallback values
- Store secrets in comments

---

## 🚨 Accidental Secret Leak?

If you accidentally commit a secret:

1. **Don't panic** - rotate it immediately
2. **Remove from git:**
   ```bash
   git filter-branch --force --index-filter \
     'git rm --cached --ignore-unmatch .env.local' \
     HEAD
   git push origin main --force-with-lease
   ```
3. **Rotate secret** in all systems
4. **Tell the team**
5. **Update GitHub Secret** with new value

Or use: https://github.com/gitleaks/gitleaks (automated scanning)

---

## 🔍 Validation at Startup

Always validate required secrets exist:

**TypeScript:**
```ts
// lib/env.ts
const requiredEnvVars = [
  'NEXT_PUBLIC_API_BASE_URL',
  'NEXT_PUBLIC_SITE_URL',
] as const

for (const envVar of requiredEnvVars) {
  if (!process.env[envVar]) {
    throw new Error(`Missing required env var: ${envVar}`)
  }
}

export const env = {
  NEXT_PUBLIC_API_BASE_URL: process.env.NEXT_PUBLIC_API_BASE_URL!,
  NEXT_PUBLIC_SITE_URL: process.env.NEXT_PUBLIC_SITE_URL!,
}
```

**Go:**
```go
package config

import (
  "log"
  "os"
)

func init() {
  required := []string{"API_KEY", "DATABASE_URL"}
  
  for _, key := range required {
    if os.Getenv(key) == "" {
      log.Fatalf("Missing required env var: %s", key)
    }
  }
}
```

---

## 📋 Checklist: Setup New Project

When creating a new project:

- [ ] Create `.env.example` with template variables
- [ ] Add `.env.local` to `.gitignore`
- [ ] Create GitHub Secrets:
  - [ ] GCP_PROJECT_ID
  - [ ] GCP_REGION
  - [ ] DOCKER_REGISTRY
  - [ ] API_BASE_URL (staging)
  - [ ] API_BASE_URL (production)
- [ ] Add environment validation to code
- [ ] Update documentation with required secrets
- [ ] Test with sample values
- [ ] Rotate initial secrets after first deploy

---

## 📞 Support

- Need a secret added? Contact Eddye
- Secret leaked? Tell team immediately
- Questions? Check this guide or ask in GitHub

---

**Last updated:** 2026-05-21
