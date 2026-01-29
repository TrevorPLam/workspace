# 2. Source Code Layout

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 1: Root Directory Structure](01-Root-Directory-Structure.md)

**Effort Estimate:**
- **New repository:** 15-30 minutes
- **Existing repository (small, < 10 source files):** 1-2 hours
- **Existing repository (medium, 10-50 source files):** 2-4 hours
- **Existing repository (large, 50+ source files):** 4-8 hours
- **Note:** Time increases significantly if import paths need updating across many files

## Questions to Answer

Before proceeding, answer these questions:

1. **What is your repository structure type?**
   - **Note:** This should match your answer from Section 0 Question 7. If different, update Section 0 answer.
   - [ ] Single Package/Library (flat structure) - **What's the main package/module name?** _______________
   - [ ] Monorepo (multiple packages/apps) - **How many apps/packages?** _______________ **What are they?** _______________
   - [ ] Application (feature-based or layer-based) - **What's the organization approach?** (features, layers, domains)

2. **Where should your source code live?**
   - [ ] `src/` directory (JavaScript/Python/Rust standard)
   - [ ] `cmd/` + `pkg/` (Go standard)
   - [ ] `apps/` + `packages/` (monorepo standard)
   - [ ] Other: _______________
   - [ ] Code currently in root - **What files need to move?** _______________

**Document your answers** in `alignment-progress.json` before proceeding.

## Before/After Examples

### Example 1: Go Project Migration

**BEFORE (source files in root):**
```
my-go-app/
├── README.md
├── go.mod
├── go.sum
├── main.go
├── handlers.go
├── database.go
├── config.go
├── utils.go
└── tests/
    └── main_test.go
```

**AFTER (proper Go structure):**
```
my-go-app/
├── README.md
├── go.mod
├── go.sum
├── cmd/
│   └── app/
│       └── main.go
├── pkg/
│   ├── handlers/
│   │   └── handlers.go
│   ├── database/
│   │   └── database.go
│   ├── config/
│   │   └── config.go
│   └── utils/
│       └── utils.go
└── tests/
    └── main_test.go
```

**Changes Made:**
- Moved `main.go` → `cmd/app/main.go` (Go convention for executables)
- Moved other source files → `pkg/` organized by domain
- Created proper Go package structure

### Example 2: Monorepo Migration

**BEFORE (flat structure):**
```
monorepo/
├── README.md
├── package.json
├── frontend/
│   └── src/
├── backend/
│   └── src/
├── shared/
│   └── utils.js
└── docs/
```

**AFTER (proper monorepo structure):**
```
monorepo/
├── README.md
├── package.json
├── pnpm-workspace.yaml
├── apps/
│   ├── frontend/
│   │   └── src/
│   └── backend/
│       └── src/
├── packages/
│   └── shared/
│       └── src/
│           └── utils.js
└── docs/
```

**Changes Made:**
- Moved `frontend/`, `backend/` → `apps/`
- Moved `shared/` → `packages/shared/`
- Added workspace configuration (`pnpm-workspace.yaml`)

## P0 — Required Actions

**Action:** Organize source code into dedicated directories

**What this means:** Move all your actual code (the files that make your application work) out of the root directory and into a clearly-named folder structure. The root should only have configuration and documentation - never actual source code.

**Why it matters:** Having source code scattered in the root directory makes it impossible to understand what's important. A dedicated source directory signals "this is where the actual application code lives."

### Task 1: Choose your source directory structure *(USER)*

**Based on your answers from Section 0, pick the right structure for your project type:**

- [ ] **For JavaScript/TypeScript/Python/Rust projects:**
  - Use `src/` directory for maintainability

**What this means:** Refine how code is organized within your source directory to make it easier to find things and understand the codebase structure.

**Why it matters:** As projects grow, poor organization leads to "code sprawl" where nobody knows where anything goes. Good organization makes onboarding faster and prevents confusion.

### Task 1: Keep directory depth shallow *(USER/AGENT)*

**Goal:** Maximum 3-4 levels deep. Deeper = harder to navigate.

- [ ] **Count current depth:**
  ```bash
  # Mac/Linux
  find src -type d | awk -F/ '{print NF-1}' | sort -rn | head -1
  
  # Or just look at your file tree
  ```

- [ ] **If deeper than 4 levels, flatten:**
  
  **Bad (too deep):**
  ```
  src/app/features/dashboard/components/charts/line/LineChart.js  # 7 levels!
  ```
  
  **Good (flattened):**
  ```
  src/components/dashboard/LineChart.js  # 3 levels
  ```

- [ ] **Consolidate overly nested directories:**
  ```bash
  # Move deeply nested files up
  git mv src/app/features/dashboard/components/ src/components/dashboard/
  ```

### Task 2: Use descriptive directory names *(USER)*

- [ ] **Review all directory names** - Are they clear?
  
  **Replace vague names:**
  - ❌ `misc/` → ✅ `utilities/` or `helpers/` (be specific!)
  - ❌ `stuff/` → ✅ Delete or categorize properly
  - ❌ `temp/` → ✅ Should not exist in version control
  - ❌ `new/` → ✅ Use descriptive name, not temporal
  - ❌ `old/` → ✅ Delete old code, don't keep it around

- [ ] **Rename unclear directories:**
  ```bash
  git mv src/misc src/utilities
  git mv src/helpers src/utilities  # Consolidate similar things
  ```

### Task 3: Consolidate shared utilities *(USER/AGENT)*

**Problem:** Multiple "utils" or "helpers" folders scattered around.

- [ ] **Find all utility directories:**
  ```bash
  find src -type d -name "utils" -o -name "helpers" -o -name "common"
  ```

- [ ] **Consolidate into single location:**
  
  **Before (scattered):**
  ```
  src/components/utils/
  src/services/utils/
  src/api/helpers/
  src/common/
  ```
  
  **After (consolidated):**
  ```
  src/utils/
  ├── date.js
  ├── string.js
  ├── validation.js
  └── http.js
  ```

- [ ] **Move and update imports:**
  ```bash
  # Move files
  mkdir -p src/utils
  git mv src/components/utils/* src/utils/
  git mv src/services/utils/* src/utils/
  
  # Update imports in files that used them
  # Find files with old imports:
  grep -r "from './utils'" src/
  # Update each to: from '../utils/' or '@/utils/'
  ```

### Task 4: Remove or scope vague directories *(USER)*

- [ ] **Audit directories with generic names:**
  
  - `helpers/` - **Action:** Rename to what they actually help with
    - ✅ Better: `date-helpers/`, `string-helpers/`, or move to `utils/`
  
  - `utils/` - **Action:** Make sure it's clearly scoped
    - ✅ Keep if: Contains genuinely reusable utilities
    - ❌ Delete if: Just one or two functions (move them where they're used)
  
  - `common/` - **Action:** Rename to be specific
    - ✅ Better: `shared/`, `core/`, or specific name like `auth/`
  
  - `misc/` or `stuff/` - **Action:** Delete or categorize
    - Every file should have a clear category

- [ ] **For each vague directory, choose:**
  - **Rename it** to be descriptive
  - **Move files** to more specific locations
  - **Delete it** if it only has 1-2 files (move those files to better locations)

### Task 5: Verify improvements *(USER)*

- [ ] **Walk through your `src/` structure** - Can you explain what each directory does?
- [ ] **Test navigation** - Can you find important files quickly?
- [ ] **Ask a teammate** - Show them the structure, do they understand it?

**Done Criteria:**
- ✅ Directory depth is ≤4 levels
- ✅ All directories have clear, descriptive names
- ✅ Utilities consolidated in single location
- ✅ No vague directories (misc, stuff, temp)
- ✅ Structure is easy to navigate and explain
- ✅ Changes committed to Git
  - Use `pkg/` for reusable packages
  - Example: `cmd/myapp/main.go`, `pkg/database/`, `pkg/api/`

- [ ] **For JavaScript/TypeScript monorepos (multiple apps/packages):**
  - Use `apps/` for applications (things that run)
  - Use `packages/` for shared libraries (things that are imported)
  - Example: `apps/web/`, `apps/api/`, `packages/ui/`, `packages/shared/`

- [ ] **Document your choice** in `alignment-progress.json`:
  ```json
  {
    "section_2": {
      "source_structure": "src" // or "cmd+pkg" or "apps+packages"
    }
  }
  ```

### Task 2: Create source directories *(AGENT)*

- [ ] **Create the main source directory:**
  - For `src/`: `mkdir src`
  - For Go: `mkdir cmd pkg`
  - For monorepo: `mkdir apps packages` and enforce structure

**What this means:** Add formal architectural patterns that enforce how different parts of your code can interact. This prevents "spaghetti code" where everything depends on everything else.

**Why it matters:** In large codebases, without boundaries, code becomes tangled. Enforcing structure at the architectural level prevents technical debt and makes the system easier to reason about.

### Task 1: Organize by domain → module → component *(AGENT)*

**Goal:** Structure reflects business domains, not just technical layers.

- [ ] **Choose an architectural pattern:**
  
  **Domain-Driven Design (DDD) - Recommended for complex business logic:**
  ```
  src/
  ├── domains/
  │   ├── user/
  │   │   ├── models/
  │   │   ├── services/
  │   │   ├── repositories/
  │   │   └── api/
  │   ├── order/
  │   │   ├── models/
  │   │   ├── services/
  │   │   └── api/
  │   └── payment/
  │       ├── models/
  │       ├── services/
  │       └── api/
  ├── shared/      # Cross-domain utilities
  └── infrastructure/  # Technical concerns
  ```
  
  **Feature-Based - Good for applications:**
  ```
  src/
  ├── features/
  │   ├── authentication/
  │   │   ├── components/
  │   │   ├── hooks/
  │   │   ├── services/
  │   │   └── types/
  │   ├── dashboard/
  │   └── profile/
  ├── shared/
  └── core/
  ```

- [ ] **Refactor existing structure** to match chosen pattern
- [ ] **Document the pattern** in `docs/architecture.md`

### Task 2: Enforce dependency rules *(AGENT)*

**Goal:** Prevent circular dependencies and enforce one-way data flow.

- [ ] **Define dependency rules:**
  
  **Example rules:**
  - `apps/` can depend on `packages/` but NOT vice versa
  - `domains/` are independent - cannot depend on each other
  - `shared/` has no dependencies on business logic
  - `infrastructure/` can depend on domains, but domains cannot depend on infrastructure
  
- [ ] **Document rules** in `docs/architecture.md`:
  ```markdown
  ## Dependency Rules
  
  1. **Apps → Packages (one-way)**
     - Applications can import from shared packages
     - Packages CANNOT import from apps
  
  2. **Domain Independence**
     - Domains are self-contained
     - Cross-domain communication through interfaces
  
  3. **Layered Architecture**
     - API → Services → Repositories → Models
     - Each layer can only depend on layers below it
  ```

- [ ] **Set up automated enforcement** (choose one):
  
  **JavaScript/TypeScript - Use dependency-cruiser:**
  ```bash
  npm install --save-dev dependency-cruiser
  
  # Create .dependency-cruiser.js
  npx depcruise --init
  
  # Add rules:
  # - Forbid apps/ from importing apps/
  # - Forbid packages/ from importing apps/
  ```
  
  **Python - Use import-linter:**
  ```bash
  pip install import-linter
  
  # Create .import-linter.ini
  [importlinter]
  root_package = src
  
  [importlinter:contract:apps-cannot-import-apps]
  name = Apps cannot import other apps
  type = forbidden
  source_modules =
      apps.*
  forbidden_modules =
      apps.*
  ```
  
  **Go - Use go-cleanarch:**
  ```bash
  go install github.com/roblaszczak/go-cleanarch@latest
  
  # Run checks
  go-cleanarch
  ```

- [ ] **Add to CI/CD** (in Section 5, add this check to your workflow)

### Task 3: Document architectural decisions (ADRs) *(USER)*

**What are ADRs?** Architecture Decision Records - lightweight docs explaining important choices.

- [ ] **Create ADR directory:**
  ```bash
  mkdir -p docs/adr
  ```

- [ ] **Create ADR template** at `docs/adr/template.md`:
  ```markdown
  # [Number]. [Title]
  
  Date: [YYYY-MM-DD]
  
  ## Status
  [Proposed | Accepted | Deprecated | Superseded]
  
  ## Context
  What is the issue we're facing? What forces are at play?
  
  ## Decision
  What decision did we make?
  
  ## Consequences
  What are the positive and negative consequences?
  ```

- [ ] **Document key decisions:**
  
  **Example ADR 001: Choose Domain-Driven Design**
  ```markdown
  # 001. Use Domain-Driven Design Structure
  
  Date: 2026-01-29
  
  ## Status
  Accepted
  
  ## Context
  Our codebase was organized by technical layers (controllers, services, models)
  which made it hard to understand business domains. Teams were stepping on
  each other's toes when working on related features.
  
  ## Decision
  Reorganize code by business domains (user, order, payment) with each domain
  being self-contained. Each domain has its own models, services, and API.
  
  ## Consequences
  **Positive:**
  - Clearer domain boundaries
  - Teams can work independently on domains
  - Easier to find business logic
  
  **Negative:**
  - Initial refactoring effort (estimated 2 weeks)
  - Some code duplication across domains
  - Team needs to learn DDD concepts
  ```

- [ ] **Create ADRs for:**
  - Directory structure choice
  - Dependency rules
  - Module boundaries
  - Any major architectural decisions

**Done Criteria:**
- ✅ Code organized by domain or feature (not just technical layers)
- ✅ Dependency rules documented
- ✅ Automated dependency enforcement configured
- ✅ At least 1 ADR created documenting architecture
- ✅ Architecture documented in `docs/architecture.md`
- ✅ Changes committed to Git
  **For `src/` structure:**
  ```bash
  mkdir -p src/components  # UI components (if applicable)
  mkdir -p src/services    # Business logic services
  mkdir -p src/utils       # Utility functions
  mkdir -p src/types       # Type definitions (TypeScript)
  ```
  
  **For Go `cmd/pkg` structure:**
  ```bash
  mkdir -p cmd/myapp       # Main application
  mkdir -p pkg/api         # API handlers
  mkdir -p pkg/database    # Database logic
  mkdir -p pkg/models      # Data models
  ```
  
  **For monorepo `apps/packages` structure:**
  ```bash
  mkdir -p apps/web/src      # Web application
  mkdir -p apps/api/src      # API server
  mkdir -p packages/ui       # Shared UI components
  mkdir -p packages/shared   # Shared utilities
  ```

### Task 3: Move source files from root to src/ *(USER/AGENT)*

- [ ] **Identify all source code files in root:**
  - Look for: `*.js`, `*.ts`, `*.py`, `*.go`, `*.rs`, etc.
  - Example: `index.js`, `app.py`, `main.go`, `server.ts`

- [ ] **Move each file to appropriate location:**
  ```bash
  # Example for JavaScript
  git mv index.js src/index.js
  git mv utils.js src/utils/utils.js
  git mv config.js src/config.js
  
  # Example for Python
  git mv app.py src/app.py
  git mv models.py src/models.py
  
  # Example for Go
  git mv main.go cmd/myapp/main.go
  ```

- [ ] **Update import paths** in files that reference moved files
  - Example (JavaScript): `import { foo } from './utils'` → `import { foo } from './utils/utils'`
  - Example (Python): `from utils import foo` → `from src.utils import foo`
  - Example (Go): Update module paths if packages moved

- [ ] **Test that application still works after moving files:**
  ```bash
  # Run your application
  npm start  # or python src/app.py, go run cmd/myapp/main.go, etc.
  
  # Run tests
  npm test  # or pytest, go test, etc.
  ```

### Task 4: Verify structure follows language conventions *(USER)*

- [ ] **Check your structure matches ecosystem standards:**
  
  **JavaScript/TypeScript:**
  - ✅ Source code in `src/`
  - ✅ Tests in `tests/` or `src/__tests__/`
  - ✅ Entry point: `src/index.js` or `src/main.ts`
  
  **Python:**
  - ✅ Package in `src/packagename/`
  - ✅ Tests in `tests/`
  - ✅ Entry point: `src/packagename/__main__.py`
  
  **Go:**
  - ✅ Commands in `cmd/appname/`
  - ✅ Packages in `pkg/`
  - ✅ Tests alongside code: `*_test.go`
  
  **Rust:**
  - ✅ Source in `src/`
  - ✅ Entry point: `src/main.rs` (binary) or `src/lib.rs` (library)
  - ✅ Tests in `tests/`

### Task 5: Ensure structure is predictable *(USER)*

- [ ] **Ask yourself:** If someone opens this repo for the first time, can they find the main code in 5 seconds?
- [ ] **Check naming:** Are directories named clearly? (`components` not `comp`, `services` not `svc`)
- [ ] **Verify entry point is obvious:** Is there a clear `main`, `index`, or `app` file?

### Task 6: Commit the reorganization *(USER)*

- [ ] **Commit your changes:**
  ```bash
  git add .
  git commit -m "Reorganize source code into src/ directory
  
  - Move all source files from root to src/
  - Update import paths
  - Maintain working application
  - Follows Section 2: Source Code Layout"
  ```

**Done Criteria:**
- ✅ All production source code is in dedicated directory (`src/`, `cmd/pkg/`, or `apps/packages/`)
- ✅ No source files (`.js`, `.py`, `.go`, `.rs`, etc.) remain in root
- ✅ Application still runs and tests pass
- ✅ Structure follows language/ecosystem conventions
- ✅ Directory names are clear and descriptive
- ✅ Changes committed to Git

## P1 — Recommended Actions

**Action:** Improve code organization
- [ ] Keep directory depth shallow (max 3-4 levels)
- [ ] Use descriptive directory names
- [ ] Consolidate shared utilities into single location
- [ ] Remove vague directories: "misc", "stuff", "helpers", "utils" (unless clearly scoped)

## P2 — Advanced Actions

**Action:** Apply architectural boundaries
- [ ] Organize by domain → module → component
- [ ] Enforce dependency rules (e.g., packages cannot depend on apps)
- [ ] Document architectural decisions in ADRs

## Decision: What is your project structure type?

**From Section 0, determine your structure:**

**Single Package/Library:**
- [ ] Use `src/` directory (JavaScript/Python/Rust)
- [ ] Use `cmd/` + `pkg/` (Go)
- [ ] Keep structure flat: `src/` → modules → files

**Monorepo (Multiple Packages):**
- [ ] Use `apps/` + `packages/` structure
- [ ] Each app/package has its own `src/` directory
- [ ] Shared code in `packages/shared/` or `packages/common/`

**Application (Web/Mobile/Desktop):**
- [ ] Use `src/` for main application code
- [ ] Separate by feature or layer: `src/components/`, `src/services/`, `src/utils/`
- [ ] Keep entry point clear: `src/index.js`, `src/main.py`, `src/main.go`

## Verification

**Manual Check:**
- [ ] Verify all production source code is in dedicated directory (not root)
- [ ] Check directory depth: `find src -type d | wc -l` (or equivalent) - should be ≤ 4 levels deep
- [ ] Verify no source files in root directory
- [ ] Check structure follows language conventions:
  - JavaScript/TypeScript: `src/` with clear module organization
  - Python: `src/` or package name directory
  - Go: `cmd/` for executables, `pkg/` for libraries
  - Rust: `src/` with `main.rs` or `lib.rs`
- [ ] Verify structure is predictable (someone new can find code easily)

**Automated Check (Future):**
```bash
./scripts/validate-section-2.sh
```

**Done Criteria:**
- ✅ All source code in dedicated directory
- ✅ Structure follows language conventions
- ✅ Directory depth ≤ 4 levels
- ✅ No vague directory names ("misc", "stuff", "helpers" without clear scope)

## Rollback Procedures

If moving source files causes issues:

**Option 1: Revert specific commit**
```bash
git revert <commit-hash>
```

**Option 2: Restore from backup branch**
```bash
# If you created backup branch before migration
git checkout backup/pre-alignment
git checkout -b restore/pre-alignment
```

**Option 3: Restore specific files**
```bash
# Restore files from previous commit
git checkout <previous-commit> -- path/to/file
```

**Option 4: Manual restoration**
```bash
# Move files back to original location
git mv src/file.js ./
# Update imports back
# Test build
```

**Prevention:** Always test after moving files before proceeding to next section.

## Common Issues & Solutions

**Issue:** "I don't know which directory structure to use"  
- **Solution:** 
  - Check Section 0 decision: What is your project type?
  - Single package → `src/`
  - Multiple packages → `apps/` + `packages/`
  - Go project → `cmd/` + `pkg/`
  - Follow language conventions (see language-specific guides)

**Issue:** "I have source files in root directory"  
- **Solution:** 
  - Create appropriate directory (`src/`, `cmd/`, etc.)
  - Move all `.js`, `.py`, `.go`, `.rs` files to source directory
  - Update import paths in code
  - Update build configuration if needed

**Issue:** "My directory structure is too deep (5+ levels)"  
- **Solution:** 
  - Flatten structure: combine related directories
  - Use feature-based organization instead of layer-based
  - Example: `src/user/profile/` instead of `src/models/user/profile/`

**Issue:** "I have vague directory names like 'utils' or 'helpers'"  
- **Solution:** 
  - Rename to be specific: `utils/` → `string-utils/`, `date-utils/`
  - Or scope by domain: `user-utils/`, `auth-utils/`
  - If truly generic, document what goes in there

**Issue:** "I'm not sure if my structure follows conventions"  
- **Solution:** 
  - Check language-specific guides:
    - **JavaScript/TypeScript:** [Node.js best practices](https://github.com/goldbergyoni/nodebestpractices), [TypeScript project structure](https://www.typescriptlang.org/docs/handbook/project-references.html)
    - **Python:** [Python packaging guide](https://packaging.python.org/), [PEP 8](https://pep8.org/), [Hitchhiker's Guide to Python](https://docs.python-guide.org/writing/structure/)
    - **Go:** [Standard Go Project Layout](https://github.com/golang-standards/project-layout), [Go project structure](https://go.dev/doc/modules/layout)
    - **Rust:** [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/), [Cargo project layout](https://doc.rust-lang.org/cargo/guide/project-layout.html)
    - **Java/Kotlin:** [Maven standard directory layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html), [Gradle project structure](https://docs.gradle.org/current/userguide/java_project_layout.html)
    - **C#/.NET:** [.NET project structure](https://docs.microsoft.com/en-us/dotnet/core/project-structure), [ASP.NET Core structure](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/)
    - **PHP:** [PSR-4 autoloading standard](https://www.php-fig.org/psr/psr-4/), [Composer project structure](https://getcomposer.org/doc/01-basic-usage.md)
    - **Ruby:** [Ruby project structure](https://guides.rubyonrails.org/getting_started.html#creating-the-blog-application), [Bundler best practices](https://bundler.io/guides/creating_gem.html)
    - **Swift:** [Swift package structure](https://www.swift.org/documentation/package-manager/), [iOS project structure](https://developer.apple.com/documentation/xcode/organizing-your-project-with-files-and-folders)