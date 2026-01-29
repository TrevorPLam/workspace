# 2. Source Code Layout

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 1: Root Directory Structure](01-Root-Directory-Structure.md)

## Questions to Answer

Before proceeding, answer these questions:

1. **What is your repository structure type?**
   - **Note:** This should match your answer from Section 0 Question 7. If different, update Section 0 answer.
   - [ ] Single Package/Library (flat structure)
   - [ ] Monorepo (multiple packages/apps)
   - [ ] Application (feature-based or layer-based)

2. **Do you have existing code in the root directory?**
   - [ ] Yes (needs migration to appropriate directory)
   - [ ] No (starting fresh)

**Document your answers** in `alignment-progress.json` before proceeding.

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
    - JavaScript: [Node.js best practices](https://github.com/goldbergyoni/nodebestpractices)
    - Python: [Python packaging guide](https://packaging.python.org/)
    - Go: [Standard Go Project Layout](https://github.com/golang-standards/project-layout)
    - Rust: [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)