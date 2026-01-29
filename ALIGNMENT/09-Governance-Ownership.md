# 9. Governance & Ownership

**Prerequisites:** 
- [Section 0: Prerequisites](00-Prerequisites.md) - Complete decision points
- [Section 1: Root Directory Structure](01-Root-Directory-Structure.md)
- [Section 5: CI/CD Structure](05-CI-CD-Structure.md) - Phase 1 (bootstrap) should be complete

## Questions to Answer

Before proceeding, answer these questions:

1. **What branching strategy do you use (or want to use)?**
   - [ ] GitHub Flow (simple: main + feature branches)
   - [ ] Git Flow (complex: main, develop, release branches)
   - [ ] Trunk-Based Development (main only, short-lived branches)
   - [ ] Other: _______________

2. **Do you have code review standards?**
   - [ ] Yes (needs documentation)
   - [ ] No (needs creation)

**Note:** Repository owners/maintainers should already be documented from Section 0 Question 10. If not, answer:
- Primary maintainer: _______________
- Additional maintainers: _______________
- Teams (if applicable): _______________

**Document your answers** in `alignment-progress.json` before proceeding.

## P0 — Required Actions

**Action:** Define ownership and contribution rules
- [ ] Create `CODEOWNERS` file in `.github/` directory (see [Section 1](01-Root-Directory-Structure.md))
- [ ] Create `CONTRIBUTING.md` with:
  - How to contribute
  - Code style guidelines
  - PR process (references CI/CD from [Section 5](05-CI-CD-Structure.md))
  - How to report issues
- [ ] Ensure no governance gaps (every area has an owner)

See [Templates](Templates.md) for CODEOWNERS and CONTRIBUTING.md templates.

## P1 — Recommended Actions

**Action:** Expand governance
- [ ] Create `maintainers.md` listing:
  - Maintainer responsibilities
  - Current maintainers
  - How to become a maintainer
- [ ] Create `waiver.md` to document intentional deviations from standards
- [ ] Document escalation process for issues

**Action:** Define branching strategy
- [ ] Document chosen branching strategy (GitHub Flow recommended for most repos, Git Flow for release-heavy projects)
- [ ] Define branch naming conventions:
  - Feature branches: `feature/description` or `feat/description`
  - Bug fixes: `fix/description` or `bugfix/description`
  - Hotfixes: `hotfix/description`
  - Documentation: `docs/description`
- [ ] Document merge vs rebase policy (typically: merge for shared branches, rebase for personal branches)
- [ ] Link branching strategy to CI/CD branch protection rules (see [Section 5 P1](05-CI-CD-Structure.md))
- [ ] Document in CONTRIBUTING.md

**Action:** Define code review standards
- [ ] Create code review checklist covering:
  - Functionality: Does it work as intended? Are edge cases handled?
  - Tests: Are tests adequate, passing, and maintainable?
  - Documentation: Is documentation updated (code comments, README, API docs)?
  - Security: Any security concerns or vulnerabilities?
  - Performance: Any performance implications or regressions?
  - Accessibility: For web apps, are a11y standards met? (see [Section 6 P1](06-Testing-Structure.md))
- [ ] Define approval requirements:
  - Minimum reviewers (typically 1-2, depending on repo size)
  - Code owner approval for sensitive areas (per CODEOWNERS)
  - All CI checks must pass before approval
- [ ] Set review SLAs:
  - Critical/urgent: 4 hours
  - Normal priority: 24-48 hours
  - Low priority: 72 hours
- [ ] Document review best practices:
  - Keep PRs small and focused (aim for <400 lines when possible)
  - Provide constructive, actionable feedback
  - Approve with suggestions when appropriate
  - Request changes only for blocking issues
- [ ] Add review guidelines to CONTRIBUTING.md and PR template (see [Section 1 P1](01-Root-Directory-Structure.md) for `.github/` folder)

## P2 — Advanced Actions

**Action:** Advanced governance
- [ ] Define repository-level SLAs:
  - Response time for issues
  - Review time for PRs
- [ ] Implement rotational ownership or stewardship model
- [ ] Track governance metrics

## Decision: What branching strategy should you use?

**Choose based on your project:**

**GitHub Flow (Recommended for most projects):**
- Simple: `main` branch + feature branches
- Good for: Continuous deployment, small teams, simple projects
- [ ] Use GitHub Flow

**Git Flow:**
- Complex: `main`, `develop`, feature branches, release branches
- Good for: Release-heavy projects, versioned libraries
- [ ] Use Git Flow

**Trunk-Based Development:**
- Very simple: `main` branch only, short-lived feature branches
- Good for: Large teams, fast iteration
- [ ] Use Trunk-Based Development

**Action:** Document your choice
- [ ] Choose branching strategy
- [ ] Document in CONTRIBUTING.md
- [ ] Define branch naming conventions
- [ ] Set up branch protection rules (Section 5 P1)

## Verification

**Manual Check:**
- [ ] Verify `CODEOWNERS` file exists in `.github/` directory
- [ ] Check `CONTRIBUTING.md` exists and is complete
- [ ] Verify CODEOWNERS syntax is valid (GitHub will validate)
- [ ] Check branching strategy is documented
- [ ] Verify code review standards are defined
- [ ] If P1 completed: Check `maintainers.md` exists

**Automated Check (Future):**
```bash
./scripts/validate-section-9.sh
```

**Done Criteria:**
- ✅ CODEOWNERS file exists and is valid
- ✅ CONTRIBUTING.md exists with contribution guidelines
- ✅ Branching strategy documented
- ✅ Code review standards defined
- ✅ No governance gaps (every area has owner)

## Common Issues & Solutions

**Issue:** "I don't know who should be in CODEOWNERS"  
- **Solution:** 
  - Start with repository maintainers/owners
  - Add team leads for specific areas
  - Use GitHub teams if available: `@org/team-name`
  - Update as team grows

**Issue:** "CODEOWNERS file doesn't work"  
- **Solution:** 
  - Check file location: Must be `.github/CODEOWNERS` or root
  - Verify syntax: `path @username` or `path @team`
  - Check usernames/teams exist in GitHub/GitLab
  - Test by creating a PR and checking if owners are requested

**Issue:** "I don't know which branching strategy to use"  
- **Solution:** 
  - Start with GitHub Flow (simplest)
  - Switch to Git Flow if you need release branches
  - Use Trunk-Based if you deploy multiple times per day
  - You can change strategies later

**Issue:** "I'm a solo developer, do I need CODEOWNERS?"  
- **Solution:** 
  - Yes, still create CODEOWNERS (put yourself)
  - Helps when you add collaborators later
  - Enables branch protection rules
  - Documents ownership clearly

**Issue:** "Code review standards seem too strict"  
- **Solution:** 
  - Start with basic checklist (functionality, tests, docs)
  - Add more standards as team grows
  - Adjust SLAs based on team size and priorities
  - Review standards should evolve with team