# Research: Source Code Layout

**Last Updated:** 2026-01-28  
**Research Scope:** Fundamentals, best practices, highest standards, innovative techniques

## Fundamentals

### Core Concepts

**Directory Structure Patterns:**
- **Single Package:** `src/` for most languages, `cmd/` + `pkg/` for Go
- **Monorepo:** `apps/` for applications, `packages/` for shared libraries
- **Application:** Feature-based or layer-based organization

**Language Conventions:**
- JavaScript/TypeScript: `src/`, `lib/`, `dist/`
- Python: `src/`, `package_name/`, or flat structure
- Go: `cmd/` (executables), `pkg/` (libraries), `internal/` (private)
- Rust: `src/` with `main.rs` or `lib.rs`
- Java/Kotlin: `src/main/java/`, `src/test/java/`

**Organization Approaches:**
- **Feature-based:** Group by feature/domain (e.g., `users/`, `orders/`)
- **Layer-based:** Group by layer (e.g., `controllers/`, `services/`, `models/`)
- **Domain-driven:** Align with business domains

## Best Practices

### Structure Principles

**Shallow Directory Depth:**
- Aim for 3-4 levels maximum
- Avoid deeply nested structures (hard to navigate)
- Balance organization with simplicity

**Clear Module Boundaries:**
- Each module/package has clear responsibility
- Minimize circular dependencies
- Use dependency injection for testability

**Naming Conventions:**
- Use consistent naming (kebab-case, snake_case, or camelCase)
- Match language conventions
- Descriptive, searchable names

### Monorepo Patterns

**Workspace Organization:**
- `apps/` - Applications (deployable units)
- `packages/` - Shared libraries/packages
- `tools/` - Development tools and scripts
- Root-level configs for workspace management

**Dependency Management:**
- Internal packages reference each other
- Shared dependencies at workspace root
- Version consistency across packages

**Build Systems:**
- Turborepo, Nx, Lerna (JavaScript)
- Bazel (polyglot)
- Gradle (Java/Kotlin)

## Highest Standards

### Architecture Patterns

**Clean Architecture:**
- Dependency rule: Inner layers don't depend on outer layers
- Entities → Use Cases → Interface Adapters → Frameworks
- Testable, maintainable, framework-independent

**Hexagonal Architecture (Ports & Adapters):**
- Core domain in center
- Adapters for external systems
- Clear boundaries between business logic and infrastructure

**Domain-Driven Design (DDD):**
- Bounded contexts
- Aggregates and entities
- Domain events
- Ubiquitous language

### Microservices Patterns

**Service Organization:**
- One repository per service (microservices)
- Or monorepo with clear service boundaries
- API contracts between services
- Shared libraries for common code

**Modular Monolith:**
- Monolith structure with clear module boundaries
- Easy to extract services later
- Shared database with module isolation

## Innovative Techniques

### Modern Approaches (2026)

**AI-Assisted Code Organization:**
- AI tools suggest better structure
- Automated refactoring suggestions
- Code organization analysis

**Architecture Decision Records (ADRs):**
- Document structural decisions
- Track evolution of architecture
- Share knowledge across team

**Automated Dependency Analysis:**
- Tools detect circular dependencies
- Suggest better module boundaries
- Visualize dependency graphs

**Domain-Driven Design Tooling:**
- Bounded context visualization
- Domain event tracking
- Aggregate identification tools

### Emerging Practices

**Component Co-location:**
- Keep related files together (component + tests + styles)
- Easier to find and maintain
- Better for code splitting

**Feature Flags Organization:**
- Organize by feature flags
- Enable/disable features easily
- A/B testing support

**Plugin Architecture:**
- Extensible plugin system
- Clear plugin boundaries
- Plugin discovery mechanisms

## Language-Specific Guidance

### JavaScript/TypeScript

**Standard Structure:**
```
src/
├── components/     # React/Vue components
├── utils/          # Utility functions
├── hooks/          # React hooks
├── services/       # API services
├── types/          # TypeScript types
└── index.ts        # Entry point
```

**Monorepo (Turborepo/Nx):**
```
apps/
├── web/            # Web application
├── api/            # API server
└── mobile/         # Mobile app
packages/
├── ui/             # Shared UI components
├── utils/          # Shared utilities
└── config/         # Shared configs
```

### Python

**Package Structure:**
```
src/
└── mypackage/
    ├── __init__.py
    ├── module1.py
    └── module2.py
```

**Application Structure:**
```
app/
├── api/            # API routes
├── models/         # Data models
├── services/       # Business logic
└── utils/          # Utilities
```

### Go

**Standard Structure:**
```
cmd/
└── app/
    └── main.go     # Application entry
pkg/
├── handlers/       # HTTP handlers
├── services/       # Business logic
└── models/         # Data models
internal/           # Private packages
tests/              # Test files
```

### Rust

**Standard Structure:**
```
src/
├── main.rs         # Binary entry
├── lib.rs          # Library entry
├── modules/
│   ├── mod1.rs
│   └── mod2.rs
└── tests/          # Integration tests
```

## Common Patterns

### Feature-Based Organization

```
src/
├── features/
│   ├── users/
│   │   ├── components/
│   │   ├── services/
│   │   ├── types.ts
│   │   └── index.ts
│   └── orders/
│       └── ...
└── shared/         # Shared across features
```

### Layer-Based Organization

```
src/
├── controllers/    # Request handlers
├── services/       # Business logic
├── repositories/   # Data access
├── models/         # Data models
└── utils/          # Utilities
```

## Anti-Patterns to Avoid

**❌ Too Many Levels:**
```
src/components/ui/buttons/primary/icon/left/arrow.tsx  # Too deep!
```

**✅ Better:**
```
src/components/ui/Button.tsx
```

**❌ Circular Dependencies:**
```
A → B → C → A  # Circular!
```

**✅ Better:**
```
A → B → C
A → C  # Direct dependency
```

**❌ Mixed Concerns:**
```
src/
├── utils.js
├── components.js
├── api.js
└── everything.js  # Everything in one file!
```

## References

- [Clean Architecture by Robert C. Martin](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [Domain-Driven Design by Eric Evans](https://www.domainlanguage.com/ddd/)
- [Go Project Layout](https://github.com/golang-standards/project-layout)
- [Python Packaging Guide](https://packaging.python.org/)
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
- [Monorepo Tools Comparison](https://monorepo.tools/)
