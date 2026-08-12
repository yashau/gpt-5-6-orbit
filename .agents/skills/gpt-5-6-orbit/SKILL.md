---
name: gpt-5-6-orbit
description: "Orchestrate engineering work across visible GPT-5.6 child tasks: Sol plans, diagnoses, and reviews; Terra implements and debugs; Luna performs mechanical edits, checks, packaging, and authorized release work. Use when the user invokes $gpt-5-6-orbit or asks to use or run GPT-5.6 Orbit with model-specific, evidence-gated handoffs."
---

# GPT-5.6 Orbit

Coordinate one outcome through the GPT-5.6 family. Keep the invoking task as the conductor, give every child one bounded responsibility, and advance only when its evidence passes a checkable gate.

## Honor the invocation contract

- Treat a direct Orbit request, whether made through `$gpt-5-6-orbit` or natural language, as authorization to create the visible child tasks required by the route with the model and effort settings below.
- Treat authorization to change a repository as authorization to create or use an isolated task branch and make scoped local checkpoint commits unless the user says otherwise.
- If Codex loads this skill only because the request implicitly matches its description, present the proposed route and obtain confirmation before creating child tasks.
- Do not treat invocation as authorization for pushing commits, deployment, publication, destructive changes, purchases, messages, or other external side effects.
- Use Codex app task or thread tools that accept explicit `model` and `thinking` values. Do not use a generic subagent mechanism when it cannot guarantee the selected model.
- If the host cannot create and inspect visible children with explicit model and effort controls, present the proposed route and stop. Do not pretend the orbit ran.
- Keep the conductor responsible for scope, state, gates, integration, and the final report.

## Reuse loaded skill context

- Load this `SKILL.md` once when the root Orbit task begins.
- Do not reread or reinject the skill on routine follow-ups, resumes, corrections, monitoring turns, or child handoffs. Continue from the route ledger and preserved task state.
- Reread the skill only when starting a different root task, when the user asks to refresh it, when the file changed after the task began, or when context recovery genuinely lost instructions required to proceed safely.
- Read `references/effort-guide.md` only when choosing a non-default effort or explaining price/performance. Reuse its conclusions for the rest of the task.
- Never paste the complete skill into a child brief. Pass only the role, slice, constraints, acceptance checks, and artifacts that child needs.

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
3. Check the working target with `git rev-parse --is-inside-work-tree`. Only when it succeeds and returns `true`, resolve the repository project and record its branch, revision, remote, push authority, and pre-existing dirty files. Otherwise classify the target as non-repository and use a projectless child target.
4. Classify the task:
   - **Mechanical:** the operation and finish line are already known.
   - **Standard:** implementation is clear but requires real coding and tests.
   - **Complex:** architecture, safety, or failure modes require judgment.
   - **Incident:** current evidence must be gathered and the safe path may change.
5. Make every unauthorized external side effect a separate blocked or deferred phase. Lack of push authority must not block local implementation and checkpoint commits.

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

## Bound every child slice

Decompose the Sol plan before creating implementation or closeout children. Call a slice bounded only when it has:

- one coherent capability or mechanical outcome;
- one primary write scope or declared integration seam;
- focused acceptance checks that a child can reasonably complete in one turn;
- explicit exclusions naming adjacent work that remains for later slices.

Split work that combines independent pages, features, or subsystems; mixes broad cleanup with feature conversion; or attaches an exhaustive browser or viewport matrix to a large implementation batch. Do not call a batch bounded merely because one model owns all of it.

Require Sol to produce an execution-slice table for multi-surface work:

```markdown
| Slice | Owner | Write scope | Inputs | Acceptance | Depends on |
| --- | --- | --- | --- | --- | --- |
```

Give each independent slice a fresh child. Run write slices sequentially in one checkout. Use parallel worktrees only when write scopes are disjoint and the integration gate is explicit.

Do not impose one-thread-per-model or a fixed child-count cap. Create one fresh Terra child per bounded implementation slice and as many Sol, Terra, or Luna children as the accepted route requires. Let dependency order and write isolation determine concurrency; do not preserve an overloaded thread merely to reduce thread count.

## Checkpoint version control

- Apply this section only when preflight confirmed a Git worktree. For a non-repository target, skip all branch, worktree, commit, remote, and push behavior; do not initialize a repository solely to create checkpoints.
- Keep repository-writing work off `main` and `master` unless the user explicitly asks to work there. If the checkout is already on a non-default task branch intended for the request, keep it. Otherwise create a descriptive `codex/<task-slug>` branch before the first edit.
- Prefer a branch in the current checkout for the normal sequential route. Use a separate worktree with its own branch only for disjoint parallel write slices or when the current checkout must remain untouched; require an explicit integration gate before accepting worktree output.
- Preserve pre-existing dirty files. Stage only explicit task paths, inspect the staged diff, and never absorb unrelated user changes into a checkpoint.
- After each accepted implementation slice or meaningful integrated milestone, run its focused checks, commit the coherent result, and record the commit hash. Do not create empty or knowingly broken commits merely to satisfy a cadence.
- Make pushing each accepted checkpoint the normal remote cadence after the user explicitly authorizes pushes. Obtain that authority once before the first push when it is absent; continue local work and commits while push remains deferred.
- On the first push, set the task branch's upstream. Push only the recorded task branch, never force-push, and never push directly to `main` or `master` unless the user explicitly requests it.
- If authentication, permissions, branch protection, or remote rejection blocks a push, preserve the local commits, record the exact failure, and continue any safe local phases. Do not rewrite history or broaden authority to recover.
- Treat checkpoint commits and push results as handoff artifacts. For parallel worktrees, integrate and verify on the designated integration branch before committing and pushing the integrated checkpoint.

## Create and manage child tasks

1. List projects and select the exact project ID for repository work.
2. Create the child with the exact model ID, effort, prompt, and local or worktree target.
3. Record the returned task or thread ID before continuing.
4. Read the child until it reaches a terminal result; creation is not completion.
5. Continue the same child only for a small correction to its current slice when its context remains focused and it made a meaningful attempt.
6. Create a fresh child when a new slice starts, responsibility changes, scope expands materially, the existing context contains unrelated project history, or the child stopped before meaningful work.
7. Brief a fresh child from canonical artifacts and current repository state. Do not copy the overloaded thread transcript into it.
8. Leave child tasks visible and unarchived unless the user asks otherwise.

Give each child a self-contained brief:

```markdown
Role: <one phase responsibility>
Outcome: <one concrete result>
Inputs: <paths, revisions, URLs, evidence, and prior artifacts>
Constraints: <scope, invariants, authority, and forbidden actions>
Excluded: <adjacent work deliberately left to other slices>
Acceptance: <checks that prove the phase is complete>
Return: <artifact, evidence, and unresolved risks>
```

Pass actual artifacts forward: plans, diffs, files, test output, commits, or production evidence. Do not replace an artifact with a conversational summary.

## Recover from premature stops

- Do not treat "I cannot finish within this turn" as a code, runtime, or permission blocker.
- Classify a Windows child failure that occurs before its first command, especially `CreateProcessAsUserW failed: 5 (Access is denied)`, as a Codex for Windows child-runner failure rather than a model, repository, or Orbit failure. The child's Full access setting does not prevent a failure during process creation.
- Report that blocker plainly: state that Codex could not launch the Windows child process, preserve the route and repository state, and recommend restarting Codex before resuming the goal and retrying with a fresh child.
- Do not suggest that changing Orbit instructions, lowering the model contract, or granting Full access again will repair this process-launch failure.
- Inspect the child record for tool use, edits, tests, and a concrete handoff.
- If the child stopped before a meaningful attempt, mark the slice as an orchestration failure, narrow it, and start a fresh child.
- If useful partial work exists and the slice remains focused, continue the same child with one exact next action and acceptance check.
- If the stop exposes ambiguity or a bad decomposition, return the evidence to Sol and rebuild the remaining slice table.
- After two ineffective attempts on one slice, require Sol diagnosis before assigning another implementation child.
- Never report that a model is unable to do the work solely because a context-heavy child self-stopped.
- Keep creating fresh bounded children until every accepted slice passes or a concrete external blocker prevents progress.

## Enforce gates

- Pass a plan only when it names affected surfaces, decisions, risks, bounded execution slices, acceptance checks, and an integration path.
- Pass implementation only when requested behavior exists, focused tests pass, and unrelated user changes remain untouched.
- Pass review only when each actionable finding is fixed and rechecked or rejected with evidence.
- Pass mechanical closeout only when the exact required checks ran, accepted changes have checkpoint commits, authorized pushes succeeded or are explicitly reported as deferred or blocked, and the results are recorded.
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

Push authority does not authorize opening a pull request, merging, releasing, or deploying. Treat each of those as a separate external action.

## Report the completed orbit

Lead with the outcome. Then report:

- the actual child task IDs, models, efforts, and phase results;
- artifacts produced, checks passed, task branch, checkpoint commits, and push state;
- skipped phases, substitutions, retries, and escalations with reasons;
- deployment revision and health evidence when release was authorized;
- remaining blockers or risks.

Never present the proposed route as the route that actually ran.
