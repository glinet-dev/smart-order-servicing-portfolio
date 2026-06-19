# NFRs and Technical Debt

## Disclaimer

This is a public portfolio simulation. Aureon Air and Nexora Airways are fictional placeholders used for anonymized airline retailing analysis. No internal airline data is used.

## NFR Principles

Order servicing is financially and operationally sensitive. A customer may be changing a live booking, paying fare difference, requesting refund, or preserving paid ancillaries. Therefore, NFRs are not secondary requirements; they are part of product value.

## Non-Functional Requirements

| NFR | Target | Product rationale |
|---|---:|---|
| Order retrieve latency | P95 < 1.5 sec | Manage-booking must feel fast and trustworthy |
| Servicing eligibility latency | P95 < 2 sec | Eligibility should not feel like a support handoff |
| Change quote latency | P95 < 3 sec | Slow repricing increases abandonment |
| Refund quote latency | P95 < 3 sec | Customer needs fast clarity before cancellation |
| Order retrieve availability | 99.9% | Servicing is business critical before travel |
| Write-operation reliability | 99.9% for eligible confirmations | Failed confirmations create payment and support risk |
| Idempotency | Required for split, change, cancel, refund, and payment operations | Prevents duplicate servicing, charges, and refunds |
| Authorization | RBAC and channel-permission checks on all order access | Protects passenger and seller data |
| PII protection | Masking, encryption at rest/in transit, least-privilege access | Protects passenger identity and payment-adjacent data |
| Auditability | Immutable order timeline for quote, confirmation, payment, refund, and error events | Enables support, finance, compliance, and dispute handling |
| Observability | Correlation ID, structured logs, metrics, traces, alerts | Enables production support and partner troubleshooting |
| API rate limiting | Per channel and partner | Protects reliability during traffic spikes and faulty integrations |
| Structured errors | Standard code, message, reason, retryability, next action | Improves direct UX and seller integration quality |
| Accessibility | WCAG-aligned UX for servicing screens | Complex servicing decisions must be understandable to all users |
| Data retention | Configurable by jurisdiction and business policy | Supports compliance and audit requirements |

## Standard Error Model

```json
{
  "errorCode": "SERVICING_NOT_ALLOWED",
  "message": "This order cannot be changed online.",
  "reasonCode": "CHANNEL_PERMISSION_RESTRICTED",
  "retryable": false,
  "nextBestAction": "CONTACT_SUPPORT_WITH_CONTEXT",
  "correlationId": "corr_12345"
}
```

## Technical Debt Backlog

| Debt ID | Item | Impact | Priority | Proposed action | NFR link |
|---|---|---|---|---|---|
| TD-001 | Missing idempotency standard across write APIs | Duplicate changes, payment captures, or refunds | High | Create platform idempotency contract and enforce keys on write endpoints | Idempotency, reliability |
| TD-002 | Inconsistent error response format | Seller integrations and UI fallback behavior become inconsistent | High | Standardize error schema across direct and seller APIs | Structured errors |
| TD-003 | Limited audit history for servicing actions | Support and finance cannot explain order changes confidently | High | Add immutable order timeline events | Auditability |
| TD-004 | Ancillary rules hardcoded in channel UX | Different channels may show different seat/baggage behavior | High | Move rules to central ancillary continuity service | Consistency, maintainability |
| TD-005 | No synthetic monitoring for partner APIs | Incidents are found late by partners or customers | Medium | Add synthetic probes and partner-specific dashboards | Observability |
| TD-006 | Manual refund reconciliation for ancillaries | Refund delays and customer complaints | Medium | Add refund quote line items and refund status tracking | Auditability, finance accuracy |
| TD-007 | No quote expiry enforcement in some flows | Customer may confirm stale price | High | Enforce quote IDs, expiry checks, and reprice path | Reliability, pricing integrity |
| TD-008 | Limited test coverage for passenger split edge cases | Production defects in family/group bookings | Medium | Add contract, integration, and regression test scenarios | Quality |
| TD-009 | Partner permission rules duplicated by channel | Servicing behavior varies across seller types | Medium | Centralize channel permission policy | Security, distribution parity |
| TD-010 | Observability lacks business-level metrics | Engineering can see errors but Product cannot see value leakage | Medium | Add business events for quote started, quote abandoned, confirmation success, and assisted handoff | Analytics |

## Quality Gates

A release candidate cannot move to production unless:

- All P0 stories pass acceptance criteria.
- Write APIs pass idempotency tests.
- Error schema is consistent across impacted endpoints.
- P95 latency targets are met in performance test environment.
- Security review is complete for PII and channel authorization.
- Audit events are verified for quote, confirmation, payment, refund, and failure paths.
- Dashboards show latency, success rate, error rate, quote abandonment, and assisted-service handoff.
- Support runbook is ready for failed quote, failed confirmation, payment failure, and refund escalation.

## Observability Events

| Event | Trigger | Key properties |
|---|---|---|
| `order_retrieved` | Order retrieved successfully | orderId, channel, partnerId, latencyMs, correlationId |
| `servicing_eligibility_checked` | Eligibility check completed | allowedActions, blockedActions, reasonCodes, correlationId |
| `split_quote_created` | Split quote generated | selectedPassengers, impactedItems, quoteExpiry, correlationId |
| `change_quote_created` | Change quote generated | oldItinerary, newItinerary, totalDue, totalRefund, ancillaryImpact, correlationId |
| `ancillary_continuity_checked` | Ancillary continuity evaluated | itemType, state, refundEligible, reselectRequired, correlationId |
| `order_change_confirmed` | Change confirmed | quoteId, paymentImpact, refundImpact, channel, correlationId |
| `refund_quote_created` | Refund quote generated | ticketRefund, ancillaryRefund, expiry, correlationId |
| `servicing_assisted_handoff` | User routed to support | reasonCode, contextCaptured, channel, correlationId |
