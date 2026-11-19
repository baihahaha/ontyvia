# Ontyvia

**An open-source, ontology-driven enterprise AI platform that unifies business semantics, data, and agentic workflows.**

> **Status: specification phase.** There is no runnable code yet, and `pip install ontyvia` is a future target — no package has been published.

---

## What is Ontyvia?

LLM agents are good with language but know nothing about how a particular enterprise is organized: what a "customer" is, how accounts relate to transactions, which rules apply, and which actions are allowed. Ontyvia provides that missing layer — an **operational ontology** — and uses AI on both sides of it:

1. **AI helps build the ontology** from enterprise data *and* business documents.
2. **AI builds business agent workflows** on top of the published ontology, and those workflows can only change business state through governed actions.

```text
Enterprise data + documents
        │
        ▼
AI-assisted ontology construction ──► human review ──► published ontology
                                                          │
                                                          ▼
                                     AI-generated agent workflows
                                                          │
                                                          ▼
                                    governed business actions ──► new enterprise data
```

## Architecture at a glance

Three logically independent layers:

| Layer | Responsibility | Prototype stack |
|---|---|---|
| **3 · AI Platform** | **AI Ontology Builder** (top-down + bottom-up + convergence, then human review) and **AI Agent Builder** / workflow runtime | TBD |
| **2 · Ontology Platform** | Object types, link types, business rules, **Action Contracts**, semantic-to-physical mappings, visual Ontology Designer | PostgreSQL + JSONB registry |
| **1 · Data Platform** | Physical data, technical schemas and catalog, persisted business state | IBM watsonx.data (on OpenShift) + Apache Iceberg + MinIO; other sources via adapters |

### Core principles

- **The ontology works without AI.** It is a standalone business semantic platform that traditional applications can use. AI is one of its authors and one of its consumers.
- **Clear ownership of actions.** The ontology owns the action *contract*; the AI layer owns *workflows* (when and how actions are called); operational systems own *implementation and state*.
- **Virtual ontology first.** Object instances stay in their source systems and are resolved through validated mappings. Selective materialization can be added later if performance requires it.
- **Federated metadata authority.** Each metadata concept has exactly one authoritative owner. The data catalog owns technical definitions, Ontyvia owns semantic definitions, and Ontyvia stores references and mappings — not copies — and detects drift.
- **Two-sided authorization.** An operation is allowed only if *both* data-access policy and business-action policy permit it.
- **No auto-publishing.** AI produces evidence-linked proposals; a human approves before anything reaches the published registry.
- **Independent versioning.** Ontology schema, data mappings and AI workflows are versioned separately.
- **One framework, many domains.** No domain-specific logic is hardcoded into the platform core.

## Validation use cases

The architecture will be derived from — and proven on — two deliberately different enterprises, each with two business domains, structured *and* unstructured data, and cross-domain scenarios:

| | UC01 · Banking (B2B) | UC02 · Consumer Services (B2C) |
|---|---|---|
| Domains | Risk Management + Investment Management | Ride-Hailing + Food Delivery |
| Shared concepts | Customer, account, money movement | User, payment, support case |
| Example scenarios | AML investigation; suitability check; large inbound transfer followed by a large investment order; conflicting customer identifiers | Driver no-show; missing food delivery; one user with ride and food complaints; ambiguous terms and out-of-order events |
| Example actions | `CreateRiskCase`, `CheckInvestmentSuitability` | `ReassignDriver`, `RequestRefund` |

All data is synthetic. Each dataset intentionally contains realistic ambiguity (mismatched identifiers, missing attributes, documents that contradict data) plus negative controls, so the AI Builder is tested on discovery rather than on copying a tidy schema.

### Evaluation comes first

Ontyvia's central claim — that AI can help build a correct, reusable ontology and generate safe workflows from it — has to be **measured**, not demonstrated. For each use case we keep a **hidden reference ontology and gold scenario outcomes**, isolated from every AI input, along with explicit metrics (for example mapping precision/recall, gap detection, and approval-gate compliance).

## Roadmap

| Phase | Deliverable | Status |
|---|---|---|
| 0 | Architecture principles and decision register (v0.7) | ✅ Done |
| 1 | **UC01 Banking** specification: business requirements, data dictionary, relationships, document corpus, scenario matrix | ⏭ Next |
| 1b | Evaluation package for UC01: provisional ontology notation, reference ontology, gold scenarios, metrics | ⏭ Next |
| 2 | UC02 Consumer specification (kept leaner than UC01) | Planned |
| 3 | Deterministic synthetic data generators and document corpus | Planned |
| 4 | Architecture v1.0, derived from both use cases | Planned |
| 5 | MVP: one shared framework running both use cases end to end | Planned |

Planned MVP scope choices: the Ontology Designer starts as a review/edit interface rather than a full visual canvas; traditional ML, a full Ontology Explorer, a materialized object store and a broad connector portfolio are out of scope for the first release.

## Planned usage (not yet available)

```bash
pip install ontyvia
ontyvia init
```

```python
from ontyvia import Ontology
```

## Documentation

- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — architecture baseline
- [docs/DECISIONS.md](docs/DECISIONS.md) — decision register
- [docs/UC01_BANKING.md](docs/UC01_BANKING.md) · [docs/UC02_CONSUMER.md](docs/UC02_CONSUMER.md) — use-case drafts
- [docs/DATA_DESIGN.md](docs/DATA_DESIGN.md) — dataset and document-corpus requirements
- [docs/ACCEPTANCE_TESTS.md](docs/ACCEPTANCE_TESTS.md) — evaluation outline

## License

[MIT](LICENSE)
