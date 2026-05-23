<p align="center">
  <img src="https://raw.githubusercontent.com/vaultexhq/.github/main/profile/vaultex-logo.svg" alt="Vaultex" width="100%" />
</p>

<h3 align="center">Distributed rate limiting — sub-millisecond decisions at the edge</h3>

<p align="center">
  Drop one middleware into your API. Rules, analytics, and team management included.
</p>

<p align="center">
  <a href="https://github.com/vaultexhq/vaultex-go"><img src="https://img.shields.io/badge/Go_SDK-00ADD8?style=flat-square&logo=go&logoColor=white" alt="Go SDK" /></a>
  <a href="https://github.com/vaultexhq/vaultex-node"><img src="https://img.shields.io/badge/Node_SDK-339933?style=flat-square&logo=node.js&logoColor=white" alt="Node SDK" /></a>
  <a href="https://github.com/vaultexhq/vaultex-python"><img src="https://img.shields.io/badge/Python_SDK-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python SDK" /></a>
</p>

---

## How it works

Your API calls `POST /v1/check` on every request — or runs the check in-process via the embedded SDK. The edge server evaluates rules against Redis in **< 2 ms** and returns a decision. No database query on the hot path.

```
Your API  →  SDK middleware  →  Edge (Redis, rules)  →  allow / block
                                      │
                               Analytics pipeline
                               (ClickHouse + Dashboard)
```

---

## Quick start

```bash
go get github.com/vaultexhq/vaultex-go@latest
```

```go
import ratelimit "github.com/vaultexhq/vaultex-go"

limiter := ratelimit.New("vx_live_your_key",
    ratelimit.WithBaseURL("https://edge.example.com"),
)

// net/http
http.Handle("/", limiter.Middleware(myHandler))

// Gin
router.Use(limiter.GinMiddleware())

// Echo
e.Use(limiter.EchoMiddleware())
```

Every blocked request returns `429` with `Retry-After`. Every allowed request gets `X-Vaultex-Remaining` and `X-Vaultex-Reset` headers automatically.

---

## Embedded mode — no HTTP round-trip

```go
limiter := ratelimit.New("vx_live_your_key",
    ratelimit.WithBaseURL("https://edge.example.com"),
    ratelimit.WithRedis("redis://localhost:6379"),  // evaluate in-process
)
```

Rules are fetched once at startup and refreshed every 30 s in the background. Each check hits Redis directly — **~0.3–1 ms latency**, no network hop to the edge.

---

## Rules

Create rules from the dashboard or API. First match wins.

| Field                   | Operators                         |
| ----------------------- | --------------------------------- |
| `ip`, `user_id`, `tier` | `eq` `not_eq` `in` `cidr`         |
| `endpoint`, `method`    | `eq` `prefix` `suffix` `contains` |
| `header.<name>`         | `eq` `contains`                   |

| Action     | Effect                           |
| ---------- | -------------------------------- |
| `throttle` | Sliding window or token bucket   |
| `block`    | Always 429                       |
| `allow`    | Whitelist — skip remaining rules |

---

## Repositories

| Repo                                                          | Description                             |
| ------------------------------------------------------------- | --------------------------------------- |
| [vaultex-go](https://github.com/vaultexhq/vaultex-go)         | Go SDK — net/http, Gin, Echo middleware |
| [vaultex-node](https://github.com/vaultexhq/vaultex-node)     | Node.js / TypeScript SDK                |
| [vaultex-python](https://github.com/vaultexhq/vaultex-python) | Python SDK                              |
