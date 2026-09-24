# right-software

A planning skill for coding agents. It guides architecture planning with the IDesign Method (Juval Löwy):
* Extract core use cases.
* Assess the two axes of volatility.
* Decompose volatility into components.
* Apply the communication rules.
* Validate with call chains.
* Produce an architecture plan document.

This skill follows the open Agent Skills standard (agentskills.io). It works in any compliant coding agent and is not tied to one vendor's ecosystem.

## Why This Skill Exists

Software requirements change constantly. An architecture that mirrors the requirements breaks when they change. Löwy states this as the design Prime Directive:

> "Any attempt at designing against the requirements will always guarantee pain, because when the requirements change, so will your design."

— Juval Löwy, InfoQ Q&A, February 2020

The common failure mode is functional decomposition: one service per requirement or per function. It pollutes clients with business logic, prevents reuse, prevents a single point of entry, and makes services too big or too small.

The alternative is volatility-based decomposition. Identify the areas of open-ended change in the system and encapsulate each one behind an architectural boundary — Löwy's "vaults":

> "you start thinking of your system as a series of vaults… With volatility-based decomposition, you open the door of the appropriate vault, toss the grenade inside, and close the door."

— Juval Löwy, *Righting Software*, Ch. 2

When a requirement changes, the change lands inside one vault. It is contained and does not spread across the architecture.

Volatility is not variability. Variability is bounded change; conditional logic handles it. Volatility is open-ended change; it invalidates structures unless it is contained. Only open-ended, expensive-to-contain change earns an architectural boundary.

This skill exists to make an agent plan systems the volatility-based way. An unguided agent tends to decompose by function or by requirement. This skill supplies the discipline that keeps the architecture stable under change.

## What This Skill Does

The skill runs a six-phase planning workflow:

1. Extract the core use cases (two to six per the canonical source).
2. Assess volatility along two independent axes: one customer over time, and different customers at the same time.
3. Decompose volatility into components of six types: Client, Manager, Engine, Resource Access, Resource, Utility.
4. Apply the communication rules, including the closed-architecture default.
5. Validate with call chains and sequence diagrams for every core use case.
6. Produce the architecture plan document.

The output is a plan document, not code. The skill does not prescribe a technology stack. It covers system design only; project design (estimation, scheduling, cost) is out of scope.

## Scope

**In scope:**

- Plans new systems: structure, decomposition, and component boundaries.
- Analyzes existing systems to identify service and component boundaries.
- Decides how to split a system into services.
- Produces an architecture plan document as the deliverable.
- Works interactively: asks for missing domain input, interviews for core use cases and volatility, and records assumptions in Open Risks.

**Out of scope:**

- Implementation work or coding. It is not an engineering tool.
- Refactoring existing code.
- Technology stack selection.
- Project design: estimation, scheduling, and cost.
- Trivial single-service systems with no change risk.

## Contents

| Path | Responsibility |
|---|---|
| `SKILL.md` | The loader-facing workflow. |
| `references/method.md` | The method in depth, with canonical quotes and sources. |
| `references/rules.md` | The canonical rule catalog and verification table. |
| `references/plan-template.md` | The output document structure and diagram conventions. |
| `INSTALL.md` | Installation instructions for any agent. |
| `LICENSE` | MIT. |

## Diagrams

Generated plans embed diagrams as Mermaid and ASCII text blocks. The skill never generates SVG or any other media file.

## Bibliography

- Löwy, Juval. *Righting Software*. Addison-Wesley, 2019. Kindle locations are cited inline in `references/method.md`.
- Löwy, Juval. *Programming WCF Services*, 4th ed. O'Reilly, 2015.
- IDesign. "The IDesign Method — Management Overview". idesign.net, February 2020.
- Löwy, Juval. "Righting Software". SDD 2022 presentation. sddvault.s3.amazonaws.com.
- IDesign. 2010-era deck, "Typical Layers" and "Architecture Validation" slides.
- InfoQ. Q&A with Juval Löwy on *Righting Software*, February 2020. infoq.com/articles/book-review-righting-software.
- IDesign. System Design service page. idesign.net/Services/System-Design.
- IDesign. Architect's Master Class outline. idesign.net/Training/Architect-Master-Class.
- IDesign. Detailed Design Clinic page. idesign.net.
- IDesign. WCF Coding Standard (Appendix F of *Programming WCF Services*).

The IDesign Design Standard and The IDesign Method whitepapers are email-gated downloads and were not accessible. They are not cited. `references/rules.md` flags every claim that could not be verified in accessible primary sources. Unverified rule names are taught by substance and never asserted as canonical.

## License

MIT. See `LICENSE`.