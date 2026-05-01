# Daily Playbook

How to use the multi-agent setup day-to-day, after the prompt has built it.

## Mental model

There are three players in this setup:

- **You** — owner of decisions: scope, merge, deploy, escalations.
- **Main agent** (Cursor chat) — the orchestrator. Only it can delegate to subagents via the Task tool.
- **Subagents / Skills / Hooks** — tools. They do nothing on their own until the main agent invokes them (or a hook fires on an event).

**Rule #1:** start a session with a command or skill, not with a free-form "build the feature". Otherwise the system degrades to plain chat.

## Daily SDLC cycle

### 1. Starting work on a task

Begin a session with one of two paths:

- **New feature / bugfix / chore** → `/create-feature <short description>`
  The skill immediately sets: branch name, scope, Definition of Done, mandatory review before PR.

- **Researching a decision before code** → `/research <question>`
  The skill returns a comparison matrix and a recommendation — without writing code.

If the task is large and unclear — do `/research` *before* `/create-feature`. Don't mix them in one run.

### 2. Implementation

During implementation the main agent reads alwaysApply rules (Product-Context, Project-Architecture, PM-Conventions) and, by globs, domain rules (backend/frontend/API).

Your job here:

- Read the plan (3–6 steps) **before** the agent starts writing code.
- Stop if you see scope creep (the agent touches modules outside the task).
- Make decisions at "Escalate to human" points from rules (auth, migrations, S3 permissions, cookies, public API changes).

**Anti-pattern:** clicking "continue" on the plan without reading it. You lose the only moment where corrections are cheap.

### 3. Local verification

`create-feature` requires running the commands that actually exist in your project (typecheck, build, lint, tests, migrations if SQL changed). The agent must show output, not "I did it".

If something fails — don't fix it yourself. Say:

`/debug <short symptom + log>` — the debugger subagent will do root-cause analysis and propose a minimal fix.

### 4. Mandatory checks before PR

`create-feature` won't reach PR without these (in v3 they're mandatory, not recommendations):

- `/review` → reviewer subagent (readonly) checks rule compliance, contract drift, missing verification.
- `/security-audit` — only if you changed auth, cookies, multipart/S3, public APIs, rate-limits, secrets. Don't invoke on cosmetic changes.
- `/qa` — proposes edge cases and adds targeted tests if needed. Can be skipped for tiny changes, but consciously.

Each subagent returns structured output (Summary / Blocking / Non-blocking / Escalate). Your job:

- **Blocking** — fix before PR.
- **Non-blocking** — accept consciously or log as a follow-up issue.
- **Escalate** — you decide, not the agent.

### 5. PR and merge

PR description is generated from the PM-Conventions template (what/why, test plan, risks, links to review reports).

You merge. The setup does not merge or push without your explicit command.

### 6. Deploy

`/deploy` runs separately from feature sessions — never mix them. The skill walks you through the deploy path described in DevOps-Infrastructure (Docker / CI / cloud baseline) and requires a post-deploy smoke test.

## What hooks do automatically

You don't invoke hooks — they fire on Cursor events:

- **beforeShellExecution** — blocks obviously destructive commands (`rm -rf /`, raw device, `curl | bash`, destructive SQL patterns). If something legitimate gets blocked, it's a signal that either the pattern is too broad or you really are doing something risky.
- **afterFileEdit** — runs your formatter (ESLint, Prettier, whatever the prompt detected). Quick formatting fix without agent involvement.
- **subagentStop** — catches QA/reviewer failures and asks the agent to react (followup_message, with `loop_limit: 3` to avoid loops).
- **stop** — reminder to commit if `git status` is dirty. No auto-commits.

These hooks do not replace CI. CI is your external safety net.

## When NOT to do things

- Don't ask "build feature X" without `/create-feature`. You lose branch hygiene, DoD, mandatory review.
- Don't invoke two subagents in parallel from the main chat — the main agent has a synchronous flow. Parallelism only happens if explicitly defined in a skill.
- Don't let the agent edit rules or agents mid-feature-session. Changes to `.cursor/` go in a separate `chore/cursor-*` branch.
- Don't use `/security-audit` and `/qa` as "insurance for everything". Each invocation costs context. Use them targeted.
- Don't ignore the "Escalate to human" section in subagent reports. It's the only formalized channel where the agent says "this decision isn't mine".

## Mini-scenarios

**Small bugfix** (1 file, 5 lines):
`/create-feature fix X` → code → `/review` (skip qa/security if cosmetic) → PR → merge.

**New API endpoint:**
`/create-feature ...` → code + zod schema + test → `/review` → `/security-audit` (if public or auth) → `/qa` → PR.

**Refactor or architectural decision:**
`/research` → discuss with you → `/create-feature` with the chosen approach.

**Production incident:**
`/debug <logs>` → root cause → `/create-feature hotfix ...` → `/review` → merge → `/deploy`.

## Is your setup still alive?

Once a week, check:

- Is `.cursor/README.md` still accurate? (New rules/skills added to the matrix?)
- Are there subagents you never invoked? If yes — delete or rethink their description, because the main agent doesn't "hear" them.
- Are there hooks firing that you ignore? Then they're noise — narrow the pattern or remove them.

The setup is not a monument. If a piece hasn't been used in two weeks — it's dead. Better to delete than keep "for later".

## Minimal demo ritual (for video / showcase)

1. `git switch -c feat/demo`
2. `/create-feature add healthcheck field`
3. Approve plan → let it write code
4. Watch `afterFileEdit` fire (formatting)
5. `/review` → show the Output contract structure
6. Try a destructive command in terminal → show `beforeShellExecution` blocks it
7. End session → show commit-reminder from `stop` hook
8. Open `.cursor/README.md` and walk through the matrix

That's enough to demonstrate the full SDLC loop in 10 minutes.
