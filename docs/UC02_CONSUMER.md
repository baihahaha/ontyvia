# Ontyvia UC02 — Consumer Services: Ride-Hailing + Food Delivery

**Status:** Two-domain direction AGREED. Detailed specs below are **DRAFT PROPOSALS** to validate before implementation.

## Objective

Prove that the same Ontyvia framework can model a multi-service consumer platform with shared identity, payment and support semantics but different operational workflows and event streams. Include both ride-hailing and food-delivery tasks, plus cross-domain customer-service investigation.

## Domain boundaries

- **Shared:** user identity, payments, refunds, promotions (optional), support conversations/cases.
- **Ride:** driver, trip/ride, fare, dispatch, status events.
- **Food:** restaurant, order, courier, delivery status events, refund policy.
- **Cross-domain:** same user places rides and food orders, using shared payment and support references.

## Candidate structured dataset inventory

| Table (draft) | Primary key (draft) | Relationships and meaning | Domain |
|---|---|---|---|
| `users` | `user_id` | consumer identity, status | Shared |
| `payments` | `payment_id` | `user_id`, optional `ride_id` OR `food_order_id`, amount/status | Shared |
| `support_cases` | `support_case_id` | `user_id`, case type, linked service order, status | Shared |
| `drivers` | `driver_id` | driver status/availability and vehicle type | Ride |
| `rides` | `ride_id` | `user_id`, driver assignment, pickup/dropoff, fare/status | Ride |
| `ride_events` | `ride_event_id` | `ride_id`, event type and occurrence time | Ride |
| `restaurants` | `restaurant_id` | merchant profile and service status | Food |
| `food_orders` | `food_order_id` | `user_id`, `restaurant_id`, courier assignment, price/status | Food |
| `couriers` | `courier_id` | fulfillment partner status / availability | Food |
| `delivery_events` | `delivery_event_id` | `food_order_id`, event type and occurrence time | Food |

Ten draft tables are suggested (prior brainstorm had nine; `couriers` is an explicit optional tenth table, not yet approved). Keep it simple: order line items could initially be structured JSON; `promotions` can wait. Shared `payments` needs an explicit constrained reference to only one order type or a clear general `service_type + service_order_id` reference; decide in data contract.

### Sample proposed record counts (NOT approved)

350 users; 80 drivers; 45 couriers; 35 restaurants; 1,500 rides; 4,000 ride events; 1,800 food orders; 4,500 delivery events; 3,000 payments; 150 support cases.

## Candidate unstructured document corpus

MinIO objects, each with stable document ID, revision/date, source and relevant service tags:

1. `ride_cancellation_policy.md` — free cancellation rules/fees/refunds.
2. `driver_operations_sop.md` — no-show, reassignment and dispatch controls.
3. `food_refund_policy.md` — late/missing delivery compensation criteria.
4. `restaurant_menu_sample.json` — item-level product descriptions and constraints (can be semi-structured).
5. `customer_support_chats.jsonl` — synthetic conversations about a ride and a food order.
6. `customer_complaints.md` — sample descriptions and ambiguous support terminology.

## Proposed test scenarios

### C1. Ride driver no-show
- Driver accepts ride but fails to arrive; agent checks event history, policy and state, then proposes a legal transition (reassign/cancel/credit) with appropriate approval.
- Evaluate state-machine legality and *event time order*, not only table joins.

### C2. Food order missing/late
- Customer reports non-delivery; agent checks order state, courier events, payment and refund policy; recommends a case outcome.
- Negative case: `DELIVERED` event is insufficient if conflicting delivery evidence exists; flag ambiguity.

### C3. Cross-domain customer support
- Same user has both ride and food complaints or payment issues. Agent must consolidate customer context without conflating the two transactions or applying ride rules to food orders.

### C4. Ambiguous terms and conflicting records
- Source schema says `user_ref`, chat says `rider` or `customer`; policy says `delivery partner`, table uses `courier_id`; events may arrive out of sequence. Record how AI proposes/validates mappings and flags uncertain state.

## Candidate actions (contracts, not implementation)

`GetCustomerOrders`, `OpenSupportCase`, `ReassignDriver`, `CancelRide`, `RequestRefund`, `AssignCourier`.

These are **governed business capabilities**, not a mandate to build a production dispatch/payment system. Mutations can be simulated through a small operational DB/service once requirements are approved. Cancellation, reassignment and refunds require valid state transitions and idempotency.

## Items to finalize in requirements review

- Role boundaries (consumer, operations agent, support agent, supervisor).
- Event timelines and status-transition rules for each service.
- Shared customer identity, payment linking and refund lifecycle.
- Precise table schemas, expected state changes, errors/retries and idempotency cases.
- Which statuses are analytical snapshots in Iceberg versus mutable operational truth.
- Document provenance, cross-domain contradictions and ground-truth labels.
