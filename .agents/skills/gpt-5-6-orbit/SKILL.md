---
name: gpt-5-6-orbit
description: "Orchestrate an explicitly invoked engineering task across visible GPT-5.6 child tasks: Sol plans, diagnoses, and reviews; Terra implements and debugs; Luna performs mechanical edits, checks, packaging, and authorized release work. Use when the user invokes $gpt-5-6-orbit or asks to run GPT-5.6 Orbit with model-specific, evidence-gated handoffs."
---

# GPT-5.6 Orbit

Coordinate one outcome through the GPT-5.6 family. Keep the invoking task as the conductor, give every child one bounded responsibility, and advance only when its evidence passes a checkable gate.

## Honor the invocation contract

- Treat explicit invocation as authorization to create the visible child tasks required by the route, with the model and effort settings below.
- Do not treat invocation as authorization for deployment, publication, destructive changes, purchases, messages, or other external side effects.
- Use Codex app task or thread tools that accept explicit `model` and `thinking` values. Do not use a generic subagent mechanism when it cannot guarantee the selected model.
- If the host cannot create and inspect visible children with explicit model and effort controls, present the proposed route and stop. Do not pretend the orbit ran.
- Keep the conductor responsible for scope, state, gates, integration, and the final report.

## Use the model contract

| Responsibility | Model ID | Default effort | Raise effort when |
| --- | --- | --- | --- |
| Plan, architecture, hard diagnosis, risky review | `gpt-5.6-sol` | `medium` | Ambiguity, cross-system decisions, conflicting evidence, or costly failure |
| Implementation, tests, refactors, bounded debugging | `gpt-5.6-terra` | `high` | Subtle invariants, broad ownership, or a high cost of first-pass failure |
| Mechanical edits, exact checks, packaging, release mechanics | `gpt-5.6-luna` | `medium` or `high` | A deterministic phase is long or has branching failure modes |

Use the exact `thinking` values accepted by the host: `low`, `medium`, `high`, `xhigh`, or `max`. Do not select `ultra` automatically; the benchmark reference does not cover it and it may introduce nested delegation.

Read [references/effort-guide.md](references/effort-guide.md) before deviating from these defaults, using `max`, or explaining a price/performance decision.

## Run preflight

1. Restate the outcome, acceptance checks, constraints, allowed mutations, and external-action authority.
2. Confirm the required task tools and the exact Sol, Terra, and Luna model IDs are available. Never silently substitute another model or lower an effort.
3. Resolve the repository project and record its branch, revision, and pre-existing dirty files. For a non-repository task, use a projectless child target.
4. Classify the task:
   - **Mechanical:** the operation and finish line are already known.
   - **Standard:** implementation is clear but requires real coding and tests.
   - **Complex:** architecture, safety, or failure modes require judgment.
   - **Incident:** current evidence must be gathered and the safe path may change.
5. Make every unauthorized external side effect a separate blocked phase.

Preflight passes when the finish line is checkable, the starting state is recorded, and the required model controls are available.

## Build the orbit

Use these starting routes, then let Sol narrow them. Every task gets a Sol planning or routing decision; do not use Sol for mechanical execution merely because it is stronger.

| Task class | Starting route |
| --- | --- |
| Mechanical | Sol `low` or `medium` route -> Luna `medium` execution; use Luna `high` for multi-file edits or branching checks |
| Standard | Sol `medium` plan -> Terra `high` implementation -> Luna `high` closeout |
| Complex | Sol `high` plan -> Terra `high` implementation -> Sol `high` review -> Luna `high` closeout |
| Incident | Sol `high` triage plan -> Luna `high` evidence collection -> Sol `high` diagnosis -> Terra `high` fix -> Luna `high` verification |

If the invoking task is confirmed to run on `gpt-5.6-sol`, it may own the Sol phase. Otherwise create a Sol child. Skip a later phase only when its deliverable would add no evidence or necessary work; record why it was skipped.

Track the actual route:

```markdown
| Phase | Child task | Model | Effort | Deliverable | Gate | Status |
| --- | --- | --- | --- | --- | --- | --- |
```

## Create and manage child tasks

1. List projects and select the exact project ID for repository work.
2. Create the child with the exact model ID, effort, prompt, and local or worktree target.
3. Record the returned task or thread ID before continuing.
4. Read the child until it reaches a terminal result; creation is not completion.
5. Send corrections to the same child without model or effort overrides while responsibility remains unchanged.
6. Create a new child when responsibility moves from Sol to Terra, Terra to Luna, or Luna back to a reasoning tier.
7. Leave child tasks visible and unarchived unless the user asks otherwise.

Give each child a self-contained brief:

```markdown
Role: <one phase responsibility>
Outcome: <one concrete result>
Inputs: <paths, revisions, URLs, evidence, and prior artifacts>
Constraints: <scope, invariants, authority, and forbidden actions>
Acceptance: <checks that prove the phase is complete>
Return: <artifact, evidence, and unresolved risks>
```

Pass actual artifacts forward: plans, diffs, files, test output, commits, or production evidence. Do not replace an artifact with a conversational summary.

## Enforce gates

- Pass a plan only when it names affected surfaces, decisions, risks, acceptance checks, and an integration path.
- Pass implementation only when requested behavior exists, focused tests pass, and unrelated user changes remain untouched.
- Pass review only when each actionable finding is fixed and rechecked or rejected with evidence.
- Pass mechanical closeout only when the exact required checks ran and their results are recorded.
- Pass release only when it was explicitly authorized and the deployed revision and health evidence are known.

Allow only one child to write to a checkout at a time. Parallelize read-only investigation or independent worktree tasks only when ownership is disjoint and the integration gate is explicit.

## Escalate economically

- Use Sol `medium` for ordinary plans and Sol `high` for ambiguous or high-risk plans.
- Use Terra `high` for real development. Use Terra `xhigh` only when subtle invariants or expensive failure justify the premium.
- Use Luna `low` only for a single exact, zero-decision operation; use `medium` for a settled runbook and `high` for multi-step mechanical work.
- Move an ambiguous Luna failure to Terra or Sol instead of raising Luna indefinitely.
- After a non-obvious Terra failure, prefer Sol `high` or `xhigh` diagnosis followed by a focused Terra correction over jumping directly to Terra `max`.
- Use `max` only when lower efforts have failed or the cost of failure clearly exceeds its benchmarked premium. Record the reason.
- Re-plan with Sol when new evidence invalidates the route.

## Separate release authority

Create a Luna release phase only when the user explicitly authorized release or deployment. Require Luna to follow the repository's existing release documentation and settled revision. If release evidence creates a new judgment call, stop the release and return the evidence to Sol; use Terra for any remediation.

## Report the completed orbit

Lead with the outcome. Then report:

- the actual child task IDs, models, efforts, and phase results;
- artifacts produced and checks passed;
- skipped phases, substitutions, retries, and escalations with reasons;
- deployment revision and health evidence when release was authorized;
- remaining blockers or risks.

Never present the proposed route as the route that actually ran.
