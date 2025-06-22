# Analysis Tools

The main tool for compliance checking is [golangci-lint](https://golangci-lint.run/).

Other tools:
1. **gosec** – security scanner for code.
2. **staticcheck** – advanced static analyzer.
3. **govulncheck** – vulnerability scanner for dependencies.
4. **golint** – reports style mistakes and coding convention violations.

## golangci-lint
Golangci-lint is a fast linters runner for Go.

It runs linters in parallel, uses caching, supports YAML configuration, integrates with all major IDEs, and includes over a hundred linters.

* Fast: runs linters in parallel, reuses Go build cache and caches analysis results.
* YAML-based configuration.
* Integrations with VS Code, Sublime Text, GoLand, GNU Emacs, Vim, GitHub Actions.
* A lot of linters included, no need to install them.
* Minimum number of false positives because of tuned default settings.
* Nice outputs: text with colors and source code lines, JSON, tab, HTML, Checkstyle, Code-Climate, JUnit-XML, TeamCity, SARIF.

To run:
```bash
golangci-lint run
```

To format your code:
```bash
golangci-lint fmt
You can choose which directories or files to analyze:
```

You can choose which directories or files to analyze:
```bash
golangci-lint fmt dir1 dir2/...
golangci-lint fmt file1.go
```

For a [list of linters, ](https://golangci-lint.run/usage/linters/) you can run:
```bash
golangci-lint help linters
```

Example YAML file `.golangci.yaml`
```yaml
run:
  timeout: 5m
  tests: true
  skip-dirs:
    - vendor
    - generated
  skip-files:
    - ".*_gen.go"

linters:
  enable:
    - govet
    - staticcheck
    - gofmt
    - goimports
    - gosimple
    - unused
    - errcheck
    - gocyclo
    - gosec
    - structcheck
    - deadcode
    - typecheck
    - ineffassign
    - misspell
    - depguard
    - exportloopref
    - nolintlint
  disable:
    - lll          # disable line length check
    - dupl         # disable code duplication check unless needed

linters-settings:
  gofmt:
    simplify: true

  gocyclo:
    min-complexity: 15

  misspell:
    locale: US

  gosec:
    # Optional: Skip checks you don't want
    excludes:
      - G104 # Ignore audit for errors not checked

  depguard:
    list-type: blacklist
    packages:
      - github.com/some/internal/package/you/must/avoid
    packages-with-error-message:
      github.com/unsafe/pkg: "Don't use unsafe in this codebase"

issues:
  exclude-use-default: false
  max-issues-per-linter: 0
  max-same-issues: 0
  exclude-rules:
    - path: _test\.go
      linters:
        - errcheck

output:
  format: colored-line-number
  sort-results: true
```

## gosec
Purpose is to analyze Go source code for common security issues.

Detects:
* Hardcoded credentials
* SQL injection risks
* Command injection
* Insecure file permissions
* Use of weak cryptography (e.g. MD5, SHA1)
* TLS config issues
* Unsafe use of exec

Command:
```bash
gosec ./...
```

## staticcheck
Performs deep static analysis to detect code bugs, performance issues, and idiomatic mistakes.

Detects:
* Redundant or unreachable code.
* Incorrect slice usage.
* Useless assignments.
* Misuse of errors.New / fmt.Errorf.
* Deprecated APIs.
* Ineffective conditionals or dead branches.

Command:
```bash
staticcheck ./...
```

## govulncheck
Scans your Go code and dependencies for known security vulnerabilities using the official Go vulnerability database.

Detects:
* Use of packages with known CVEs (common vunerabilites and exposures).
* Calls to known-vulnerable functions.
* Vulnerable dependencies.


## golint
Purpose is to reports style mistakes and coding convention violations.

Enforces compliance in:
* Comments.
* Naming.
* Idioms.
* Readability.

```bash
golint ./...
```
