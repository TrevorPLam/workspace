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

**Action:** Organize source code
- [ ] Create dedicated source directory based on ecosystem:
  - JavaScript/Python/Rust: `src/`
  - Go: `cmd/` + `pkg/`
  - JS monorepos: `apps/` + `packages/`
- [ ] Move all production source files out of root
- [ ] Ensure structure follows language conventions
- [ ] Verify structure is predictable and intentional

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