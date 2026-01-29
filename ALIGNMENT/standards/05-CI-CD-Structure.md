# 5. CI/CD Structure

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 1: Root Directory Structure](01-Root-Directory-Structure.md)
- [Section 3: Configuration & Environment](03-Configuration-Environment.md)
- [Section 4: Dependency Management](04-Dependency-Management.md)

## Questions to Answer

Before proceeding, answer these questions:

1. **What CI/CD pipeline stages do you need?**
   - [ ] Build (compile/package code)
   - [ ] Test (unit, integration, e2e)
   - [ ] Lint (code quality checks)
   - [ ] Security scanning (SAST, dependency scanning, secret scanning)
   - [ ] Deploy (staging, production)
   - [ ] Other: _______________

2. **What security scanning should your pipeline include?**
   - [ ] Static code analysis (ESLint security plugins, Bandit, Gosec, CodeQL)
   - [ ] Dependency vulnerability scanning (Snyk, Dependabot, npm audit)
   - [ ] Secret scanning (truffleHog, git-secrets)
   - [ ] Container scanning (if using containers: Trivy, Snyk Container)
   - [ ] Other: _______________

**Document your answers** in `alignment-progress.json` before proceeding.

## Before/After Examples

### Example 1: GitHub Actions Setup

**BEFORE (no CI/CD):**
```
project/
├── README.md
├── package.json
└── src/
```

**AFTER (GitHub Actions CI/CD):**
```
project/
├── README.md
├── package.json
├── .github/
│   └── workflows/
│       └── ci.yml
└── src/
```

**ci.yml contents:**
```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run build
      - run: npm run lint
      - run: npm test
      - run: npm audit
```

**Changes Made:**
- Created `.github/workflows/ci.yml`
- Added build, lint, test, and security checks
- Configured to run on push and PR

### Example 2: GitLab CI Setup

**BEFORE (no CI/CD):**
```
project/
├── README.md
└── src/
```

**AFTER (GitLab CI):**
```
project/
├── README.md
├── .gitlab-ci.yml
└── src/
```

**.gitlab-ci.yml contents:**
```yaml
stages:
  - build
  - test
  - security

build:
  stage: build
  script:
    - npm ci
    - npm run build

test:
  stage: test
  script:
    - npm test

security:
  stage: security
  script:
    - npm audit
```

**Effort Estimate:**
- **New CI/CD setup (GitHub Actions):** 2-4 hours
- **New CI/CD setup (GitLab CI):** 2-4 hours
- **New CI/CD setup (Jenkins):** 4-8 hours (if self-hosted)
- **Existing CI/CD alignment:** 1-3 hours
- **Phase 1 (Bootstrap):** 1-2 hours
- **Phase 2 (Complete with tests):** 1-2 hours additional

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
- **GitHub Actions** → `.github/workflows/ci.yml`
  - [Documentation](https://docs.github.com/en/actions)
  - [Workflow syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- **GitLab CI** → `.gitlab-ci.yml` in root
  - [Documentation](https://docs.gitlab.com/ee/ci/)
  - [.gitlab-ci.yml reference](https://docs.gitlab.com/ee/ci/yaml/)
- **Jenkins** → `Jenkinsfile` in root
  - [Documentation](https://www.jenkins.io/doc/)
  - [Pipeline syntax](https://www.jenkins.io/doc/book/pipeline/syntax/)
- **CircleCI** → `.circleci/config.yml`
  - [Documentation](https://circleci.com/docs/)
  - [Configuration reference](https://circleci.com/docs/config-intro/)
- **Azure DevOps** → `azure-pipelines.yml`
  - [Documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/)
  - [YAML schema](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema)
- **Bitbucket Pipelines** → `bitbucket-pipelines.yml`
  - [Documentation](https://support.atlassian.com/bitbucket-cloud/docs/get-started-with-bitbucket-pipelines/)
- **Travis CI** → `.travis.yml`
  - [Documentation](https://docs.travis-ci.com/)
- **Other** → [platform-specific config location]

**Action:** Create CI/CD config file in correct location
- [ ] Create config file in platform-specific location
- [ ] Use platform-specific syntax (YAML for most platforms)
- [ ] Reference platform documentation (links above)
- [ ] Test configuration in feature branch before merging

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

## Rollback Procedures

If CI/CD changes break builds:

**Option 1: Restore previous CI/CD config**
```bash
# GitHub Actions
git checkout <previous-commit> -- .github/workflows/ci.yml

# GitLab CI
git checkout <previous-commit> -- .gitlab-ci.yml

# Jenkins
git checkout <previous-commit> -- Jenkinsfile
```

**Option 2: Disable CI/CD temporarily**
```bash
# Rename config file to disable
mv .github/workflows/ci.yml .github/workflows/ci.yml.disabled
# Or comment out in GitLab CI
```

**Option 3: Revert to backup**
```bash
# If you backed up config before changes
cp .github/workflows/ci.yml.backup .github/workflows/ci.yml
```

**Option 4: Fix incrementally**
- Revert to Phase 1 (build + lint only)
- Fix issues
- Re-add test step after fixing

**Prevention:** Test CI/CD in feature branch before merging to main.

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
