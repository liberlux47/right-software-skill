---
name: volatility-based-architecture
description: "Plan a software system's architecture by decomposing on volatility, per Juval Löwy's IDesign Method. Use when the task is architecture planning. The workflow extracts core use cases, assesses the two axes of volatility, maps volatility to components, and validates with call chains. Output is an architecture plan document. Not for implementation work. Does not prescribe a technology stack."
license: MIT
compatibility: "Agent Skills spec (agentskills.io). Loads in any compliant coding agent"
metadata:
  source: "Righting Software (Löwy 2019), IDesign Method, idesign.net"
---

# Volatility-Based Architecture Planning

Plan a software system by decomposing it on volatility, not functionality. This skill guides the full planning workflow from the IDesign Method: core use cases, volatility assessment, component decomposition, communication rules, and call-chain validation. The output is an architecture plan document.

Read `references/method.md` for the method in depth. Read `references/rules.md` for the canonical rule catalog and its verification table. Use `references/plan-template.md` for the output document structure.

## When to Use This Skill

Use this skill for architecture planning tasks:

- Design a new system's structure or decomposition.
- Identify service and component boundaries for an existing system.
- Decide how to split a system into services.
- Answer "how should we structure this system" questions.
- Produce an architecture plan document for a project.

## When Not to Use

Do not use this skill for:

- Implementation work or coding.
- Refactoring existing code.
- Technology stack selection.
- Estimation, scheduling, or cost analysis. Project design is out of scope for this skill.
- Trivial single-service systems with no change risk.

## Workflow Overview

The workflow proceeds through six phases:

1. Extract core use cases.
2. Assess volatility along two axes.
3. Decompose volatility into components.
4. Apply the communication rules.
5. Validate with call chains and sequence diagrams.
6. Produce the architecture plan document.

Work interactively. Ask the user for missing domain input. Do not invent facts about the business. Record assumptions in the plan's Open Risks section.

## Phase 1 — Core Use Cases

Extract the core use cases. A core use case expresses the essential business purpose of the system. Most systems have two to six core use cases. Training variants cite three to five or four to six. Use the book's two to six range.

A core use case is an abstraction of other use cases. Every other use case must be a variation of one core use case. If a use case is not a variation, you missed a core use case.

Do not extract the use cases from the requirements document alone. Core use cases are rarely stated explicitly. Interview stakeholders.

Validate the count. A Manager count above five usually means you extracted too many use cases and drifted into functional decomposition. Eight Managers is a hard failure signal.

## Phase 2 — Volatility Assessment

Assess volatility along two independent axes:

- Same customer over time. How will a single customer's needs evolve during the system's lifetime?
- Different customers at the same time. How do customer segments use the system differently today?

Ask two probes for every candidate component:

- Could you use this component, as is, with this customer, forever?
- Could you use this design across all customers now?

A component is stable only if it survives both probes. A negative answer marks a volatility to encapsulate.

Volatility is not variability. Variability means bounded change. Conditional logic handles it. Volatility is open-ended change. It invalidates structures if not contained.

Expose hidden volatility:

- Re-express solutions as requirements. "Send email notification" is a solution. The requirement is "notify the user". Notification transport is a volatility to encapsulate.
- Compare competitors. If competitors do something differently, it is likely volatile. If all competitors do it identically, it is probably business nature.

Walk the seven candidate areas in interviews:

1. User types.
2. Client applications.
3. Security methods.
4. Notification transports.
5. Storage options.
6. Workflow variations.
7. Regulatory requirements.

The output is the Volatilities List. It is an unstructured list, not a scored table. No canonical scoring artifact exists. Tag each entry with its axis.

## Phase 3 — Decomposition

Map each volatility to a component. The mapping is rarely one to one. One component may encapsulate several related volatilities. Some volatilities map to operational concepts, such as queues or events. Some map to third-party services.

Use the six component types:

| Type | Purpose | Encapsulates |
|---|---|---|
| Client | Handle user interaction | Client technology volatility |
| Manager | Orchestrate business use cases | Volatility in use case sequences |
| Engine | Execute specific business logic | Volatility in business rules |
| Resource Access | Access storage and external systems | Storage and integration volatility |
| Resource | Physical storage and systems | The resource itself |
| Utility | Handle cross-cutting concerns | Infrastructure volatility |

The five-type teaching variant merges Resource into another category. Use the six-type model. The system is four layers plus a Utilities bar: Clients, Business Logic (Managers and Engines), Resource Access, Resources, and Utilities crossing all layers.

Volatility decreases top-down. Reuse increases top-down.

Follow the closed architecture default. Components call downward only. Justify semi-open relaxation, calling more than one layer down, only for infrastructure and rarely-changed code.

Name every service with two-part PascalCase. The suffix is always the type: Manager, Engine, or Access. Examples: `TradesAccess`, `TradeWorkflow`, `MembershipManager`.

Target the minimal set. A typical system contains ten services in order of magnitude. The composition is two to five Managers, two to three Engines, three to eight Resource Access and Resources, and about six Utilities. A dozen or two blocks total.

Red flags:

- Eight Managers means you did functional decomposition.
- A large number of Engines means you may have done functional decomposition.
- Resource Access organized by database type instead of volatility area.

Create an Engine only when implementation volatility exists. Do not create abstraction before a demonstrated volatility.

Represent the static architecture as a Mermaid `flowchart TB` with layered subgraphs, or the ASCII layer table in `references/plan-template.md`. Diagrams are text blocks. Never generate SVG files.

## Phase 4 — Communication Rules

Apply the communication rules to the component graph:

- Flow control goes top-down only.
- Each component can access any component below it.
- Managers call other Managers only asynchronously, through a queue. The queue is a Resource. The call goes down, not sideways.
- Engines never call each other.
- Engines never receive queued calls.
- Engines do not publish or subscribe to events.
- Engines may be shared between Managers.
- Resource Access services never call each other.
- Clients are the single point of entry. They contain no business logic.
- Every service can access Utilities.
- Utilities are fully domain-agnostic. A Utility must pass the litmus test: could it plausibly run in a smart cappuccino machine?
- Each service maintains independent business objects.

State every relaxation of the closed architecture in the plan. Justify each one.

## Phase 5 — Validation

Validate the design against every core use case. Produce, per core use case:

- A call-chain flowchart. Use a Mermaid `flowchart`. Synchronous calls use solid edges. Dashed edges mark queued calls. Draw one chain per core use case onto the static architecture.
- A sequence diagram. Use a Mermaid `sequenceDiagram`.
- An ASCII fallback for both. Terminal-only readers cannot render Mermaid.

Look for symmetry. Good architectures are symmetric. Expect repeated call patterns across use cases. Absence of symmetry is a cause for concern.

Apply the Design Don'ts as red flags:

- Never queue calls to Engines.
- Never queue calls to Resource Access.
- Engines never call each other.
- Resource Access never calls each other.
- Engines and Resource Access do not publish or subscribe to events.
- Clients do not call multiple Managers in a single use case.

Iterate until every core use case has a valid interaction between components. A valid design exists when every core use case has a working interaction.

## Phase 6 — Output

Produce the architecture plan document. Follow `references/plan-template.md` exactly. The document contains:

- System Overview.
- Glossary.
- Core Use Cases.
- Volatilities List.
- Static Architecture.
- Communication Rules.
- Call Chains.
- Design Validation.
- Open Risks.

Embed the diagram blocks from Phase 5 in the Call Chains and Static Architecture sections. Diagrams are Mermaid plus ASCII. Never embed images. Never generate SVG files.

Ask the user to review the plan before you finalize it.

## Boundaries

- This skill plans. It is not an engineering tool.
- This skill never prescribes a technology stack.
- This skill does not estimate schedules or costs. Project design is out of scope.
- This skill teaches verified substance. Some IDesign rule names, such as no data in base, are not found in accessible primary sources. `references/rules.md` flags every such case. Do not present unverified names as canonical.
- Do not invent design rules beyond the catalog in `references/rules.md`.

## Additional Resources

- `references/method.md` — the method in depth, with canonical quotes and sources.
- `references/rules.md` — the canonical rule catalog and verification table.
- `references/plan-template.md` — the output document structure and diagram conventions.
