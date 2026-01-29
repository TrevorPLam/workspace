# Research: Prerequisites & Setup

**Last Updated:** 2026-01-28  
**Research Scope:** Fundamentals, best practices, highest standards, innovative techniques

## Fundamentals

### Core Concepts

**Repository Initialization:**
- Git as version control standard (distributed VCS)
- Package manager selection based on language ecosystem
- Project structure planning before coding begins

**Decision Points:**
- Versioning strategy (SemVer, CalVer, or internal)
- Project type classification (library, application, CLI, API, infrastructure)
- Language/framework selection
- CI/CD platform choice
- Public vs private repository considerations

## Best Practices

### Repository Setup

1. **Initialize Git First**
   - Start with `git init` or clone from template
   - Configure `.gitignore` before first commit
   - Set up remote early for backup

2. **Choose Package Manager Early**
   - JavaScript/TypeScript: pnpm (fastest, disk-efficient) or npm (most compatible)
   - Python: Poetry (best dependency resolution) or pip (simplest)
   - Go: Built-in `go mod` (standard)
   - Rust: Cargo (built-in, excellent)

3. **Document Decisions**
   - Record decision points in `alignment-progress.json`
   - Document rationale for future reference
   - Update decisions as project evolves

### Decision-Making Framework

**Versioned Project Criteria:**
- Will others install/use as dependency? → Yes
- Has public API? → Yes
- Internal tool only? → No
- Prototype/experiment? → No

**Project Type Selection:**
- Library: Reusable code, published to registries
- Application: Standalone executable/service
- CLI: Command-line tool
- API: Backend service with endpoints
- Infrastructure: IaC, configs, deployment
- Monorepo: Multiple related packages/apps

## Highest Standards

### Industry Standards

**Semantic Versioning (SemVer):**
- MAJOR.MINOR.PATCH format
- MAJOR: Breaking changes
- MINOR: New features, backward compatible
- PATCH: Bug fixes
- Pre-release: 1.0.0-alpha.1, 1.0.0-beta.1, 1.0.0-rc.1

**Package Manager Standards:**
- Lockfiles must be committed (reproducibility)
- Use exact versions for critical dependencies
- Document minimum supported versions

**CI/CD Platform Selection:**
- GitHub Actions: Best for GitHub repos, free for public
- GitLab CI: Built-in, excellent for self-hosted
- Jenkins: Most flexible, requires maintenance
- CircleCI: Good for complex workflows
- Azure DevOps: Best for Microsoft ecosystem

## Innovative Techniques

### Modern Approaches

**Repository Templates:**
- GitHub template repositories
- Scaffolding tools (Yeoman, Cookiecutter, etc.)
- Automated setup scripts

**Decision Documentation:**
- ADRs (Architecture Decision Records) for major decisions
- Decision trees for complex choices
- Automated decision validation

**Progressive Enhancement:**
- Start minimal (MVA - Minimum Viable Alignment)
- Add complexity as needed
- Document what was skipped and why

### Emerging Practices (2026)

**AI-Assisted Setup:**
- AI code generators for boilerplate
- Automated dependency analysis
- Intelligent project structure suggestions

**Dev Containers:**
- VS Code Dev Containers for consistent environments
- GitHub Codespaces integration
- Docker-based development environments

**Repository Health Scoring:**
- Automated compliance checking
- Health dashboards
- Remediation suggestions

## Unique Methodologies

### Domain-Driven Design (DDD)
- Organize by business domains, not technical layers
- Clear bounded contexts
- Ubiquitous language

### Trunk-Based Development
- Main branch only (or very short-lived branches)
- Continuous integration
- Feature flags for incomplete features

### GitOps Principles
- Infrastructure as code
- Version-controlled everything
- Automated deployments

## References

- [Semantic Versioning Specification](https://semver.org/)
- [Git Best Practices](https://git-scm.com/book)
- [Package Manager Comparisons](https://pnpm.io/feature-comparison)
- [CI/CD Platform Comparison](https://www.atlassian.com/continuous-delivery/continuous-integration/tools)
