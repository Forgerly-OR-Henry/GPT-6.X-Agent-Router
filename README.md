# GPT-6.X Agent Router

A Codex agent-routing skill for the GPT-6 series, with GPT-6.1-Sol or GPT-6-Astra as the parent, GPT-6.1-Sol preferred for routine subagent work, GPT-6-Astra specialists for specific difficult problems, and GPT-6.1-Terra dedicated to Computer Use skill execution.

The name identifies the target model family. Only the model and effort combinations listed below are configured; it does not imply support for every GPT-6-series model or future model families.

## Install with Codex

Copy this repository URL into Codex and ask it to install the skill:

[https://github.com/Forgerly-OR-Henry/GPT-6.1-Sol-Router](https://github.com/Forgerly-OR-Henry/GPT-6.1-Sol-Router)

You can paste this request directly:

```text
Install GPT-6.X Agent Router from https://github.com/Forgerly-OR-Henry/GPT-6.1-Sol-Router.
Review the repository, then install skills/gpt-6-x-agent-router into my personal Codex skills directory.
Preserve existing local changes and report any installation conflicts instead of overwriting them.
Verify the installed skill without modifying config.toml or creating custom agent definitions.
```

This is the standard installation method. No installation scripts are provided. Codex handles installing the skill files into `$CODEX_HOME/skills/gpt-6-x-agent-router/`, or `~/.codex/skills/gpt-6-x-agent-router/` when `CODEX_HOME` is unset.

If the skill does not appear after installation, start a new chat or restart the client.

## Contents

```text
./
├── README.md
└── skills/
    └── gpt-6-x-agent-router/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

The installable skill is in `skills/gpt-6-x-agent-router`. The display name is **GPT-6.X Agent Router**; the internal skill identifier is `gpt-6-x-agent-router`.

## Usage

**GPT-6.1-Sol is the recommended parent model.** GPT-6-Astra can also serve as the parent. Select the main chat model yourself; **High** is recommended for ordinary goals and **XHigh** for complex goals. The skill does not inspect or confirm the main model, change it, or check your quota before starting.

Invoke it with a concrete task:

```text
$gpt-6-x-agent-router Complete this task within the stated scope and verify the result.
```

Normal implicit skill discovery remains enabled. Its description targets requests for this particular model-routing workflow rather than every coding task.

The parent handles small tasks directly, except for Computer Use skill execution, which it delegates to GPT-6.1-Terra. Allowed subagent configurations are:

| Configuration | Model ID | Reasoning effort |
| --- | --- | --- |
| GPT-6.1-Sol High | `gpt-6.1-sol` | `high` |
| GPT-6.1-Sol XHigh | `gpt-6.1-sol` | `xhigh` |
| GPT-6-Astra High | `gpt-6-astra` | `high` |
| GPT-6-Astra XHigh | `gpt-6-astra` | `xhigh` |
| GPT-6.1-Terra High | `gpt-6.1-terra` | `high` |
| GPT-6.1-Terra XHigh | `gpt-6.1-terra` | `xhigh` |

Regardless of the parent model, routine subagent work still defaults to GPT-6.1-Sol High, with XHigh for complex reasoning. GPT-6-Astra subagents are not a required planning or review step. The parent chooses them for specific difficult problems, then returns routine work to GPT-6.1-Sol. The skill does not promise a fixed saving or a speed improvement.

## Computer Use delegation

GPT-6.1-Terra executes only the Computer Use skill and its required runtime operations. Use **High for simple interfaces** and **XHigh for complex interfaces**, as judged by the parent for the current task.

The parent defines Terra's objective, operating scope, independent decision boundary, and the agent responsible for decisions in each task. The skill does not prescribe a fixed catalogue of business-action permissions. Within that grant, Terra can choose buttons, navigation paths, and input steps. Deeper decisions or actions beyond the grant go to the parent or a designated GPT-6.1-Sol or GPT-6-Astra subagent; Terra waits for the decision before continuing the affected action. A designated subagent may decide only within the authority assigned by the parent. Only the parent may revise Terra's grant within existing user and platform permissions. XHigh does not expand Terra's authority.

This is an instruction-level division of work, not a separate tool sandbox. Only one agent operates a shared interface at a time. For a handoff, pause the previous operator's UI work and pass the current state and completed actions to the replacement; a handoff alone does not authorize closing the application or ending its ongoing tasks.

## Requirements

- A local Codex environment that discovers standalone skills.
- Native subagent tools that support explicit model and reasoning-effort selection. The included instructions describe the `collaboration` interface, including `spawn_agent` and `fork_turns: "none"`.
- The Computer Use skill and its runtime tools must be available to the Terra subagent when UI execution is needed.
- Account and client access to a selected configuration when that configuration is needed. Unavailable configurations are reported rather than silently replaced.

`agents/openai.yaml` contains skill UI metadata. It is not a standalone worker definition. No custom agent TOML file, model-detection script, API key, or `config.toml` change is required for this package.

## Maintain your copy

The source folder and installed copy are separate. Edit this source first, validate it, and then explicitly synchronize an update into the installed directory.

## Validate

When the bundled Skill Creator validator is available, run it against `skills/gpt-6-x-agent-router`:

```text
python <skill-creator-directory>/scripts/quick_validate.py skills/gpt-6-x-agent-router
```

The validator requires PyYAML and checks basic skill structure. Passing it does not establish live model availability, correct routing on every task, task quality, or quota savings. Assess those with representative tasks in the target Codex environment.
