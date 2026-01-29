# 0. Prerequisites & Setup

**Prerequisites:** None (this is the starting point)

Before beginning repository alignment, ensure you have completed these foundational steps.

## Pre-Flight Checklist

### Essential Setup
- [ ] Git repository initialized (`git init`) or cloned from remote
- [ ] Package manager chosen and configured:
  - JavaScript/TypeScript: npm, yarn, or pnpm
  - Python: pip, poetry, or pipenv
  - Go: Go modules (go.mod)
  - Rust: Cargo
  - Other: [specify]
- [ ] Basic project structure exists (even if minimal)
- [ ] You have write access to the repository

### Decision Points

Before proceeding, answer these questions to determine your path. **Document your answers** in `alignment-progress.json` for reference throughout implementation.

#### 1. Is this a versioned project?
**Check if:**
- Library/package that others will install → **Yes**
- CLI tool with releases → **Yes**
- API with versioned endpoints → **Yes**
- Internal application → **No**
- One-off script → **No**
- Prototype/experiment → **No**

**If Yes:** You'll need `CHANGELOG.md` (see [Section 12](12-Change-Management.md))  
**If No:** Skip `CHANGELOG.md` requirements

**Your Answer:** [ ] Yes [ ] No

#### 2. What is your project type?
- **Library/Package:** Reusable code others install
- **Application:** Standalone app (web, mobile, desktop)
- **CLI Tool:** Command-line interface
- **API/Service:** Backend service or API
- **Infrastructure:** IaC, configs, deployment code
- **Monorepo:** Multiple packages/apps in one repo
- **Documentation:** Docs-only repository
- **Other:** [specify]

**Impact:** Some sections will have type-specific guidance

**Your Answer:** [ ] Library/Package [ ] Application [ ] CLI Tool [ ] API/Service [ ] Infrastructure [ ] Monorepo [ ] Documentation [ ] Other: _______________

#### 3. What is your primary language/framework?
- JavaScript/TypeScript (Node.js, React, Vue, etc.)
- Python (Django, Flask, FastAPI, etc.)
- Go
- Rust
- Java/Kotlin
- C/C++
- Other: [specify]

**Impact:** Language-specific conventions will apply

**Your Answer:** [ ] JavaScript/TypeScript [ ] Python [ ] Go [ ] Rust [ ] Java/Kotlin [ ] C/C++ [ ] Other: _______________

#### 4. What package manager will you use?
- npm/yarn/pnpm (JavaScript/TypeScript)
- pip/poetry/pipenv (Python)
- go mod (Go - built-in)
- Cargo (Rust - built-in)
- Maven/Gradle (Java)
- Other: [specify]

**Impact:** Determines lockfile requirements and dependency management

**Your Answer:** [ ] npm/yarn/pnpm [ ] pip/poetry/pipenv [ ] go mod [ ] Cargo [ ] Maven/Gradle [ ] Other: _______________

#### 5. Which CI/CD platform will you use?
- GitHub Actions
- GitLab CI
- Jenkins
- CircleCI
- Azure DevOps
- Other: [specify]
- None (local only)

**Impact:** Determines CI/CD configuration location and format

**Your Answer:** [ ] GitHub Actions [ ] GitLab CI [ ] Jenkins [ ] CircleCI [ ] Azure DevOps [ ] Other: _______________ [ ] None

#### 6. Is this a public or private repository?
- **Public:** LICENSE file is mandatory
- **Private:** LICENSE is optional but recommended

**Your Answer:** [ ] Public [ ] Private

#### 7. What is your repository structure type?
- **Single Package/Library:** Flat structure with `src/` or `cmd/`+`pkg/`
- **Monorepo:** Multiple packages/apps (`apps/` + `packages/`)
- **Application:** Feature-based or layer-based structure

**Impact:** Determines directory organization (see [Section 2](02-Source-Code-Layout.md))

**Your Answer:** [ ] Single Package/Library [ ] Monorepo [ ] Application

#### 8. Do you have existing code in the root directory?
- **Yes:** Needs migration to appropriate directory
- **No:** Starting fresh

**Impact:** Determines if Section 2 requires file migration tasks

**Your Answer:** [ ] Yes [ ] No

#### 9. Do you have secrets currently in the repository?
⚠️ **CRITICAL:** If yes, these must be removed immediately before proceeding!

- **Yes:** Needs immediate remediation (see [Section 3](03-Configuration-Environment.md) and [Section 10](10-Security-Compliance.md))
- **No:** Needs prevention setup

**Your Answer:** [ ] Yes ⚠️ [ ] No

#### 10. Who are the repository owners/maintainers?
**Required for:** CODEOWNERS file (see [Section 9](09-Governance-Ownership.md))

- Primary maintainer: _______________
- Additional maintainers: _______________
- Teams (if applicable): _______________

**Your Answer:** Primary: _______________ Additional: _______________ Teams: _______________

## Key Concepts Explained

### What is a lockfile?
A lockfile (e.g., `package-lock.json`, `poetry.lock`, `Cargo.lock`) locks dependency versions to ensure consistent installs across environments. **Always commit lockfiles** to version control.

### What is CI/CD?
**CI (Continuous Integration):** Automated testing and validation on every code change  
**CD (Continuous Deployment/Delivery):** Automated deployment to environments  
**Purpose:** Catch issues early, ensure quality, automate releases

### What is a monorepo?
A repository containing multiple related projects/packages. Examples: `apps/` + `packages/` structure, or multiple services in one repo.

### What is SAST?
**Static Application Security Testing** - Automated code analysis to find security vulnerabilities before deployment.

### What is structured logging?
Logging in a consistent format (typically JSON) that machines can parse, enabling better search, filtering, and analysis.

### What is a CODEOWNERS file?
A file that specifies who must approve changes to specific parts of the codebase. Required for code review automation.

## Document Your Answers

**What this means:** After answering the 10 decision point questions above, you need to save your answers in a JSON file (a structured text file that both humans and computers can read).

**Why it matters:** Your answers guide which tasks you need to do in later sections. Documenting them ensures you (and AI agents) can reference them consistently throughout the alignment process.

### Task: Create alignment-progress.json *(USER/AGENT)*

- [ ] **Create or find the file location:**
  - If `meta/` folder exists → Create `meta/alignment-progress.json`
  - If not → Create `alignment-progress.json` in root (you'll move it to `meta/` in Section 1)

- [ ] **Copy this template and fill in YOUR answers:**
  ```json
  {
    "repository": "my-project-name",
    "started": "2026-01-29",
    "decision_points": {
      "is_versioned_project": true,
      "project_type": "application",
      "primary_language": "JavaScript",
      "package_manager": "npm",
      "cicd_platform": "github",
      "is_public_repo": true,
      "structure_type": "application",
      "has_code_in_root": true,
      "has_secrets_in_repo": false,
      "repository_owners": ["your-github-username"]
    }
  }
  ```

- [ ] **Replace the example values** with your actual answers from above:
  - `"my-project-name"` → your repository name
  - `"2026-01-29"` → today's date
  - `true` → `false` (or vice versa) based on your answers
  - `"application"` → your actual project type
  - `"JavaScript"` → your actual primary language
  - etc.

- [ ] **Save the file**

- [ ] **Commit to Git:**
  ```bash
  git add alignment-progress.json
  git commit -m "Add alignment decision points"
  ```

**Example for a Python web API:**
```json
{
  "repository": "my-api",
  "started": "2026-01-29",
  "decision_points": {
    "is_versioned_project": true,
    "project_type": "api",
    "primary_language": "Python",
    "package_manager": "poetry",
    "cicd_platform": "github",
    "is_public_repo": false,
    "structure_type": "application",
    "has_code_in_root": true,
    "has_secrets_in_repo": false,
    "repository_owners": ["api-team"]
  }
}
```

**Note:** Additional questions will appear in later sections as needed. You'll add them to this same file when you reach those sections.

## Verification

**Before proceeding to Section 1, verify:**
- [ ] Git repository is initialized/cloned
- [ ] Package manager is chosen
- [ ] You've answered all 10 decision point questions above
- [ ] Answers documented in `alignment-progress.json`
- [ ] ⚠️ If secrets exist in repo: Remediation plan created
- [ ] You understand the key concepts (or have them bookmarked for reference)

## Common Issues & Solutions

**Issue:** "I haven't chosen a package manager yet"  
- **Solution:** Choose based on your language:
  - JavaScript/TypeScript → npm (default) or pnpm (faster)
  - Python → pip (simple) or poetry (better dependency management)
  - Go → Built-in (`go mod`)
  - Rust → Cargo (built-in)

**Issue:** "I don't know if my project is versioned"  
- **Solution:** Ask: "Will others install/use this as a dependency?" If yes → versioned. If no → internal tool/app.

**Issue:** "I'm not sure which CI/CD platform to use"  
- **Solution:** 
  - GitHub repo → GitHub Actions (easiest)
  - GitLab repo → GitLab CI (built-in)
  - Self-hosted → Jenkins
  - Small project → Start with GitHub Actions (free for public repos)

**Issue:** "I don't understand some of these concepts"  
- **Solution:** That's okay! The ALIGNMENT standard will guide you. Bookmark this page and refer back as needed. You can also skip advanced concepts (P2) initially.

## Next Steps

Once prerequisites are complete, proceed to:
- **[Section 1: Root Directory Structure](01-Root-Directory-Structure.md)** - Start here

---

**Note for AI Agents:** Before executing Section 1, ensure all decision points above are answered. Store answers in `alignment-progress.json` for context.
