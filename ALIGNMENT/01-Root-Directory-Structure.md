# 1. Root Directory Structure

**Prerequisites:** [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points first

**Note:** This section assumes you've completed Section 0 and answered the decision point questions.

## P0 — Required Actions

**Action:** Clean up root directory

- [ ] Remove all non-essential files from root
- [ ] Keep only: `README.md`, `LICENSE`, `.gitignore`, version manager file, main package/build file
- [ ] For versioned projects: ensure `CHANGELOG.md` exists (basic file; format it properly in [Section 12](12-Change-Management.md))
- [ ] Remove build artifacts, temporary files, and environment secrets
- [ ] Verify every root-level file has an obvious purpose
- [ ] Configure `.gitignore` to exclude:
  - Environment files: `.env`, `.env.local`, `.env.*.local`
  - Build artifacts: `dist/`, `build/`, `*.o`, `*.exe`
  - IDE files: `.idea/`, `.vscode/` (or document if committed)
  - OS files: `.DS_Store`, `Thumbs.db`

**Files to include:**

- `README.md` (required)
- `LICENSE` (required for public repos)
- `.gitignore` (required - see [Configuration & Environment](03-Configuration-Environment.md) for details)
- Package manager file: `package.json`, `go.mod`, `Cargo.toml`, `pyproject.toml` (as applicable)
- `CHANGELOG.md` (required for versioned projects)

## P1 — Recommended Actions

**Action:** Organize related files

- [ ] If using GitHub: create `.github/` folder (required for CI/CD - see [CI/CD Structure](05-CI-CD-Structure.md))
- [ ] Create `docs/` folder for extended documentation
  - **Note:** Create the folder structure here; populate with content in [Documentation Standards](07-Documentation-Standards.md)

## P2 — Advanced Actions

**Action:** Add repository index

- [ ] Create `INDEX.md` or `INDEX.json` describing directory purposes
- [ ] Add `NOTICE` file for legal/compliance when applicable

**Action:** Create repository template (for new repository creation)
- [ ] Create ALIGNMENT-compliant repository template:
  - [ ] Include all P0 requirements pre-configured
  - [ ] Pre-populated CI/CD workflows
  - [ ] Standard directory structure
  - [ ] Template files (README, CONTRIBUTING, etc.)
- [ ] Create setup automation:
  - [ ] CLI tool or script for instant repo creation
  - [ ] Template variables (project name, type, etc.)
  - [ ] Automated configuration
- [ ] GitHub template repository:
  - [ ] Create template repo on GitHub
  - [ ] Configure template variables
  - [ ] Document usage
- [ ] Document repository template usage in `docs/repository-templates.md`
- [ ] **Note:** Enables one-command repository creation following ALIGNMENT standards

## Decision: Is this a versioned project?

**If you answered "Yes" in Section 0:**
- [ ] Create `CHANGELOG.md` file (basic structure, will be formatted in [Section 12](12-Change-Management.md))
- [ ] Use semantic versioning (SemVer) for releases

**If you answered "No" in Section 0:**
- [ ] Skip `CHANGELOG.md` creation
- [ ] You can still use versioning internally, but it's not required

## Verification

**Manual Check:**
- [ ] Run `ls -la` (or `dir` on Windows) and verify root has ≤ 10 files (excluding hidden files)
- [ ] Verify `README.md` exists and contains:
  - What the repo is (1-2 sentences)
  - How to install/run it
  - Where the code lives
  - How to contribute
- [ ] Check `.gitignore` excludes: `.env`, `build/`, `dist/`, `node_modules/`, `*.o`, `*.exe`
- [ ] Verify no secrets are committed (run: `git log --all --full-history -- .env` or similar)
- [ ] If public repo: verify `LICENSE` file exists
- [ ] If versioned project: verify `CHANGELOG.md` exists

**Automated Check (Future):**
```bash
./scripts/validate-section-1.sh
```

**Done Criteria:**
- ✅ Root directory contains only essential files
- ✅ All required files exist (README, LICENSE if public, .gitignore)
- ✅ `.gitignore` properly configured
- ✅ No build artifacts or secrets in repository
- ✅ CHANGELOG.md exists if versioned project

## Common Issues & Solutions

**Issue:** "I don't have a LICENSE file"  
- **Solution:** 
  - Public repos: Use [choosealicense.com](https://choosealicense.com/) to select appropriate license
  - Common choices: MIT (permissive), Apache 2.0 (patent protection), GPL (copyleft)
  - Copy license text to `LICENSE` file in root

**Issue:** ".github folder doesn't exist"  
- **Solution:** Create it: `mkdir -p .github/workflows` (or `mkdir .github\workflows` on Windows)
- **Note:** Only needed if using GitHub. For GitLab, create `.gitlab-ci.yml` in root instead

**Issue:** "I'm not using GitHub"  
- **Solution:** See [Section 5](05-CI-CD-Structure.md) for GitLab, Jenkins, CircleCI alternatives
- **Note:** `.github/` folder is only for GitHub Actions. Other platforms have different config locations

**Issue:** "I have too many files in root"  
- **Solution:** 
  - Move config files to `config/` directory (see [Section 3](03-Configuration-Environment.md))
  - Move documentation to `docs/` directory
  - Move scripts to `scripts/` directory
  - Keep only: README, LICENSE, .gitignore, package manager file, CHANGELOG (if versioned)

**Issue:** "I don't know if I need CHANGELOG.md"  
- **Solution:** Refer to Section 0 decision point. If others will install/use your code → Yes. If internal tool → No.

**Issue:** ".gitignore is missing common patterns"  
- **Solution:** Use language-specific .gitignore templates:
  - [GitHub gitignore templates](https://github.com/github/gitignore)
  - Add patterns for your language/framework
  - Always include: `.env*`, `node_modules/`, `dist/`, `build/`, `.DS_Store`