# nextjs-scaffold

Create a new NextJS repository following EPA Digital standards.

## Details

- **Type:** Scaffold/Generator
- **Trigger:** "create NextJS repo", "new Next project", "scaffold Next app"
- **Language:** TypeScript
- **Stack:** Next.js 15, Tailwind CSS, Vitest, Playwright

## What This Skill Does

1. Clones epa-standards-frontend as a template
2. Updates project name and configuration
3. Customizes CLAUDE.md with project-specific paths
4. Sets up GitHub workflows (test, deploy-staging, deploy-prod)
5. Initializes Git with initial commit
6. Provides checklist for GitHub setup (secrets, branch rules, etc.)

## Inputs

- Project name (e.g., "dashboard", "app-name")
- GitHub organization (defaults to epa-datos)
- GCP project ID (for deployment)
- API base URL (for staging/production)

## Outputs

- Complete NextJS project with EPA standards
- Ready to push to GitHub
- .env.example configured
- GitHub workflows ready
- All dependencies listed
- CLAUDE.md customized

## Implementation

```bash
# The skill should:

# 1. Clone template
git clone https://github.com/epa-datos/epa-standards-frontend.git $PROJECT_NAME
cd $PROJECT_NAME

# 2. Replace project identifiers
find . -type f -exec sed -i "s/epa-frontend/$PROJECT_NAME/g" {} \;
sed -i "s|epa-datos/$PROJECT_NAME|$GITHUB_ORG/$PROJECT_NAME|g" .github/workflows/*.yml

# 3. Update package.json
sed -i "s/\"name\": \"epa-frontend\"/\"name\": \"$PROJECT_NAME\"/" package.json

# 4. Customize CLAUDE.md
sed -i "s|https://github.com/epa-datos/epa-standards|https://github.com/$GITHUB_ORG/epa-standards|g" CLAUDE.md

# 5. Create .env.example (user fills in values)
# 6. Git init
git init
git add .
git commit -m "initial: EPA Digital NextJS template"

# 7. Output checklist
echo "✓ Project created. Next steps:"
echo "  1. Create repo: https://github.com/new?template=epa-datos/epa-standards-frontend"
echo "  2. git remote add origin https://github.com/$GITHUB_ORG/$PROJECT_NAME.git"
echo "  3. git push -u origin main"
echo "  4. Configure GitHub secrets (STAGING_API_BASE_URL, etc.)"
echo "  5. Enable branch protection rules"
```

## Example Usage

```
User: "Create a new NextJS app called my-dashboard"

Skill Output:
✓ Created my-dashboard
✓ Configured for epa-datos organization
✓ Ready to GitHub

Next steps:
1. Create repo at https://github.com/new
2. git push origin main
3. Configure secrets
```

## Notes

- For experienced devs: Skip the scaffold and clone directly
- All config files (tsconfig, tailwind, etc.) are pre-configured
- Tests setup included but can be customized
- Docker included for production build
