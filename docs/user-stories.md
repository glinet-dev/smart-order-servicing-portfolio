# User Stories, Definition of Ready, and Definition of Done

## Disclaimer

This is a public portfolio simulation. Aureon Air and Nexora Airways are fictional placeholders used for anonymized airline retailing analysis. No internal airline data is used.

## Product Goal

Enable eligible customers, agents, and sellers to complete order servicing with clear passenger, payment, refund, and ancillary impact before confirmation.

## Definition of Ready

A Product Backlog Item is ready for Sprint Planning when:

- Business value is clear.
- Target user or stakeholder is identified.
- Acceptance criteria are written and testable.
- API impact is identified.
- NFR impact is reviewed.
- Dependencies are known or explicitly marked as risks.
- Test scenarios are understood.
- UX/content requirements are clear.
- Analytics events are defined where relevant.
- Edge cases and fallback behavior are discussed.
- Compliance, security, privacy, or payment implications are identified.

## Definition of Done

A story is done only when:

- Functional acceptance criteria pass.
- API contract is documented and versioned where applicable.
- Structured error responses and reason codes are implemented.
- Security and authorization checks are validated.
- PII handling is reviewed.
- Audit events are created.
- Observability captures success, failure, latency, and correlation ID.
- Performance targets are tested for the impacted flow.
- UX content is reviewed for clarity.
- Regression tests cover direct and seller-channel scenarios where applicable.
- Product analytics events are implemented.
- Support/runbook notes are updated for operational flows.

## Epics

| Epic ID | Epic | Outcome |
|---|---|---|
| SOS-EPIC-01 | Unified Order Retrieve | Customer, agent, or seller can view complete order and ancillary state |
| SOS-EPIC-02 | Passenger-Level Servicing Eligibility | User understands whether one passenger, selected passengers, or all passengers can be changed |
| SOS-EPIC-03 | Split Order Workflow | Eligible passenger split can be quoted and confirmed safely |
| SOS-EPIC-04 | Change Quote and Repricing | User can view fare difference, penalties, taxes, and total payable/refundable amount |
| SOS-EPIC-05 | Ancillary Continuity | Paid seats, baggage, meals, and other extras are transferred, reselected, repriced, or refunded |
| SOS-EPIC-06 | Refund Quote and Cancellation | Customer can see ticket and ancillary refund estimate before cancellation |
| SOS-EPIC-07 | Distribution API and Observability | Seller channels receive consistent servicing APIs with measurable reliability |

## Detailed User Stories

### SOS-101 - Retrieve complete order

**Epic:** SOS-EPIC-01  
**Priority:** P0  
**Story points:** 5

As a traveler, agent, or seller, I want to retrieve the complete order, so that I can understand itinerary, passengers, fare, payment, and purchased ancillaries before servicing.

**Acceptance criteria**

- Given a valid order ID and authorized user context, when the order is retrieved, then itinerary, passengers, fare, payment status, and order items are returned.
- The response includes seats, baggage, meals, and other ancillaries with state: fulfilled, pending, refundable, transferable, non-transferable, or requires reselection.
- Unauthorized users cannot retrieve another passenger's order.
- The response includes correlation ID, channel, and audit reference.

### SOS-102 - Authenticate order access

**Epic:** SOS-EPIC-01  
**Priority:** P0  
**Story points:** 3

As a platform owner, I want secure access control for order retrieval, so that passenger and payment-adjacent information is protected.

**Acceptance criteria**

- Direct customers must pass order reference and passenger validation or authenticated profile validation.
- Agents and sellers must pass channel authorization and servicing permission checks.
- Failed authorization returns a structured error without exposing PII.
- All failed access attempts are logged with reason code and correlation ID.

### SOS-103 - Show servicing eligibility

**Epic:** SOS-EPIC-02  
**Priority:** P0  
**Story points:** 5

As a traveler or seller, I want to know which servicing actions are available, so that I do not start a change, cancellation, or refund flow that cannot be completed.

**Acceptance criteria**

- Eligibility response lists allowed, blocked, and assisted-service actions.
- Each blocked action includes a reason code and next best action.
- Response includes eligibility at order level and passenger level.
- Eligibility checks consider fare rules, order status, channel permissions, payment status, and departure timing.

### SOS-104 - Select passenger-level change

**Epic:** SOS-EPIC-02  
**Priority:** P0  
**Story points:** 5

As a traveler with a multi-passenger order, I want to select one or more passengers for a change, so that I do not accidentally change the full group itinerary.

**Acceptance criteria**

- The user can select one passenger, multiple passengers, or all passengers.
- The system explains whether a split is required before change confirmation.
- If online passenger-level change is not available, the system creates an assisted-service handoff with context.
- The UI clearly states which passengers will remain unchanged.

### SOS-105 - Generate split quote

**Epic:** SOS-EPIC-03  
**Priority:** P1  
**Story points:** 8

As a traveler or seller, I want to see the impact of splitting selected passengers, so that I understand what will happen before continuing with a passenger-level change.

**Acceptance criteria**

- Split quote shows selected passengers, remaining passengers, impacted order items, and downstream servicing actions.
- Quote includes warnings for shared ancillaries, payment references, or special service requests.
- Quote has expiry timestamp.
- Quote generation failure returns structured reason and assisted-service route.

### SOS-106 - Confirm split idempotently

**Epic:** SOS-EPIC-03  
**Priority:** P1  
**Story points:** 8

As a platform owner, I want split confirmation to be idempotent, so that retries do not create duplicate order splits.

**Acceptance criteria**

- Split confirmation requires idempotency key.
- Duplicate retry with same idempotency key returns original result.
- Original and split order IDs are linked in the timeline.
- Audit event records selected passengers, channel, user context, and timestamp.

### SOS-107 - Search alternative flights for change

**Epic:** SOS-EPIC-04  
**Priority:** P0  
**Story points:** 5

As a traveler, I want to view available alternative flights for an eligible change, so that I can choose a new itinerary.

**Acceptance criteria**

- Alternative search uses original order context and selected passengers.
- Results show flight times, cabin, fare-family compatibility, and indicative price impact.
- Unavailable options are not shown as selectable.
- Search results expire according to offer/quote rules.

### SOS-108 - Generate change quote

**Epic:** SOS-EPIC-04  
**Priority:** P0  
**Story points:** 8

As a traveler or seller, I want a complete change quote, so that I can understand fare difference, penalties, taxes, and total payable or refundable amount.

**Acceptance criteria**

- Quote includes old itinerary, new itinerary, fare difference, fee, tax difference, and total amount due/refundable.
- Quote includes selected passengers and quote expiry.
- Quote includes ancillary impact summary.
- Quote cannot be confirmed after expiry.

### SOS-109 - Confirm change idempotently

**Epic:** SOS-EPIC-04  
**Priority:** P0  
**Story points:** 8

As a platform owner, I want change confirmation to be idempotent, so that duplicate retries do not create duplicate charges, refunds, or order updates.

**Acceptance criteria**

- Confirmation requires valid change quote ID and idempotency key.
- Duplicate confirmation attempts return existing confirmed result.
- Payment collection or refund initiation is linked to the servicing transaction.
- Order timeline records old itinerary, new itinerary, payment impact, ancillary impact, user/channel, and timestamp.

### SOS-110 - Check paid seat continuity

**Epic:** SOS-EPIC-05  
**Priority:** P0  
**Story points:** 5

As a traveler, I want to know what happens to my paid seat after a change, so that I can retain, reselect, or refund it.

**Acceptance criteria**

- Continuity check identifies whether the same seat is available on the new sector.
- If unavailable, the user sees equivalent seat options or refund eligibility.
- Seat price difference is shown where repricing applies.
- Final confirmation records seat state: retained, reselected, repriced, refund requested, or forfeited by rule.

### SOS-111 - Check extra baggage continuity

**Epic:** SOS-EPIC-05  
**Priority:** P1  
**Story points:** 5

As a traveler, I want to know whether extra baggage moves to my new flight, so that I do not lose paid baggage unexpectedly.

**Acceptance criteria**

- Continuity check evaluates route, cabin, passenger, fare, and timing rules.
- Baggage state is shown as retained, repriced, not transferable, or refund eligible.
- If additional collection is required, amount is included in the change quote.
- If refund is required, refund quote includes baggage line item separately.

### SOS-112 - Prompt meal reselection

**Epic:** SOS-EPIC-05  
**Priority:** P2  
**Story points:** 3

As a traveler, I want to know when meal preferences need reselection, so that my changed journey still reflects my preferences.

**Acceptance criteria**

- If meal cannot be retained after sector change, the user is prompted to reselect.
- If meal selection window is closed, the user sees a clear reason and support guidance.
- Confirmation summary shows meal retained, reselected, unavailable, or pending.
- Meal status is included in order timeline.

### SOS-113 - Generate refund quote for ticket and extras

**Epic:** SOS-EPIC-06  
**Priority:** P0  
**Story points:** 8

As a traveler, I want ticket refund and ancillary refund shown separately, so that I understand what I will receive before cancelling.

**Acceptance criteria**

- Refund quote displays fare refund, taxes, penalties, and ancillary refund separately.
- Each ancillary line shows refundable, non-refundable, or separate-review status.
- Quote includes estimated refund amount, currency, and expiry.
- Non-refundable items include reason code and plain-language explanation.

### SOS-114 - Cancel eligible order

**Epic:** SOS-EPIC-06  
**Priority:** P1  
**Story points:** 5

As a traveler or seller, I want to cancel an eligible order after reviewing refund impact, so that I can complete cancellation without contacting support.

**Acceptance criteria**

- Cancellation requires valid refund quote ID and idempotency key.
- Successful cancellation updates order status and creates refund request where applicable.
- Failed cancellation returns structured error and next best action.
- Audit trail records cancellation request, user/channel, quote ID, and refund reference.

### SOS-115 - Provide seller servicing API with standard errors

**Epic:** SOS-EPIC-07  
**Priority:** P1  
**Story points:** 8

As a travel seller or OTA partner, I want consistent servicing APIs and error responses, so that I can support customers without switching channels unnecessarily.

**Acceptance criteria**

- Seller API supports order retrieve, eligibility, change quote, ancillary continuity check, refund quote, and cancellation.
- All API responses include correlation ID and structured error schema.
- Partner permissions are applied before servicing actions are returned.
- API telemetry captures partner ID, latency, success, failure, and reason code.

## Notes for GitHub Issues

Import or recreate each story as a GitHub Issue with labels such as:

- `epic:SOS-EPIC-01`
- `priority:P0`
- `domain:order-servicing`
- `nfr:idempotency`
- `channel:direct`
- `channel:seller-api`
- `safe:pi-1`
- `pspo:backlog-item`
