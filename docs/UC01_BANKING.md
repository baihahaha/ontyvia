# Ontyvia UC01 — Banking Enterprise: Risk + Investment

**Status:** Two-domain direction AGREED. The detailed requirements, tables, relationships, volumes, workflows and document corpus below are **DRAFT PROPOSALS for validation**.

## Objective

Demonstrate that a single governed enterprise ontology can connect a bank's **Risk Management** and **Investment Management** domains through shared Customer/Account concepts; discover this model from structured + unstructured sources; then produce multiple task-specific AI workflows from the published ontology.

## Domain boundaries

- **Shared:** customers, bank accounts, money movements, identifiers and authorized personnel.
- **Risk:** transaction monitoring, AML alerts, investigation cases and approvals.
- **Investment:** investment accounts, instruments, portfolios/holdings, order history and suitability context.
- **Cross-domain:** same customer may own deposit and investment accounts. Risk and investments must not invent divergent Customer business definitions.

## Candidate structured dataset inventory

| Table (draft) | Primary key (draft) | Relationships and meaning | Domain |
|---|---|---|---|
| `customers` | `customer_id` | customer attributes, segment, risk/suitability profile refs | Shared |
| `accounts` | `account_id` | `customer_id` -> customer; balance/currency/account status | Shared |
| `transactions` | `txn_id` | `account_id` -> bank account; counterparty, amount, timestamp | Shared / Risk |
| `risk_alerts` | `alert_id` | `txn_id` and/or `customer_ref`; trigger/rationale/severity | Risk |
| `risk_cases` | `case_id` | `customer_id`, source alert, investigation status, approver | Risk |
| `investment_accounts` | `investment_account_id` | `party_ref` -> same customer via documented ID mapping | Investment |
| `securities` | `security_id` | instrument type, risk rating, issuer, currency | Investment |
| `holdings` | composite `investment_account_id + security_id` | positions/quantities/current value | Investment |
| `investment_orders` | `order_id` | `investment_account_id`, `security_id`, side, quantity, status | Investment |

**Important draft ambiguity:** `investment_accounts.party_ref` and `customers.customer_id` deliberately differ; define a *verifiable* source mapping/identifier alias, not an LLM guess. Customer-account ownership can be one-to-many initially; joint accounts are a later extension unless needed by a scenario. A customer can have no investment account.

### Sample proposed record counts (NOT approved)

300 customers; 400 bank accounts; 6,000 transactions; 200 risk alerts; 40 risk cases; 180 investment accounts; 50 securities; 800 holdings; 900 investment orders. Values should be revisited after drafting actual test scenarios.

## Candidate unstructured document corpus

Store documents as MinIO objects with stable document IDs and document metadata (type, date, author role, source, related customer/case, revision). All customer text must be synthetic.

1. `customer_onboarding_sop.md` — customer/account requirements and entity semantics.
2. `aml_investigation_sop.md` — alert triage, evidence thresholds, escalation approvals.
3. `investment_suitability_policy.md` — suitability, investment risk and restrictions.
4. `security_factsheet_fund_A.md` — example security risk/eligibility details.
5. `rm_interaction_notes.jsonl` — dated customer notes on investment goals.
6. `customer_messages.jsonl` — customer email/requests about portfolio or transfers.

**Design-time separation:** Policies/SOPs can feed Top-down. Table schemas/catalog and carefully sampled data can feed Bottom-up. Do not leak hidden reference ontology or test expected answers into those inputs.

## Proposed test scenarios

### B1. Risk investigation (single domain)
- **Business task:** investigate a suspicious transfer pattern, produce evidence-backed recommendation, request `CreateRiskCase` if policy supports it.
- **Expected ontology coverage:** Customer -> Account -> Transaction -> Alert, linked RiskCase; Action Contract.
- **Negative case:** large transfer alone is not sufficient evidence to assert fraud. Enforce approval.

### B2. Investment request (single domain)
- **Business task:** customer asks for portfolio summary and suitability check on a proposed order.
- **Expected coverage:** Customer -> InvestmentAccount -> Holding/Security/InvestmentOrder; suitability policy from docs.
- **Guardrail:** no autonomous financial trading, only a documented simulation/recommendation or approval-gated order action.

### B3. Cross-domain risk + investment
- **Business task:** a customer receives a large transfer and places an unusually sized investment order. Agent must combine source-of-funds evidence, transaction facts, current holdings and suitability rules to produce an escalation recommendation.
- **Expected coverage:** join deposit/investment account identities through *verified* mapping; distinguish evidence from hypothesis; do not equate large funds with criminality.

### B4. Ambiguous mapping / conflicting evidence
- A business document uses `Client`, investment schema uses `party_ref`, and risk data uses `customer_id`; one note contradicts an out-of-date structured field.
- AI Ontology Builder should suggest mappings with evidence, mark ambiguity and refer unresolved matters to review rather than silently merge.

## Candidate actions (contracts, not implementation)

`GetCustomer360`, `GetRiskAlerts`, `CreateRiskCase`, `RequestCaseApproval`, `GetPortfolioSummary`, `CheckInvestmentSuitability`.

- Read-oriented capabilities may be **functions/query operations** rather than mutation Actions. Decide during detailed design.
- Do not claim an Action can execute just because a SOP mentions a process. Implementations and approvals may be simulated.

## Items to finalize in requirements review

- Actor personas/authorizations (risk analyst, investment RM, case approver).
- Exact columns/types/keys, record histories, timestamps and example values.
- Which documented policies should be enforced automatically vs human interpreted.
- Canonical customer identity resolution and deterministic mapping fixtures.
- Cross-domain queries, negative tests, document provenance and exact expected outcomes.
- Mutating business-state home for risk case updates (separate operational store if needed; do not assume Iceberg OLTP).
