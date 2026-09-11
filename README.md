# .github

Organization-wide defaults for FabianBruengerOrg.

## Reusable workflows

### `rust-security.yml`

Shared security scan for Rust/Leptos apps: `cargo audit`, TruffleHog secret
scanning, Semgrep (rust + security-audit + owasp-top-ten rulesets),
`actions/dependency-review-action` on PRs, and optional opt-in checks
(strict clippy security lints, unwrap/expect/panic grep, SQL-injection grep).

Usage from any repo in the org (public workflow, so it can also be called
from personal-account repos like `headquarter`):

```yaml
jobs:
  security:
    uses: FabianBruengerOrg/.github/.github/workflows/rust-security.yml@main
    with:
      working-directory: . # or e.g. apps/bikeconfig
      strict-lints: false
      check-unsafe-patterns: false
      check-sql-injection: false
    permissions:
      contents: read
```

This repo is public since it contains no secrets — only CI workflow
definitions — so any repo can reference it regardless of owner.
