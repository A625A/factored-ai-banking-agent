# Evidence Ledger

Original evidence review: October 5, 2026, America/Guatemala. Machine observations continued through `2026-10-06T05:53:36Z`. Public source was pinned rather than assumed to match a moving default branch. Historical test results below were read from reports; application tests and destructive demo verification were not rerun for this documentation review.

Portfolio refresh: October 6, 2026, America/Guatemala. GitHub reads reconfirmed Andrew's authorship of the README's primary contribution commits, PR #9's original and later regression results, the published ETL reconciliation, and the team classifier report. Public read-only probes returned HTTPS 200, live `ok`, ready `ready`, and 404 for the public operational metrics route. The earlier server inspection is retained as dated evidence; it was not repeated for this refresh.

## Sources and Claims

| Claim | Evidence | Scope |
| --- | --- | --- |
| S3 ingestion through boto3 | [`extract/source.py`](https://github.com/yehosuah/FactoredAI_base/blob/c222173ee03df46be914cbfacfb097823f6a7196/src/factored_bank/extract/source.py) | Actual transport, pagination, conditional reads, bounded retries and integrity checks. Team implementation. |
| Historical acquisition | [Extract acceptance](https://github.com/yehosuah/FactoredAI_base/blob/c222173ee03df46be914cbfacfb097823f6a7196/docs/design/extract-card-support/verification-v1.md) | 5,489 objects, 1,265,179,481 bytes, 6,114,029 logical records; zero new bytes on documented repeated acquisition. Historical evidence, not a fresh S3 run. |
| Nine-table ETL reconciliation | [ETL verification](https://github.com/yehosuah/FactoredAI_base/blob/c222173ee03df46be914cbfacfb097823f6a7196/docs/design/etl-system/verification.md) | 6,114,029 input = 6,020,768 accepted + 93,261 quarantined. Organizer synthetic data, not production customers. |
| Incremental/repeatable pipeline | [Transform](https://github.com/yehosuah/FactoredAI_base/blob/c222173ee03df46be914cbfacfb097823f6a7196/src/factored_bank/etl/transform.py), [ETL contract](https://github.com/yehosuah/FactoredAI_base/blob/c222173ee03df46be914cbfacfb097823f6a7196/docs/etl.md) | Incremental object acquisition and prepared-release reuse; not fully incremental row transformations. |
| Personal quality profiling | [Authored commit](https://github.com/yehosuah/FactoredAI_base/commit/12073d8efe05367948447254937ad3b79be0e9d2) | DuckDB/SQL, Parquet verification, aggregate-only reporting and modeling-suitability checks. |
| Persistent conversation adapter | [Authored commit](https://github.com/yehosuah/FactoredAI_BCK/commit/9ad187f73a8b642622e3c8ce55d78d0e6610f089) | Personal backend seam/stub and persisted workflow state; not classifier authorship. |
| Classifier and packaging | [Training implementation](https://github.com/yehosuah/factored-hackathon-2026-waqi-wuqu/blob/24395b49addb8bb1dc47b9fff5e935730fb442c8/backend/src/factored_bck/intent/training.py), [model report](https://github.com/yehosuah/factored-hackathon-2026-waqi-wuqu/blob/24395b49addb8bb1dc47b9fff5e935730fb442c8/backend/docs/ml-intent-router.md) | Team scikit-learn training, JSON export and local serving. |
| Operational metrics | [Authored metrics commit](https://github.com/yehosuah/FactoredAI_BCK/commit/bd2e74a929aa9445fd9368f80262c4d5bb3db4d9) | Bounded process-local HTTP aggregates and persisted action reporting; not central production monitoring. |
| Public source identities | [Consolidated manifest](https://github.com/yehosuah/factored-hackathon-2026-waqi-wuqu/blob/24395b49addb8bb1dc47b9fff5e935730fb442c8/source-manifest.json) | ETL `c222173`, backend `1e0321d`, frontend `15c50c9`; distinct from deployment. |
| Oracle hosting, TLS, network and running images | [Deployment audit](deployment.md) | Direct OCI metadata/container/source inspection and public GET/HEAD probes, supplemented by existing server reports. Those server reports are not public upstream repository artifacts. |

## Evaluation

### System outcomes

[Base PR #9](https://github.com/yehosuah/FactoredAI_base/pull/9) preserves the original authored run and subsequent team regression evidence.

| Run | Exact provenance | Result and interpretation |
| --- | --- | --- |
| Original `acceptance-20261005-03` | Base `fce3a2c4134de76d582ab22624fe56c5b99a115b`; BCK `bed5452e7fad96f4bc79e1c476ff2ab912f19ac1` | 60 executed, 44 matched, 16 failed. Safe automated resolution 18/60; containment 52/60; correct/missed/unnecessary escalation 8/2/0. Completed measurement, not full acceptance. |
| Later `pr9-receipt-regression` | Reviewed Base `480aff81d9c01d1fb99819b97d2d95b4a8e3001a`, merged as `c222173ee03df46be914cbfacfb097823f6a7196`; local BCK `1a10dc3b567947bf44934a4f3e4982b6080ac2cf` | 60 executed, 60 matched; six cases had verified action evidence. Team post-fix regression, not an untouched held-out test or deployed-revision evaluation. |

The frozen reference checksum is `98ac934c5a9be2ecc9169c57ce216752153e138dbbe2731259a1a1293da84d43`. There are 30 Spanish and 30 Portuguese cases, divided into **40 classifier conversations, 12 direct API probes and eight controlled-seam cases**. A score over all 60 is not classifier accuracy. Paired language cases are not independent population samples.

The original run observed zero unsafe outcomes in 60 cases but produced no verified action receipts for the failing action-flow references. The later regression also reported zero observed unsafe outcomes. Neither establishes zero real-world risk. Queued/assigned handoffs and prepared confirmations do not prove completed resolution. Earlier failures were retained; policy and oracle fixes make later evidence regression evidence.

The deployed backend `3a087a3` is distinct from both evaluated backend revisions. Public availability and the saved focused browser checks are useful runtime evidence, but they do not transfer the 60/60 score to this candidate.

### Model validation

The team report describes 280 synthetic training examples, ten intents and character TF-IDF/logistic regression. Reported five-fold accuracy is 0.746 and macro F1 is 0.740, versus keyword-baseline accuracy 0.589 and macro F1 0.612. These are team-authored offline results, not Andrew’s personal training result or production metrics.

The report acknowledges selection bias, insufficient paraphrase grouping, draft held-out annotations and synthetic-domain limitations. A separate 72-case draft diagnostic reported in [backend PR #7](https://github.com/yehosuah/FactoredAI_BCK/pull/7) records 61/72 primary-intent matches (approximately 0.847 accuracy); it is not an accepted independent benchmark. The pinned training notebook predates this diagnostic and marks that run pending. These remain team model-validation results, separate from Andrew's system evaluator.

### Historical component checks

The consolidated [evidence report](https://github.com/yehosuah/factored-hackathon-2026-waqi-wuqu/blob/24395b49addb8bb1dc47b9fff5e935730fb442c8/docs/evidence.md) records 378 ETL tests, 486 backend tests with one optional parity test skipped, and 65 frontend tests for its source snapshots. Component success did not resolve its documented chat-contract mismatch. Test counts do not establish full-product acceptance, and were not rerun here.

## Explicitly Unsupported Claims

- AWS-hosted application, or AWS services beyond the verified S3 ingestion path.
- Andrew independently built the complete pipeline, trained the classifier, or deployed the entire application.
- Real bank execution, real-customer model generalization, independent production safety certification, or an LLM-based assistant.
- Proven production scale, invented concurrent-user counts, throughput, load tests, autoscaling, Kubernetes, ECS, distributed databases or a verified load balancer.
- Complete MLOps: model registry, drift alerts, automated retraining and continuous production model governance were not demonstrated.

## Demo Screenshots

The five images in [`assets/demo/`](../assets/demo/) were captured directly from the public application on October 6, 2026, using the designated synthetic customer and agent accounts. They show the existing product UI without mockups, generated imagery, or UI modifications. The dashboard uses a viewport crop; the card and agent pages use full-page captures. JPEG files retain the browser's native screenshot output.

One new conversation was reused for the Spanish card query, clarification, simulated pause preparation, and explicit human handoff. The pause request was cancelled without executing a card mutation. Only one handoff was created; its assigned state was captured in the agent workspace, then that same case was accepted and resolved through the normal agent UI. Existing cases and their history were preserved. The assigned-state image is a capture-time view, not a claim that the case remains open.

Visible card values, movements, and agent identifiers belong to the synthetic demo. No passwords, cookies, session tokens, private data, developer tools, terminals, or administrative infrastructure appear in the images. This documentation walkthrough did not rerun the evaluation suite or change deployment, configuration, services, or model behavior.

## Original Review Method

Read-only source inspection used committed Git objects to avoid treating preexisting local edits as published evidence. Live GitHub reads checked public source and attribution. The server was inspected with read-only commands; existing verification scripts were not rerun. Public checks used GET/HEAD and targeted TCP connections. No login, conversation creation, banking action, deployment update, container restart, ETL acquisition or model training was performed during this audit.
