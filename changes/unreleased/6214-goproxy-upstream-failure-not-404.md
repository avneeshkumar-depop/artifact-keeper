---
section: Fixed
issues: [#6214]
---
- **Go `@v/list`, `@latest` and `.info` answer an upstream outage with a retryable error instead of 404** (#6214). When a Go Remote could not reach its upstream, or the upstream answered 5xx or 429, these three routes answered `404`, while `.mod` and `.zip` answered `502`/`503`. `go` reads a 404 as "this proxy does not have the module" and moves on to the next `GOPROXY` entry, so an outage turned into direct VCS fetches, or into a not-found build failure with a single entry. They now answer what `.mod` answers: `502` or `503` on a Remote, and `503` with `Retry-After` through a Virtual when no member served and one could not answer. A database fault while resolving them is retryable too. An upstream `404` or `410` is still a `404`.
