# 3. Configuration & Environment

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 1: Root Directory Structure](01-Root-Directory-Structure.md)
- [Section 2: Source Code Layout](02-Source-Code-Layout.md)

**Principles:**
- [Configuration Principles](../principles/Configuration-Principles.md) - Core philosophy: *Treat config like code: versioned, validated, layered, and boring.*
  - Key concepts: Layering model (Base → Env → Region/Tenant → Secrets → Runtime), schema validation, "one key, one home" rule

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

**Action:** Secure configuration and prevent secret leaks

**What this means:** Ensure sensitive information (passwords, API keys, tokens) is NEVER committed to Git. Configure your repository to prevent accidents and provide safe templates for configuration.

**Why it matters:** Leaked secrets are the #1 cause of security breaches. Once a secret is in Git history, it's extremely difficult to remove and must be considered compromised.

### Task 1: Verify .gitignore is configured *(USER)*

- [ ] **Open `.gitignore` file** (should exist from Section 1)

- [ ] **Ensure these patterns are included:**
  ```
  # Environment files (CRITICAL - prevents secret leaks)
  .env
  .env.local
  .env.*.local
  .env.production
  
  # Build artifacts that may contain secrets
  dist/
  build/
  
  # IDE files that may contain sensitive settings
  .vscode/settings.json
  .idea/workspace.xml
  ```

- [ ] **Save and commit** if you added anything:
  ```bash
  git add .gitignore
  git commit -m "Ensure .gitignore excludes environment files"
  ```

### Task 2: Scan for existing secrets (CRITICAL!) *(USER/AGENT)*

**WARNING:** If you find secrets, you have a security incident! Follow the remediation steps.

- [ ] **Install a secret scanner:**
  ```bash
  # Option 1: git-secrets (AWS)
  brew install git-secrets  # Mac
  # Or download from: https://github.com/awslabs/git-secrets
  
  # Option 2: truffleHog
  pip install truffleHog
  
  # Option 3: gitleaks
  brew install gitleaks  # Mac
  ```

- [ ] **Scan current repository:**
  ```bash
  # Using git-secrets
  git secrets --scan-history
  
  # Using truffleHog
  trufflehog filesystem . --json
  
  # Using gitleaks
  gitleaks detect --source . --verbose
  ```

- [ ] **If secrets found - IMMEDIATE ACTION REQUIRED:**
  1. **Rotate the secret immediately** - Change the password/key/token
  2. **Remove from Git history:**
     ```bash
     # Use BFG Repo-Cleaner or git-filter-repo
     # https://github.com/newren/git-filter-repo
     git filter-repo --path .env --invert-paths
     ```
  3. **Force push** (coordinate with team first!):
     ```bash
     git push --force-with-lease origin main
     ```
  4. **Document incident** in `docs/security-incidents.md`
  5. **See [Section 10: Security](10-Security-Compliance.md) for full remediation**

- [ ] **If no secrets found:** ✅ Good! Continue to next task.

### Task 3: Create .env.example template *(USER)*

**Purpose:** Provide a template showing what environment variables your application needs, WITHOUT including actual secrets.

- [ ] **Identify required environment variables:**
  - What does your app need to connect to databases?
  - What API keys does it use?
  - What configuration can change between environments?

- [ ] **Create `.env.example` file** with placeholders:
  ```bash
  # Database Configuration
  DATABASE_URL=postgresql://user:password@localhost:5432/dbname
  DATABASE_POOL_SIZE=10
  
  # API Keys (get from: https://dashboard.example.com/api-keys)
  API_KEY=your_api_key_here
  API_SECRET=your_api_secret_here
  
  # Application Settings
  NODE_ENV=development
  PORT=3000
  LOG_LEVEL=info
  
  # Feature Flags
  ENABLE_FEATURE_X=false
  ENABLE_ANALYTICS=true
  
  # External Services
  REDIS_URL=redis://localhost:6379
  EMAIL_SERVICE_URL=https://api.emailservice.com
  ```

- [ ] **Add comments explaining each variable:**
  - What is it for?
  - Where do you get the value?
  - Is it required or optional?
  - What's the default if not set?

- [ ] **Use placeholder values, NOT real secrets:**
  - ❌ BAD: `API_KEY=abc123realkey`
  - ✅ GOOD: `API_KEY=your_api_key_here`
  - ✅ GOOD: `DATABASE_PASSWORD=<get-from-password-manager>`

- [ ] **Commit .env.example (this is safe to commit):**
  ```bash
  git add .env.example
  git commit -m "Add environment variable template"
  ```

### Task 4: Set up local .env file (DO NOT COMMIT) *(USER)*

- [ ] **Copy the template:**
  ```bash
  cp .env.example .env
  ```

- [ ] **Fill in YOUR actual values in `.env`:**
  - Replace placeholders with real credentials
  - Get secrets from password manager or teammate
  - Never share this file or commit it to Git

- [ ] **Verify .env is not tracked by Git:**
  ```bash
  git status
  # Should NOT show .env file
  # If it does, .gitignore is not configured correctly - go back to Task 1
  ```

- [ ] **Test your application:**
  ```bash
  # Run your application
  npm start  # or python app.py, go run main.go, etc.
  
  # Verify it loads environment variables correctly
  # Check logs for "Configuration loaded" or similar messages
  ```

### Task 5: Document environment setup *(USER)*

- [ ] **Add to README.md or docs/development.md:**
  ```markdown
  ## Environment Setup
  
  1. Copy the environment template:
     ```
     cp .env.example .env
     ```
  
  2. Fill in the required values:
     - `DATABASE_URL`: Get from [where teammates get this]
     - `API_KEY`: Generate at [service dashboard URL]
     - `API_SECRET`: Get from team password manager
  
  3. Never commit `.env` to version control!
  ```

**Done Criteria:**
- ✅ `.gitignore` excludes `.env`, `.env.local`, `.env.*.local`
- ✅ Secret scanning completed (no secrets found, or found secrets remediated)
- ✅ `.env.example` exists with all required variables (using placeholders, not real values)
- ✅ `.env.example` is committed to Git
- ✅ Local `.env` file created with actual values (NOT committed)
- ✅ Application runs successfully with environment variables
- ✅ Environment setup documented in README or docs/

## P1 — Recommended Actions

**Action:** Centralize configuration
- [ ] Create `config/` directory for configuration files
  - **Note:** If code already exists, this may require moving config files
- [ ] Implement layering model (see [Configuration Principles](../principles/Configuration-Principles.md)):
  - [ ] Create `config/schema.*` (the contract) - defines configuration structure
  - [ ] Create `config/default.*` (base defaults) - safe, minimal, works locally
  - [ ] Create `config/env/dev.*`, `config/env/stage.*`, `config/env/prod.*` (deltas only)
  - [ ] Create `config/regions/*` or `config/tenants/*` (optional, only if needed)
  - [ ] Create `config/loader.*` (the only code that merges layers + validates)
  - [ ] Follow "one key, one home" rule - no duplicate keys across layers
  - [ ] Document override precedence: Base → Env → Region/Tenant → Secrets → Runtime
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

**Action:** Validate configuration (see [Configuration Principles](../principles/Configuration-Principles.md) for rationale)
- [ ] Use schema validation (Zod, JSON Schema, Pydantic, Yup, etc.)
- [ ] Generate typed configuration objects
- [ ] Add configuration validation on:
  - [ ] App startup (fail fast, fail loud)
  - [ ] CI (every PR)
  - [ ] Deployment
- [ ] Implement strong typing + constraints (not just "string" - validate ranges, enums, URL formats)
- [ ] Log final resolved config on startup (redacting secrets) - no "magic"
- [ ] Document each config key (what/why/default/owner) - see "Golden rules" in [Configuration Principles](../principles/Configuration-Principles.md)

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
- ✅ Layering model implemented (if P1 completed) - Base → Env → Region/Tenant → Secrets → Runtime
- ✅ Schema validation implemented (if P2 completed) - validates on startup, CI, and deployment
- ✅ Configuration follows "Golden rules" from [Configuration Principles](../principles/Configuration-Principles.md) (if P2 completed)

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