# Security Policy

Open Exchange is open-source exchange infrastructure. The current release, `v1.0.0-nightly.20261105`, is a nightly prerelease. It has not had an external security audit and is not intended for production deployments holding real funds.

## Scope

The project provides the exchange infrastructure layer: matching, the order and ledger core, cluster machinery, the settlement bridge, projectors and operations tooling. Deployments provide their own identity and access management, KYC/AML, custody and network perimeter. Vulnerabilities in those integration layers belong to the integrating system; vulnerabilities in what these repositories ship belong here.

Reports we especially care about:

- Order, balance or ledger integrity violations (theft, double spend, conservation breaks)
- Authentication or authorization bypass in shipped endpoints
- Remote code execution, injection or deserialization issues
- Denial of service that a single unprivileged client can trigger

Out of scope: issues that require the attacker to already control the host or the deployment configuration.

## Supported versions

| Version | Supported |
| ------- | --------- |
| Latest tagged release | Yes |
| `main` | Yes |
| Older tags | No |

## Reporting a vulnerability

Report vulnerabilities privately. Do not open a public issue.

- Email: **info@openexchange.dev** (include the repository name, an impact summary and reproduction steps)
- Or use GitHub private vulnerability reporting where it is enabled (Security tab, "Report a vulnerability")

You will get an acknowledgement within a few days. This is a small project, so triage and fixes are best effort, but integrity issues in the matching, ledger or settlement path are treated as top priority. Please allow a reasonable window for a fix before public disclosure. Reporters are credited in the release notes unless they prefer otherwise.
