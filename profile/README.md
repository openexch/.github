# Open Exchange

**An ultra-low-latency crypto trading exchange, built end-to-end.**

Open Exchange is a complete trading platform — matching engine, order management, operations gateway, and trading UI — designed for sub-microsecond order matching with Raft-replicated state and atomic settlement. Every component in the stack is open and lives in this organization.

---

## Highlights

- **Sub-microsecond matching** — LMAX Disruptor + Agrona zero-allocation data structures, fixed-point arithmetic, O(1) price-time priority lookups.
- **3-node Aeron Cluster** — Raft consensus for the matching engine with snapshots, log replay, and zero-downtime rolling updates.
- **Pre-trade risk pipeline** — Seven sequential checks per order: rate limit, circuit breaker, size, price collar, open-orders, balance, and position.
- **Double-entry ledger** — Atomic hold / release / settle backed by Redis (with Lua-scripted operations) or in-memory; idempotent on duplicate executions.
- **Synthetic order types** — Stop-loss, stop-limit, trailing stop, and iceberg, evaluated against the live market data feed.
- **Time-in-force** — GTC, IOC, FOK, GTD.
- **Real-time trading UI** — React 18 + lightweight-charts, streaming order book and trade feed over WebSocket.
- **HTTP-driven cluster ops** — Rolling updates, snapshots, log retrieval, and hot-reload of risk config — all via the admin API.

---

## Architecture

```
┌────────────────┐      HTTP/REST      ┌────────────────┐
│                │ ──────────────────► │                │
│  Trading UI    │      WebSocket      │      OMS       │
│ (React + Vite) │ ◄────────────────── │ (risk, ledger) │
│                │                     │                │
└────────────────┘                     └───────┬────────┘
       ▲                                       │ Aeron ingress
       │ WebSocket :8081                       ▼
       │                              ┌────────────────┐
       │                              │  Match Engine  │
       └──────────────────────────────│  3-node Raft   │
                  market data feed    │   (Aeron)      │
                                      └────────────────┘
                                              ▲
                                              │ HTTP control
                                      ┌───────┴────────┐
                                      │ Admin Gateway  │
                                      │     (Go)       │
                                      └────────────────┘
```

### Port allocation

| Port        | Service              | Owner      | Purpose                                |
| ----------- | -------------------- | ---------- | -------------------------------------- |
| `8080`      | REST API             | OMS        | Order submission, account queries      |
| `8081`      | WebSocket            | Match      | Market data — order book, trades       |
| `8082`      | HTTP API             | Admin      | Cluster operations, process control    |
| `9090`      | gRPC                 | OMS        | Streaming order / balance updates      |
| `9000–9200` | Aeron Cluster        | Match      | Raft consensus (internal, node-to-node)|

---

## Repositories

| Repo | Stack | Role |
| ---- | ----- | ---- |
| [`match`](https://github.com/openexch/match) | Java 21 · Aeron · SBE · Disruptor | Matching engine on a 3-node Raft cluster |
| [`oms`](https://github.com/openexch/oms) | Java 21 · Netty · gRPC · Postgres · Redis | Risk pipeline, ledger, synthetic orders, lifecycle |
| [`admin-gateway`](https://github.com/openexch/admin-gateway) | Go 1.22 · chi | Process manager + cluster ops HTTP API |
| [`trading-ui`](https://github.com/openexch/trading-ui) | React 18 · TypeScript · Vite | Real-time trading interface |

---

## Quickstart

Clone each repo and bring it up locally:

```bash
# 1. Match engine (Java 21 + Maven required)
git clone git@github.com:openexch/match.git && cd match && make install && make build && cd ..

# 2. Order management system
git clone git@github.com:openexch/oms.git && cd oms && mvn package -DskipTests && cd ..

# 3. Admin gateway (Go 1.22 required)
git clone git@github.com:openexch/admin-gateway.git && cd admin-gateway && make build && cd ..

# 4. Trading UI (Node 22 required)
git clone git@github.com:openexch/trading-ui.git && cd trading-ui && npm install && npm run dev
```

In production, the [admin gateway](https://github.com/openexch/admin-gateway) orchestrates everything via its HTTP API — start, stop, snapshot, and roll the cluster without touching systemd directly.

---

## Performance & ops

- **Load testing** — `match/run-load-test.sh` drives the matching engine at up to 10k orders/sec; the `oms-loadtest` module generates HTTP traffic against the OMS with p50/p95/p99 reporting.
- **OS tuning** — `make optimize-os` in the match repo tunes CPU governors, network buffers, and VM settings for low-latency operation. `make os-check` shows the current state.
- **Cluster resilience** — Snapshots, log replay, chaos test scripts, and stability runners live under `match/scripts/`.

---

## Contributing

Issue and pull-request templates in this org repo apply org-wide as defaults — any of our repos that doesn't define its own will use them.

## Status

Active development — APIs may change.
