# Research: Root Directory Structure

**Last Updated:** 2026-01-28  
**Research Scope:** Fundamentals, best practices, highest standards, innovative techniques

## Fundamentals

### Core Principles

**Clean Root Directory:**
- Only essential files at root level
- Maximum 8-10 files (excluding hidden files)
- Clear purpose for each file
- No build artifacts or temporary files

**Essential Files:**
- `README.md` - Project overview and getting started
- `LICENSE` - Legal terms (required for public repos)
- `.gitignore` - Version control exclusions
- Package manager file (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`)
- `CHANGELOG.md` - Version history (for versioned projects)

## Best Practices

### File Organization

**Root-Level Files:**
```
project-root/
├── README.md          # Required: Project overview
├── LICENSE            # Required for public repos
├── .gitignore         # Required: Exclude secrets, build artifacts
├── package.json       # Package manager file (language-specific)
├── CHANGELOG.md       # Version history (if versioned)
└── [max 2-3 config files if absolutely necessary]
```

**Directory Structure:**
```
project-root/
├── .github/           # GitHub-specific (workflows, templates)
├── config/            # Configuration files
├── docs/              # Extended documentation
├── scripts/           # Utility scripts
├── src/               # Source code (or cmd/pkg for Go)
├── tests/             # Test files
└── infra/             # Infrastructure as code (if applicable)
```

### README.md Standards

**Required Sections:**
1. **Title & Description** - What the project is (1-2 sentences)
2. **Installation** - How to install dependencies
3. **Usage** - How to run/use the project
4. **Project Structure** - Where code lives
5. **Contributing** - How to contribute
6. **License** - License information

**Optional Sections:**
- Badges (build status, coverage, version)
- Screenshots/demos
- Roadmap
- Acknowledgments

### .gitignore Best Practices

**Always Exclude:**
- Environment files: `.env`, `.env.local`, `.env.*.local`
- Build artifacts: `dist/`, `build/`, `*.o`, `*.exe`, `*.class`
- Dependencies: `node_modules/`, `venv/`, `__pycache__/`
- IDE files: `.idea/`, `.vscode/` (unless team standardizes)
- OS files: `.DS_Store`, `Thumbs.db`, `*.swp`

**Language-Specific:**
- Use GitHub's gitignore templates
- Update regularly as ecosystem evolves
- Document any intentional inclusions

## Highest Standards

### Industry Standards

**Keep a Changelog Format:**
- [Keep a Changelog](https://keepachangelog.com/) standard
- Sections: Added, Changed, Deprecated, Removed, Fixed, Security
- Unreleased section for upcoming changes
- Links to versions and diffs

**SPDX License Identifiers:**
- Use SPDX license identifiers in package files
- Clear license text in LICENSE file
- License compatibility checking

**Semantic Versioning:**
- Follow SemVer strictly
- Tag releases properly
- Document breaking changes clearly

### Repository Templates

**GitHub Template Repositories:**
- Pre-configured structure
- CI/CD workflows included
- Documentation templates
- Best practices baked in

**Scaffolding Tools:**
- Yeoman generators
- Cookiecutter templates
- Custom CLI tools

## Innovative Techniques

### Modern Approaches

**Repository Index Files:**
- `INDEX.md` or `INDEX.json` describing structure
- Automated generation from codebase
- Searchable metadata

**Badge Systems:**
- Build status badges
- Coverage badges
- Dependency badges
- Version badges
- Use shields.io or custom badges

**Automated Structure Validation:**
- Linters for directory structure
- CI checks for required files
- Automated README validation

### Emerging Practices (2026)

**AI-Generated Documentation:**
- Auto-generate README from code
- Intelligent project structure suggestions
- Automated changelog generation

**Repository Health Dashboards:**
- Real-time health metrics
- Compliance scoring
- Remediation suggestions

**Multi-Language Support:**
- README in multiple languages
- Localized documentation
- Language-specific examples

## Unique Methodologies

### Feature-First Organization
- Organize by features, not layers
- Each feature is self-contained
- Easier to understand and maintain

### Domain-Driven Structure
- Reflect business domains in structure
- Clear boundaries between domains
- Domain-specific documentation

### Monorepo Patterns
- `apps/` for applications
- `packages/` for shared libraries
- `tools/` for development tools
- Workspace configuration at root

## Anti-Patterns to Avoid

**Root Directory Clutter:**
- ❌ Too many config files in root
- ❌ Source files mixed with configs
- ❌ Build artifacts committed
- ❌ Temporary files in root

**Poor Documentation:**
- ❌ Missing or outdated README
- ❌ No installation instructions
- ❌ Unclear project structure
- ❌ Missing license for public repos

## References

- [Keep a Changelog](https://keepachangelog.com/)
- [SPDX License List](https://spdx.org/licenses/)
- [GitHub gitignore Templates](https://github.com/github/gitignore)
- [Semantic Versioning](https://semver.org/)
- [Shields.io Badges](https://shields.io/)
