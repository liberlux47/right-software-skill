# Installation

This skill implements the Agent Skills open specification (agentskills.io). Any agent that reads `SKILL.md` from its skill discovery paths can load it. One directory installs everywhere. You need no per-agent conversion.

## The single source of truth

The skill directory is `right-software`. It contains `SKILL.md` and a `references/` directory.

Installation means placing this directory in the agent's configured skills location. Copy it, or link it with a junction or symlink. The rest of this document names the locations per agent.

## Portability guarantees

One copy works in any compliant agent because:

- Frontmatter uses only the five portable fields: `name`, `description`, `license`, `compatibility`, `metadata`.
- No vendor-specific frontmatter fields.
- No vendor-specific body syntax. No dynamic-context injection. No `$ARGUMENTS` substitutions.
- All asset references are relative.
- `SKILL.md` is under 500 lines.v

## Universal discovery paths

Most compliant agents read these paths:

| Scope | Path |
|---|---|
| Personal | `~/.agents/skills/<name>/SKILL.md` |
| Personal | `~/.claude/skills/<name>/SKILL.md` |
| Project | `.agents/skills/<name>/SKILL.md` |
| Project | `.claude/skills/<name>/SKILL.md` |

Place a copy in the path the agent documents. Check the agent's own documentation for its discovery paths.

## Worked example A — Claude Code

Personal scope. Use a junction (matches the verified local precedent, no elevation needed):

```powershell
New-Item -ItemType Junction -Path "$HOME\.claude\skills\right-software" -Target "$HOME\.agents\skills\right-software"
```

Fallback: copy the directory.

```powershell
Copy-Item -Recurse "$HOME\.agents\skills\right-software" "$HOME\.claude\skills\right-software"
```

Project scope: place the directory in `.claude/skills/` inside the repository.

## Worked example B — opencode

Personal scope: zero action needed. opencode auto-loads `~/.agents/skills/`. File access permissions for that path are already granted in the opencode configuration.

Alternatives:

- Global: `~/.config/opencode/skills/<name>/SKILL.md`
- Project: `.opencode/skills/<name>/SKILL.md`

Restart opencode after installation. Skills load once at startup. The running session keeps the old configuration.

## Worked example C — Pi (pi.dev)

Personal scope: zero action needed. Pi reads `~/.agents/skills/` per the Agent Skills specification.

Alternatives:

- Personal: `~/.pi/agent/skills/<name>/SKILL.md`
- Project: `.pi/skills/<name>/SKILL.md`

Invoke the skill with `/skill:right-software`.

## Any other agent

- Consult the agent's documentation for its skill discovery path.
- The Agent Skills client showcase lists compliant products: agentskills.io/clients.
- Copy the skill directory into that path.
- A spec-compliant agent needs no other change.

## Verification

1. Check the frontmatter. `name` must equal the directory name. The description must be present.
2. Check that the skill loads. Ask the agent to list its skills, or invoke the skill.
3. Run the STE linter if available:

```bash
python ~/.agents/skills/asd-ste100/scripts/ste-lint.py ~/.agents/skills/right-software/SKILL.md
```

4. Restart opencode after installation. Other agents may also cache configuration at startup.
