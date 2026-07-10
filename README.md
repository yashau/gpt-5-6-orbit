# GPT-5.6 Orbit

Plan with Sol. Build with Terra. Close out with Luna.

GPT-5.6 Orbit is a Codex skill for routing one engineering outcome through visible, model-specific child tasks. Sol owns planning and hard judgment, Terra owns most development, and Luna owns deterministic execution and verification. The invoking task remains the conductor and advances the work through evidence-gated handoffs.

## Why Orbit

The GPT-5.6 family already has a natural division of labor:

| Model | Orbit role | Default effort |
| --- | --- | --- |
| `gpt-5.6-sol` | Planning, architecture, diagnosis, risky review | Medium; High when ambiguous |
| `gpt-5.6-terra` | Implementation, tests, refactors, debugging | High |
| `gpt-5.6-luna` | Mechanical edits, checks, packaging, release mechanics | Medium or High |

Orbit makes that division explicit without forcing every model into every task. Each child receives one deliverable, one acceptance gate, and the artifacts needed for its phase.

## Default routes

```text
Mechanical
  Sol route -> Luna execute

Standard feature or fix
  Sol plan -> Terra build -> Luna close out

Complex or high-risk change
  Sol plan -> Terra build -> Sol review -> Luna close out

Incident
  Sol triage -> Luna gather evidence -> Sol diagnose -> Terra fix -> Luna verify
```

Effort selection is informed by the [DeepSWE all-effort-levels data](.agents/skills/gpt-5-6-orbit/references/effort-guide.md). The durable defaults target the price/performance knees rather than automatically buying the highest effort.

## Install

### Repository scope

Copy the skill directory into the target repository:

```text
.agents/skills/gpt-5-6-orbit/
```

### Personal scope

Copy `.agents/skills/gpt-5-6-orbit/` from this repository to:

```text
~/.agents/skills/gpt-5-6-orbit/
```

Codex normally detects skill changes automatically. Restart Codex if it does not appear.

Do not install a second copy under `~/.codex/skills`. Duplicate or legacy copies can make discovery inconsistent.

## Use

Invoke the skill with a skill mention:

```text
Use $gpt-5-6-orbit to implement this feature and verify it.
```

Other examples:

```text
Use $gpt-5-6-orbit to diagnose this production-only failure, implement a fix, and stop before deployment.
```

```text
Use $gpt-5-6-orbit to make this mechanical repository-wide change and run the required checks.
```

You can also ask in natural language:

```text
Use GPT-5.6 Orbit to implement this feature and verify it.
```

A direct request to use Orbit authorizes the visible child tasks needed by the route. An incidental implicit match does not. Neither form authorizes deployment, publication, destructive operations, or other external side effects.

### Troubleshooting discovery

If Codex says Orbit is unavailable:

1. Confirm the personal copy is at `~/.agents/skills/gpt-5-6-orbit/SKILL.md`.
2. Remove duplicate copies from legacy locations such as `~/.codex/skills/gpt-5-6-orbit`.
3. Restart Codex so it rebuilds the skill catalog.
4. Type `$` and select `gpt-5-6-orbit`, or ask directly to use GPT-5.6 Orbit.

## Requirements

Orbit requires a Codex host that can:

- create visible child tasks or threads;
- set an explicit model and reasoning effort on creation;
- read child status and results;
- continue a child with follow-up instructions;
- access `gpt-5.6-sol`, `gpt-5.6-terra`, and `gpt-5.6-luna`.

If these controls are unavailable, Orbit returns the proposed route and stops. It never silently substitutes another model or claims that a route ran when it did not.

## Design principles

- Sol makes planning and judgment calls.
- Terra performs most development work.
- Luna performs deterministic and mechanical work.
- Only one child writes to a checkout at a time.
- Dependent phases wait for actual artifacts, not summaries.
- Corrections stay with the responsible child; responsibility changes create a new child.
- Effort increases only when uncertainty or failure cost justifies it.
- Deployment is always a separate, explicitly authorized phase.

## Repository layout

```text
.
├── AGENTS.md
├── README.md
└── .agents/
    └── skills/
        └── gpt-5-6-orbit/
            ├── SKILL.md
            ├── agents/
            │   └── openai.yaml
            └── references/
                └── effort-guide.md
```

## Status

This is an independent community skill built for the GPT-5.6 model family. It is not affiliated with or endorsed by OpenAI or DataCurve.
