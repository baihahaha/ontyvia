# Ontyvia — Evaluation & Acceptance Outline (Draft)

**Status:** Provisional test plan for the two-use-case design process, not an approved benchmark.

## Why two separate test oracles?

The system is intended to **discover** the ontology and **compose** workflows. If we pass the expected ontology or gold workflow as a source input, a good output proves nothing. Keep an independent gold set for evaluation only.

## Tests for EACH use case

| Capability | Representative acceptance check |
|---|---|
| Top-down discovery | From business documents and objectives, identify required entities, business vocabulary, rules and action candidates with traceable citations to source documents. |
| Bottom-up discovery | From catalog/table metadata and data samples, infer plausible entities and source relations, preserving source evidence rather than hallucinating semantic certainty. |
| Convergence | Correctly align equivalent concepts; surface conflicting/missing evidence and *do not* silently create unsupported fields/mappings. |
| Designer independence | A human can view/edit/save/review a schema without any AI dependency; no auto-publish of model proposals. |
| Mapping federation | Semantic property maps to catalog asset ID/column with source provenance; invalid reference flags drift. |
| Virtual ontology | Query selected business object through a real/simulated adapter; no full instance copy in schema registry. |
| Agent generation | Create a workflow proposal using published object/action contracts rather than hardcoded per-case workflows. |
| Safe action | A state-changing action is validated, authorized, approval-gated if required, and persisted by an operational adapter or clear simulation. |
| Cross-domain semantics | Correctly join shared business identity while honoring domain-specific rules and avoiding incorrect conflation. |
| Evidence and errors | Cite input records/documents, identify uncertainty, and produce auditable failure paths. |

## UC01 independent checks

1. `B1`: risk investigation joins customer/accounts/transactions/alerts correctly and considers SOP.
2. `B2`: investment summary connects customer to investment account and policy; does **not** place a live trade.
3. `B3`: cross-domain funding + investment scenario uses verified same-customer identity and documents unsupported claims.
4. `B4`: ambiguous customer IDs or contradictory note are flagged, not blindly reconciled.

## UC02 independent checks

1. `C1`: delayed driver no-show and status-change flow respects transitions and authorization.
2. `C2`: non-delivery/refund proposal checks courier events, payment and policy; avoids automatic payment movement.
3. `C3`: shared user joins ride and food histories without mixing order IDs/business rules.
4. `C4`: naming/temporal ambiguity produces justified mapping candidates and uncertainty notes.

## Scope / quality gates

- **Gate A:** Both case briefs + complete data dictionaries + document manifests + scenario matrices approved.
- **Gate B:** Synthetic dataset generation specification + test oracles approved.
- **Gate C:** Only then draft Architecture v1.0, selecting necessary shared components and cross-layer contracts.
- **Gate D:** Only after separate approval implement MVP slices; demonstrate both cases on a single framework without code paths that hardcode business identities.

**No arbitrary accuracy threshold is agreed yet.** During specification, propose measures (e.g., grounded entity/link precision, verified mapping recall, gap detection, workflow completion/approval compliance), then negotiate useful pass criteria against the actual gold set.
