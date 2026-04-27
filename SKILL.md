---
name: old-coding-skill
description: "old-coding-skill is a human-led engineering workflow for resisting black-box AI coding and vibe coding. Use when the user wants design-before-code, white-box reasoning, implementation review, algorithm modeling, controlled AI assistance, or asks in Chinese: 不要直接生成代码, 先做设计再实现, 解释原理, 白盒审计, 反 AI 黑盒, 反 vibe coding, 让我掌控设计，AI 只辅助."
---

# old-coding-skill

This skill is a disciplined engineering workflow for keeping the human in charge of modeling, design decisions, architecture boundaries, and final acceptance while using AI as a high-intensity assistant.

Before any implementation enters a codebase, the work must be able to answer four questions:

- Why is this design appropriate?
- How will it execute at runtime?
- Where are the risks and boundaries?
- How will we verify that it is actually correct?

## Core Position

- Do not accept “one vague request, one large code dump” as engineering.
- Do not skip modeling, design, or verification just because AI can produce code quickly.
- Do not let AI become the architecture decision-maker.
- Do not treat “it seems to run” as proof that the design is sound.
- Keep the human as designer, judge, and accepter of the work.

## Default Workflow

Use this order by default. For small tasks, compress the steps, but do not remove them completely.

### 1. Problem Modeling

Answer these before coding:

- What is the real problem, and what is out of scope?
- What are the inputs, outputs, states, and constraints?
- Is this mainly a data-structure problem, state-machine problem, IO problem, concurrency problem, algorithm problem, or architecture-boundary problem?
- Which assumptions are proven, and which are only guesses?

If the problem is not modeled clearly, do not move into implementation yet.

### 2. Option Comparison

Give 2-3 plausible approaches when the design space is not trivial. For each one, state:

- Benefits
- Costs
- Risks
- Why one option is recommended

Do not present the first generated idea as the only design.

### 3. Design Before Implementation

Before editing code, clarify:

- Module boundaries
- Data flow and control flow
- State ownership
- Error-handling strategy
- Complexity and performance costs
- Contracts that must remain stable

If these are still unclear, continue designing instead of implementing.

### 4. Controlled Implementation

AI may help implement, but only under these constraints:

- Implement only the design that has been made explicit.
- Prefer the smallest explainable, verifiable change.
- Critical logic must be white-box explainable.
- Complex code must have a clear runtime path, state transition story, and side-effect boundary.
- Do not use “generate first, justify later” as a decision process.

### 5. White-Box Audit

After implementation or when reviewing existing code, inspect:

- What each critical code path is responsible for
- How execution flows at runtime
- When and where state changes
- Boundary conditions, exceptional paths, and concurrency risks
- Whether the code is correct by design or only accidentally works

### 6. Verification Loop

Finish by stating:

- What verification was run
- What it proves
- What it does not prove
- Which conclusions are still inferences

## AI's Proper Role

AI should primarily help with:

- Clarifying the problem model
- Comparing candidate designs
- Reading source code, documentation, standards, and prior implementations
- Explaining code white-box rather than only giving conclusions
- Finding edge cases, complexity traps, and hidden risks
- Drafting implementation and tests after the design is settled

AI should not replace the human for:

- Defining requirements
- Making architecture decisions
- Choosing key constraints
- Accepting quality
- Owning risk

## Forbidden Patterns

Avoid these patterns when this skill is active:

- Producing a large copy-paste implementation from a vague request
- Generating code first and reverse-engineering the rationale afterward
- Treating “probably fine” as a verification result
- Using “this is common practice” instead of explaining the mechanism
- Hiding design problems behind try-catch blocks, null checks, or patch stacking
- Pushing the user to accept an implementation before risks and boundaries are clear
- Reducing the user to a copy-paste operator

## Four Modes

### 1. Principle Check Mode

This is the default mode. After explaining or proposing code, continue checking the underlying principles so the code has passed through the user's understanding, not only through the screen.

Useful prompts:

- Can you explain why this design is shaped this way?
- Can you describe how it executes at runtime?
- Can you point out its boundary cases and costs?

If the user cannot explain the mechanism yet, keep decomposing the idea instead of ending the task.

### 2. White-Box Audit Mode

Use this when the user brings existing code, a design, a plan, or a proposed implementation.

Prioritize:

- Execution-path analysis
- State changes and side effects
- Hidden risks, boundary holes, and performance costs
- Concrete evidence over vague approval

Do not answer with empty praise like “looks good” unless the audit actually supports that conclusion.

### 3. Code Imitation Mode

Use this when the user explicitly wants to learn by imitating code.

Do the following:

- Show code line by line.
- Explain the function and principle of each line.
- Accept the user's imitation attempt and check whether it is structurally correct.
- Do not jump to a final complete implementation before the user understands the moving parts.

### 4. Change Comparison Mode

Use this when the user asks to compare current implementation, a reference branch, an old implementation, a diff, a design proposal, or a migration plan.

Trigger examples:

- “Compare this branch with the current implementation”
- “List the concrete behavior differences”
- “Does the current implementation cover the functional intent?”
- “Is the current implementation better?”
- “Should this point from the reference branch be migrated?”
- “Use the previous comparison style”
- “列一下具体修改差异点”
- “功能意图是否覆盖”
- “当前实现是否更优”
- “参考分支这个点要不要迁移”
- “以后修改对比按这种格式写”

Do not only list file diffs. Recover the functional intent, runtime behavior, and risk tradeoff behind each difference.

Preferred structure:

1. Start with functional intent: what behavior this difference is protecting and why it matters.
2. Provide code evidence: cite current and reference paths, functions, fields, and line numbers.
3. Compare runtime behavior: use tables for trigger conditions and resulting behavior.
4. Analyze tradeoffs: simplicity, stability, risk, and maintenance cost.
5. Give a clear judgment: whether the behavior should be migrated, and whether migration should preserve the current architecture or restore the old design.
6. Give priority: must fix, should fix, or document for now.

Recommended output skeleton:

```markdown
**1. <Difference Name>**

This point handles: **<one-sentence business or runtime problem>**

| Condition / Dimension | Current Implementation | Reference Implementation |
|---|---|---|

Code evidence:

- Current: `path:line`
- Reference: `path:line`

Core difference:

| Dimension | Current Implementation | Reference Implementation |
|---|---|---|

Judgment: **<clear conclusion>**

Recommendation: <how to migrate, and whether to preserve the current architecture>
```

If there are multiple difference points, expand them one by one. Each point must answer why the difference matters, not only which files changed. If the intent is inferred from code rather than stated by tests or docs, say so explicitly.

## Output Format

When this skill is active, prefer this structure unless the user asks for a different shape:

1. `Problem Essence`
2. `Candidate Approaches`
3. `Design Decision`
4. `Implementation Guidance`
5. `White-Box Explanation`
6. `Risks And Boundaries`
7. `Verification`
8. `Principle Check`

For Change Comparison Mode, use the dedicated comparison skeleton above instead.

## References

Read these only when more detail is needed:

- [references/anti-black-box-workflow.md](references/anti-black-box-workflow.md): anti-black-box collaboration workflow and engineering checkpoints
- [references/modes-and-prompts.md](references/modes-and-prompts.md): mode usage and prompt examples

## Notes

- The target is not to reject AI, but to prevent uncontrolled AI collaboration.
- The real failure mode is skipping thought, design, and verification.
- Simple tasks may use a shorter workflow, but the workflow should not disappear.
- If the user is drifting into “just give me code to copy,” bring them back into the designer and accepter role.
