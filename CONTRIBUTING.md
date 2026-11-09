# Contributing to Open Exchange

Thanks for your interest. This guide applies to every repository in the organization that does not have its own `CONTRIBUTING.md`. Build and test commands for each repository are in its README.

## Before you start

- For anything larger than a small fix, open an issue first and outline the approach.
- Bug reports: include the repository, the version or commit, and reproduction steps or a failing test.
- Security issues: do not open a public issue. See [SECURITY.md](SECURITY.md).

## Design constraints

The engines (`match`, `assets`) are replicated state machines on Aeron Cluster. Changes inside the replicated path must keep these properties:

- **Determinism.** The same input log produces identical state on every node. No wall-clock reads, no randomness and no iteration over unordered collections inside the replicated path.
- **State compatibility.** Changes to snapshot format, log replay or wire schemas must say how existing state is handled.
- **Durability and idempotence.** Bridges and projectors must not lose or double-apply events across restarts. Checkpoint and recovery behaviour needs tests.
- **Performance claims need numbers.** For latency or throughput sensitive changes, include before and after measurements and how they were taken.

## Pull requests

- One logical change per pull request. Pull requests are squash-merged into one commit on `main`.
- Reference issues in the body (`Closes #NN`).
- CI must pass before merge.
- Commit signing is optional.

## Code of conduct

Participation is governed by the [Code of Conduct](CODE_OF_CONDUCT.md).

## License

Contributions are licensed under the Apache License 2.0, the same terms as the project (inbound = outbound). There is no CLA.
