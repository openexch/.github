# Open Exchange

Open-source exchange infrastructure: a replicated matching engine and a replicated order and ledger core, built on Aeron Cluster.

- Website: https://openexchange.dev
- Documentation: https://openexchange.dev/docs (introduction, quickstart, architecture, configuration, API, operations, security, benchmarks)
- Releases: published in each repository's GitHub Releases

## Status

Current release: `v1.0.0-nightly.20261105`, a nightly prerelease. It is not intended for production use or for real funds.

## v1 repositories

These five repositories are released together under one version.

| Repo | Language | Role |
| ---- | -------- | ---- |
| [`match`](https://github.com/openexch/match) | Java 21, Aeron Cluster | Matching engine on a Raft-replicated cluster |
| [`assets`](https://github.com/openexch/assets) | Java 21, Aeron Cluster | Order and ledger core: order lifecycle, risk, holds and the ledger in one replicated cluster |
| [`cluster-kit`](https://github.com/openexch/cluster-kit) | Java 21 | Cluster machinery shared by both engines: snapshot bundles, staging archives, log housekeeping |
| [`cluster-tools`](https://github.com/openexch/cluster-tools) | C++20 | Cluster client, matching engine forwarder and settlement bridge |
| [`tools`](https://github.com/openexch/tools) | mixed | Probe and test tooling |

v1 does not have a user-facing gateway yet.

## Previous stack (v0.5)

The live demo at https://trade.openexchange.dev still runs the v0.5 stack. These repositories belong to it:

| Repo | Role |
| ---- | ---- |
| [`oms`](https://github.com/openexch/oms) | Order management system of the v0.5 stack |
| [`admin`](https://github.com/openexch/admin) | Process supervision and cluster operations for one box |
| [`trading-ui`](https://github.com/openexch/trading-ui) | Web trading interface |

## Contributing

The issue templates, pull request template and community files in this `.github` repository apply to every repository in the organization that does not define its own. Security reports go to info@openexchange.dev, not to public issues.

## License

Apache License 2.0, unless a repository states otherwise.
