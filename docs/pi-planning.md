# SAFe-Aligned PI Planning Simulation

## Disclaimer

This is a public portfolio simulation. Aureon Air and Nexora Airways are fictional placeholders used for anonymized airline retailing analysis. No internal airline data is used.

## PI Theme

**Reduce post-booking servicing uncertainty by making passenger-level eligibility, change impact, refund impact, and ancillary continuity visible before confirmation.**

## Product Goal for PI-1

Deliver the MVP foundation for Smart Order Servicing and Ancillary Continuity across direct and seller-style channels.

## PI Objectives

| PI Objective | Description | Business Value | Commitment | Success measure |
|---|---|---:|---|---|
| PI-OBJ-1 | Unified order retrieve | 9 | Committed | 95% of eligible orders return itinerary, passenger, payment, and ancillary state |
| PI-OBJ-2 | Passenger-level servicing eligibility | 10 | Committed | Eligibility response shows order-level and passenger-level allowed/blocked actions |
| PI-OBJ-3 | Change quote with ancillary impact | 10 | Committed | Eligible changes show fare, tax, fee, total due/refund, and ancillary impact |
| PI-OBJ-4 | Refund quote for ticket and extras | 8 | Committed | Refund quote separates ticket and ancillary line items |
| PI-OBJ-5 | Seller servicing API pilot | 8 | Stretch | Pilot seller can retrieve order and generate eligibility/change quote using API |
| PI-OBJ-6 | Observability and operational readiness | 7 | Committed | Dashboard shows latency, error rate, success rate, and assisted handoff |

## Iteration Plan

| Iteration | Sprint goal | Planned scope |
|---|---|---|
| Iteration 1 | Establish order retrieve foundation | SOS-101, SOS-102, base schemas, audit event model |
| Iteration 2 | Make servicing eligibility visible | SOS-103, SOS-104, eligibility reason codes, channel permissions |
| Iteration 3 | Quote passenger split and change | SOS-105, SOS-107, SOS-108, pricing/fare rule adapter integration |
| Iteration 4 | Confirm changes and handle ancillaries | SOS-106, SOS-109, SOS-110, SOS-111, idempotency and continuity logic |
| Iteration 5 | Refund and seller API pilot | SOS-112, SOS-113, SOS-114, SOS-115, dashboard and support runbook |
| IP iteration | Hardening and demo | Performance testing, security review, PI system demo, ROAM review, backlog refinement |

## Dependencies

| Dependency | Owning area | Impact | Mitigation |
|---|---|---|---|
| Fare rules and repricing | Pricing / revenue management | Required for change quote and refund quote | Agree quote request/response contract before Iteration 3 |
| Inventory availability | Inventory integration | Required for alternative flight search | Mock early, integrate incrementally |
| Seat inventory and seat map | Ancillary platform | Required for paid seat continuity | Define fallback states for unavailable seat data |
| Baggage rules | Ancillary catalog | Required for extra baggage transfer/refund | Build route/cabin/fare rule matrix |
| Payment/refund service | Payments / finance | Required for additional collection and refund initiation | Use payment reference in MVP; defer full gateway complexity |
| Seller permissions | Distribution platform | Required for seller servicing API | Centralize permission policy |
| Authentication and RBAC | Platform security | Required for safe order access | Complete security design in Iteration 1 |
| Audit/event platform | Platform engineering | Required for traceability | Define canonical servicing events early |

## ROAM Board

| Item | Type | ROAM status | Owner | Response |
|---|---|---|---|---|
| Passenger split complexity may exceed MVP assumptions | Risk | Mitigated | Product + Order Service | Start with quote and assisted handoff for complex cases |
| Ancillary continuity differs by route, fare, aircraft, and channel | Risk | Owned | Ancillary Service | Build rules-driven states and plain-language fallbacks |
| Refund quote may not equal final settlement amount | Risk | Accepted | Finance Product Owner | Mark as estimate until final validation; track quote accuracy |
| Seller channel permission rules are unclear | Risk | Mitigated | Distribution | Add permission matrix before seller API pilot |
| Change confirmation retry could duplicate payment/refund | Risk | Resolved | Platform Engineering | Mandatory idempotency key for write APIs |
| Quote latency may exceed target due to multiple downstream calls | Risk | Owned | Architecture | Add timeout, caching where allowed, and degraded fallback |

## SAFe Principles Applied

| SAFe principle | PI planning application |
|---|---|
| 1. Take an economic view | Prioritize highest Cost of Delay items: change quote clarity, paid ancillary continuity, refund transparency |
| 2. Apply systems thinking | Plan across order, pricing, inventory, ancillaries, payments, sellers, audit, and support |
| 3. Assume variability; preserve options | Keep assisted handoff and partial automation options for complex split/refund cases |
| 4. Build incrementally with fast learning cycles | Deliver order retrieve first, then eligibility, quote, continuity, refund, and seller API |
| 5. Base milestones on objective evaluation | Use integrated demos with quote, continuity, idempotency, and audit evidence |
| 6. Make value flow without interruptions | Reduce handoffs between manage-booking, contact center, seller, payment, and refund operations |
| 7. Apply cadence and synchronize planning | Align dependencies through PI Planning, iteration goals, system demos, and IP iteration hardening |
| 8. Unlock intrinsic motivation | Give teams outcome ownership for customer confidence and self-service completion |
| 9. Decentralize decision-making | Teams decide implementation details; product/commercial own policy and value decisions |
| 10. Organize around value | Organize the backlog around order servicing value stream, not component silos |

## System Demo Script

1. Retrieve an order with four passengers and paid ancillaries.
2. Select one passenger for return-date change.
3. Display passenger-level eligibility and split requirement.
4. Generate split quote.
5. Generate change quote for new itinerary.
6. Show ancillary impact: seat reselect/refund, baggage retained/repriced, meal reselect.
7. Confirm change using idempotency key.
8. Show updated order timeline and audit events.
9. Generate refund quote for a separate cancellation scenario.
10. Show observability dashboard with latency, error rate, and assisted handoff.

## PI Metrics

| Metric | Baseline assumption | PI target |
|---|---:|---:|
| Self-service change completion | Current baseline to be measured | +20% after MVP cohort rollout |
| Assisted handoff caused by unclear ancillary impact | Current baseline to be measured | -15% |
| Change quote abandonment | Current baseline to be measured | -10% |
| Ancillary retention after change | Current baseline to be measured | +12% |
| P95 order retrieve latency | TBD | <1.5 sec |
| P95 change quote latency | TBD | <3 sec |
| Order servicing API error rate | TBD | <1% |
| Duplicate write incidents | TBD | 0 critical incidents |
