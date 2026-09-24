# Canonical IDesign Design Rules — Catalog and Verification

This is the rule catalog for the volatility-based-architecture skill. Each rule states the canonical definition, its source, and its confidence. The verification table at the end distinguishes canonical rules from teaching variants and from rules that could not be verified in accessible primary sources.

The skill teaches verified substance. Where a rule name is not found in primary sources, the skill teaches the substance and flags the name as unverified. Never present unverified names as canonical.

## Verified Canonical Rules

### 1. The Prime Directive

Never design against the requirements. Requirements change, so the architecture must not mirror them. Functional decomposition is the canonical anti-pattern.
Source: InfoQ Q&A, 2020. Confidence: High.

### 2. Volatility-based decomposition

Design the system based on volatility. Identify areas of change and encapsulate them in components. The architecture is a series of vaults.
Sources: IDesign Method Management Overview, *Righting Software* loc. 1091. Confidence: High.

### 3. Composable design

Find the smallest set of building blocks that satisfies all requirements, present and future, known and unknown. A typical system contains ten services in order of magnitude.
Sources: InfoQ Q&A 2020, *Righting Software* loc. 2205 and 2270. Confidence: High.

### 4. The Method = System Design + Project Design

The Method is a two-part formula: system design plus project design. Project design produces options trading schedule, cost, and risk. There is no single "THE Project".
Sources: IDesign Method Management Overview p.2, InfoQ Q&A. Confidence: High.

### 5. The four tenets of service orientation

- Service boundaries are explicit.
- Services are autonomous. A service needs nothing from its clients. The service operates and versions independently.
- Services share contracts and data schema, not type-specific metadata.
- Services are compatible based on policy.
Source: *Programming WCF Services*, 4th ed., Appendix A. Confidence: High.

### 6. Four contract types

Service contracts (operations), data contracts (data types), fault contracts (errors), message contracts (direct message interaction).
Source: *Programming WCF Services*, Ch. 1. Confidence: High. This corrects the three-type trio that omits message contracts.

### 7. Contract-first design (substance)

Design contracts before implementation. Contracts are the decoupling mechanism. Factoring metrics: strive for three to five members per contract, never more than twenty.
Name note: "contract-first" is the standard industry term. It is not a verbatim IDesign principle label in the accessible sources.
Sources: IDesign Detailed Design Clinic page, AMC outline, WCF Coding Standard. Confidence: High on substance.

### 8. No business logic in storage (substance)

The database is a resource. Only Resource Access services touch it. Refer to storage as Storage, not Database. Resource Access exposes atomic business verbs, not CRUD.
Name note: the name "no data in base" is not found in any accessible primary source. The substance is canonical.
Sources: SDD 2022 deck slides 44 and 57, *Righting Software* loc. 1710–1728. Confidence: High on substance.

### 9. Do not call us, we call you (substance)

Events notify subscribers of occurrences on the publisher side. Event operations are one-way, void, with no outgoing parameters. Factor events to a separate callback contract. Prefer a dedicated publish-subscribe pair over raw callbacks.
Name note: the literal phrase is not found in the retrieved sources. The substance is canonical.
Sources: *Programming WCF Services* Ch. 5 and Appendix D, WCF Coding Standard. Confidence: High on substance.

### 10. Interface ownership (substance)

The service owns its contract and its evolution. Consumers program against the published contract, not the service type. Services are autonomous and version independently of clients.
Name note: "interface ownership" as a named rule is not found verbatim.
Source: *Programming WCF Services*, Appendix A tenets. Confidence: High on substance.

### 11. Versioning

Data contracts version independently per side. Support IExtensibleDataObject on data contracts. Provide meaningful namespaces. Use the company URL or URN with year and month for outward-facing contracts.
Sources: WCF Coding Standard, *Programming WCF Services* Ch. 3. Confidence: High.

### 12. Layering

Closed architecture is the default. Components call the adjacent lower layer. Semi-open (calling more than one layer down) is a relaxation justified for infrastructure and rarely-changed code.
Source: *Righting Software* loc. 1997–2004. Confidence: High.

### 13. Six component types

Client, Manager, Engine, Resource Access, Resource, Utility. Four layers plus a Utilities bar. Volatility decreases top-down. Reuse increases top-down.
Sources: *Righting Software* loc. 1651 and 2016, SDD 2022 deck slides 53–57, AMC outline. Confidence: High.

### 14. Naming convention

Service names are two-part compound words in Pascal case. The suffix is always the type: Manager, Engine, or Access.
Source: *Righting Software* loc. 1757. Confidence: High.

### 15. Manager-to-Manager communication is asynchronous

Managers queue calls to other Managers. The queue is a Resource. The call goes down to the queue, not sideways.
Source: *Righting Software* loc. 2046–2051. Confidence: High.

### 16. Engine rules

Engines never call each other. Never queue calls to Engines. Engines do not publish or subscribe to events. Engines may be shared between Managers.
Sources: 2010-era deck Design Don'ts, *Righting Software* loc. 1698. Confidence: High.

### 17. Utility litmus test

A component qualifies as a Utility only if it could plausibly be used in any other system, such as a smart cappuccino machine.
Source: *Righting Software* loc. 2026. Confidence: High.

### 18. Single point of entry

The Client layer advocates a single point of entry into the system. Functional decomposition prevents it.
Source: SDD 2022 deck, slide 53. Confidence: High.

### 19. Fault contracts

Exceptions and exception handling are technology-specific. They do not cross the service boundary un-declared. Declare faults via fault contracts. Publish them with the service metadata.
Source: *Programming WCF Services*, Ch. 6. Confidence: High.

### 20. Per-call instance mode preference

Prefer the per-call instance mode when scalability is a concern. Avoid a singleton unless you have a natural singleton.
Source: WCF Coding Standard, Instance Management. Confidence: High.

## Verification Table

| Claim | Verdict | Evidence |
| --- | --- | --- |
| Five component types | Not canonical as stated. Canonical is six, with Resource as its own type. | Book loc. 1651 and 2016, SDD deck 53–57, AMC outline |
| Exactly 3–5 core use cases | Training folklore. Book says 2–6, 2010 deck says 4–6. | Book loc. 2193, alumnus account, 2010 deck |
| Semi-open / any-below access as the pattern | Wrong. Closed is the default. Semi-open is a justified relaxation. | Book loc. 1997–2004 |
| ~10 services | Canonical as order of magnitude. Composition: 2–5 Managers, 2–3 Engines, 3–8 RA+Resources, ~6 Utilities. | Book loc. 2270, 2276 |
| No data in base (name) | Not found by name. Substance is canonical. | SDD deck 44, 57 |
| Volatility scoring table | No public artifact. The canonical artifact is an unstructured list. | Book loc. 1333 |

## Sources

- idesign.net — Management Overview PDF, Architect's Master Class, Detailed Design Clinic, Downloads, System Design.
- Löwy, Juval. *Righting Software*. Addison-Wesley, 2019.
- Löwy, Juval. *Programming WCF Services*, 4th ed. O'Reilly, 2015.
- SDD 2022 presentation slides, "Righting Software".
- 2010-era IDesign deck.
- InfoQ Q&A with Löwy, February 2020.
- WCF Coding Standard (IDesign, Appendix F of *Programming WCF Services*).

The IDesign Design Standard and The IDesign Method whitepapers are email-gated. They may hold the unverified names, such as interface ownership and no data in base.
