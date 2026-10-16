# pstack: workflow and reusable orchestration

[Arabic diagram atlas](diagrams/pstack-workflow.html) · [Overview SVG](diagrams/pstack-overview.svg) · [Review loop SVG](diagrams/pstack-review-loop.svg) · [Proposed generalization SVG](diagrams/pstack-generalization.svg)

This analysis describes Cursor pstack 0.15.25 at `f713f14203cd6420cec13cd7c55c32d598dbab68` and the Open Pstack adaptations in this candidate. It is a source-grounded model, not a claim that a live agent run exercised every workflow.

## What executes what

pstack is primarily an instruction system, not a general-purpose workflow engine. The current chat reads `poteto-mode`, classifies intent into one of 23 playbooks, opens a todo list from the selected steps, and invokes focused skills as needed. The latest upstream explicitly makes that chat the runner; `poteto-agent` is a delegate. Principles constrain how the agent reasons. Only selected concerns have executable enforcement: plan checking, orchestration state, provider receipts, PR watching, and this port's live-gate verifier.

The `orch` CLI stores facts. It never spawns, waits for, or wakes an agent. The parent harness performs those operations. Unit states are strings in `Unit.state`, not an exhaustively enforced finite-state machine. The diagrams show conceptual workflow transitions rather than claiming a scheduler implements them all.

## Triggers and routing

- Explicit `poteto-mode`, a task needing rigor, or the Claude startup route selects a playbook. Codex requires the skill to be requested; it does not load the Claude startup hook.
- Questions about current structure route to Investigation and `how`; historical intent uses `why` and available source/MCP categories. Read-only work can finish with an evidenced answer.
- Changes route to Feature, Bug fix, Refactoring, Perf, Prototype, or another specific playbook. Verification is chosen for the actual surface.
- A single long task uses Autonomous run with a done predicate and event wake or heartbeat. A standing multi-day program uses Orchestrate; it should collapse back to simpler execution when one session can finish it.
- Autopilot-full handles independent PR owners with landing authority. Autopilot-stack delivers a verified stack for the operator to land. A request to state a plan is not execution approval.
- `in-your-own-words` confirms understanding, `sharpen` improves task framing, and `ketchup` explains changes and pending user decisions. Ketchup is read-only and uses existing artifacts.

## Roles and ownership

| Role | Owns | Boundary |
| --- | --- | --- |
| User/operator | Goal, constraints, reserved decisions, stop/go | An autonomy grant does not override an explicit gate |
| Parent/coordinator | Routing, briefs, state, queue drains, aggregate decisions | Orchestrate coordinator delegates code changes |
| Track coordinator | A bounded track when one root cannot drain it | Summaries upward; no raw child floods |
| Worker / PR owner | One unit or PR, isolated output, durable evidence | One writer per branch/worktree; no topology changes in Orchestrate |
| Verifier / review panel | Independent evidence or critique | A self-report and green CI are not an independent verdict |
| Stacker | Serial changes to a stack's topology | One stacker per stack; current heads and bases matter |
| Watcher | Completion or CI/ref events | Wakes the parent; does not grant merge permission |

Arena generates parallel complete candidates, applies a private rubric, cross-judges after candidates finish, synthesizes the best result, then verifies. Swarm partitions coverage, races identical briefs, or mixes both; the selection rule is declared first. Missing coverage remains a gap. Interrogate produces a synthesized verdict and does not automatically apply its suggestions.

## Durable artifacts and states

| Artifact | Meaning |
| --- | --- |
| `preferences.md` | Numbered standing orders carried into delegation and resumption |
| brief | Goal, scope, acceptance, verification, forbidden actions, report contract; dependencies relay context |
| `units.tsv` | Unit id, track, state, branch, PR, SHA, brief path |
| `inbox/` | Completion pointers: agent, unit, status, report path |
| `ledger.tsv` | PR + exact SHA + verdict + evidence + verifier + timestamp |
| `frontier.json` | Computed PR chain, branch/head identities, generation, lowest unmerged PR |
| `gates.md` | Open/resolved human decisions with options and recorded answers |
| `decisions.tsv` / `overview.md` | Decision trail and durable program record |
| `status.md` | Derived counts and gates; not a second manually maintained truth |
| runner receipt | Provider/model/effort and terminal result of an external lane |

Conceptually work moves from framed → ready → running → needs-verify → accepted/landed. Failures produce fix work, explicit blocks, smaller retries, abandonment/replanning, or a human gate. Late zombie results are reconciled against current state. The implementation records rather than universally enforces these state transitions. Ledger verdicts are typed: `live-ui-verified`, `unit-test-verified`, `type-check-only`, `verifier-blocked`, `verifier-failed`. A new SHA invalidates the prior verdict. Blocked is not pass.

## Loops and gates

1. **Pilot before scale.** Test one unit's brief, acceptance recipe, and integration path before a rolling window of workers. Small repetitive work uses a proportionally small pilot.
2. **Completion queue.** A notification enqueues a pointer. Drain after a critical section, track rollup, frontier wake, or before a human report. A drain classifies pointers, updates state, emits status, and refills ready work. Deep review becomes another unit.
3. **Verification/fix loop.** Collect actual artifact evidence, challenge it independently when warranted, send proven findings to its owner, fix the root cause, and verify the new version. Tests do not replace proof on the actual user surface.
4. **Delivery loop.** Freeze the stack, verify each patch, check current head/base and authorization, land only the permitted verified frontier, then recompute. This port retains stricter fork, queue, expected-head and lease protections than upstream's shortened instructions. This task authorizes no merge.
5. **Liveness/recovery.** Retain child handles and durable outputs. This port requires affirmative failure evidence or an explicit caller deadline; elapsed time alone is not failure. Retry/replan must preserve the configured provider policy. Pause records a handoff; pickup reconstructs from artifacts, not a stale summary.
6. **Learning loop.** Reflect reviews the actual transcript through multiple lenses, synthesizes Accepted/Rejected/Backlog, and seeks approval for accepted skill edits. Correct prefers structural enforcement over repeated prose. These are situational skills, not mandatory steps on every task.

The Open Pstack repository adds an exact-head `live-gate` before merge/release. Builders cannot publish it. The verifier records publisher and candidate revisions separately and checks real installed Claude/Codex behavior. For this candidate, doctor stops on Linux because its supported operator environment is Darwin; unit/static results do not satisfy that gate.

## The portable layer versus software adapters

| Reusable logic | Current software-specific realization | Proposed adapter outside SWE |
| --- | --- | --- |
| Goal + acceptance contract | Feature behavior, defect reproduction | Research question, design brief, operational outcome |
| Isolated ownership | Branch/worktree | Versioned document, dataset snapshot, work package |
| Alternative generation | Arena code/design attempts | Competing analyses or creative directions |
| Evidence-based judgment | Tests, UI recordings, traces | Source audit, rubric, controlled observation |
| Immutable version identity | Git SHA | Content digest + source snapshot/version ID |
| Dependency frontier | PR base chain | Work-package dependency DAG and release order |
| Bounded authority | Merge/release/deploy gates | Publish/send/spend/commitment gates |
| Durable recovery | TSV/JSON, pushes, receipts | Append-only run log and durable artifacts |
| Learning | Skill/lint/test edits | Approved method, checklist, policy or tool improvements |

This last column is a proposal, not an existing non-SWE product. Start with one domain adapter specifying input sources, allowed tools, artifact schema, acceptance evaluator, permissions, and delivery action. Preserve independent judgment where it adds value. For research, require traceable sources, contradictions, and freshness checks; for design, require a rubric and actual visual/user evaluation; for operations, require state reconciliation and action authorization. Do not relabel CI as evidence for every domain, assume subjective goals have a binary oracle, or automatically repeat irreversible delivery actions. Pilot one domain before building a universal engine.

## Source map

All source links pin the inspected upstream snapshot:

- [Router and principles](https://github.com/cursor/plugins/blob/f713f14203cd6420cec13cd7c55c32d598dbab68/pstack/skills/poteto-mode/SKILL.md)
- [Orchestrate roles, store, queue, gates and recovery](https://github.com/cursor/plugins/blob/f713f14203cd6420cec13cd7c55c32d598dbab68/pstack/skills/poteto-mode/playbooks/orchestrate.md)
- [Executable state types and store](https://github.com/cursor/plugins/blob/f713f14203cd6420cec13cd7c55c32d598dbab68/pstack/skills/poteto-mode/scripts/orch/store.ts)
- [Autopilot-full](https://github.com/cursor/plugins/blob/f713f14203cd6420cec13cd7c55c32d598dbab68/pstack/skills/poteto-mode/playbooks/autopilot-full.md), [Autopilot-stack](https://github.com/cursor/plugins/blob/f713f14203cd6420cec13cd7c55c32d598dbab68/pstack/skills/poteto-mode/playbooks/autopilot-stack.md)
- [Arena](https://github.com/cursor/plugins/blob/f713f14203cd6420cec13cd7c55c32d598dbab68/pstack/skills/arena/SKILL.md), [Swarm](https://github.com/cursor/plugins/blob/f713f14203cd6420cec13cd7c55c32d598dbab68/pstack/skills/swarm/SKILL.md), [Interrogate](https://github.com/cursor/plugins/blob/f713f14203cd6420cec13cd7c55c32d598dbab68/pstack/skills/interrogate/SKILL.md)
- [Local harness boundary](../plugins/pstack/skills/poteto-mode/references/codex-tools.md), [provider dispatch](../plugins/pstack/skills/poteto-mode/references/provider-dispatch.md), [repository gate](../AGENTS.md)
- [Diagram-design 2.6](https://github.com/cathrynlavery/diagram-design/blob/main/skills/diagram-design/SKILL.md), MIT; default palette explicitly selected. Original diagram markup follows its layout guidance without copying template code. Arabic uses the available DejaVu Sans fallback; no external font dependency.
