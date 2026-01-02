# git-secrets-auditor

Fast Git secrets auditor written in Rust.

A high-performance scanner for detecting secrets in Git repositories. It identifies API keys, passwords, tokens, and other sensitive data with low false positives.

## Features

- Significantly faster than similar tools due to Rust, parallel processing, memory mapping, and Aho-Corasick algorithm
- Multi-stage detection: pattern matching combined with Shannon entropy analysis and heuristics
- Incremental scanning: subsequent runs are much faster
- Git-aware: --diff mode scans only changed files, suitable for CI/CD
- Supports multiple output formats: JSON, Markdown, SARIF
- Respects .gitignore and allows custom patterns

## Capabilities

- Multi-threaded parallelism
- Zero-copy reading of large files
- .gitignore support
- Incremental caching
- Git diff scanning
- Option to disable entropy checks (--no-entropy)
- Non-zero exit code on findings (--exit-code)
- Progress bar

## Installation

```bash
cargo install git-secrets-auditor
```

From source:

```bash
git clone https://github.com/skladkoles44/git-secrets-auditor.git
cd git-secrets-auditor
cargo install --path .
```

## Usage

```bash
# Scan current directory
git-secrets-auditor

# Scan specific path
git-secrets-auditor --path ~/projects

# Incremental scan
git-secrets-auditor --incremental

# Scan only changed files
git-secrets-auditor --diff

# Use custom patterns
git-secrets-auditor --patterns custom.toml

# SARIF output
git-secrets-auditor --format sarif --output ./results

# Quiet mode with exit code on findings
git-secrets-auditor --quiet --exit-code
```

## Performance (SSD, 8+ cores)

| Workload                       | git-secrets-auditor | gitleaks (Go) | trufflehog (Python) |
|--------------------------------|---------------------|---------------|--------------------|
| 10,000 files (~500 MB)         | 1–3 seconds        | 5–10 seconds | 15–30 seconds     |
| 100 repositories               | 3–10 seconds       | 15–30 seconds| 30–60 seconds     |
| Memory usage                   | <100 MB            | 200–500 MB   | 300–800 MB        |

## Development

```bash
cargo build --release
cargo test
cargo bench
cargo clippy -- -D warnings
cargo fmt
```

## License

MIT OR Apache-2.0
