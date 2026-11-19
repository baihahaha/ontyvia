# Ontyvia — Dataset and Document Corpus Design v0.1

**Purpose:** Requirements for the TWO test datasets. This file does **not** assert that the proposed schemas/volumes in UC files are already approved. Do not generate data until both detailed contracts have been reviewed.

## 1. What each case must include

1. At least **two business domains** with a shared entity/identity concept.
2. **Structured** tables: catalog-visible, typed columns, keys and timestamps, seeded coherent sample data, mapped to Iceberg for analytical reads.
3. **Unstructured** sources: business SOPs/policies and customer/employee free text. Store as MinIO objects; document IDs and metadata enable discovery and provenance.
4. **Cross-domain** stories where data from both domains is necessary.
5. Genuine but controlled ambiguities: mismatched field labels, missing attributes, conflicting SOP/data evidence, incomplete relationships and delayed event updates.
6. At least one **read-only** scenario and one **state-changing governed action** scenario, with simulation and approval gates.

## 2. Source-of-truth rules and storage

| Artifact | Proposed default home | Authority |
|---|---|---|
| Analytical/historical structured rows | Iceberg tables on MinIO, queried via watsonx.data | Data layer |
| Source technical schemas/catalog | watsonx.data-connected catalog or chosen test catalog | Data catalog |
| Policies/SOPs, chats, notes | MinIO object files with a document manifest | Source document repository |
| Ontology type definitions, properties, links, Action Contracts | Ontyvia PostgreSQL JSONB registry (later) | Ontology layer |
| Property-to-source mappings | Ontyvia PostgreSQL mapping registry (later) | Ontyvia mapping governance |
| Mutable ride/order/case state, if needed | Small operational service/DB, TBD | Operational source |
| Generated ontology/workflow proposals | AI design-time drafts, separate from published registry | AI authoring store |
| Reference ontology / gold expected outcomes | **Test-only isolated artifacts** | QA oracle, never model input |

JSONL chat files are unstructured text with metadata. JSON menu files are **semi-structured**; don't count menus alone as proof of unstructured language understanding.

## 3. Deliverables from specification phase

For **each** use case, produce:

- `business_requirements.md`: actors, goals, service outcomes, rules and controlled actions.
- `data_dictionary.md`: table names, each column/type/nullable/domain, PK/FK, source system, temporal semantics, sample rows, sensitive classification, business meaning where documented.
- `relationships.md`: source-known join evidence vs hypothesized semantic links; deterministic ID mapping where appropriate.
- `document_manifest.csv`: doc ID, object path, title, author role, revision/date, related domain, provenance, confidence/evidence label.
- Document content drafts: short coherent SOP/policy and chat/note samples with explicit version/date and traceable cross-document consistency.
- `scenario_matrix.md`: user input, required source facts, relevant policy, expected workflow outcome, allowed actions, failure/uncertainty cases.
- `generation_spec.md`: random seed, distributions, temporal logic, referential-integrity rules, exact intended anomalies and counts.
- A separately held `reference_ontology` and `gold_scenarios` spec for assessment, NOT accessible to the AI Builder at generation time.

## 4. Synthetic data rules

- **No real customer PII, account numbers, operational bank data, payment card numbers, or credential tokens.** Use fabricated names/IDs and synthetic addresses if any.
- Reproducible generation with fixed seed(s), explicit generator version and deterministic event timelines.
- Preserve PK uniqueness, required FKs and reasonable status/date ordering by default; deliberately label exceptions and expected outcomes in the hidden QA manifest.
- Data should not be unrealistically perfect: e.g., `party_ref` vs `customer_id`, `rider` vs `user`, delayed events, conflicting documentation versions. **Each ambiguity must be resolvable, or explicitly marked as unresolvable from source evidence.**
- Include positive and negative controls. A high dollar amount **alone** must not imply fraud; a `DELIVERED` status alone need not settle a delivery dispute.
- Use bounded, realistic cardinality and chronology. Synthetic investment holdings must reconcile with accounts and dated order activity as appropriate to each scenario.
- Clear distinction: **observed raw fact**, **derived feature**, **business interpretation**, **human-approved decision**.
- Keep the reference ontology/evaluation oracle out of AI prompts, retrieval stores and documents used in Top-down/Bottom-up analysis.

## 5. Multi-stage validation

- **Specification gate:** business scenarios, columns, rules, document content and synthetic anomalies approved.
- **Data integrity gate:** keys, FK consistency, distributions, types, time semantics, documented anomalies verified.
- **Ontology discovery gate:** top-down and bottom-up proposals independently evaluated, convergence evidence and gaps reviewed.
- **Agent workflow gate:** generated flows use correct business concepts/actions, adhere to approval/policy gates, and do not confuse domains.

## 6. Explicit non-goals for first specification pass

- No need for millions of rows, embeddings at scale, realtime Kafka clusters, actual payment/refund, ML model training or production-grade multi-agent convergence.
- Do not prescribe Python/React/FastAPI/GraphDB/LLM vendor yet; discuss only if required by a concrete data/acceptance need.
