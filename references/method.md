# The IDesign Method — Method Reference

This reference carries the volatility-based decomposition method in depth. It is the companion to `rules.md` (the canonical rule catalog) and `plan-template.md` (the output document). Quotes are verbatim from the cited sources. Source locations refer to Juval Löwy's *Righting Software* (Addison-Wesley, 2019) Kindle locations unless noted.

## The Prime Directive

Never design against the requirements. Requirements change constantly. An architecture that mirrors the requirements breaks when they change.

> "This is more than a statement; it is the design *Prime Directive*. It is the only way to handle the unavoidable and highly welcomed changes to the requirements… Any attempt at designing against the requirements will always guarantee pain, because when the requirements change, so will your design… Designing against the requirements guarantees the lack of ability to quickly respond to changes."

— Juval Löwy, InfoQ Q&A, February 2020

Functional decomposition is the canonical anti-pattern. It pollutes clients with business logic, prevents reuse, prevents a single point of entry, and makes services too big or too small.

## Volatility and Variability

> "Not everything that is variable is also volatile. You resort to encapsulating a volatility at the system design level only when it is open-ended and, unless encapsulated in a component of the architecture, would be very expensive to contain."

— *Righting Software*, loc. 1237

Variability means bounded change. Conditional logic handles it. Volatility is open-ended change. Encapsulate it behind an architectural boundary.

The official IDesign formulation:

> "The Method prescribes designing the system based on volatility – identifying areas of change in the system, and encapsulating these in components. The required behaviors of the system are the integrations of these components. Now when the requirements change, the changes are contained and are not spread across the architecture and the existing software. Conceptually, the architecture is a series of vaults, where each of the vaults (as a component of the architecture) encapsulates some volatility."

— IDesign Method Management Overview, idesign.net, February 2020

The vaults metaphor from the book:

> "you start thinking of your system as a series of vaults… With volatility-based decomposition, you open the door of the appropriate vault, toss the grenade inside, and close the door."

— *Righting Software*, Ch. 2

## Core Use Cases

Requirements capture behavior, not functionality.

> "Requirements should capture the required behavior rather than the required functionality."

— *Righting Software*, loc. 1585

The canonical count:

> "Most systems have as few as two or three core use cases, and the number seldom exceeds six."

— *Righting Software*, loc. 2193

Training variants exist. The Architect's Master Class teaches three to five, per a published alumnus account. Löwy's 2010 deck says four to six. Use the book's two to six range.

A core use case is an abstraction:

> "A core use case will almost always be some kind of an abstraction of other use cases, and it may even require a new term or name to differentiate it from the rest."

— *Righting Software*, loc. 2199

The core use cases are rarely explicit in the requirements document. Identify them through requirements analysis and stakeholder interviews. Time spent identifying core use cases and volatility is requirements analysis, not design.

## The Two Axes of Volatility

> "There is a simple technique I call **axes of volatility**. This technique examines the ways the system is used by customers... In any business, there are only two ways your system could face change: the first axis is at the same customer over time... The second way change could come is at the same time across customers."

— *Righting Software*, loc. 1243–1253

The axes must be independent:

> "Almost always, the axes should be independent... If areas of change cannot be isolated to one of the axes, it often indicates a functional decomposition in disguise."

— *Righting Software*, loc. 1270

## The Volatilities List

The artifact is a list, not a scored table:

> "Prior to decomposing a system and creating an architecture, you should simply compile a list of the candidate areas of volatility as a natural part of requirements gathering and analysis... Ask what could change along the axes of volatility... Do not commit yet to the actual design."

— *Righting Software*, loc. 1333

No scoring or ranking artifact exists in any public canonical source. Identify volatility qualitatively through the axes, interviews, solution scrubbing, the competitor test, and the longevity heuristic.

Volatility decreases top-down across the layers. Clients are the most volatile. Resources are the least volatile. Reuse increases top-down.

## Decomposition

> "Decompose based on volatility. Volatility-based decomposition identifies areas of potential change and encapsulates those into services or system building blocks. You then implement the required behavior as the interaction between the encapsulated areas of volatility."

— *Righting Software*, loc. 1091

The mapping is rarely one to one:

> "Sometimes a single component can encapsulate more than one area of volatility. Some areas of volatility may not be mapped directly to a component but rather to an operational concept such as queuing or publishing an event. At other times, the volatility of an area may be encapsulated in a third-party service."

— *Righting Software*, Ch. 2, "System Decomposition"

Design for your competitor too. Do not encapsulate the nature of the business. If all competitors do something identically, it is probably business nature. If competitors diverge, it is likely volatile.

## Component Taxonomy

Six component types:

| Type | Definition |
|---|---|
| Client | The client layer. End-user applications or other systems. Encapsulates client technology volatility. Advocates a single point of entry. |
| Manager | Encapsulates volatility in the sequence of use cases and workflows. A collection of related use cases. |
| Engine | Encapsulates volatility in business rules and activities. A Manager may use zero or more Engines. Engines may be shared between Managers. |
| Resource Access | Encapsulates volatility in accessing a resource. Exposes atomic business verbs, not CRUD. |
| Resource | The actual physical resources: a database, a file system, a cache, a message queue. The database is a resource. |
| Utility | Common infrastructure to all services. Security, diagnostics, logging, pub/sub, hosting, message bus. |

The book structures the system as four layers plus a Utilities bar:

> "The Method calls for four layers in the system architecture" — client layer, business logic layer (Managers + Engines), resource access layer, resource layer.

— *Righting Software*, loc. 1651

The Utilities bar:

> "a vertical bar on the side of the layers... allowing any component in the architecture to use any Utility"

— *Righting Software*, loc. 2016

The five-type teaching variant (Client, Manager, Engine, Resource Access, Utility) is what the Architect's Master Class teaches as "exactly 5 types of services". It merges Resource elsewhere. The six-type model is the book's model.

## Naming Convention

> "Names of services must be two-part compound words written in Pascal case. The suffix of the name is always the service's type—for example, Manager, Engine, or Access."

— *Righting Software*, loc. 1757

Examples: `TradesAccess`, `TradeWorkflow`, `MembershipManager`.

## The Minimal Set

> "Your mission as an architect is to identify the smallest set of components that you can put together to satisfy all the core use cases."

— *Righting Software*, loc. 2205

> "Once you can produce an interaction between your services for each core use case, you have produced a valid design."

— *Righting Software*, loc. 2222

The order-of-magnitude target:

> "The smallest set of services required in a typical software system contains 10 services in order of magnitude (e.g., sets of both 12 and 20 are on the order of 10)."

— *Righting Software*, loc. 2270

The composition:

> "Using The Method, even in a large system you are commonly looking at two to five Managers, two to three Engines, three to eight ResourceAccess and Resources, and a half-dozen Utilities. The total number of building blocks will be a dozen or two at the most."

— *Righting Software*, loc. 2276

Red flags:

> "If your system has eight Managers, then you have already failed to produce a good design: The large number of Managers strongly indicates you have done a functional or domain decomposition."

— *Righting Software*, loc. 1809

> "If your design contains a large number of Engines, you may have inadvertently done a functional decomposition."

— *Righting Software*, loc. 1803

## Layering

Closed architecture is the default. Components call only the adjacent lower layer.

> "A semi-closed/semi-open architecture allows calling more than one layer down."

— *Righting Software*, loc. 1997

Justify semi-open relaxation in two cases: infrastructure, and rarely-changed code. State every relaxation in the plan and justify each one.

Manager-to-Manager calls do not violate closure when they go through a queue:

> "While Managers should not call directly sideways to other Managers, a Manager can queue a call to another Manager... the proxy is a ResourceAccess to the underlying Resource, the queue; that is, the call actually goes down, not sideways."

— *Righting Software*, loc. 2046–2051

## Resource Access

Organize Resource Access by volatility area, never by database or API type.

The key abstraction:

> "Referring to the storage as Storage and not as Database."

— Löwy, SDD 2022 deck, slide 44

A Resource Access contract of Select, Insert, Delete betrays the database. A well-designed access exposes atomic business verbs. The book's trading example names Access components per volatility area: Trades Access, Workflow Access, Feed Access, Customers Access.

Map database structures to business-meaningful interfaces. Transform data inside the access. Keep business logic out of the access layer.

## Validation Instruments

### Call chains

> "A call chain demonstrates the interaction between components required to satisfy a particular use case. You can literally superimpose the call chain onto the layered architecture diagram."

— *Righting Software*, loc. 2233

Notation: solid black arrow for synchronous, dashed gray for queued.

### Sequence diagrams

Sequence diagrams are the second validation instrument. The book's case study validates each core use case with sequence diagrams.

### Symmetry

> "all good architectures are symmetric... symmetry manifests in repeated call patterns across use cases. You should expect symmetry, and its absence is a cause for concern."

— *Righting Software*, loc. 2119–2124

### Design Don'ts

The classic formulation (2010-era deck, WCF-era):

- Never queue calls to Engines.
- Never queue calls to Resource Access.
- Engines never call each other.
- Resource Access never calls each other.
- Engines and Resource Access do not publish or subscribe to events.
- Clients should not call multiple Managers in a single use case.

The book softens some of these in the "Relaxing the Rules" section and its message-bus-first example. Treat the classic list as the default, the book's relaxations as justified exceptions.

### Vertical slice and stress testing

IDesign's System Design service practice complements the architecture with a vertical slice of the proposed system and stress tests the slice. This eliminates design and technology from the risk list. Validation happens early: ideally one week into the project you must know whether the architecture holds water.

## Sources

- Löwy, Juval. *Righting Software*. Addison-Wesley, 2019. Kindle locations cited inline.
- IDesign Method Management Overview. idesign.net, February 2020.
- SDD 2022 presentation, "Righting Software". sddvault.s3.amazonaws.com.
- 2010-era deck, "Typical Layers" and "Architecture Validation" slides.
- InfoQ Q&A with Löwy, February 2020. infoq.com/articles/book-review-righting-software.
- IDesign System Design service page. idesign.net/Services/System-Design.
- IDesign Architect's Master Class outline. idesign.net/Training/Architect-Master-Class.

The IDesign Design Standard and The IDesign Method whitepapers are email-gated downloads. Some named rules may live there. They are not cited here because they were not accessible.
