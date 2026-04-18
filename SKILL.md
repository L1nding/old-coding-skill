---
name: old-coding-skill
description: Understand and safely modify inherited or legacy software systems. Use when Codex needs to take over an unfamiliar codebase, reconstruct the current architecture and runtime state, identify entry points and risky modules, explain "how it works now", or prepare low-risk changes in a fragile system with incomplete docs or tests. Trigger on requests like "看懂这个系统", "梳理当前状态", "接手老项目", "先理解再修改", "分析运行链路", "定位改动点", "legacy system", "old code", or "old coding skill".
---

# 古法编程 Old Coding Skill

Treat a legacy system as evidence to be reconstructed, not as a black box to guess at. Build a current-state map first, then choose the smallest safe change surface, then verify with the narrowest useful checks before expanding outward.

## Core Rules

- Prefer primary evidence: source code, runtime logs, tests, configs, build scripts, and recent commits.
- Reconstruct the current behavior before proposing improvements.
- Name unknowns explicitly instead of smoothing them over with confident guesses.
- Distinguish verified facts from inferred conclusions.
- Prefer the smallest change that preserves existing contracts unless the user asks for a refactor.
- Explain execution paths, ownership boundaries, and side effects before editing critical logic.

## Default Workflow

### 1. Build a system snapshot

Start by identifying:

- entry points, startup path, and primary execution loop
- major modules and who owns state
- external dependencies, IO boundaries, and background workers
- configuration layers, feature flags, and environment assumptions
- test surfaces, fixtures, logs, and replay artifacts
- known unknowns, weakly documented areas, and high-blast-radius files

For a detailed checklist, read [references/system-snapshot.md](references/system-snapshot.md).

### 2. Decide the task mode

Use one of these modes:

- `understand-only`: produce a current-state map, risk list, and recommended next probes
- `debug`: reproduce, isolate the failing path, identify the first bad state transition, then patch
- `modify`: identify the smallest edit set, contracts to preserve, and verification ladder
- `refactor`: define invariants first, then separate mechanical moves from behavioral changes

If the user asks to modify the system before it is understood, still do a lightweight snapshot first.

### 3. Find the safe change surface

Before editing, identify:

- the owning module for the behavior
- upstream inputs and downstream consumers
- state transitions that must remain valid
- side effects: storage, network, worker messages, timers, UI events
- guardrails: types, tests, runtime assertions, logs, KPI gates, replays

For the detailed pre-edit checklist, read [references/change-playbook.md](references/change-playbook.md).

### 4. Change with white-box reasoning

When implementing:

- narrate the intended execution path in plain language
- modify the minimum number of files required
- preserve contracts unless the requested behavior explicitly changes them
- add or update tests where they best lock the intended behavior
- leave behind clearer evidence: tighter names, focused comments, better diagnostics, or narrower APIs

### 5. Verify and report honestly

Run the narrowest meaningful checks first, then widen only as needed:

1. targeted tests or repro steps
2. type-check or build checks
3. module-level or workflow-level regression tests
4. broader suites only when the touched path is critical

Report results in this form:

- `Verified`: what was actually run and what it proved
- `Inferred`: what is likely true but not directly exercised
- `Unknown`: what still needs runtime confirmation, production data, or broader validation

## Output Shape

When this skill is active, prefer producing these artifacts for the user:

1. `Current state`: startup path, module map, data flow, and critical files
2. `Risk map`: fragile seams, unclear ownership, missing tests, and external dependencies
3. `Change plan`: smallest edit surface and preserved contracts
4. `Implementation`: the actual patch when appropriate
5. `Verification`: exact checks run and remaining uncertainty

## Optional Interaction Modes

Use these only when they help the user:

- `原理反问模式`: after explaining or implementing, ask the user to restate the critical execution path or invariant in their own words
- `白盒走读模式`: walk line by line through a critical function and explain state changes, side effects, and failure points
- `改动预演模式`: before editing, simulate what should happen at runtime and where it could break

## Notes

- Do not confuse old code with bad code. Preserve valuable constraints that were learned the hard way.
- Do not over-refactor just because a legacy design looks unfamiliar.
- When documentation and code disagree, trust the code first and note the mismatch.
- When runtime artifacts exist, use them to validate architecture claims.
