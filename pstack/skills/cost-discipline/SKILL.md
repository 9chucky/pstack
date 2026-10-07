---
name: cost-discipline
description: "Set and enforce a run's budget: subagent caps, cheap-run habits, one spend line per reply, and splitting work that outgrows its budget."
disable-model-invocation: true
---

# Cost discipline

A budget is a cap the user sets in words, for example "cheap run", "budget: 5 subagents", or "spare no tokens". Unset means economy by habit, not a hard cap. The cap covers subagents, cloud runs, and re-runs. It does not cover judgment, which the hardest tasks keep on the strongest model.

## Habits

Economy is routing, not corners.

- Reference by path. Never paste a file's content into the thread when a path works.
- Route bulk reads, sweeps, and long outputs to subagents. Summaries come back, bulk stays out, per **principle-guard-the-context-window**.
- Fire a fresh subagent with consolidated scope instead of resume chains. A resume that silently drops directives costs a rerun, which is the expensive outcome.
- One mission per thread. A thread that switches subjects mid-run pays its history on every turn. Suggest "new task".
- Do not re-read what is already in context. Do not spawn a subagent for a two-line answer you can see.

## Over budget

A task that will not fit its budget in one run routes to the Multi-phase plan playbook (`playbooks/multi-phase-plan.md`) instead of burning one context. Say the split, the phases, and the per-phase budget in the reply, and start the first phase. A phase boundary is also a checkpoint, so a later phase starts from files, not from chat history.

## Gates and the bar

A budget never waives a constitution gate. If the only affordable path violates a gate, say so and stop. A budget scales quantity, not the quality bar. Production mode never cuts verification to save tokens. Prototype mode is the sanctioned way to run cheap work.

## Spend line

Every playbook reply carries one spend line, for example "Spend. 3 subagents, all local" or "Spend. 2 subagents plus 5 cloud runs". Count what fired, not dollars, unless a real measurement exists. A number with no source is a guess, labeled as one.
