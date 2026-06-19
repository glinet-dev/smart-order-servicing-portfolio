# Smart Order Servicing and Ancillary Continuity

**Public Airline Retailing Product Owner Case Study**

## Disclaimer

This is a public portfolio simulation based on publicly observable airline retailing patterns and industry concepts. It does **not** use internal airline data, confidential information, or claim employment experience with any airline. **Aureon Air** and **Nexora Airways** are fictional placeholders used for anonymized benchmarking.

## 1. Executive Summary

Aureon Air is used as the focus airline persona: a premium global hub carrier with mature direct booking, manage-booking, and ancillary retailing capabilities. Nexora Airways is used as the competitor benchmark persona: a premium network competitor with strong online trip management and distribution-facing servicing.

The core opportunity is not basic online booking. The larger product opportunity is post-purchase confidence: when customers change, split, cancel, or refund an order, they need a clear explanation of what happens to the flight, passenger, payment, refund, and every ancillary item.

The proposed solution is a **Smart Order Servicing and Ancillary Continuity Hub**. It gives customers, agents, and seller channels a consistent order-aware servicing experience across direct and partner distribution.

## 2. Product Context

Airline retailing is moving from fragmented shopping, booking, ticketing, and servicing records toward a more modern Offer-and-Order model. IATA describes Modern Airline Retailing as a move toward personalization, efficiency, and seamless experiences across channels, supported by NDC, ONE Order, and related standards.

This case study applies that direction to a practical Product Owner problem: how to make order servicing understandable, reliable, and commercially effective when ancillaries are attached to the booking.

## 3. Anonymized Public Benchmark Snapshot

| Area | Aureon Air pattern | Nexora Airways pattern | Product implication |
|---|---|---|---|
| Offer and booking | Supports online flight search, fare comparison, cabin choice, passenger count, promo-code style inputs, fare-family visibility, and add-on exposure during booking | Promotes online booking, offers, trip management, fare options, and digital support across web and app | The shopping stage is already strong. Differentiation comes from confidence after purchase. |
| Manage booking | Allows booking retrieval, itinerary review, contact update, seat selection, upgrades, meals, extra baggage, and eligible changes | Emphasizes self-service trip management, add-ons, seat choice, upgrades, meals, support, and notifications | Customers expect “control my trip” to include clear servicing outcomes, not just access to the booking. |
| Ancillary retailing | Offers paid or included seats, extra baggage, meal choices, upgrades, and other journey extras depending on route, fare, cabin, and eligibility | Offers a broad add-on journey, including seats, baggage, lounge-like services, connectivity, assistance, and trip extras | Selling extras is mature. Retaining or refunding extras during servicing is the product gap. |
| Change, cancel, refund | Passenger-level changes, split handling, changed sectors, and paid extras can create unclear outcomes for customers | Competitor-style servicing suggests a need for seller-visible order servicing and ancillary handling | The user needs a pre-confirmation impact summary before changing or cancelling. |
| Distribution | Direct channels, agents, partner sellers, and API-style integrations require consistent offer and servicing behavior | Distribution-facing capabilities increasingly include order retrieve, servicing, ancillaries, and structured support | A scalable product must support direct and indirect servicing parity. |

## 4. User Problem

Customers can complete the initial booking and add paid extras, but confusion increases when the order needs servicing.

A customer may ask:

- Can I change only one passenger in a multi-passenger booking?
- Will my paid seat move to the new flight?
- If the same seat is unavailable, will I get an equivalent seat or refund?
- Will extra baggage transfer to the new flight?
- Will meal preferences remain after a sector change?
- Are paid extras refunded together with the ticket or separately?
- Why does my travel seller see a different servicing option than the airline website?

The real problem is not only the change or refund transaction. The real problem is **lack of a single, order-aware explanation of the impact before the customer confirms the servicing action.**

## 5. Target Personas

| Persona | Need | Pain point |
|---|---|---|
| Family traveler | Change one passenger without disrupting the rest of the booking | Worried about changing the whole group accidentally |
| Business traveler | Change quickly and retain seat, baggage, and meal preferences | Needs speed and certainty |
| Travel agent | Service the order on behalf of the customer | Needs consistent API rules and actionable error messages |
| Contact center agent | Resolve servicing issues efficiently | Needs complete order history and ancillary state |
| Commercial stakeholder | Protect revenue and customer trust | Needs ancillary retention and lower servicing abandonment |
| Finance/refund stakeholder | Ensure accurate refunds and audit | Needs traceable payment and refund decisions |
| Engineering team | Build reliable servicing capabilities | Needs clear APIs, NFRs, dependencies, and error contracts |

## 6. Realistic Scenario

A family of four books a long-haul journey through a hub. During booking, they choose paid preferred seats, add extra baggage, and select meals. Later, one passenger needs to return on a different date.

The customer enters manage booking and discovers that the servicing journey is not a simple “change flight” decision. The system must answer several questions before confirmation:

| Item | Current state | Possible after-change state | Required product behavior |
|---|---|---|---|
| Passenger | Four passengers in one order | One passenger changes date | Show passenger-level eligibility and split impact |
| Flight | Original return sector | New return sector | Quote fare, tax, and fee impact |
| Seat | Paid preferred seat | Same seat may be unavailable | Transfer, reselect, reprice, or refund |
| Baggage | Extra baggage purchased | May be transferable depending on rules | Show retained, repriced, or refund-required state |
| Meal | Meal selected for original sector | May need reselection | Prompt user before confirmation |
| Payment | Original payment captured | Additional collection or refund may apply | Show total payable/refundable amount |
| Audit | Existing order history | New servicing event | Record quote, decision, confirmation, and failures |

## 7. Product Vision

Create a unified servicing experience where eligible customers and sellers can retrieve an order, understand passenger-level servicing options, view ancillary impact, receive a clear payment/refund quote, and confirm or route the request to assisted service with complete context.

## 8. Product Goal

Enable eligible customers, agents, and sellers to complete order servicing with clear passenger, payment, refund, and ancillary impact before confirmation.

## 9. Proposed Solution

### Smart Order Servicing and Ancillary Continuity Hub

| Capability | Description |
|---|---|
| Unified order view | Shows flights, passengers, fare brand, ticket/order state, payment reference, ancillary items, channel of sale, and servicing eligibility |
| Passenger-level servicing | Allows the user to check whether one passenger, selected passengers, or all passengers can be changed |
| Split workflow | Quotes and confirms order split when passenger-level servicing requires separation |
| Change quote engine | Reprices selected itinerary and shows fare difference, penalties, taxes, and payment/refund impact |
| Ancillary continuity engine | Determines whether seats, baggage, meals, lounge, Wi-Fi, assistance, or other extras are transferred, repriced, reselected, or refunded |
| Refund quote engine | Shows ticket refund and ancillary refund separately, including reason codes |
| Distribution servicing API | Gives seller channels consistent order retrieve, eligibility, quote, and confirmation behavior |
| Audit and observability | Tracks quotes, confirmations, payment/refund events, errors, partner requests, and order timeline |

## 10. MVP Scope

### In Scope

1. Retrieve order with complete flight, passenger, fare, payment, and ancillary state.
2. Show servicing eligibility at passenger and order level.
3. Generate split quote for eligible multi-passenger orders.
4. Generate change quote for eligible direct and seller-channel bookings.
5. Identify whether ancillaries can be retained, repriced, reselected, or refunded.
6. Generate refund quote for ticket and paid extras.
7. Provide API contracts for direct, agent, and seller channels.
8. Add observability for quote failures, confirmation failures, refund failures, latency, and ancillary mismatch.

### Out of Scope for MVP

1. Full disruption recovery automation.
2. Complex interline settlement.
3. Full loyalty redemption servicing.
4. Airport day-of-departure exception handling.
5. Complete global regulatory refund automation.
6. All partner-operated ancillary servicing cases.

## 11. Offer, Order, Ancillary, and Distribution Flow

```mermaid
flowchart LR
    A[Search / AirShopping] --> B[Offer Creation]
    B --> C[Offer Pricing]
    C --> D[Ancillary Offer]
    D --> E[Order Creation]
    E --> F[Payment and Fulfilment]
    F --> G[Order Retrieve]
    G --> H[Servicing Eligibility]
    H --> I[Split / Change / Refund Quote]
    I --> J[Ancillary Continuity Check]
    J --> K[Customer or Seller Confirmation]
    K --> L[Updated Order]
    L --> M[Audit Event and Analytics]
```

## 12. Epics

| Epic ID | Epic | Outcome |
|---|---|---|
| SOS-EPIC-01 | Unified Order Retrieve | Customer, agent, or seller can view complete order and ancillary state |
| SOS-EPIC-02 | Passenger-Level Servicing Eligibility | User understands whether one passenger, selected passengers, or all passengers can be changed |
| SOS-EPIC-03 | Split Order Workflow | Eligible passenger split can be quoted and confirmed safely |
| SOS-EPIC-04 | Change Quote and Repricing | User can view fare difference, penalties, taxes, and total payable/refundable amount |
| SOS-EPIC-05 | Ancillary Continuity | Paid seats, baggage, meals, and other extras are transferred, reselected, repriced, or refunded |
| SOS-EPIC-06 | Refund Quote and Cancellation | Customer can see ticket and ancillary refund estimate before cancellation |
| SOS-EPIC-07 | Distribution API and Observability | Seller channels receive consistent servicing APIs with measurable reliability |

## 13. Prioritized MVP Features

| Feature | Priority | Rationale |
|---|---|---|
| Unified order retrieve | Must have | Foundation for all servicing flows |
| Passenger-level eligibility | Must have | Directly addresses multi-passenger booking confusion |
| Change quote with ancillary impact | Must have | Highest customer and commercial value |
| Idempotent confirmation | Must have | Prevents duplicate servicing and duplicate payment/refund risk |
| Refund quote for extras | Should have | Reduces customer confusion and support contacts |
| Seller servicing API | Should have | Enables distribution-channel consistency |
| Advanced partner-operated servicing | Later | High complexity and dependency risk |

## 14. Product Metrics with Targets

| Metric | Type | Example target |
|---|---|---:|
| Self-service change completion rate | Customer experience | +20% |
| Assisted-service deflection for eligible changes | Operational | +15% |
| Contact center contacts for seat/baggage confusion | Operational | -15% |
| Change quote abandonment rate | Funnel | -10% |
| Ancillary retention after itinerary change | Commercial | +12% |
| Refund quote accuracy | Finance/operations | >98% final-to-quote alignment for eligible cases |
| Order servicing API error rate | Technical | <1% |
| P95 order retrieve latency | Technical | <1.5 sec |
| P95 change quote latency | Technical | <3 sec |
| Duplicate payment/refund incidents from retries | Risk | 0 critical incidents |

## 15. PSPO / Scrum Alignment

| PSPO / Scrum concept | Application in this case study |
|---|---|
| Product value maximization | Prioritize reduced servicing confusion, higher self-service completion, ancillary retention, and lower support effort |
| Product Goal | Enable clear order servicing with passenger, payment, refund, and ancillary impact before confirmation |
| Product Backlog ownership | Maintain epics, stories, acceptance criteria, NFRs, risks, dependencies, and ordering |
| Transparency | Make eligibility, quote expiry, refund amount, and ancillary state visible to customers and stakeholders |
| Inspection | Review quote abandonment, failed confirmations, refund escalations, and seller API errors during Sprint Reviews |
| Adaptation | Adjust UX, rules, backlog priority, and API behavior based on feedback and production telemetry |
| Definition of Ready | Ensure value, acceptance criteria, dependencies, API impact, NFR impact, and test scenarios are clear before sprint commitment |
| Definition of Done | Release only when functional, API, NFR, security, audit, observability, and regression checks are complete |
| Scrum values | Use focus, openness, respect, courage, and commitment to expose complexity and deliver value incrementally |

## 16. SAFe Principles Applied

| SAFe principle | Application |
|---|---|
| 1. Take an economic view | Prioritize change quote, ancillary continuity, and refund clarity because they reduce support cost and protect ancillary revenue |
| 2. Apply systems thinking | Treat servicing as a system across web, app, agents, sellers, pricing, inventory, payment, refunds, and audit |
| 3. Assume variability; preserve options | Build flexible adapters for pricing, inventory, ancillary rules, payments, and seller permissions |
| 4. Build incrementally with fast learning cycles | Start with order retrieve and eligibility, then add split, change quote, continuity, refund, and seller APIs |
| 5. Base milestones on objective evaluation | Use demos showing real order retrieve, quote, continuity check, and audit event creation |
| 6. Make value flow without interruptions | Reduce handoffs between booking, servicing, payment, refund, and support teams |
| 7. Apply cadence and synchronize planning | Use PI Planning to align pricing, payments, order, ancillary, seller, security, and operations dependencies |
| 8. Unlock intrinsic motivation | Give squads ownership of outcomes such as “reduce ancillary confusion” rather than component tasks only |
| 9. Decentralize decision-making | Let teams decide API and UX implementation details while centralizing fare, finance, compliance, and refund policy decisions |
| 10. Organize around value | Organize around “Manage My Trip / Order Servicing” rather than separate seat, baggage, refund, and payment silos |

## 17. Key Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Passenger split is complex in legacy booking/order systems | High | Start with eligibility and assisted routing, then automate eligible cases incrementally |
| Ancillary rules vary by route, cabin, aircraft, fare, channel, and passenger type | High | Use rules-driven continuity engine with clear fallback states |
| Refund quote may differ from final refund | Medium | Mark quote as estimate until final validation and show confidence/expiry |
| Seller channel may not have authority to service every order | High | Include channel permissions and next-best-action in every API response |
| Retry behavior may duplicate change/payment/refund actions | High | Make all write operations idempotent |
| Complex rules may confuse users | High | Use plain-language summaries and explicit “before you confirm” screens |

## 18. Final Narrative

Aureon Air and Nexora Airways are anonymized premium hub-carrier personas used to study airline retailing patterns without naming real carriers. Both personas represent mature digital booking and ancillary selling capabilities. The strategic opportunity is to move beyond selling rich offers into servicing rich orders.

When a customer changes an itinerary, the product must explain not only the new fare but also what happens to every order item: seat, baggage, meal, lounge, connectivity, assistance, payment, refund, and passenger-level eligibility.

The Smart Order Servicing and Ancillary Continuity Hub addresses this by creating a unified order view, eligibility engine, split workflow, change quote, refund quote, ancillary continuity logic, seller-facing API, and full observability. As a Technical Product Owner artifact, the case study demonstrates domain learning, structured backlog ownership, acceptance criteria, OpenAPI thinking, NFR accountability, technical debt management, PI planning, product metrics, and SAFe/PSPO alignment.

## 19. References for Industry and Delivery Concepts

- IATA Airline Retailing: https://www.iata.org/en/programs/airline-distribution/retailing/
- Scrum Guide: https://scrumguides.org/scrum-guide.html
- SAFe Lean-Agile Principles: https://framework.scaledagile.com/safe-lean-agile-principles/
- SAFe Product Owner: https://framework.scaledagile.com/product-owner/
