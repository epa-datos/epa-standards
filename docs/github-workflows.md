# GitHub Workflows - CI/CD

EPA Digital's automated testing and deployment workflows.

## 📚 Workflow Files

All workflows are in `.github/workflows/`:

### `test.yml`
Runs on every push and PR.

**Runs:**
- TypeScript/Go linting
- Unit tests
- TypeScript strict mode check

**Status:** ✅ Passing required for merge

---

### `deploy-staging.yml`
Runs on merge to `staging`.

**Deploys to:** Staging environment  
**Duration:** ~5-10 minutes  
**Auto-triggers:** Yes (on staging merge)

---

### `deploy-production.yml`
Runs on merge to `main`.

**Deploys to:** Production environment  
**Duration:** ~10-15 minutes  
**Auto-triggers:** Yes (on main merge)

---

## ✅ Test Workflow (`test.yml`)

Runs on:
- Every push to any branch
- Every pull request

**Steps:**

### 1. Setup
```yaml
- uses: actions/checkout@v4
- uses: actions/setup-node@v4
  with:
    node-version: 20
```

### 2. Dependencies
```bash
npm ci          # Clean install
npm run build   # Build
```

### 3. Type Check
```bash
npm run typecheck  # TypeScript strict mode
```

### 4. Lint
```bash
npm run lint       # ESLint
```

### 5. Tests
```bash
npm run test:run   # Unit tests
npm run test:e2e   # E2E tests
```

**Result:** ✅ Pass or ❌ Fail (blocks merge)

---

### For Go Backends

```yaml
- uses: actions/checkout@v4
- uses: actions/setup-go@v4
  with:
    go-version: 1.22

- run: go mod download
- run: go test ./...
- run: golangci-lint run
```

---

## 🚀 Deploy Workflow (`deploy-staging.yml`)

Runs on:
- Merge to `staging` branch

**Steps:**

### 1. Build Docker Image
```bash
docker build -t gcr.io/$GCP_PROJECT/$APP_NAME:$SHA .
```

### 2. Push to Registry
```bash
docker push gcr.io/$GCP_PROJECT/$APP_NAME:$SHA
```

### 3. Deploy to Cloud Run
```bash
gcloud run deploy $APP_NAME \
  --image gcr.io/$GCP_PROJECT/$APP_NAME:$SHA \
  --region us-central1 \
  --set-env-vars GCP_PROJECT_ID=$GCP_PROJECT_ID
```

### 4. Verify Health
```bash
curl https://$APP_NAME-staging.run.app/health
```

**Result:** ✅ Deployed to staging

---

## 🔐 Required Secrets

Configure in GitHub Settings → Secrets and variables → Actions

### Google Cloud (GCP)

```
GCP_PROJECT_ID        = my-project-123456
GCP_REGION            = us-central1
DOCKER_REGISTRY       = gcr.io/my-project-123456
```

### For Service Account Auth

```
GCP_SA_KEY            = (JSON key from service account)
```

Or use Workload Identity Federation:

```
GCP_WORKLOAD_IDENTITY_PROVIDER = projects/123456/locations/global/workloadIdentityPools/pool/providers/provider
GCP_SERVICE_ACCOUNT_EMAIL      = github-actions@my-project.iam.gserviceaccount.com
```

### API Configuration

```
STAGING_API_BASE_URL      = https://api-staging.example.com
PRODUCTION_API_BASE_URL   = https://api.example.com
```

---

## 📊 Workflow Status Checks

### Required Checks (Before Merge)

All of these must be ✅:

- `test (lint)` - ESLint passing
- `test (typecheck)` - TypeScript strict mode
- `test (tests)` - Unit tests passing
- `test (e2e)` - E2E tests passing (if applicable)

---

## 🔔 Notifications

### Slack Notifications

Sent automatically:

- ✅ PR created
- ✅ PR approved
- ❌ Tests failed
- ✅ Merge to staging
- ✅ Deploy started
- ✅ Deploy completed
- ❌ Deploy failed

---

## 🛠️ Manual Deployment

If automated deploy fails:

### Deploy Staging Manually
```bash
gcloud run deploy my-api \
  --source . \
  --region us-central1 \
  --set-env-vars GCP_PROJECT_ID=my-project
```

### Deploy Production Manually
```bash
gcloud run deploy my-api \
  --source . \
  --region us-central1 \
  --set-env-vars GCP_PROJECT_ID=my-project \
  --no-traffic  # Don't route traffic yet

# After verification
gcloud run services update-traffic my-api --to-revisions LATEST=100
```

---

## 🐛 Debugging Workflows

### View Logs
1. Go to repository
2. Click "Actions" tab
3. Click workflow run
4. Click job
5. Expand failed step

### Common Issues

**"Docker build failed"**
- Check Dockerfile syntax
- Ensure dependencies are in package.json / go.mod
- Verify base image is available

**"Out of memory"**
- Optimize Docker image (remove dev dependencies)
- Increase Cloud Run memory allocation

**"Secret not found"**
- Check secret name spelling
- Ensure it's configured in Settings
- Restart workflow

**"Deployment timed out"**
- Check service startup logs
- Ensure health check endpoint exists
- Increase timeout in workflow

---

## 📈 Best Practices

### ✅ DO

- Run tests before push
- Fix failing CI before merge
- Use meaningful commit messages
- Review CI logs on failure
- Document workflow changes

### ❌ DON'T

- Ignore failing tests
- Merge without CI passing
- Hardcode secrets in code
- Skip security checks
- Push to main without PR

---

## 🔗 Monitoring

### Cloud Run Metrics
View at: https://console.cloud.google.com/run

Metrics include:
- Request latency
- Error rate
- CPU/Memory usage
- Concurrent requests

---

## 📝 Workflow Configuration Example

### `.github/workflows/test.yml`
```yaml
name: Test

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      
      - run: npm ci
      - run: npm run typecheck
      - run: npm run lint
      - run: npm run test:run
```

---

**Last updated:** 2026-05-21
