# Ontyvia — Architecture and Product Decision Register

`Agreed` means the project owner explicitly accepted the principle. `Proposed` is a design suggestion not yet ratified. `Deferred` means do not start implementation solely on this topic.

| ID | Decision | Status |
|---|---|---|
| ADR-001 | Three logical layers: Data, independent Operational Ontology, AI | **Agreed** |
| ADR-002 | Ontology works without AI and can support traditional applications | **Agreed / hard requirement** |
| ADR-003 | Ontology owns Object/Link/Rule semantics and reusable **Action Contracts** | **Agreed** |
| ADR-004 | AI owns Agent Workflow definition/execution; operational services/adapters persist business writes | **Agreed** |
| ADR-005 | Initial analytical stack: IBM watsonx.data + Iceberg + MinIO; future source adapters | **Agreed** |
| ADR-006 | Virtual Ontology first; future selective materialization possible | **Agreed** |
| ADR-007 | Initial Ontology Schema and Mapping Registry: PostgreSQL + JSONB | **Agreed for prototype** |
| ADR-008 | Metadata Federation Service links authoritative sources; catalog owns technical definitions, ontology owns semantic definitions | **Agreed** |
| ADR-009 | Visual Ontology Designer included; full Explorer later | **Agreed** |
| ADR-010 | AI Ontology Builder uses Top-down + Bottom-up + simple LLM Convergence + Human Review | **Agreed** |
| ADR-011 | AI Agent Builder uses ontology to propose reusable business agent workflows | **Agreed** |
| ADR-012 | Traditional ML capability deferred | **Agreed** |
| ADR-013 | **Define TWO business use cases and their data BEFORE Architecture v1.0 or implementation** | **Agreed** |
| ADR-014 | UC01: Banking has BOTH Risk and Investment domains | **Agreed direction** |
| ADR-015 | UC02: Consumer Services has BOTH Ride-Hailing and Food Delivery domains | **Agreed direction** |
| ADR-016 | Both use cases require structured AND unstructured data, multiple scenarios and cross-domain exploration | **Agreed direction** |
| ADR-017 | One general framework must serve both use cases, not two bespoke hardcoded pipelines | **Agreed design objective** |
| ADR-018 | Detailed ontology schema lifecycle, action executor internals, permissions, event architecture | **Deferred** |
| ADR-019 | Exact table lists, columns, volumes, docs, workflows and gold answers in `UC*.md` | **Proposed / requires review** |
| ADR-020 | Precise Architecture v1.0, language/framework selection, MVP scope and cloud deployment | **Pending** |
| ADR-021 | Project/product name **Ontyvia**; intended repo/PyPI/import namespace `ontyvia`; earlier acronym ODEIP is historical only | **Agreed naming; release registration pending** |
| ADR-022 | watsonx.data is deployed on OpenShift (OCP), accessed through a data-source adapter | **Agreed (2026-11-19)** |
| ADR-023 | Evaluation-first: hidden reference ontology, gold scenarios and metrics are core deliverables, kept in an isolated `eval/` area; a provisional ontology notation may be defined for them before Architecture v1.0 | **Agreed (2026-11-19)** |
| ADR-024 | Sequencing: UC01 specified in depth first; UC02 after UC01 review, deliberately leaner | **Agreed (2026-11-19)** |
| ADR-025 | MVP Ontology Designer is a review/edit interface, not a full visual canvas | **Agreed (2026-11-19)** |
| ADR-026 | License: MIT | **Agreed (2026-11-19)** |

## What changed since older handoff

- **Do not** ask to select one business vertical slice: **both cases are selected as directions**.
- **Do not** start with Architecture v1.0 or code. First finish two case narratives, data definitions, document sets, test truth/fixtures and evaluation criteria.
- The detailed designs below are **proposals**; preserve agreed architectural boundaries when editing them.

## Branding migration (v0.7)

The project was previously described as **ODEIP** (Ontology-Driven Enterprise AI Platform). **Ontyvia** is now the project-facing name. This is a naming change only; no earlier architectural decisions have been replaced. `pip install ontyvia` is an aspirational package command, not evidence of a published release.
