# 3. Configuration & Environment

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 1: Root Directory Structure](01-Root-Directory-Structure.md)
- [Section 2: Source Code Layout](02-Source-Code-Layout.md)

**Effort Estimate:**
- **Basic configuration (P0):** 30-60 minutes
- **Centralized config + pre-commit hooks (P1):** 2-4 hours
- **Containerization standards (P1):** 2-4 hours additional
- **Schema validation (P2):** 2-3 hours additional
- **Secret remediation (if needed):** 2-8 hours (CRITICAL - do immediately)

## Before/After Examples

### Example 1: Configuration Centralization

**BEFORE (config files scattered):**
```
project/
├── README.md
├── package.json
├── .eslintrc.json
├── .prettierrc
├── jest.config.js
├── webpack.config.js
├── tsconfig.json
├── .env
├── config.json
└── src/
```

**AFTER (centralized config):**
```
project/
├── README.md
├── package.json
├── .env.example
├── config/
│   ├── eslint.config.js
│   ├── prettier.config.js
│   ├── jest.config.js
│   ├── webpack.config.js
│   ├── tsconfig.json
│   └── app.config.json
├── .gitignore  (excludes .env)
└── src/
```

**Changes Made:**
- Moved all config files → `config/` directory
- Created `.env.example` (no secrets)
- Removed `.env` (now in `.gitignore`)
- Centralized configuration management

### Example 2: Pre-commit Hooks Setup

**BEFORE (no hooks):**
```
project/
├── .git/
└── (no pre-commit validation)
```

**AFTER (pre-commit hooks configured):**
```
project/
├── .git/
│   └── hooks/
│       └── pre-commit  (runs linting, formatting, secret scanning)
├── .pre-commit-config.yaml  (or .husky/pre-commit)
└── package.json
    └── scripts:
        - "lint": "eslint ."
        - "format": "prettier --write ."
```

**Changes Made:**
- Added pre-commit hook configuration
- Hooks run: linting, formatting, secret scanning
- Prevents bad commits before CI/CD

## Questions to Answer

Before proceeding, answer these questions:

1. **What secrets/credentials does your application need?**
   - ⚠️ **CRITICAL:** If any secrets exist in the repository, they must be removed immediately!
   - [ ] Database credentials (host, username, password, port)
   - [ ] API keys (third-party services, internal APIs)
   - [ ] Service URLs and endpoints
   - [ ] Feature flags and configuration values
   - [ ] Other: _______________
   - [ ] None (application uses only hardcoded/public values)

2. **How will you manage these secrets?**
   - [ ] Environment variables (.env files for local, platform env vars for deployment)
   - [ ] Secret management service (AWS Secrets Manager, HashiCorp Vault, Azure Key Vault, etc.)
   - [ ] CI/CD secrets (GitHub Secrets, GitLab CI Variables, etc.)
   - [ ] Other: _______________

**Document your answers** in `alignment-progress.json` before proceeding.

## P0 — Required Actions

**Action:** Secure configuration
- [ ] Verify `.gitignore` is properly configured (see [Section 1](01-Root-Directory-Structure.md))
- [ ] Verify no secrets are committed (run secret scanning)
- [ ] Create `.env.example` template with required variables (no secrets)
  - **Note:** Complete this after understanding what environment variables your application needs

## P1 — Recommended Actions

**Action:** Centralize configuration
- [ ] Create `config/` directory for configuration files
  - **Note:** If code already exists, this may require moving config files
- [ ] Document configuration precedence: environment variables → config files → defaults
- [ ] Add configuration documentation in `docs/configuration.md`
  - **Note:** If `docs/` folder doesn't exist yet, create it (see [Section 1 P1](01-Root-Directory-Structure.md)). For docs structure guidance, see [Documentation Standards](07-Documentation-Standards.md)

**Action:** Set up pre-commit hooks
- [ ] Install pre-commit framework (Husky for Node.js, pre-commit for Python, or language-agnostic pre-commit framework)
- [ ] Configure hooks for:
  - [ ] Linting (ESLint, Prettier, Black, RuboCop, etc. - language-specific)
  - [ ] Formatting checks (Prettier, Black, gofmt, etc.)
  - [ ] Secret scanning (git-secrets, truffleHog) - complements [Section 10](10-Security-Compliance.md)
  - [ ] Commit message validation (commitlint) - enforces [Section 12](12-Change-Management.md) conventions
  - [ ] Quick unit tests (fast tests only, full suite runs in CI)
- [ ] Document setup in `docs/development.md` or CONTRIBUTING.md
- [ ] Ensure hooks run in CI as well (don't rely solely on local hooks)
- [ ] **Note:** Pre-commit hooks complement but don't replace CI checks (see [Section 5](05-CI-CD-Structure.md))

**Action:** Standardize containerization and development environments
- [ ] Dev Containers configuration:
  - [ ] Create `.devcontainer/devcontainer.json`
  - [ ] Configure development environment (tools, extensions, settings)
  - [ ] Document usage in `docs/development.md`
- [ ] Docker Compose for local development:
  - [ ] Create `docker-compose.yml` for local services
  - [ ] Configure services (app, database, cache, etc.)
  - [ ] Document usage and common commands
- [ ] Standardized Dockerfile patterns:
  - [ ] Multi-stage builds for optimization
  - [ ] Layer optimization (minimize layers, use .dockerignore)
  - [ ] Security best practices (non-root user, minimal base images)
  - [ ] Document Dockerfile standards in `docs/containers.md`
- [ ] Container image scanning (complements [Section 10 P1](10-Security-Compliance.md))
- [ ] Document containerization standards in `docs/containers.md`

## P2 — Advanced Actions

**Action:** Validate configuration
- [ ] Use schema validation (Zod, JSON Schema, etc.)
- [ ] Generate typed configuration objects
- [ ] Add configuration validation on startup

## Decision: What environment variables do you need?

**Identify required variables:**
- [ ] **Database:** What connection details are needed? (host, port, database name, credentials)
- [ ] **APIs:** What API keys or tokens are required? (service names, key purposes)
- [ ] **Services:** What service URLs or endpoints are needed? (internal/external services)
- [ ] **Feature flags:** What configuration flags exist? (feature names, default values)
- [ ] **Other:** What other environment-specific values exist? (log levels, timeouts, etc.)

**If you identified variables:**
- [ ] Create `.env.example` listing all required variables with descriptions (no actual values)
- [ ] Document each variable's purpose, format, and whether it's required or optional
- [ ] Ensure `.env` is in `.gitignore` (from Section 1)

**If no variables needed:**
- [ ] Skip `.env.example` creation
- [ ] Still ensure `.gitignore` excludes `.env*` files for future-proofing

## Verification

**Manual Check:**
- [ ] Verify `.gitignore` excludes: `.env`, `.env.local`, `.env.*.local`
- [ ] Run secret scanning: `git secrets --scan-history` or use truffleHog
- [ ] Check no secrets in committed files: `grep -r "password\|secret\|key" --include="*.env" .git/` (should return nothing)
- [ ] If using environment variables: verify `.env.example` exists with all required vars (no secrets)
- [ ] Verify configuration precedence is documented (env vars → config files → defaults)
- [ ] If using pre-commit hooks: verify they run: `git commit --dry-run`

**Automated Check:**
```bash
# Run validation script (from repository root)
./scripts/validate-section-3.sh

# Or if scripts are in ALIGNMENT directory
../ALIGNMENT/scripts/validate-section-3.sh .
```

**Note:** See [scripts/README.md](../scripts/README.md) for validation script usage.

**Done Criteria:**
- ✅ No secrets committed to repository
- ✅ `.env.example` exists if using environment variables
- ✅ `.gitignore` properly configured
- ✅ Pre-commit hooks configured (if P1 completed)
- ✅ Configuration centralized in `config/` directory (if P1 completed)

## Rollback Procedures

If configuration changes cause issues:

**Option 1: Restore .gitignore**
```bash
git checkout HEAD -- .gitignore
```

**Option 2: Restore config files**
```bash
# If moved config files, restore them
git checkout <previous-commit> -- config/
```

**Option 3: Remove pre-commit hooks**
```bash
# Remove pre-commit hooks
rm .git/hooks/pre-commit
# Or for Husky
rm -rf .husky
```

**Option 4: Restore .env.example**
```bash
git checkout HEAD -- .env.example
```

**Critical:** If secrets were exposed, rotate them immediately (cannot rollback secrets).

## Common Issues & Solutions

**Issue:** "I don't know what environment variables I need"  
- **Solution:** 
  - Review your code for hardcoded values (URLs, credentials, API keys)
  - Check your dependencies (databases, services) for required configuration
  - Start with `.env.example` as you discover needs
  - Document each variable's purpose

**Issue:** "I accidentally committed a secret"  
- **Solution:** 
  - **Immediate:** Rotate the secret (it's compromised)
  - Remove from git history: `git filter-branch` or BFG Repo-Cleaner
  - Add to `.gitignore` to prevent future commits
  - Set up secret scanning (pre-commit + CI) to catch future issues

**Issue:** "Pre-commit hooks aren't running"  
- **Solution:** 
  - Verify installation: `ls -la .git/hooks/` (should show pre-commit)
  - Reinstall: `pre-commit install` (or `npx husky install` for Node.js)
  - Test: `pre-commit run --all-files` (or `npm run lint` for Husky)
  - Check file permissions: hooks must be executable

**Issue:** "I'm not using GitHub, can I still use pre-commit?"  
- **Solution:** Yes! Pre-commit framework works with any git repository
- Use language-agnostic `pre-commit` framework (Python-based but works for all languages)
- Or use language-specific: Husky (Node.js), pre-commit (Python), git hooks (any)

**Issue:** "I don't know where to put configuration files"  
- **Solution:** 
  - Create `config/` directory in root
  - Move all `.json`, `.yaml`, `.toml` config files there
  - Keep only essential config in root (like `package.json`, `pyproject.toml`)
  - Document config file purposes in `docs/configuration.md`

**Issue:** "My config/ directory doesn't exist yet"  
- **Solution:** Create it: `mkdir config` (or `mkdir config\` on Windows)
- This is a P1 action, so it's okay to skip initially and add later