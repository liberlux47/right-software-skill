# volatility-based-architecture

A planning skill for coding agents. It guides architecture planning with the IDesign Method (Juval Löwy). Extract core use cases. Assess the two axes of volatility. Decompose volatility into components. Apply the communication rules. Validate with call chains. Produce an architecture plan document.

This skill follows the open Agent Skills standard (agentskills.io). It works in any compliant coding agent. It is not tied to one vendor's ecosystem.

## Scope

- Plans software systems. It is not an engineering tool.
- Never prescribes a technology stack.
- Covers system design only. Project design (estimation, scheduling, cost) is out of scope.

## Contents

| Path | Responsibility |
|---|---|
| `SKILL.md` | The loader-facing workflow. |
| `references/method.md` | The method in depth, with canonical quotes and sources. |
| `references/rules.md` | The canonical rule catalog and verification table. |
| `references/plan-template.md` | The output document structure and diagram conventions. |
| `INSTALL.md` | Installation instructions for any agent. |
| `LICENSE` | MIT. |

## Provenance

The skill derives from two vault documents:

- `idesign_6-component_arch.md`
- `volatility_based_software_architecture.md`

It extends them with canonical material from primary sources. Sources: *Righting Software* (Löwy, Addison-Wesley, 2019), idesign.net, the SDD 2022 presentation, and *Programming WCF Services* (Löwy, O'Reilly, 2015). `references/rules.md` flags every claim that could not be verified in accessible primary sources. Unverified rule names are taught by substance. They are never asserted as canonical.

## Diagrams

Generated plans embed diagrams as Mermaid and ASCII text blocks. The skill never generates SVG or any other media file.

## License

MIT. See `LICENSE`.
