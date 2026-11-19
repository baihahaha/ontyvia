# First Task for Claude Code — Specification Design Only

> **Historical (v0.7 handoff prompt).** Still valid as a scope guard. Sequencing has since been refined by ADR-023–025: UC01 + its evaluation package first, then UC01 review, then UC02. Doc paths now live under `docs/`.

**Paste the prompt below into Claude Code in Plan Mode.**

---

Read `README.md`, `DECISIONS.md`, `ARCHITECTURE.md`, `UC01_BANKING.md`, `UC02_CONSUMER.md`, `DATA_DESIGN.md`, and `ACCEPTANCE_TESTS.md` completely.

We have ALREADY AGREED to two use-case directions:

1. **UC01 Banking Enterprise:** Risk Management + Investment Management, including cross-domain customer cases.
2. **UC02 Consumer Services:** Ride-Hailing + Food Delivery, including cross-domain customer support.

Each case MUST include structured tables plus unstructured policies, SOPs, chats/notes and multiple distinct scenarios. We will use these to drive the design of one shared Ontyvia framework **AFTER** the use cases and datasets are designed.

Your first deliverable is a **documentation-only specification plan**. Please:

1. Summarize briefly the agreed business/architecture boundaries. Distinguish agreed decisions from unapproved table/field suggestions.
2. Assess the draft UC01 and UC02 cases: are the domains, scenarios, cross-domain joins and actions adequate to stress the architecture without needless complexity? Point out gaps or contradictions.
3. Propose a concrete specification package for **both** cases: actors/business rules, scenario acceptance steps, data dictionaries with columns/types/PK/FKs, relationship evidence, document manifests, suggested synthetic record distributions, and designed ambiguity/negative-control fixtures.
4. Explicitly distinguish Top-down inputs, Bottom-up inputs, Convergence outputs and hidden reference ontology / test oracle. Avoid data leakage.
5. Identify a SMALL number of essential business assumptions that need owner approval before generating the datasets.
6. Recommend a staged sequence for writing and reviewing the two case specs and datasets. Start with documents; defer any code, generators and deployment.

**Important:** Do NOT propose choosing only one use case. Do NOT start with Architecture v1.0, implementation scaffolding, source code, synthetic data generation, installing dependencies, API development or cloud provisioning. Do NOT create any new files until I approve your proposed specification plan. After approval, work on UC01 specification, then UC02, with review checkpoints.

When responding, give me a precise, bounded proposal and the first specification review checkpoint, not a generic tutorial.
