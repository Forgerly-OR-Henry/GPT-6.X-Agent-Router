---
name: gpt-6-1-sol-router
description: Use when the user wants GPT-6.1-Sol to lead work and select GPT-6.1-Sol or GPT-6-Astra subagents by task difficulty. Keep routine work with GPT-6.1-Sol and involve GPT-6-Astra only for specific difficult judgments or problems. Subagents use only high or xhigh reasoning effort.
---

# GPT-6.1-Sol Router

## Usage agreement

Recommend that the user select GPT-6.1-Sol as the parent model before invoking this skill, with at least High reasoning effort; recommend XHigh for complex goals. This is user setup guidance, not a startup gate.

Start the requested work immediately after invocation:
- Do not query, infer, or verify the parent model or its reasoning effort.
- Do not inspect configuration, session logs, or usage limits to confirm model identity.
- Do not ask the user to confirm their model again or switch the main chat's model.
- The user controls the actual parent model; this skill does not guarantee that the parent is GPT-6.1-Sol.

This skill changes the division of work, not the task's authorization. Discussion, planning, and read-only review remain read-only. File changes and external actions retain the current task's authorization requirements.

Apply this routing policy to the remaining stages of the current goal until it is completed or the user changes the requirements.

## Parent responsibilities

Understand the goal, establish constraints, choose how to execute, coordinate dependencies, handle feedback, and deliver the result.

Complete small, straightforward tasks directly when the parent can do so reliably. Create a subagent when independent execution, parallel work, or keeping substantial investigation output out of the main context offers a concrete benefit.

Default to GPT-6.1-Sol. Do not schedule GPT-6-Astra for mandatory opening plans, continuous supervision, or final reviews. Every GPT-6-Astra assignment must address a specific unresolved problem.

## Allowed subagent configurations

Explicitly supply both the model and reasoning effort on every spawn. Use only these combinations:

| Configuration | `model` | `reasoning_effort` | Primary use |
| --- | --- | --- | --- |
| GPT-6.1-Sol High | `gpt-6.1-sol` | `high` | Default for implementation with an established approach, investigation, tests, documentation, and focused fixes. |
| GPT-6.1-Sol XHigh | `gpt-6.1-sol` | `xhigh` | Bounded tasks requiring complex reasoning, ordering analysis, or preservation of interacting constraints. |
| GPT-6-Astra High | `gpt-6-astra` | `high` | Difficult tradeoffs, checking premises or task decomposition, conflicting evidence, or a key problem GPT-6.1-Sol cannot effectively advance. |
| GPT-6-Astra XHigh | `gpt-6-astra` | `xhigh` | The hardest key problems, including strongly conflicting constraints, difficult root-cause analysis, and deep reasoning about a core design. |

Do not use other models or effort levels, and do not omit parameters in reliance on parent inheritance. The user's latest explicit restrictions on model use take precedence over these defaults.

## Choosing a configuration

Start with GPT-6.1-Sol High unless the task provides a concrete reason for another configuration.

Use GPT-6.1-Sol XHigh for a specific reasoning challenge, such as:
- State transitions, concurrency ordering, transaction consistency, or a complex algorithm.
- Coupled modules where a local change must preserve behavior elsewhere.
- Several plausible causes that must be eliminated using evidence.
- Verification requiring meaningful counterexamples or subtle boundary analysis.

Use GPT-6-Astra High when stronger judgment could resolve a specific obstacle, such as:
- The current approach or task decomposition may rely on a false premise.
- Several reasonable approaches require a consequential tradeoff that GPT-6.1-Sol cannot adequately justify.
- Conflicting evidence prevents a reliable conclusion.
- Evidence-based attempts by GPT-6.1-Sol have not advanced a key part of the task.

Use GPT-6-Astra XHigh for the most demanding forms of these problems, particularly when constraints conflict and a consequential conclusion is difficult to validate.

Choose an appropriate configuration directly; do not require a progression through all four. File count, code length, elapsed time, or an ordinary test failure alone does not justify escalation.

Missing permissions, unavailable environments, insufficient information, and unclear user goals need their corresponding remedy. Changing models does not resolve these blockers.

## Delegating work

Use the native subagent tools available in the current chat. Do not create separate user-owned chats, launch another Codex process, or switch to a paid API to implement delegation.

With the `collaboration` interface:
- Create subagents with `spawn_agent`.
- Explicitly set `model` and `reasoning_effort` to an allowed combination.
- Set `fork_turns: "none"` and provide the necessary context in the task message.
- Continue work with the same configuration through follow-up messages or `followup_task`.
- To change model or effort, create a subagent with the required configuration. Do not claim that the existing agent's configuration has changed.

Give each work package:
- Its objective and expected deliverable.
- User requirements, constraints, and important original wording that must be preserved.
- Relevant files or source locations and established facts.
- Allowed actions and scope, including whether the assignment is read-only or may edit files.
- Dependencies, interface agreements, and acceptance criteria.
- Previous attempts, existing results, and unresolved questions.
- Conditions that require stopping and reporting to the parent.

Delegate independently verifiable outcomes. Do not mechanically split reading files, changing functions, and running commands into different agents.

Pass necessary context and evidence locations rather than the entire conversation or unrelated logs. Distinguish facts, hypotheses, and unverified conclusions; do not present the parent's assumptions as facts.

## Parallel work and file ownership

Start with one subagent. Add parallel work only when assignments can progress independently. By default, run at most two subagents concurrently, subject to the environment's actual limits.

Assign file ownership for editing tasks. Give a file to only one writer at a time, and sequence changes with dependencies.

Tell subagents to complete their assignment without spawning further agents. Keep additional delegation under the parent's control.

While waiting, the parent may handle independent work that does not conflict with an assignment. Do not duplicate delegated work.

## GPT-6-Astra assignments

Give GPT-6-Astra the original goal, key constraints, relevant evidence, and the specific difficulty. Explicitly allow it to:
- Identify incorrect premises.
- Challenge the current task decomposition.
- Point out missing constraints or acceptance criteria.
- Recommend reframing the problem.

Do not restrict GPT-6-Astra to validating a conclusion the parent already prefers.

As needed, assign GPT-6-Astra a design judgment, a focused review, or implementation of a difficult part. Once that problem is resolved, return routine implementation, testing, and documentation to GPT-6.1-Sol.

Request a conclusion with supporting evidence, conditions under which it applies, unresolved uncertainty, and instructions for subsequent execution and verification. Do not retain GPT-6-Astra on routine work merely to preserve its context.

## Feedback, escalation, and blockers

Ask subagents to execute, verify, and fix ordinary failures autonomously within their authorization instead of seeking permission for each step.

When a premise fails, the scope needs to expand, the evidence cannot support a conclusion, or one evidence-based correction still produces no progress, require a report with:
- The current blocker.
- Established facts and evidence locations.
- Attempted approaches and their results.
- The specific decision or reassignment needed from the parent.

Use that report to gather facts, narrow the task, revise the approach, or select another allowed configuration. Do not repeatedly spawn agents with unchanged input or require a new agent to redo valid completed work.

If the selected model or effort is unsupported, quota is exhausted, or the worker cannot start:
- Do not repeatedly probe or silently substitute a model or effort.
- Briefly report the actual error.
- Continue authorized work that does not depend on the missing result.
- Explain options for blocked work and ask only when a user decision is necessary.

Select models autonomously under these rules. Do not add a confirmation step for every delegation. Existing authorization requirements and platform approvals still apply.

## Verification and delivery

Require proportionate verification from execution subagents and a concise report of:
- Completed outputs and their file or evidence locations.
- Checks actually performed and their results.
- Incomplete, failed, or unverified aspects.
- Anything the parent must address.

Inspect key outputs, scope, interfaces, and verification evidence. Do not accept only a completion claim, and do not routinely repeat the worker's entire investigation or test run.

Add independent review or repeat checks only for a concrete concern, significant risk, or new change. GPT-6-Astra is not a mandatory delivery gate.

When expert conclusions conflict with observed results, return the evidence for reconciliation. Do not conceal the disagreement or rewrite conclusions without justification.

Before delivery, ensure relevant execution has finished and clean up temporary validation processes started for this task according to the current project's rules.

Use normal progress updates to explain meaningful assignments briefly. When calling GPT-6-Astra, identify the specific problem it will address without producing a lengthy routing log.

Report results, verification, and remaining limitations, and briefly state whether GPT-6-Astra was used. Do not claim a specific allowance, monetary, or percentage saving without actual measurement.
