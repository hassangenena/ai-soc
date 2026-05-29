# AISOC Farm

An in-chat, multi-agent **AI Security Operations Center** built entirely from
structured prompts — no application code, no external services, no
infrastructure. The whole farm runs inside a single chat session of an
LLM-powered IDE assistant, under a central **Orchestrator** that follows a
**Plan → Approve → Execute** protocol.

This repository is a **student project template** for a Cybersecurity & Network
Security course. Each student owns one agent, authored purely as a
[RICTOC](#rictoc) prompt, and the agents cooperate through the Orchestrator's
chat-based dispatch.

> **Prompts are the product.** There is intentionally no runnable code here.
> The deliverables are markdown prompt files, version-controlled, tested, and
> reviewed like software.

---

## Portability is the rule

Every agent prompt must work **identically** in the two mandatory target
environments:

- **GitHub Copilot Chat** (VS Code)
- **Claude Code**

A prompt that works in only one is considered incomplete. The agent prompts
themselves are plain markdown and depend only on natural-language conventions —
never on vendor features. The files under `.claude/` and `.github/` are **thin
invocation wrappers only**: they load the canonical prompts under `.aisoc/` and
must never change agent behaviour. **The canonical source of truth is always
`.aisoc/`.**

---

## Quick start

The farm is booted the same way in both environments: paste the boot skill,
then the catalogue skill, then a scenario, then approve the Orchestrator's plan
and paste each agent when it is dispatched.

### Manual boot (works everywhere — the canonical flow)

1. Open this repository in your IDE and start a fresh chat session.
2. Paste the body of [`.aisoc/skills/boot/SKILL.md`](.aisoc/skills/boot/SKILL.md).
   The assistant replies: `AISOC Farm boot loaded — waiting for catalogue.`
3. Paste the body of [`.aisoc/skills/catalogue/SKILL.md`](.aisoc/skills/catalogue/SKILL.md).
   It replies: `Catalogue registered, 20 agents available — ready for scenario.`
4. Paste a scenario, e.g. [`.aisoc/scenarios/01-beaconing.md`](.aisoc/scenarios/01-beaconing.md).
5. Review the Orchestrator's **PLAN** and reply `approve` / `revise <text>` / `abort`.
6. When an agent is dispatched, paste the matching
   `.aisoc/agents/NN-<name>.agent.md` file plus the requested input block.

### Claude Code (convenience wrappers)

Slash commands under [`.claude/commands/`](.claude/commands/) wrap the manual
flow:

| Command | Effect |
| --- | --- |
| `/aisoc-init` | Boot **and** register the catalogue in one step. |
| `/aisoc-boot` | Run the boot sequence only. |
| `/aisoc-catalogue` | Register the agent catalogue only. |
| `/aisoc-agent <N>` | Print agent `#N`'s prompt verbatim for dispatch. |

The same three boot/catalogue/init wrappers are also available as auto-discovered
**skills** under [`.claude/skills/`](.claude/skills/) (`aisoc-init`,
`aisoc-boot`, `aisoc-catalogue`) for when you'd rather let Claude pick them up by
intent than type a slash command.

### GitHub Copilot Chat (convenience wrappers)

- [`.github/copilot-instructions.md`](.github/copilot-instructions.md) is loaded
  automatically and teaches Copilot the farm protocol.
- Prompt files under [`.github/prompts/`](.github/prompts/) mirror the Claude
  Code slash commands (`/aisoc-init`, `/aisoc-boot`, `/aisoc-catalogue`,
  `/aisoc-agent`). Enable them with the
  `chat.promptFiles` setting in VS Code.
- A custom chat mode lives at
  [`.github/chatmodes/aisoc-orchestrator.chatmode.md`](.github/chatmodes/aisoc-orchestrator.chatmode.md).

> Wrappers are sugar, not requirements. If a wrapper ever disagrees with the
> file it loads, the file under `.aisoc/` wins.

---

## Repository layout

```text
.aisoc/                              # Canonical source of truth (vendor-neutral)
├── agents/                          # Orchestrator reference + 20 catalogue agents + RICTOC template
│   ├── aisoc-orchestrator.agent.md  #   reference RICTOC for the Orchestrator role
│   ├── _rictoc-template.agent.md    #   blank skeleton — copy this to start your agent
│   ├── 03-dns-sentinel.agent.md     #   the one complete worked example
│   └── NN-<short-name>.agent.md     #   student-authored stubs (01, 02, 04 … 20)
├── skills/
│   ├── boot/SKILL.md                # Pasted first  — boots the Orchestrator
│   └── catalogue/SKILL.md           # Pasted second — registers the 20 agents
├── schema/finding.json             # Shared 8-key finding schema (JSON Schema 2020-12)
├── scenarios/                       # Farm-level reference scenarios for testing
├── commands/                        # Reserved: operator-command vocabulary reference
└── docs/
    ├── project-proposal/            # Full proposal, glossary, test worksheets
    └── architecture/                # Reserved: standalone architecture docs

.claude/commands/                    # Claude Code slash-command wrappers (sugar)
.claude/skills/                      # Claude Code skill wrappers — boot/catalogue/init (sugar)
.github/copilot-instructions.md      # GitHub Copilot workspace instructions (sugar)
.github/prompts/                     # Copilot prompt-file wrappers (sugar)
.github/chatmodes/                   # Copilot custom chat mode (sugar)
```

Start with the proposal:
[`.aisoc/docs/project-proposal/proposal.md`](.aisoc/docs/project-proposal/proposal.md).

---

## RICTOC

Every agent prompt is authored with six labelled sections —
**R**ole, **I**nput, **C**ontext, **T**ask, **O**utput, **C**onstraints. Copy
the skeleton at
[`.aisoc/agents/_rictoc-template.agent.md`](.aisoc/agents/_rictoc-template.agent.md)
and study the worked example at
[`.aisoc/agents/03-dns-sentinel.agent.md`](.aisoc/agents/03-dns-sentinel.agent.md).

Every agent returns a single JSON object with the shared eight keys defined in
[`.aisoc/schema/finding.json`](.aisoc/schema/finding.json): `agent`, `summary`,
`severity`, `confidence`, `evidence`, `attck`, `recommendation`, `rationale`.
