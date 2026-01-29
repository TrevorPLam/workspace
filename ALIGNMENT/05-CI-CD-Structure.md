# 5. CI/CD Structure

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 1: Root Directory Structure](01-Root-Directory-Structure.md)
- [Section 3: Configuration & Environment](03-Configuration-Environment.md)
- [Section 4: Dependency Management](04-Dependency-Management.md)

## Questions to Answer

Before proceeding, answer these questions:

1. **Do you already have CI/CD configured?**
   - [ ] Yes (needs alignment with ALIGNMENT standards)
   - [ ] No (needs full setup)

2. **What security scanning tools do you currently use (or plan to use)?**
   - [ ] None (needs setup)
   - [ ] ESLint with security plugins (JavaScript/TypeScript)
   - [ ] Snyk
   - [ ] Bandit (Python)
   - [ ] Gosec (Go)
   - [ ] CodeQL (GitHub)
   - [ ] Other: _______________

**Document your answers** in `alignment-progress.json` before proceeding.

## P0 — Required Actions

**Action:** Set up bootstrap CI/CD pipeline (Phase 1 - Do First)
- [ ] Create `.github/` folder if using GitHub (see [Section 1 P1](01-Root-Directory-Structure.md))
- [ ] Create CI/CD config in standard location:
  - GitHub: `.github/workflows/ci.yml`
  - GitLab: `.gitlab-ci.yml`
  - Jenkins: `Jenkinsfile`
  - CircleCI: `.circleci/config.yml`
- [ ] Configure minimal pipeline to run:
  - [ ] Build step (verify code compiles/builds)
  - [ ] Lint step (code quality checks)
  - [ ] Basic security checks:
    - [ ] SAST (Static Application Security Testing)
    - [ ] Dependency vulnerability scanning (from [Section 4](04-Dependency-Management.md))
    - [ ] Secret scanning (from [Section 3](03-Configuration-Environment.md))
- [ ] **Note:** Test step will be added in Phase 2 (after [Section 6](06-Testing-Structure.md))
- [ ] Ensure pipeline blocks merges on failures
- [ ] Document CI/CD process in README or `docs/ci-cd.md`
  - **Note:** If `docs/` folder doesn't exist yet, document in README (see [Section 1 P1](01-Root-Directory-Structure.md) for creating `docs/` folder)

**Action:** Complete CI/CD pipeline (Phase 2 - After Section 6)
- [ ] Add test step to pipeline (tests from [Section 6](06-Testing-Structure.md))
- [ ] Configure test step to run all test suites
- [ ] Ensure test failures block merges
- [ ] Update CI/CD documentation with test information

## P1 — Recommended Actions

**Action:** Enhance CI/CD pipeline
- [ ] Add caching for dependencies and build artifacts
- [ ] Configure matrix builds for multiple environments/versions
- [ ] Set up deployment workflows (staging/production)
- [ ] Add performance testing in CI
- [ ] Configure branch protection rules

## P2 — Advanced Actions

**Action:** Advanced CI/CD practices
- [ ] Implement canary deployments
- [ ] Add automated rollback capabilities
- [ ] Set up multi-region deployment pipelines
- [ ] Create CI/CD metrics dashboard
- [ ] Implement progressive delivery

## Decision: Which CI/CD platform are you using?

**From Section 0, identify your platform:**
- **GitHub** → `.github/workflows/ci.yml`
- **GitLab** → `.gitlab-ci.yml` in root
- **Jenkins** → `Jenkinsfile` in root
- **CircleCI** → `.circleci/config.yml`
- **Azure DevOps** → `azure-pipelines.yml`
- **Other** → [platform-specific config location]

**Action:** Create CI/CD config file in correct location
- [ ] Create config file in platform-specific location
- [ ] Use platform-specific syntax (YAML for most platforms)
- [ ] Reference platform documentation if needed

## Verification

**Manual Check:**
- [ ] Verify CI/CD config file exists in correct location
- [ ] Check config file syntax is valid (use platform's validation)
- [ ] Verify pipeline runs: Push a commit and check CI/CD status
- [ ] Confirm pipeline includes: build, lint, security checks (Phase 1)
- [ ] Verify pipeline blocks merges on failure (check branch protection settings)
- [ ] If Phase 2 complete: verify test step runs and blocks on failure
- [ ] Check CI/CD documentation exists (README or `docs/ci-cd.md`)

**Automated Check (Future):**
```bash
./scripts/validate-section-5.sh
```

**Done Criteria:**
- ✅ CI/CD config file exists and is valid
- ✅ Pipeline runs successfully (build, lint, security checks)
- ✅ Pipeline blocks merges on failure
- ✅ CI/CD process documented
- ✅ Test step added (if Phase 2 completed)

## Common Issues & Solutions

**Issue:** "I don't know which CI/CD platform to use"  
- **Solution:** 
  - GitHub repository → GitHub Actions (easiest, free for public repos)
  - GitLab repository → GitLab CI (built-in, free)
  - Self-hosted → Jenkins (requires setup)
  - Small project → Start with GitHub Actions

**Issue:** "My CI/CD pipeline isn't running"  
- **Solution:** 
  - Check config file syntax: Use platform's validation tool
  - Verify config file is in correct location
  - Check branch protection: Ensure CI is required to pass
  - Review platform logs for errors
  - Test locally first: `npm run build`, `npm run lint`, etc.

**Issue:** "I'm not using GitHub, but instructions mention .github/"  
- **Solution:** 
  - GitHub Actions → `.github/workflows/`
  - GitLab → `.gitlab-ci.yml` in root
  - Jenkins → `Jenkinsfile` in root
  - CircleCI → `.circleci/config.yml`
  - See platform-specific documentation

**Issue:** "Pipeline passes but should fail"  
- **Solution:** 
  - Check pipeline exit codes (should exit 1 on failure)
  - Verify branch protection rules require CI to pass
  - Ensure test/lint commands actually fail on errors
  - Test by introducing a deliberate error

**Issue:** "I don't know what SAST tools to use"  
- **Solution:** 
  - JavaScript/TypeScript: ESLint with security plugins, Snyk
  - Python: Bandit, Safety
  - Go: Gosec, golangci-lint
  - Rust: Clippy, cargo-audit
  - Multi-language: SonarQube, CodeQL (GitHub)

**Issue:** "Secret scanning isn't finding secrets"  
- **Solution:** 
  - Use multiple tools: git-secrets (pre-commit) + truffleHog (CI)
  - Configure custom patterns for your secret formats
  - Scan git history: `trufflehog git file://. --json`
  - Rotate any found secrets immediately
