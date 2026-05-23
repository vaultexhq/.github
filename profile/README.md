<p align="center">
  <img src="https://raw.githubusercontent.com/vaultexhq/.github/main/profile/vaultex-logo.svg" alt="Vaultex" width="128" height="128" />
</p>

<p align="center">
  Distributed rate limiting for APIs.<br/>
  Sub-millisecond edge decisions, flexible rules, and real-time analytics.
</p>

<p align="center">
  <a href="https://github.com/vaultexhq/sdks/go">
    <img src="https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white" />
  </a>
  <a href="https://github.com/vaultexhq/sdks/node">
    <img src="https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=node.js&logoColor=white" />
  </a>
  <a href="https://github.com/vaultexhq/sdks/python">
    <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" />
  </a>
</p>

## What is Vaultex

Vaultex is distributed rate limiting service built for production APIs. It runs as a lightweight edge server that your existing services call before processing any request — returning an allow or block decision in under 2 milliseconds.

Rules are defined once through the dashboard or API and applied consistently across every service in your stack. All decisions flow into a real-time analytics pipeline, giving you full visibility into traffic patterns, blocked requests, and latency across your entire API surface.

## Why Vaultex

Most rate limiters live inside your app — a counter in Redis wired up by hand, duplicated across services, with no visibility into what's actually being blocked or why.

Vaultex moves that logic to a dedicated edge layer. One API key, one middleware import, and every service in your stack shares the same rules, the same counters, and the same analytics dashboard — without touching your application code.

**What you get out of the box:**

- **< 2 ms decisions** — rules evaluated against Redis via atomic Lua scripts, no database on the hot path
- **Sliding window & token bucket** — choose per rule, not per deployment
- **Embedded mode** — evaluate rules in-process with a direct Redis connection, skipping the network hop entirely (~0.3–1 ms)
- **Flexible conditions** — match on IP, user ID, tier, endpoint, country, or any request header
- **Real-time analytics** — every decision logged to ClickHouse; blocked IPs, top endpoints, latency percentiles all visible instantly
- **Fail-safe by default** — configurable fail-open or fail-closed when infrastructure is unavailable
- **SDKs for Go, Node.js, and Python** — net/http, Gin, Echo, Express, FastAPI supported out of the box
