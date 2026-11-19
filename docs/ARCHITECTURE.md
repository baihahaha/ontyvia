# Ontyvia — Architecture Baseline v0.7

**Status:** Agreed architecture principles and boundaries. This is **NOT** an approved Architecture v1.0, data model, or implementation plan.

## Vision

Ontyvia is a vendor-neutral, three-layer Enterprise AI Platform with a standalone operational Business Semantic Platform. AI helps enterprises **build their ontology** and **build business-agent flows** from it. Ontology must function normally **without an LLM or AI layer**.

## Three layers

### Layer 1 — Unified Data Foundation
- Default analytical environment for the prototype: **IBM watsonx.data + Apache Iceberg tables + MinIO (S3-compatible object storage)**.
- Structured analytical/historical data in Iceberg; unstructured documents as files/objects (e.g. Markdown, TXT, PDF, JSON), with catalog/index references when appropriate.
- Data catalog owns technical schemas and data-asset metadata. A catalog **is not** the same thing as an operational ontology.
- Iceberg is **not** mandatory for mutable operational state/OLTP. Keep operational services/DBs behind adapters where needed, especially UC02.
- Future data source adapters may connect external tables, S3 buckets, relational systems, APIs and other catalogs. No need to implement all in v1.0.

### Layer 2 — Operational Ontology / Business Semantic Platform
- Independently owns: Object Types, Properties, Link Types, Business Rules, **business Action Contracts**, and business semantic identifiers.
- **Schema Registry:** PostgreSQL + JSONB for the initial prototype (authoritative ontology definitions). Schema format and detailed lifecycle are not finalized.
- **Metadata Federation Service:** discovers external catalog metadata, resolves references, validates mappings, observes drift. **This is metadata integration**, NOT runtime data transfer or action execution.
- **Mapping Registry:** mapping records in Ontyvia PostgreSQL, referencing authoritative physical assets (do not duplicate their schema as a separately editable source of truth).
- **Visual Ontology Designer:** manual editing, visual relationships, review, approval/publishing; must work without AI. Full Ontology Explorer deferred.
- **Virtual Object Resolution first:** object instances stay in data sources. Ontology exposes business object views via validated mappings and source adapters. Possible targeted caches/graph materialization later, not at outset.
- **Actions:** ontology owns the *contract and business semantics*; operational adapters/services own implementation and persistence. Ontology does not have to become the main OLTP database.

### Layer 3 — AI Platform
- **AI Ontology Builder:** Top-down (business goals/SOPs/requirements) + Bottom-up (tables, metadata, APIs, data samples) + **simple LLM-assisted Convergence** (matches, conflicts, gaps) -> evidence-linked proposals -> human review in Designer -> publish to Ontology Registry.
- **AI Agent Builder / Workflow Runtime:** uses published objects, relationships, business functions/rules and Action Contracts to propose and run governed business workflows. AI layer owns workflow definition/metadata, planning, orchestration, evaluation.
- Traditional ML intentionally deferred; keep room for future reuse.

## Runtime and governance paths

```text
Business documents + requests          Catalog / Iceberg / data samples
            |                                    |
     AI top-down analysis                  AI bottom-up analysis
            +------------------+-----------------+
                               |
                   Convergence proposal
                               |
                  Ontology Designer review
                               |
                       Published Registry
                               |
        Non-AI applications <--+--> AI Agent Builder / Runtime
                               |
               Virtual Ontology Query / Action Contracts
                  |                            |
          Runtime read adapters      Governed action adapters
                  |                            |
       Data & operational sources     Operational system persists state
```

- **Metadata plane**: catalog metadata <-> mapping references <-> ontology schema. Do not conflate with business data queries.
- **Read path**: ontology runtime resolves objects from validated schema/mappings and an authorized runtime data connector.
- **Write path**: workflow/application calls published Action Contract; validations/authorization run; operational adapter executes against authoritative state store; ontology runtime resolves resulting state. Metadata federation is not the write path.
- **Security**: data access checks + business/action policy checks both apply; no assumed superuser bypass. For MVP, use simulations and approvals for consequential operations.

## Design rules

1. Ontology must be useful **without AI**; AI is optional author/consumer.
2. Three layers remain logically independent; no data table = ontology object by default.
3. Business semantics and action contracts are governed in ontology; AI workflow metadata is governed in AI.
4. Physical data/catalog metadata remain authoritative in their source; Ontology Metadata Federation stores references, mappings and provenance.
5. Schema versions, mapping versions and workflow versions are independent.
6. Virtual Ontology first; optional Hybrid extension.
7. For design-time generation, accept drafts with evidence, uncertainty and gaps; **no auto-publishing**.
8. One framework must support **both** business cases; avoid hardcoded domain-specific platform behavior.
9. Business use cases and data contracts must be agreed **before** Architecture v1.0 and before coding.

## Decisions parked or pending

- Detailed ontology schema DSL, migrations and lifecycle; RDF/OWL/SHACL support.
- Detailed write-back consistency, eventing, action execution semantics and security architecture.
- Exact endpoints/framework libraries/UX/infra deployment choices.
- Precise table schemas and document content; see UC files as **drafts**.
- Architecture v1.0 must be *derived from* the finalized two use cases, not preconceived.
