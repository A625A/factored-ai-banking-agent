# Deployment & Production Engineering

Server evidence reviewed October 5, 2026, America/Guatemala (October 6 UTC in machine timestamps). This separates direct observations, saved deployment reports, source configuration and unverified operational properties.

Public availability rechecked October 6, 2026, America/Guatemala: HTTPS root returned 200 with normal certificate validation, `/api/health/live` returned `ok`, `/api/health/ready` returned `ready`, and `/api/operations/metrics` returned 404. Those availability checks used no login or state-changing workflow. A subsequent [screenshot walkthrough](evidence.md#demo-screenshots) used the public synthetic accounts. Server metadata, source comparisons, and image identities below retain their original inspection date.

## Public Application

**Application:** [Waqi’wuqu public demo](https://factored-ai.163-192-145-116.sslip.io/).

**Hosting:** Oracle Cloud Infrastructure. A read-only request to the server’s OCI instance metadata endpoint returned region `mx-queretaro-1` and shape `VM.Standard.A1.Flex`; the host reports `aarch64`. This verifies Oracle hosting independently of an earlier local deployment plan that had stopped before deployment. AWS S3 belongs to the historical ingestion pipeline, not hosting.

The running stack consists of a Caddy edge container, FastAPI backend, offline synthetic ETL and PostgreSQL. The edge serves frontend assets and proxies same-origin `/api/*` requests to `127.0.0.1:8010`, removing `/api`. Backend operational routes and API documentation are blocked at the public proxy. Caddy runs in Docker with host networking; the inactive host `caddy` system service is not evidence that the edge is down.

| Check | Evidence and result |
| --- | --- |
| Public application | HTTPS root returned HTTP 200 during this review. |
| HTTP redirect | HTTP root returned 308 to the HTTPS URL. |
| TLS | HTTPS requests succeeded with normal certificate validation; a saved focused network report additionally records TLS 1.3. |
| Security headers | Observed HSTS, same-origin CSP, frame denial, `nosniff`, `no-referrer`, and restrictive Permissions Policy. |
| Liveness | `/api/health/live` returned `status: ok`. |
| Readiness | `/api/health/ready` returned `status: ready`. |
| Operational route | Public `/api/operations/metrics` returned 404, matching the Caddy rule. |
| Browser workflow | Saved focused browser report passed customer login, Spanish chat, explicit handoff, agent accept/resolve, reconnect and mobile login. This review did not replay those state-changing actions. |

Readiness does not validate every classifier decision or user journey. These are point-in-time checks, not an uptime/SLO measurement. The [README](../README.md#public-demo-credentials) provides the designated public synthetic-demo accounts; private credentials and session tokens are excluded.

## Source Snapshot Versus Deployed Revision

The public consolidated repository was inspected at `24395b49addb8bb1dc47b9fff5e935730fb442c8`. Its [manifest](https://github.com/yehosuah/factored-hackathon-2026-waqi-wuqu/blob/24395b49addb8bb1dc47b9fff5e935730fb442c8/source-manifest.json) identifies:

| Component | Consolidated source snapshot | Verified deployed candidate |
| --- | --- | --- |
| ETL | `c222173ee03df46be914cbfacfb097823f6a7196` | Same revision recorded by the saved deployment report; not independently byte-compared in this review. |
| Backend | `1e0321dc2c01ad5b4834183466b43f4563be18e4` | `3a087a3100f8b7f4cfc1a61a95277fd13bf44612` |
| Frontend | `15c50c903073a437ec7af3130311a9dec33c1605` | `8ab2eb5e0363046150c49e8b290bebd2d47d81f5` |

**The snapshot is not byte-for-byte the deployed application.** Its [evidence document](https://github.com/yehosuah/factored-hackathon-2026-waqi-wuqu/blob/24395b49addb8bb1dc47b9fff5e935730fb442c8/docs/evidence.md) identifies a frontend `selected_product_id` request field missing from the snapshot backend’s strict schema, which can produce HTTP 422. The later deployed sources include the explicit conversation-handoff endpoint and matching UI changes. A working deployment does not repair the older source snapshot.

Read-only SHA-256 comparison found **106/106 backend tracked files** and **55/55 frontend tracked files** in the server source directories matching the supplied local candidate commits, with no missing or differing files. The server’s `.git` metadata retains parent HEADs (`bed5452` backend and `24d0856` frontend), because candidate files were synchronized over those directories. Therefore reporting server `git HEAD` alone would misidentify deployed source.

An additional direct check compared **35 installed backend Python/JSON files** with the candidate commit and found no mismatches. The saved deployment verifier records these image identities; current container inspection returned the same identities:

| Image | Identity |
| --- | --- |
| Backend | `sha256:e21212213f4905b77ef029a1efb9ef825e45dd925239f0c953c52303b09da1ec` |
| Edge/frontend | `sha256:27f94572bd27d2fc2c9705f48ec3b2b929811d8f7e4ea28ff6bd8266c0370c2d` |

The deployed candidate commits were available in local original-repository checkouts, but GitHub commit lookups returned 422 for both exact SHAs during review. They are not presented as publicly retrievable commit links. Public source provides implementation evidence for shared components; local commits and server byte checks establish the candidate provenance. The portfolio does not claim the public repositories alone reproduce the final deployment.

## Network Boundaries

- Caddy listens on public interfaces for ports 80/443 and communicates with the backend over host loopback.
- The backend publishes container port 8000 only as `127.0.0.1:8010`.
- PostgreSQL listens on container port 5432 inside `factored-public_default` and has **no published host port**. Backend/ETL communication with it stays on that Docker network.
- The saved focused external network report records 5432, 8000 and 8010 as unreachable. Current external TCP checks to those three ports each timed out after three seconds; container/socket inspection independently supports the private application topology. A timeout is a point-in-time observation, not proof about every possible network path.
- This does **not** prove that every host service is inaccessible. SSH port 22 and an RPC listener on port 111 were also observed. Oracle security-list rules and all host firewall rules were not exhaustively audited; no claim that “only required ports are public” is made for the entire VM.

This distinguishes a publicly reachable API **through the controlled HTTPS proxy** from a directly Internet-exposed API listener. Private networking is also not a claim of TLS encryption on internal PostgreSQL traffic.

## Internal Infrastructure

### Persistence and controlled execution

PostgreSQL uses the named `factored-public_demo-postgres` volume. Conversations, turns, events, sessions, confirmations, simulated actions and handoffs are represented in database storage. Caddy has separate persistent certificate/configuration volumes. ETL raw/manifest/curated state uses host mounts outside ephemeral request execution.

The saved preservation report confirms unchanged volume names/secrets and that PostgreSQL and ETL were not restarted during the candidate update. The saved browser report verifies a resolved handoff surviving customer reload/relogin and conversation reconnection. These do not establish VM-reboot recovery, tested backup restoration, disaster recovery or high availability.

Authentication uses separate customer/agent flows, opaque sessions and password hashing. Mutations require server-owned confirmation and ownership/state rechecks. Idempotent conversation and action operations preserve identity across retries; the model never directly authorizes an action. [Original conversation implementation](https://github.com/yehosuah/FactoredAI_BCK/blob/9ad187f73a8b642622e3c8ce55d78d0e6610f089/docs/conversations.md)

### Configuration, secrets, dependencies and logging

Compose uses environment configuration and runtime secret files, including password-file settings; inspected mounts place database secrets outside repository source. Secret values were not read. This verifies a separation mechanism, not that every possible historical leak has been ruled out. Public demo credentials intentionally listed in the README are distinct from administrative credentials and runtime secrets.

The backend Dockerfile builds with `uv sync --locked --no-dev --no-editable`, runs a non-root user and disables access logs. Python `uv.lock` and frontend `package-lock.json` pin dependency resolution. The edge runs read-only with reduced capabilities, persistent certificate storage, restart policy and bounded local Docker logs (`10m`, three files). Those edge log bounds are not assumed for every service.

Backend exception logging records a request ID and exception type without raw request payloads. Bounded process-local HTTP metrics expose request/error counts and recent latency percentiles through an authenticated operational route; the public proxy blocks it. No centralized log search, alert manager, Prometheus/Grafana deployment or fleet-wide metrics aggregation was verified.

## Application Deployment Versus Production ML

| Layer | Supported conclusion |
| --- | --- |
| Model integration | A learned TF-IDF/logistic-regression model is exported as JSON and consumed by an in-process adapter. |
| Container deployment | Backend, data runtime and edge containers were observed running; image identities are recorded above. |
| Public hosting | An Oracle-hosted application and health/readiness endpoints were reachable through trusted HTTPS. |
| Production ML maturity | Partial: packaging and serving are demonstrated, but real-world model validation, drift monitoring, automated retraining, tested rollback and production SLOs are not established. |

No performance benchmark, autoscaling, load balancer, Kubernetes, distributed database, AWS application hosting or millions-of-users claim is supported by this evidence.

## Evidence Handling

Server configuration and existing verification JSON reports were read, not executed or edited. Only public configuration facts, aggregate checks and source/image identities are reproduced here. Private identifiers, private credentials, raw customer data and operational report contents are excluded. The earlier local “blocked before deployment” log remains historical; it is superseded for hosting availability at the inspection date by direct server and public-endpoint observations, not silently rewritten.
