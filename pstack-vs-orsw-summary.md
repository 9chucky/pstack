# pstack vs. orsw-vibe-dev-framework — Comparison Summary

> Generated 2026-10-07. Sources: `~/.claude/skills/pstack/pstack/` (Cursor plugins marketplace repo, pstack plugin v0.15.15) and `~/.claude/skills/orsw-vibe-dev-framework/`.

---

## 1. What pstack is

**pstack** is a plugin by Lauren Tan ("poteto"), living in the `pstack/` folder of the Cursor plugins marketplace repo. Tagline: *"if you want to go fast, go deep first. pstack helps you write less, but higher quality code. rigorous agent workflows you can parallelize with confidence."*

### How it works
- The entry point is the **`/poteto-mode`** skill — a router (a "mode"). You give it a goal; it matches the task to one of **~25 playbooks** (bug-fix, feature, refactoring, perf-issue, hillclimb, investigation, prototype, babysit, shipping, orchestrate, autopilot, session-pickup, worktree-cleanup, …), copies the playbook's steps verbatim into a todo list, and invokes the right **leaf skills** as each step fires. You never need to name the playbook — "repro first, then fix and verify" is enough routing signal.
- **24 named principles** (Laziness Protocol, Foundational Thinking, Model the Domain, Prove It Works, Fix Root Causes, Guard the Context Window, Never Block on the Human, …) that the agent must name in its replies when they shaped a decision — so you can redirect mid-task by principle name.
- **Specialized skills** called from playbooks: `how` (understand before changing), `architect` (parallel design exploration), `interrogate` (multi-model adversarial review), `arena` (design/code bakeoffs), `unslop` (prose de-AI-ification), `no-comments`, `benchmark-checklist`, `correct`, `figure-it-out` (bespoke playbook designer), `poteto-help`.
- **Agents + automation**: `poteto-agent` subagent with per-role model config (`/setup-pstack`; defaults like `claude-opus-5-5-xhigh` for judgment, a fast model for mechanical edits), fresh-subagent-by-default policy, plus TS scripts (`orch`, `watch-pr`, `check-plan`) and a `benny` issue-triage automation.
- **Verification culture**: prove on the real artifact (not "it compiles"), evidence labels on every claim (measured / inferred / guess), benchmark checklist before trusting any number, adversarial review before shipping, "green is not safe."
- **Autonomy stance**: "Just do it" — proceed without asking on reversible work; pause only for irreversible writes (deploys, force-pushes, deletions). Never block on the human for a question an experiment could answer.

### Structure
```
pstack/
├── .cursor-plugin/plugin.json   # plugin manifest (v0.15.15)
├── skills/                      # ~20 skills, poteto-mode is the router
│   └── poteto-mode/
│       ├── SKILL.md             # the router: triggers, principles, playbooks
│       ├── playbooks/           # 25 step-by-step playbooks
│       ├── references/          # e.g. bugbot-triage
│       └── scripts/             # TS tooling (orch, watch-pr, check-plan)
├── agents/                      # poteto-agent, comment-sicko
├── automations/benny/           # issue triage + reproduce-and-fix automation
└── docs/guide/                  # 10-page user guide
```

---

## 2. What orsw-vibe-dev-framework is

**ORSW Vibe Dev Framework** is a tool-agnostic, zero-dependency markdown framework: *"One method. Every tool. The whole team."* It is explicitly **not a plugin** — it integrates through each tool's project-instructions mechanism (CLAUDE.md, `.kiro/steering/`, `.cursor/rules/*.mdc`, GEMINI.md, AGENTS.md, WINDSURF.md), distributed to projects via **git submodule**.

### How it works
- **The 5-step operating loop** — every mission runs: **Research → Plan → Execute → Verify → Checkpoint**. Ground in real sources, produce a plan file, get it **approved and LOCKED**, execute tasks in order one at a time, prove with evidence (output + environment named), then checkpoint and start the next mission clean. One mission per thread.
- **P0–P4 priority rules**: P0 non-negotiables (no hardcoded env values, real data only, verify before "done", no secrets in repo, respect the locked plan); P1 strong defaults (smallest working change, tests accompany features, idempotent ops); P2 engineering standards (types, error handling, logging, dependency hygiene, security); P3 style; P4 nice-to-have. Lower number always wins.
- **7 principles**: Platform-First, Truth Over Invention, Contracts Over Conversations (agreements live in files, not chat), Governance Is a Feature, Ship Small Prove Often, Context Is Expensive, Make It Work/Right/Fast — in that order.
- **9 skills** invoked by keyword (research, plan, execute, verify, checkpoint, investigate, brainstorm, review, ship) — the agent is *instructed* to recognize keywords and read the corresponding file. **No automatic routing.**
- **4 modes** as quality presets: `prototype` (speed > polish, P0 only), `production` (full P0–P4, tests required), `enhance` (regression-safe modification), `refactor` (behavior-preserving).
- **Per-project templates**: CONTEXT.md (stack/layout/mission), FLAGS.md (keyword → skill routing), SYMBOLS.md (domain shorthand), PANEL.md (output standards), PLAN_TEMPLATE.md (locked plan format).
- **Cost discipline** is a first-class concern: reference by `@path` instead of pasting, plan file as shared memory, subagents return summaries only, checkpoint resets context.

### Structure
```
orsw-vibe-dev-framework/
├── core/        # THE METHOD (immutable, shared): RULES, PRINCIPLES, LOOP, PATTERNS
├── templates/   # copy & customize per project: CONTEXT, FLAGS, SYMBOLS, PANEL, PLAN_TEMPLATE
├── skills/      # 9 keyword-triggered behavioral guides
├── modes/       # 4 quality presets: prototype, production, enhance, refactor
├── adapters/    # entry points for 6 tools (Claude Code, Kiro, Cursor, Gemini, Codex, Windsurf)
├── examples/    # 3 worked missions (web API, data pipeline, frontend)
└── INSTALL.md
```

---

## 3. Side-by-side comparison

| Dimension | **pstack** | **orsw-vibe-dev-framework** |
|---|---|---|
| **What it is** | Native plugin (skills + agents + hooks + scripts) | Plain-markdown method, not a plugin |
| **Author / origin** | Lauren Tan ("poteto"), Cursor marketplace | Independent framework, team/oriented |
| **Tool support** | Claude Code / Cursor native | Claude Code, Kiro, Cursor, Gemini, Codex, Windsurf |
| **Installation** | Install plugin, slash commands | Copy templates + adapter file into each project, git submodule for updates |
| **Core control unit** | `/poteto-mode` router → 25 playbooks | 5-step loop (Research→Plan→Execute→Verify→Checkpoint) |
| **Routing** | **Automatic** — router picks the playbook from the goal | **Manual/keyword** — agent reads the file when the keyword appears |
| **Human gate** | Minimal — "just do it," never block on the human, pause only for irreversible writes | Heavy — plan must be **approved and locked** before execute; deviation must be surfaced |
| **Parallelism** | Central — subagent fan-out, swarms, bakeoffs, multi-model adversarial review, overnight/orchestrate runs | Peripheral — subagents only to offload research and return summaries; execution is strictly sequential |
| **Verification** | Prove on the real artifact; evidence labels (measured/inferred/guess); benchmark checklist; adversarial `interrogate` review | Verify step: run checks, show output, name environment; P0.3 rule; tests accompany features |
| **Cost posture** | Token-heavy by design (deep, rigorous, parallel) | Token-frugal by design (one mission per thread, context-by-reference, checkpoint resets) |
| **Scale of content** | Large: ~20 skills, 25 playbooks, agents, TS scripts, automations, 10-page guide | Small: 9 skills, 4 modes, 4 core docs, 5 templates |
| **Team story** | Personal agent style; one agent at a time | Central: org-wide method via submodule; project templates never overwritten; zero training needed |
| **Governance** | Principles as behavioral contract, audited by the agent naming them | Explicit P0–P4 priority system; rules are absolute; audit trails valued |
| **Writing style rules** | Strict unslop prose rules (short sentences, no em-dashes, no filler) | Panel/output standards per project |
| **Customization** | `/setup-pstack` for model-per-role; `/correct` for repeated mistakes | Templates you edit per project; `core/` immutable, propose upstream |

### Where they agree (shared DNA)
- **Verify before "done"** — both refuse to call work complete without running something and showing evidence.
- **Root causes over symptom patches** — pstack's "Fix Root Causes," orsw's "Truth Over Invention."
- **Smallest change that works** — pstack's "Laziness Protocol," orsw's P1.3 "Smallest working change."
- **Context is expensive** — both push context-by-reference and subagent delegation for heavy reads.
- **Anti-hallucination** — pstack's "never fabricate a link, every claim carries its evidence or its label"; orsw's "real data only."
- **Contracts in files** — pstack's locked plans / decision trails; orsw's "Contracts Over Conversations" and PLAN_TEMPLATE.

### Where they diverge (the real fork)
1. **Autonomy vs. approval.** pstack is built for high-autonomy operation (overnight runs, autopilot, "don't block on the human"). orsw is built around human checkpoints (plan approval is a P0 rule). These two philosophies are near-contrary.
2. **Depth vs. breadth of method.** pstack invests in task-specific rigor (25 playbooks, adversarial review, measurement checklists). orsw invests in a single universal loop that is shallow per task but identical everywhere.
3. **One agent vs. one team.** pstack makes one agent excellent. orsw makes a heterogeneous team consistent.
4. **Cost.** pstack spends to buy rigor and parallelism; orsw treats tokens as a governed budget.
5. **Ecosystem.** pstack assumes Cursor/Claude Code native plugin mechanics (agents, scripts, hooks). orsw assumes nothing beyond a markdown-reading agent.

---

## 4. When to use which

| Situation | Better fit |
|---|---|
| Solo power user on Claude Code/Cursor wanting deep, verified, parallelizable work | **pstack** |
| Long autonomous runs ("run until done", overnight, queue of PRs) | **pstack** |
| Multi-tool team (Kiro + Cursor + Claude Code …) needing one shared method | **orsw** |
| Governance, auditability, cost control, onboarding without training | **orsw** |
| Greenfield prototype vs. enterprise production quality presets | **orsw** (modes) |
| Serious bug hunts, perf work, refactors, PR driving, UI parity | **pstack** (playbooks) |

**They are complementary, not competing:** orsw can be the *governance layer* every team member and tool loads (loop + P0 rules + templates), while pstack is the *execution engine* inside Claude Code/Cursor sessions when the work needs depth, parallelism, and autonomy.

---

## 5. TL;DR

- **pstack** = a rigorous, opinionated, autonomy-first agent *style* for Claude Code/Cursor: a router (`/poteto-mode`) that turns any goal into one of 25 verified playbooks, steered by 24 named principles, with subagents, bakeoffs, and adversarial review. Deep per-task quality, high token spend, minimal human gates.
- **orsw-vibe-dev-framework** = a portable, zero-dependency *team method* loaded through project instructions across 6 AI tools: one 5-step loop, P0–P4 priority rules, 4 quality modes, and per-project templates, distributed by git submodule. Universal discipline, cost control, human approval gates.
- **Pick pstack** when you want one agent to go deep and fast on hard work. **Pick orsw** when you want every agent, on every tool, across a team, to behave the same and stay cheap and governed. **Use both** to get governed depth: orsw as the constitution, pstack as the executor.
