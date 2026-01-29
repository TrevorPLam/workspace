# 1. Root Directory Structure

**Prerequisites:** [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points first

**Note:** This section assumes you've completed Section 0 and answered the decision point questions.

**Effort Estimate:**
- **New repository:** 30-60 minutes
- **Existing repository (small, < 20 files):** 1-2 hours
- **Existing repository (medium, 20-50 files):** 2-4 hours
- **Existing repository (large, 50+ files):** 4-8 hours

## Before/After Examples

### Example 1: JavaScript/TypeScript Project

**BEFORE (25 files in root):**
```
my-project/
├── README.md
├── package.json
├── package-lock.json
├── .gitignore
├── index.js
├── utils.js
├── config.js
├── config.prod.js
├── config.dev.js
├── .eslintrc.json
├── .prettierrc
├── jest.config.js
├── webpack.config.js
├── docker-compose.yml
├── Dockerfile
├── .env.example
├── setup.sh
├── deploy.sh
├── docs/
│   └── api.md
├── scripts/
│   └── build.sh
├── tests/
│   └── index.test.js
└── src/  (empty)
```

**AFTER (8 files in root):**
```
my-project/
├── README.md
├── LICENSE
├── package.json
├── package-lock.json
├── .gitignore
├── CHANGELOG.md
├── .github/
│   └── workflows/
│       └── ci.yml
├── config/
│   ├── eslint.config.js
│   ├── prettier.config.js
│   ├── jest.config.js
│   └── webpack.config.js
├── scripts/
│   ├── setup.sh
│   ├── deploy.sh
│   └── build.sh
├── docs/
│   └── api.md
├── src/
│   ├── index.js
│   └── utils.js
├── tests/
│   └── index.test.js
├── docker-compose.yml
└── Dockerfile
```

**Changes Made:**
- Moved `index.js`, `utils.js` → `src/`
- Moved `config.js`, `config.*.js` → `config/`
- Moved `.eslintrc.json`, `.prettierrc`, `jest.config.js`, `webpack.config.js` → `config/`
- Moved `setup.sh`, `deploy.sh` → `scripts/`
- Added `LICENSE` (required for public repos)
- Added `CHANGELOG.md` (versioned project)
- Created `.github/workflows/` for CI/CD

### Example 2: Python Project

**BEFORE (18 files in root):**
```
my-python-app/
├── README.md
├── requirements.txt
├── setup.py
├── main.py
├── config.py
├── database.py
├── .env
├── .env.example
├── .gitignore
├── .flake8
├── pytest.ini
├── Dockerfile
├── docker-compose.yml
├── deploy.sh
└── docs/
    └── README.md
```

**AFTER (7 files in root):**
```
my-python-app/
├── README.md
├── LICENSE
├── requirements.txt
├── setup.py
├── .gitignore
├── CHANGELOG.md
├── .github/
│   └── workflows/
│       └── ci.yml
├── config/
│   ├── config.py
│   ├── .flake8
│   └── pytest.ini
├── scripts/
│   └── deploy.sh
├── docs/
│   └── README.md
├── src/
│   ├── main.py
│   └── database.py
├── docker-compose.yml
└── Dockerfile
```

**Changes Made:**
- Moved `main.py`, `database.py` → `src/`
- Moved `config.py`, `.flake8`, `pytest.ini` → `config/`
- Moved `deploy.sh` → `scripts/`
- Removed `.env` (should be in `.gitignore`, not committed)
- Added `LICENSE`
- Added `CHANGELOG.md`

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

**Automated Check:**
```bash
# Run validation script (from repository root)
./scripts/validate-section-1.sh

# Or if scripts are in ALIGNMENT directory
../ALIGNMENT/scripts/validate-section-1.sh .
```

**Note:** See [scripts/README.md](../scripts/README.md) for validation script usage.

**Done Criteria:**
- ✅ Root directory contains only essential files
- ✅ All required files exist (README, LICENSE if public, .gitignore)
- ✅ `.gitignore` properly configured
- ✅ No build artifacts or secrets in repository
- ✅ CHANGELOG.md exists if versioned project

## Rollback Procedures

If root directory cleanup causes issues:

**Option 1: Restore moved files**
```bash
# Restore files from previous commit
git checkout <previous-commit> -- filename
```

**Option 2: Restore entire root structure**
```bash
# Restore all root files from backup
git checkout backup/pre-alignment -- .
```

**Option 3: Revert commit**
```bash
git revert <commit-hash>
```

**Option 4: Manual restoration**
- Move files back from `config/`, `scripts/`, `docs/`
- Update any references
- Test build

**Prevention:** Create backup branch before major restructuring: `git checkout -b backup/pre-alignment`

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