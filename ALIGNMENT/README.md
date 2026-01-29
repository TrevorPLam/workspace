# Repository Alignment Standard

**Version:** 1.1.0  
**Last Updated:** 2026-01-28  
*(Human-Friendly, Hybrid Tone, P0 → P1 → P2)*

This standard defines how every repository should be organized, documented, and maintained. It is platform-agnostic, language-agnostic, and suitable for monorepos, polyrepos, app repos, infra repos, and library repos.

> **Note:** When implementing ALIGNMENT, specify which version you're following (e.g., "ALIGNMENT v1.0.0") in your repository documentation for future reference.

## Table of Contents

- [Quick Start Checklist](Quick-Start-Checklist.md) - Start here for new repos
- [Minimum Viable Alignment (MVA)](Minimum-Viable-Alignment.md) - **Quick start** (~90 minutes)
- [Migration Guide](Migration-Guide.md) - **For existing repositories** - Incremental adoption strategies
- [Reference Guide](#reference-guide) - Detailed standards by category
- [Workflow Guides](Workflow-Guides.md) - Step-by-step processes
- [Quick Reference](Quick-Reference.md) - Cheat sheets and templates
- [Templates](Templates.md) - File templates and examples
- [Success Metrics](Success-Metrics.md) - Defining "successfully aligned"
- [Cost-Benefit Analysis](Cost-Benefit-Analysis.md) - ROI and investment analysis
- [Common Pitfalls](Common-Pitfalls.md) - Mistakes to avoid
- [FAQ](FAQ.md) - Frequently asked questions
- [Getting Help](Getting-Help.md) - Where to find help
- [Contributing to ALIGNMENT](CONTRIBUTING.md) - How to improve this standard
- [For AI Agents](#for-ai-agents) - Execution guidance for AI agents

---

## Priority Tiers

- **P0** — Required (must have)
- **P1** — Strongly Recommended (should have)
- **P2** — Advanced / Optional / Scale-Driven (nice to have)

---

## When Not to Align

ALIGNMENT is comprehensive, but it's not always necessary. Consider skipping or using a minimal subset if:

- **Tiny projects:** Single-file scripts, one-off utilities, or prototypes (< 100 lines)
- **Temporary projects:** Experiments, proofs-of-concept, or throwaway code
- **Legacy systems:** Very old codebases where alignment would be disruptive without clear benefit
- **External constraints:** Projects with strict organizational requirements that conflict with ALIGNMENT
- **Time-critical:** When alignment would delay critical deliverables (align incrementally instead)

**Minimal Alignment:** Even for small projects, consider at minimum:
- README.md with basic info
- `.gitignore` configured
- LICENSE file (if public)
- Basic CI/CD (if code is shared)

For existing repositories, see [Migration Guide](Migration-Guide.md) for incremental adoption strategies.

---

## Reference Guide

Detailed standards organized by category. Use this as a reference when implementing specific requirements.

### Core Structure
0. [Prerequisites & Setup](00-Prerequisites.md) - **Start here first**
1. [Root Directory Structure](01-Root-Directory-Structure.md)
2. [Source Code Layout](02-Source-Code-Layout.md)
3. [Configuration & Environment](03-Configuration-Environment.md)
4. [Dependency Management](04-Dependency-Management.md)

### Automation & Quality
5. [CI/CD Structure](05-CI-CD-Structure.md)
6. [Testing Structure](06-Testing-Structure.md)

### Documentation & Governance
7. [Documentation Standards](07-Documentation-Standards.md)
8. [Infrastructure / IaC Structure](08-Infrastructure-IaC.md)
9. [Governance & Ownership](09-Governance-Ownership.md)

### Security & Operations
10. [Security & Compliance](10-Security-Compliance.md)
11. [Observability & Monitoring](11-Observability-Monitoring.md)
12. [Change Management](12-Change-Management.md)
13. [Quality & Health Checks](13-Quality-Health-Checks.md)

---

## For AI Agents

**Execution Order:**
1. **Start with [Section 0: Prerequisites](00-Prerequisites.md)** - Complete all decision points first
2. Complete Section 1 fully before proceeding to Section 2
3. Sections 3-4 can be done in parallel (after Section 1)
4. Section 5 Phase 1 (Bootstrap CI/CD) before Section 6
5. Section 5 Phase 2 (Complete CI/CD) after Section 6
6. Sections 7-13 can be done in order, respecting prerequisites

**Validation:**
- After each section, verify completion before proceeding
- Use `alignment-progress.json` to track state (see [Templates](Templates.md))
- If task fails, check "Common Issues & Solutions" in that section
- Run verification checks listed in each section

**Checkpoints:**
- Save progress after each section completion
- Format: `alignment-progress-{timestamp}.json`
- Store decision points from Section 0 for context
- Track blockers and resolve before proceeding

**Decision Points (from Section 0):**
- Store answers to: Is versioned project? Project type? Language? CI/CD platform? Public/private?
- Use answers to skip or include conditional tasks in later sections
- Update progress tracker with decision point answers

**Error Handling:**
- If prerequisite section incomplete: Complete it first
- If task fails: Check "Common Issues & Solutions" in that section
- If circular dependency detected: Follow Phase 1 → Phase 2 pattern (Sections 5-6)
- If validation fails: Review "Verification" subsection for that section

**Progress Tracking:**
- Initialize `alignment-progress.json` with Section 0 decision points
- Update after each section: status, tasks_complete, tasks_total
- Mark sections as "completed" only after verification passes
- Update `next_section` field to guide next steps

**Best Practices:**
- Read entire section before starting tasks
- Complete all P0 tasks before moving to next section
- P1 and P2 tasks can be done later if needed
- Verify each section before proceeding (use verification checks)
- Document any deviations or blockers in progress tracker
