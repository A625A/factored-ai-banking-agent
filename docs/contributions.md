# Individual and Team Contributions

Attribution follows authored commits and their changes, rather than repository ownership, merge authorship, or the presence of a technology in the final system. “Personal” describes Andrew’s authored engineering contribution; it does not imply exclusive authorship of the final evolved component.

## Andrew’s Authored Work

| Contribution | What the change implemented | Primary evidence |
| --- | --- | --- |
| Dataset quality profiler | Aggregate DuckDB/SQL analysis of validated Parquet releases; manifest/row reconciliation, key/null metrics, ownership-aware joins, repeated text, temporal checks, sanitized output. | Base [`12073d8`](https://github.com/yehosuah/FactoredAI_base/commit/12073d8efe05367948447254937ad3b79be0e9d2) |
| Optional ML handoff | Made omitted ML artifacts optional while preserving explicit-input failure behavior; separated optional Compose configuration. | Base [`28d4b85`](https://github.com/yehosuah/FactoredAI_base/commit/28d4b851a1de2ea1ab90c48827aefce8731e65db) |
| ML file validation | Hardened auxiliary-file handling, membership and unsafe-path behavior. | Base [`de0d3ea`](https://github.com/yehosuah/FactoredAI_base/commit/de0d3eaed6cfeb8aa5c31545a9d160d5938c248d) |
| Operational metrics | Bounded HTTP request/error/latency aggregates and authenticated operational reporting. | BCK [`bd2e74a`](https://github.com/yehosuah/FactoredAI_BCK/commit/bd2e74a929aa9445fd9368f80262c4d5bb3db4d9) |
| Controlled tools | Explicit dispatcher between model/tool intent and authorized backend capabilities. | BCK [`8328d74`](https://github.com/yehosuah/FactoredAI_BCK/commit/8328d74e5b523529842b098cf8248bb4cfe7e41c) |
| Human fallback | Persistent handoff routing and audited recovery. | BCK [`fd40d2c`](https://github.com/yehosuah/FactoredAI_BCK/commit/fd40d2cb5a4fe6142aae776a3742f57ae8eebf9f), [`033dccb`](https://github.com/yehosuah/FactoredAI_BCK/commit/033dccbe4ef84f447f093755908a14a87e14bf11) |
| Confirmation boundary | Server-owned pending confirmations with ownership/state rechecks before simulated mutation. | BCK [`142608b`](https://github.com/yehosuah/FactoredAI_BCK/commit/142608b51706ca2637e3e8ba815379ed21d3c9e6) |
| Conversation integration seam | Authenticated PostgreSQL-backed turns/events, idempotency, strict untrusted proposals, bounded adapter context, and an explicit stub. | BCK [`9ad187f`](https://github.com/yehosuah/FactoredAI_BCK/commit/9ad187f73a8b642622e3c8ce55d78d0e6610f089) |
| System evaluation | Frozen 60-case ES/PT suite, outcome/evidence checks, lane-specific metrics, exact-source provenance and sanitized reports. | Base [`f716fc7`](https://github.com/yehosuah/FactoredAI_base/commit/f716fc734e920fe4d5cd760f39a28c83663683ef), [`fce3a2c`](https://github.com/yehosuah/FactoredAI_base/commit/fce3a2c4134de76d582ab22624fe56c5b99a115b), [PR #9](https://github.com/yehosuah/FactoredAI_base/pull/9) |

The initial conversation commit explicitly did not implement a learned model. Calling this work a **model-adapter integration contract** is supported; claiming that Andrew trained the classifier is not.

## Team Work

- The S3 transport, historical acquisition, core transformations and transactional publication are team pipeline work. Andrew contributed downstream profiling and specific ETL reliability changes, not the entire S3 ingestion implementation.
- Fabian Prado’s classifier commits implement datasets, scikit-learn training/export, metrics and the learned adapter. Yehosua Hercules’s integration commits connect and harden the classifier in the shared backend. These remain team ML work.
- Frontend delivery, public hosting and the full deployment are presented as team outcomes. The existence of local deployment-related commits with a similar first name is not sufficient to assign that work personally to Andrew.
- Later evaluator/oracle repairs and the 60/60 regression are team follow-up work on Andrew’s evaluator. They do not replace the original 44/60 result or become a personal model-accuracy claim.

## Scope of the Portfolio

This repository contains English documentation and attribution. The README includes only the designated public synthetic-demo credentials. Organizer datasets, private credentials, private conversation records, and the original application history remain outside this portfolio.
