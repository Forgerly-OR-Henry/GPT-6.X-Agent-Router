# GPT-6.X Agent Router: 6.2

A Codex agent-routing skill that prefers GPT-6.2 Sol for routine work and GPT-6.2 Astra for specific difficult problems. It routes routine Computer Use to GPT-6.1 Terra and interactive diagnosis to GPT-6.2 Sol or Astra, depending on the task.

The name identifies the target model family. Only the model and effort combinations listed below are configured; it does not imply support for every GPT-6-series model or future model families.

## Install

The [6.2 release](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases/tag/v6.2) provides global prompt only, Skill only, and combined ZIPs. See [installation instructions for the ZIPs](../README.md#install-from-a-release).

### Install only the Skill from the repository

Copy this repository URL into Codex and ask it to install the skill:

[https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router)

You can paste this request directly:

```text
Install GPT-6.X Agent Router from https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router.
Review the repository, then install 6.2/skills/gpt-6-x-agent-router into my personal Codex skills directory.
Preserve existing local changes and report any installation conflicts instead of overwriting them.
Verify the installed skill without modifying config.toml or creating custom agent definitions.
```

This is the standard installation method. No installation scripts are provided. Codex handles installing the skill files into `$CODEX_HOME/skills/gpt-6-x-agent-router/`, or `~/.codex/skills/gpt-6-x-agent-router/` when `CODEX_HOME` is unset.

The [6.1 baseline](../6.1/README.md) uses the same skill identifier and installation path. Choose one version for that location.

If the skill does not appear after installation, start a new chat or restart the client.

## Contents

```text
./
├── README.md
├── global-prompt/
│   ├── global-AGENTS.md
│   └── instructions/
│       └── task-progress.md
└── skills/
    └── gpt-6-x-agent-router/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

The installable skill is in [`skills/gpt-6-x-agent-router`](skills/gpt-6-x-agent-router), relative to this README. The display name is **GPT-6.X Agent Router**; the internal skill identifier is `gpt-6-x-agent-router`.

The [6.2 global working agreements](global-prompt/global-AGENTS.md) and [task progress workflow](global-prompt/instructions/task-progress.md) cover requirement checks, verification status, and blocker reporting. Personal preferences remain excluded. The workflow applies only when progress tracking is warranted. Both files are references outside the skill package; installing the skill does not apply them to Codex.

## Usage

**GPT-6.2 Sol is the recommended parent model.** GPT-6.2 Astra can also serve as the parent. Select the main chat model yourself; **High** is recommended for ordinary goals and **XHigh** for complex goals. The skill does not inspect or confirm the main model, change it, or check your quota before starting.

Invoke it with a concrete task:

```text
$gpt-6-x-agent-router Complete this task within the stated scope and verify the result.
```

Normal implicit skill discovery remains enabled. Its description targets requests for this particular model-routing workflow rather than every coding task.

The parent handles small tasks directly and routes Computer Use according to the judgment the UI task requires. Allowed subagent configurations are:

| Configuration | Model ID | Reasoning effort |
| --- | --- | --- |
| GPT-6.2 Sol High | `gpt-6.2-sol` | `high` |
| GPT-6.2 Sol XHigh | `gpt-6.2-sol` | `xhigh` |
| GPT-6.2 Astra High | `gpt-6.2-astra` | `high` |
| GPT-6.2 Astra XHigh | `gpt-6.2-astra` | `xhigh` |
| GPT-6.1 Terra High | `gpt-6.1-terra` | `high` |
| GPT-6.1 Terra XHigh | `gpt-6.1-terra` | `xhigh` |

Regardless of the parent model, routine subagent work still defaults to GPT-6.2 Sol High, with XHigh for complex reasoning. GPT-6.2 Astra subagents are not a required planning or review step. The parent chooses them for specific difficult problems, then returns routine work to GPT-6.2 Sol. The skill does not promise a fixed saving or a speed improvement.

## Computer Use routing

| What the UI task requires | Route |
| --- | --- |
| Click a button and verify the expected result, or follow known reproduction steps. | **GPT-6.1 Terra High**; use XHigh if the procedure is known but the UI takes more effort to navigate or inspect. |
| Investigate a failure when each live observation determines which UI action or other diagnostic check comes next. | **GPT-6.2 Sol High** for a focused diagnosis; use XHigh for several interacting states or plausible causes. |
| Investigate a difficult issue with contradictory evidence or an uncertain root cause that requires live UI interaction. | **GPT-6.2 Astra**, at High or XHigh according to difficulty. The parent can assign Astra directly. |

Terra is limited to Computer Use and its runtime. If a known UI check fails, Terra reports the observation. Sol or Astra can analyze existing evidence without taking over the interface. When diagnosis needs live interaction, the parent can operate directly or assign it to a Sol or Astra subagent.

For every operator, the parent sets the objective, permitted actions, independent decisions, and who handles escalations. Stop any action outside that authority. Keep one active operator per interface; on handoff, pass the current state, completed actions, evidence, open decisions, and applicable permissions before the next operator acts. Neither a model switch nor higher reasoning effort expands the user's authorization.

## Requirements

- A local Codex environment that discovers standalone skills.
- Native subagent tools that support explicit model and reasoning-effort selection. The included instructions describe the `collaboration` interface, including `spawn_agent` and `fork_turns: "none"`.
- The Computer Use skill and its runtime tools must be available to the selected operator in Codex. The [OpenAI model catalog](https://developers.openai.com/api/docs/models) lists Computer Use for all three models; actual access depends on the selected Codex environment.
- Account and client access to a selected configuration when that configuration is needed. Unavailable configurations are reported rather than silently replaced.

`agents/openai.yaml` contains skill UI metadata. It is not a standalone worker definition. No custom agent TOML file, model-detection script, API key, or `config.toml` change is required for this package.

## Maintain your copy

The source folder and installed copy are separate. Edit this source first, validate it, and then explicitly synchronize an update into the installed directory.

## Validate

From the repository root, run the bundled Skill Creator validator against `6.2/skills/gpt-6-x-agent-router`:

```text
python <skill-creator-directory>/scripts/quick_validate.py 6.2/skills/gpt-6-x-agent-router
```

The validator requires PyYAML and checks basic skill structure. Passing it does not establish live model availability, correct routing on every task, task quality, or quota savings. Assess those with representative tasks in the target Codex environment.
