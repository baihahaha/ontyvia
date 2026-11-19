# Ontyvia — Claude Code Project Rules (v0.7)

You are collaborating on the **specification-first** design of Ontyvia. Read `README.md`, then `docs/DECISIONS.md`, `docs/ARCHITECTURE.md`, both `docs/UC*.md`, `docs/DATA_DESIGN.md`, and `docs/ACCEPTANCE_TESTS.md` before working.

## Repository layout

- `docs/` — working design documents. Edit these.
- `FromChatGPT_Design/` — original ChatGPT handoff (v0.7) and the full design transcript (`Ontology_draft0.pdf`, in Chinese). **Read-only reference; never modify. Git-ignored.**
- `eval/` (planned) — hidden reference ontologies, gold scenarios and metrics. **Never use anything under `eval/` as input to the AI Ontology Builder, Agent Builder, prompts or retrieval corpus.**

## Current priorities (agreed 2026-11-19)

1. UC01 Banking specification first, in depth; UC02 follows after UC01 review and stays leaner.
2. Evaluation is a first-class deliverable: write the UC01 reference ontology, gold scenarios and metrics alongside the spec, using a provisional ontology notation (not Architecture v1.0).
3. MVP Ontology Designer = review/edit interface, not a full visual canvas.
4. watsonx.data runs on OpenShift (OCP); keep it behind a data-source adapter.

## Naming — mandatory

- Use **Ontyvia** as the project and product name, `ontyvia` as the future repository/PyPI/import namespace, and “Ontology-Driven Enterprise AI Platform” as the descriptive subtitle.
- ODEIP is a historical working acronym only; do not introduce it into new code or new user-facing documents.
- Do **not** assume the PyPI package exists, is reserved, or is installable. Do not publish a package, create repositories, or claim a CLI exists without approval.
- Naming is locked as a project preference; package availability and trademark clearance are separate release checks.

## Current mission (important change)

**Do NOT start Architecture v1.0 implementation.** The owner explicitly changed the sequence: design **TWO use cases and their structured/unstructured synthetic data**, get approval, THEN derive Architecture v1.0 from the two scenarios, THEN build one platform and implement both cases.

## Non-negotiable architectural boundaries

- Data, independent Ontology, and AI are separate logical platforms.
- Ontology remains independently useful without an LLM or agent system. Ontology Designer must support manual editing.
- Ontology Registry stores schema/contracts and mappings, NOT full customer/ride/order instance datasets.
- Prototype analytical stack: IBM watsonx.data + Apache Iceberg on MinIO. Other sources behind adapters; mutable operational state need not be Iceberg.
- Metadata Federation stores catalog references and mappings, not copies of authoritative technical definitions; it is not business query execution or action write-back.
- Ontology owns governed business Action **Contracts**, AI owns workflow definitions/orchestration; operational services/adapters own state persistence.
- AI Ontology Builder = top-down + bottom-up + **simple** convergence (LLM-assisted) + human review before publication.
- Two confirmed **directions**: Banking Risk + Investments; Consumer Ride-Hailing + Food Delivery. Both require structured and unstructured data and cross-domain scenarios.
- Traditional ML, complete Ontology Explorer, materialized object store, production multi-agent swarm and broad external connector portfolio are out of scope for early MVP.

## Operating mode and approvals

1. Start in **Plan Mode**: summarize what is agreed, what's merely proposed, and the next documentation steps.
2. First deliverables are **business/data specification files only**. You may suggest precise table schemas, stories, document text, or synthetic data rules, but flag them `Proposed` until the owner approves.
3. Do not scaffold an application, write implementation modules, install packages, generate datasets, start services, deploy resources, read credentials or incur cloud charges without approval for the corresponding phase.
4. Avoid fake certainty: source identifier equivalences, business rules, legal/compliance meaning and workflow authorization must be explicitly backed by test evidence or identified as assumptions.
5. Always keep synthetic data and hidden evaluation oracles separate. Never leak gold ontology and expected answers into model inputs or the retrieval corpus.
6. Do not turn Ontology into a domain-specific Agent Framework, or hardcode risk/ride terms into a generic platform core.
7. Use small, reviewable documentation updates. Summarize proposed changes, open questions, and what was NOT done.
8. Keep ADRs updated only when the user accepts a change; record unapproved ideas in a proposals section rather than silently amending decisions.

## Sequence after specification approval

1. Produce deterministic synthetic dataset generators and corpus fixtures (with integrity tests) only when approved.
2. Derive a common Architecture v1.0 from BOTH domains and demonstrated cross-domain requirements.
3. Propose minimal MVP feature set and delivery phases.
4. Implement one reusable framework plus the two validation cases, with tests and review gates.
