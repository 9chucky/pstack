---
name: quality-modes
description: "Set the run's quality bar: prototype, production, enhance, or refactor. Scales playbook intensity, never which playbook runs. Gates always apply."
disable-model-invocation: true
---

# Quality modes

A mode sets the quality bar for a run. The playbook still owns the work and its steps. The mode owns how strict each step runs. The user names it as a prefix or a phrase, for example "/poteto-mode prototype: build the upload sketch" or "switch to production mode". Unset is production.

## The four modes

**Prototype.** Speed over polish. Happy path only, inline config allowed, no tests required, README as the only doc. A crash is information. Used for a sketch that settles a design, a demo, or a spike. A sketch that graduates to real code reruns under production mode. Gates 1, 2, 3, and 5 hold. Gate 4 holds at the sketch's own bar, the sketch runs for real and the run or its output shows in the reply.

**Production.** The full bar. Tests accompany features, error handling over happy path, strict types, structured logging, dependency hygiene, security defaults. This is the default and the bar every playbook is written against.

**Enhance.** A change to an existing app, regression-safe first. Run the existing tests before touching code. Impact analysis before the change. Preserve backward compatibility or document the break. A rollback plan for anything a user sees.

**Refactor.** Structure changes, behavior does not. Tests exist and stay green before, during, and after. Small moves, one concern per commit. A behavior change stops the mode. Surface it, and rerun the work in production mode.

## What a mode changes

A mode scales the bar, not the shape. It changes whether a step's tests are required or skipped, the depth of error handling, typing, logging, and security a step owes, and whether a docs, review, or perf step runs at all.

A mode never changes which playbook runs. A mode never widens scope. A mode never waives a constitution gate. The modes compose with the Prototype and Refactoring playbooks, which own when a throwaway sketch or a behavior-preserving shape is the right work. Prototype mode on a Feature playbook means the feature lands at the prototype bar.

## Setting and clearing

- Prefix the first prompt, as in "/poteto-mode prototype: sketch the layout picker".
- Say it mid-run, as in "switch to production mode before you land this".
- "drop the mode" clears back to production.
- A run can change modes per phase. Prototype the sketch, then name the switch, then build at the production bar.

## Report

Name the mode near the top of the reply, and again if it changes, for example "Mode. Production, unchanged." An unset mode reports as production.
