# GitHub Development Workflow

**Scope:** GitHub-hosted public and private repositories, including OSS and commercial projects.
**Purpose:** Execute authorized changes autonomously, validate them reliably, bound automatic recovery, and report clearly.

## Principles

- Follow explicit user instructions and repository-specific conventions; operational defaults here apply when neither overrides them. Never bypass authorization, confidentiality, or repository protections.
- Do not claim success merely because a write, commit, push, or API call succeeded.
- Respect the full set of applicable **pull-request validation** checks, not arbitrary workflows.
- Favor bounded, evidence-based recovery over indefinite retries or speculative changes.
- Treat a pull request as potentially spanning several separately authorized implementation rounds.

## Implementation rounds and milestones

An **implementation round** is a bounded unit of authorized work: an initial feature, a user-requested change, or a user-requested review-fix pass. It owns independent CI recovery counters (lint 3, code 5, transient 3). Counters persist across recovery fixes and pushes within the round. A new, genuinely authorized round resets them. "Continue checking", a CI rerun, or an autonomous recovery fix is **not** a new round. Merely inspecting a review doesn't reset counters.

An **implementation milestone** is the point when a coherent implementation or recovery attempt is complete and ready for CI validation. A round can contain multiple milestones. Milestones **never** reset recovery counters.

Implement by inspecting repository guidance and existing patterns, applying appropriate changes and relevant local validation. Git commits and GitHub File API updates can create multiple commits. **Do not launch the full CI-observation procedure for every intermediate commit.** At each milestone, record the latest PR head SHA, then discover and validate checks against that SHA. Multiple changes during one recovery attempt count as one attempt in its category.

## PR CI discovery

Identify checks applicable to the PR and milestone SHA, using GitHub's reported checks plus workflow event, branch, and path filters where practical. Monitor relevant build, lint, test, security, and external PR validation checks. Exclude unrelated deployment, release, scheduled, and maintenance workflows.

A configured workflow does not necessarily apply to every change. Avoid waiting for demonstrably inapplicable path- or branch-filtered workflows. Conversely, a **required** check that remains pending even because its workflow was filtered out remains a merge blocker. Do not assume missing checks are successful. If applicability is genuinely unclear, explain the uncertainty.

After the milestone push, query immediately and poll **up to 6 times total**, at **30-second intervals**, to discover all expected applicable PR checks. Stop only when the expected set is accounted for, not as soon as the first job appears. If checks expected to appear never materialize, report **BLOCKED** with the evidence and missing checks. An empty checks response is not success.

## PR CI execution

Once applicable checks are discovered, query their statuses immediately and poll **up to 10 times total**, at **30-second intervals**. Await **all applicable checks** to reach terminal states before acting on failures; an early failed lint job is not a reason to ignore still-running tests. Associate every result with the milestone SHA; earlier successful commits do not validate a newer head.

Evaluate skipped, cancelled, neutral, or superseded checks according to their meaning and required-check policy; do not blindly count them as success. Required checks must meet repository merge requirements.

If all relevant checks complete successfully, the milestone passes. If checks fail, examine the *complete* results, classify failures, and enter bounded recovery. If checks remain queued/running when execution polling is exhausted, report **PENDING**, not failed or blocked. Polling itself consumes no recovery attempts.

### Polling budgets and continuation

Discovery and execution are **independent observation budgets**, each fresh for a new milestone SHA. Their first poll is immediate; six and ten polls represent approximately three and five minutes of allowance, respectively.

Users may preemptively or subsequently override intervals or budgets for a specific round, or establish explicit repository-specific defaults. For historically long-running CI, an agent may recommend concrete larger values and explain why, but **must not silently extend the configured limit**.

On **PENDING**, retain/report repository, PR, head SHA, unresolved checks, polling state, and remaining recovery counters. A later user request such as "continue checking CI" resumes observation of the same SHA with a *fresh execution polling budget*, without restarting implementation or resetting recovery counters. If the head SHA changed, re-establish which SHA to validate instead of attributing old results to it. If CI has finished on resumption, process the outcome immediately.

## Autonomous CI recovery

Keep independent counters throughout an implementation round:

| Category | Max attempts | Permitted recovery |
| --- | ---: | --- |
| Lint / formatting | 3 | Fix straightforward formatter or lint issues within scope |
| Code regression | 5 | Fix build/test/validation regressions caused by the authorized changes |
| Transient CI / infrastructure | 3 | Retry evidenced temporary runner, network, or external-service failures |

Classify based on evidence from job logs and results. Increment a counter when actually initiating a recovery attempt, not simply detecting a failure. Reclassification cannot circumvent an exhausted budget. For a code or lint fix, perform related edits and local checks, finish the recovery milestone, and then validate the latest head SHA with fresh polling budgets. Infrastructure retries need not create a new commit; observe the relevant rerun.

Stop and report **BLOCKED** if the relevant category budget is exhausted, diagnosis is ambiguous, required access is unavailable, the correction would change requirements/architecture without authorization, or an operation would be destructive or out of scope. Never fabricate validation results.

## Review handling

Review processing is initiated by the user, not automatically after every milestone. An explicit user message saying a Copilot review was requested authorizes **active waiting**: record the newest prior review identifier/timestamp; poll for a *new* review every 30 seconds for up to 10 minutes. If none arrives, stop and report pending review. Otherwise use **reactive mode** when the user says a review has arrived or requests review processing. Do not initiate a new Copilot or other agentic review without user authorization.

Read the **whole review**, including review body and general summary, inline comments, threads, and actionable findings; do not limit evaluation to inline comments. Assess each finding, implement accepted fixes, and explain rejected findings.

- **Agentic/Copilot:** respond where supported, explicitly address or justify findings, and resolve addressed agent-owned threads when appropriate.
- **Human reviewers:** respond to feedback, leave a final comment tagging the human reviewer explaining how the review was handled, and **never resolve human-owned conversations** automatically. Preserve discussion for human participants.
- Do not imply a generic review body has an individually resolvable thread when it does not.

User-authorized implementation arising from a review is a *new implementation round* with fresh recovery counters. Validate its resulting milestone(s) through the same PR CI process. Do not trigger another agentic review automatically.

## Merge and post-merge validation

Green CI is not merge authorization. Merge only when the user has explicitly authorized it and repository policies allow it. Confirm required PR checks and mergeability, then merge using repository conventions.

After merging, identify the target-branch commit and monitor **applicable post-merge validation checks** (where configured), using the same discovery/execution polling limits and pending/resumption behavior. These are an explicit post-merge exception to the PR-only scope of pre-merge monitoring; do not wait for unrelated deployment or release workflows. Do not call the *merged contribution fully verified* until required post-merge validation passes. If post-merge checks fail, diagnose and report; **do not push directly to the default branch or create corrective changes without authorization**.

## End-of-round reporting and review recommendation

Report the repository/PR, validated SHA, implemented changes, check status, merge status, all consumed and remaining recovery budgets, and any pending or blocked operations.

Use precise states:
- **READY:** implementation validated, but awaiting merge authorization or further review.
- **SUCCESS:** authorized work validated, including post-merge verification if a merge was requested and executed.
- **PENDING:** checks still running when observation budget expired, or an explicitly awaited review has not arrived; continuation is possible.
- **BLOCKED:** missing expected checks, exhausted recovery budget, ambiguous/unsafe change, or other intervention needed.

At the **end of each implementation round**, recommend whether another independent **agentic review** should be **RECOMMENDED**, **OPTIONAL**, or **NOT NECESSARY**, with a brief risk-based reason. Consider architectural complexity, security/data risk, change scope, recovery churn, and prior review findings. A recommended review is **not** itself a block and does not authorize requesting one.

## Session continuity

Observation is not a background service. If execution stops, a later invocation may reconstruct the current round from the conversation and GitHub evidence; do not pretend to be polling when no agent is executing. Include enough state in the report to make resumption practical.
