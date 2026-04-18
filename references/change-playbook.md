# Safe Change Playbook

Use this reference when the user wants to modify a legacy system without breaking hidden behavior.

## Pre-Edit Questions

Answer these before editing:

1. What exact behavior is changing?
2. Which module currently owns that behavior?
3. What contract must remain stable for callers or downstream consumers?
4. What hidden side effects could this code trigger?
5. What is the cheapest check that proves the change worked?

## Minimal Edit Strategy

Prefer this order:

1. patch the owning module
2. adjust nearby tests
3. add diagnostics only where they clarify the critical path
4. widen the refactor only if the small patch cannot preserve the contract

Avoid cross-cutting cleanup during a behavior change unless it directly reduces risk.

## White-Box Reasoning Template

Before or during the patch, write a short explanation covering:

- trigger: what input or event enters the system
- path: which functions or modules handle it
- state: what mutable state changes and who owns it
- effects: network, storage, worker, DOM, logging, timers
- failure modes: null states, stale caches, race windows, ordering issues, boundary conditions

If you cannot explain this path simply, keep exploring before editing.

## Verification Ladder

Run checks from narrow to broad:

1. smallest reproducer or targeted test
2. type-check, lint, or compile gate relevant to the touched files
3. subsystem tests or replay scripts
4. broader regression suite when the blast radius is high

Always tell the user which rung of the ladder you actually completed.

## High-Risk Legacy Patterns

Treat these as danger signs:

- duplicated state with weak synchronization
- hidden global singletons
- derived state cached in multiple places
- timer- or worker-driven updates without clear ownership
- "temporary" flags that now gate core behavior
- tests that assert snapshots but not invariants
- scripts or logs that encode behavior more accurately than docs

## Good Final Reporting

End with:

- `What changed`
- `What stayed intentionally unchanged`
- `What was verified`
- `What still needs confirmation in real runtime conditions`

## Anti-Patterns

- Do not silently widen scope from bugfix to refactor.
- Do not delete "weird" code until you know which production edge case it protects.
- Do not claim certainty where only inference exists.
