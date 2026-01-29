# Language-Specific Quick Start Guides

Quick start guides for common languages. These provide language-specific shortcuts to ALIGNMENT compliance.

## JavaScript/TypeScript Quick Start

### Structure
```
project/
├── README.md
├── LICENSE
├── package.json
├── package-lock.json
├── .gitignore
├── .github/
│   └── workflows/
│       └── ci.yml
├── src/
│   └── index.ts
└── tests/
    └── index.test.ts
```

### Essential Files

**package.json:**
```json
{
  "name": "project-name",
  "version": "1.0.0",
  "scripts": {
    "build": "tsc",
    "test": "jest",
    "lint": "eslint .",
    "format": "prettier --write ."
  }
}
```

**.gitignore:**
```
node_modules/
dist/
build/
.env
.env.local
*.log
.DS_Store
```

**CI/CD (.github/workflows/ci.yml):**
```yaml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '18'
      - run: npm ci
      - run: npm run build
      - run: npm run lint
      - run: npm test
      - run: npm audit
```

### Key Tools
- **Linting:** ESLint
- **Formatting:** Prettier
- **Testing:** Jest, Vitest
- **Type checking:** TypeScript compiler

### Time Estimate: 2-4 hours

## Python Quick Start

### Structure
```
project/
├── README.md
├── LICENSE
├── pyproject.toml
├── poetry.lock
├── .gitignore
├── .github/
│   └── workflows/
│       └── ci.yml
├── src/
│   └── app/
│       └── __init__.py
└── tests/
    └── test_app.py
```

### Essential Files

**pyproject.toml:**
```toml
[tool.poetry]
name = "project-name"
version = "1.0.0"

[tool.poetry.dependencies]
python = "^3.10"

[tool.pytest.ini_options]
testpaths = ["tests"]
```

**.gitignore:**
```
__pycache__/
*.py[cod]
*.so
.Python
env/
venv/
.env
.env.local
.pytest_cache/
.coverage
```

**CI/CD (.github/workflows/ci.yml):**
```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v4
        with:
          python-version: '3.10'
      - run: pip install poetry
      - run: poetry install
      - run: poetry run pytest
      - run: poetry run black --check .
      - run: poetry run flake8 .
```

### Key Tools
- **Linting:** Flake8, Ruff
- **Formatting:** Black
- **Testing:** pytest
- **Dependencies:** Poetry (recommended) or pip

### Time Estimate: 2-4 hours

## Go Quick Start

### Structure
```
project/
├── README.md
├── LICENSE
├── go.mod
├── go.sum
├── .gitignore
├── .github/
│   └── workflows/
│       └── ci.yml
├── cmd/
│   └── app/
│       └── main.go
├── pkg/
│   └── lib/
│       └── lib.go
└── internal/
    └── private/
        └── private.go
```

### Essential Files

**go.mod:**
```go
module github.com/org/project-name

go 1.20
```

**.gitignore:**
```
*.exe
*.exe~
*.dll
*.so
*.dylib
*.test
*.out
.env
.env.local
```

**CI/CD (.github/workflows/ci.yml):**
```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v4
        with:
          go-version: '1.20'
      - run: go mod download
      - run: go build ./...
      - run: go test ./...
      - run: go vet ./...
      - run: golangci-lint run
```

### Key Tools
- **Linting:** golangci-lint
- **Formatting:** gofmt (built-in)
- **Testing:** go test (built-in)
- **Dependencies:** go mod (built-in)

### Time Estimate: 1-3 hours

## Rust Quick Start

### Structure
```
project/
├── README.md
├── LICENSE
├── Cargo.toml
├── Cargo.lock
├── .gitignore
├── .github/
│   └── workflows/
│       └── ci.yml
├── src/
│   └── main.rs
└── tests/
    └── integration_test.rs
```

### Essential Files

**Cargo.toml:**
```toml
[package]
name = "project-name"
version = "1.0.0"
edition = "2021"

[dependencies]
```

**.gitignore:**
```
target/
Cargo.lock  # Note: Usually committed, but some projects ignore
.env
.env.local
```

**CI/CD (.github/workflows/ci.yml):**
```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions-rs/toolchain@v1
        with:
          toolchain: stable
      - run: cargo build
      - run: cargo test
      - run: cargo clippy -- -D warnings
      - run: cargo fmt -- --check
      - run: cargo audit
```

### Key Tools
- **Linting:** Clippy (built-in)
- **Formatting:** rustfmt (built-in)
- **Testing:** cargo test (built-in)
- **Dependencies:** Cargo (built-in)

### Time Estimate: 1-3 hours

## Common Patterns Across Languages

### All Languages Need:
1. **README.md** - What, install, run, contribute
2. **LICENSE** - If public repository
3. **.gitignore** - Language-specific patterns
4. **CI/CD** - Build, test, lint, security
5. **Source organization** - Dedicated directory
6. **Lockfile** - Committed to git

### Language-Specific Resources

**JavaScript/TypeScript:**
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)

**Python:**
- [Python Packaging Guide](https://packaging.python.org/)
- [Hitchhiker's Guide to Python](https://docs.python-guide.org/)

**Go:**
- [Standard Go Project Layout](https://github.com/golang-standards/project-layout)
- [Effective Go](https://go.dev/doc/effective_go)

**Rust:**
- [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
- [The Rust Book](https://doc.rust-lang.org/book/)

## Next Steps

After quick start:
1. Complete [Section 0](00-Prerequisites.md) decision points
2. Follow full ALIGNMENT sections in order
3. Use [Success Metrics](Success-Metrics.md) to measure progress

---

**Remember:** Quick starts provide structure. Full alignment adds governance, security, and advanced features.
