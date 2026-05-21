---
name: nextjs-scaffold
description: Create a new NextJS repository with EPA Digital standards
keywords:
  - create NextJS repo
  - new Next project
  - scaffold Next app
  - NextJS template
---

# NextJS Scaffold - EPA Digital Standard

Create a new NextJS repository with EPA Digital standards, architecture, and GitHub workflows.

## What This Skill Does

1. ✅ Clones the EPA Digital NextJS template
2. ✅ Renames project and updates package.json
3. ✅ Customizes CLAUDE.md with project-specific paths
4. ✅ Sets up GitHub workflows (.github/workflows/)
5. ✅ Initializes Git with initial commit
6. ✅ Provides GitHub setup checklist

## Inputs

Ask the user for:

- **Project name** (e.g., "my-dashboard", "customer-portal")
  - Validate: kebab-case, 2-50 characters, no special chars
- **GitHub organization** (e.g., "epa-datos")
- **GCP project ID** (for Cloud Run deployment)
- **Staging API URL** (e.g., "https://api-staging.example.com/api/v1")
- **Production API URL** (e.g., "https://api.example.com/api/v1")

## Workflow

```
1. Validate inputs
   └─ Project name format
   └─ GitHub org access
   └─ GCP project exists

2. Clone template
   └─ git clone https://github.com/epa-datos/epa-standards-frontend.git {project-name}

3. Update files
   ├─ package.json
   │  └─ name: {project-name}
   │  └─ description: customized
   ├─ src/lib/env.ts
   │  └─ Update API_BASE_URL default
   ├─ CLAUDE.md
   │  └─ Replace {PROJECT_NAME} with actual name
   └─ tailwind.config.ts
      └─ Update any hardcoded paths

4. Add GitHub workflows
   ├─ .github/workflows/test.yml
   ├─ .github/workflows/deploy-staging.yml
   └─ .github/workflows/deploy-production.yml

5. Initialize Git
   ├─ git init
   ├─ git add .
   ├─ git commit -m "initial: {project-name}"
   └─ git branch -M main

6. Provide checklist
   └─ GitHub repo creation instructions
   └─ Secrets configuration
   └─ Branch protection rules
   └─ CODEOWNERS setup
```

## GitHub Setup Checklist

Provide user with this after scaffolding:

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
  - [ ] STAGING_API_BASE_URL = {staging-url}
  - [ ] PRODUCTION_API_BASE_URL = {production-url}
  - [ ] GCP_PROJECT_ID = {gcp-project}

- [ ] Enable Branch Protection:
  Go to Settings → Branches → Add rule
  - Branch name pattern: \`main\`
  - [x] Require pull request reviews (1 approval)
  - [x] Require status checks to pass
  - [x] Dismiss stale PR approvals

- [ ] Add CODEOWNERS:
  Create .github/CODEOWNERS with team email addresses

- [ ] Invite team members as collaborators
```

## Success Output

After completion, user should have:

```
{project-name}/
├── src/
│   ├── app/
│   ├── components/
│   ├── features/
│   ├── lib/
│   └── ...
├── .github/
│   └── workflows/
│       ├── test.yml
│       ├── deploy-staging.yml
│       └── deploy-production.yml
├── CLAUDE.md (customized)
├── package.json (updated)
├── .env.example
├── README.md
└── .git (initialized)
```

## Next Steps

1. Push to GitHub (use checklist above)
2. Read CLAUDE.md in the project
3. Run `npm install` then `npm run dev`
4. Start building features in `src/features/`

---

For detailed architecture info, see CLAUDE.md in the generated project.
