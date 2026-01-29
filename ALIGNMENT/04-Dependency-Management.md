# 4. Dependency Management

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 1: Root Directory Structure](01-Root-Directory-Structure.md)

## P0 — Required Actions

**Action:** Secure dependencies
- [ ] Commit lockfiles: `package-lock.json`, `pnpm-lock.yaml`, `Cargo.lock`, `go.sum`
- [ ] Verify license compatibility (no incompatible licenses)
- [ ] Document dependency scanning process
- [ ] **Note:** Configure automated vulnerability scanning in CI/CD (see [CI/CD Structure](05-CI-CD-Structure.md))

## P1 — Recommended Actions

**Action:** Automate dependency management
- [ ] Enable Dependabot or Renovate for automated updates
- [ ] Create dependency policy document:
  - Allowed version ranges
  - Banned packages
  - Update frequency
- [ ] Configure auto-merge for patch/minor updates (if appropriate)

## P2 — Advanced Actions

**Action:** Advanced dependency tracking
- [ ] Create internal dependency dashboard
- [ ] Enforce dependency boundaries (e.g., packages cannot depend on apps)
- [ ] Track dependency health metrics

## Decision: Which package manager are you using?

**From Section 0, identify your lockfile:**
- npm → `package-lock.json`
- pnpm → `pnpm-lock.yaml`
- yarn → `yarn.lock`
- pip → `requirements.txt` (no lockfile, consider poetry/pipenv)
- poetry → `poetry.lock`
- pipenv → `Pipfile.lock`
- Go → `go.sum`
- Rust → `Cargo.lock`
- Other → [identify lockfile]

**Action:** Verify lockfile exists and is committed
- [ ] Check lockfile exists for your package manager
- [ ] Verify lockfile is committed to git: `git ls-files | grep lock`
- [ ] If no lockfile exists, generate it: `npm install`, `poetry lock`, `go mod tidy`, etc.

## Verification

**Manual Check:**
- [ ] Verify lockfile exists and is committed: `git ls-files | grep -E "lock|sum"`
- [ ] Check lockfile is not in `.gitignore` (it should be committed)
- [ ] Verify no incompatible licenses: Review dependency licenses
- [ ] Check dependency scanning is configured in CI/CD (from Section 5)
- [ ] If using Dependabot/Renovate: verify config exists (`.github/dependabot.yml` or `renovate.json`)

**Automated Check (Future):**
```bash
./scripts/validate-section-4.sh
```

**Done Criteria:**
- ✅ Lockfile exists and is committed
- ✅ No incompatible licenses in dependencies
- ✅ Dependency vulnerability scanning configured in CI/CD
- ✅ Dependabot/Renovate enabled (if P1 completed)

## Common Issues & Solutions

**Issue:** "I don't have a lockfile"  
- **Solution:** 
  - Generate it: `npm install` (creates package-lock.json), `poetry lock`, `go mod tidy`, etc.
  - Commit it to git (lockfiles should be committed)
  - Add to `.gitignore` only if your package manager doesn't use lockfiles (rare)

**Issue:** "My lockfile is in .gitignore"  
- **Solution:** 
  - Remove lockfile from `.gitignore`
  - Commit the lockfile: `git add package-lock.json` (or equivalent)
  - **Exception:** Some teams use lockfiles differently, but ALIGNMENT standard recommends committing them

**Issue:** "I don't know if I have incompatible licenses"  
- **Solution:** 
  - Use license checking tools:
    - npm: `license-checker`
    - Python: `pip-licenses`
    - Go: `go-licenses`
  - Review licenses for compatibility with your project's license
  - Common incompatible: GPL (if your project is not GPL), AGPL (restrictive)

**Issue:** "Dependabot/Renovate isn't working"  
- **Solution:** 
  - GitHub: Check `.github/dependabot.yml` exists and is valid
  - Renovate: Check `renovate.json` or `.github/renovate.json` exists
  - Verify bot has access to repository
  - Check GitHub/GitLab settings for dependency automation

**Issue:** "I have too many outdated dependencies"  
- **Solution:** 
  - Start with critical security updates first
  - Update dependencies incrementally (don't update everything at once)
  - Test after each update
  - Use Dependabot/Renovate to automate future updates