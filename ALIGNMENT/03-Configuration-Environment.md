# 3. Configuration & Environment

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 1: Root Directory Structure](01-Root-Directory-Structure.md)
- [Section 2: Source Code Layout](02-Source-Code-Layout.md)

## Questions to Answer

Before proceeding, answer these questions:

1. **Do you have secrets currently in the repository?**
   - ⚠️ **CRITICAL:** If yes, these must be removed immediately!
   - [ ] Yes (needs immediate remediation)
   - [ ] No (needs prevention setup)

2. **What is your secret management approach?**
   - [ ] Environment variables (.env files)
   - [ ] Secret management service (AWS Secrets Manager, HashiCorp Vault, etc.)
   - [ ] CI/CD secrets (GitHub Secrets, GitLab CI Variables)
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

## Decision: Do you need environment variables?

**Check if your application needs:**
- Database credentials → **Yes**
- API keys → **Yes**
- Service URLs → **Yes**
- Feature flags → **Yes**
- Hardcoded values only → **No**

**If Yes:**
- [ ] Create `.env.example` with all required variables (no values)
- [ ] Document each variable in `.env.example` comments
- [ ] Ensure `.env` is in `.gitignore` (from Section 1)

**If No:**
- [ ] Skip `.env.example` creation
- [ ] Still ensure `.gitignore` excludes `.env*` files

## Verification

**Manual Check:**
- [ ] Verify `.gitignore` excludes: `.env`, `.env.local`, `.env.*.local`
- [ ] Run secret scanning: `git secrets --scan-history` or use truffleHog
- [ ] Check no secrets in committed files: `grep -r "password\|secret\|key" --include="*.env" .git/` (should return nothing)
- [ ] If using environment variables: verify `.env.example` exists with all required vars (no secrets)
- [ ] Verify configuration precedence is documented (env vars → config files → defaults)
- [ ] If using pre-commit hooks: verify they run: `git commit --dry-run`

**Automated Check (Future):**
```bash
./scripts/validate-section-3.sh
```

**Done Criteria:**
- ✅ No secrets committed to repository
- ✅ `.env.example` exists if using environment variables
- ✅ `.gitignore` properly configured
- ✅ Pre-commit hooks configured (if P1 completed)
- ✅ Configuration centralized in `config/` directory (if P1 completed)

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