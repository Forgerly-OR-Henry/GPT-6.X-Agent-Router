# GPT-6.1-Sol Router

A Codex skill for GPT-6.1-Sol-led work with GPT-6-Astra specialists used only when a specific problem warrants them.

## Install with Codex

Copy this repository URL into Codex and ask it to install the skill:

[https://github.com/Forgerly-OR-Henry/GPT-6.1-Sol-Router](https://github.com/Forgerly-OR-Henry/GPT-6.1-Sol-Router)

You can paste this request directly:

```text
Install GPT-6.1-Sol Router from https://github.com/Forgerly-OR-Henry/GPT-6.1-Sol-Router.
Review the repository, then install skills/gpt-6-1-sol-router into my personal Codex skills directory.
Preserve existing local changes and report any installation conflicts instead of overwriting them.
Verify the installed skill without modifying config.toml or creating custom agent definitions.
```

This is the standard installation method. No installation scripts are provided. Codex handles installing the skill files into `$CODEX_HOME/skills/gpt-6-1-sol-router/`, or `~/.codex/skills/gpt-6-1-sol-router/` when `CODEX_HOME` is unset.

If the skill does not appear after installation, start a new chat or restart the client.

## Contents

```text
GPT-6.1-Sol Router/
├── README.md
└── skills/
    └── gpt-6-1-sol-router/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

The installable skill is in `skills/gpt-6-1-sol-router`. The display name is **GPT-6.1-Sol Router**; the internal skill identifier is `gpt-6-1-sol-router`.

## Usage

Select **GPT-6.1-Sol** as the main chat model yourself. Use at least **High** reasoning effort; **XHigh** is recommended for complex goals. The skill does not inspect or confirm the main model, change it, or check your quota before starting.

Invoke it with a concrete task:

```text
$gpt-6-1-sol-router Complete this task within the stated scope and verify the result.
```

Normal implicit skill discovery remains enabled. Its description targets requests for this particular model-routing workflow rather than every coding task.

The parent handles small tasks directly. When delegation helps, it chooses only:

| Configuration | Model ID | Reasoning effort |
| --- | --- | --- |
| GPT-6.1-Sol High | `gpt-6.1-sol` | `high` |
| GPT-6.1-Sol XHigh | `gpt-6.1-sol` | `xhigh` |
| GPT-6-Astra High | `gpt-6-astra` | `high` |
| GPT-6-Astra XHigh | `gpt-6-astra` | `xhigh` |

GPT-6-Astra is not a required planning or review step. The parent chooses it for a specific difficult problem, then returns routine work to GPT-6.1-Sol. The skill does not promise a fixed saving or a speed improvement.

## Requirements

- A local Codex environment that discovers standalone skills.
- Native subagent tools that support explicit model and reasoning-effort selection. The included instructions describe the `collaboration` interface, including `spawn_agent` and `fork_turns: "none"`.
- Account and client access to a selected configuration when that configuration is needed. Unavailable configurations are reported rather than silently replaced.

`agents/openai.yaml` contains skill UI metadata. It is not a standalone worker definition. No custom agent TOML file, model-detection script, API key, or `config.toml` change is required for this package.

## Maintain your copy

The source folder and installed copy are separate. Edit this source first, validate it, and then explicitly synchronize an update into the installed directory.

## Validate

When the bundled Skill Creator validator is available, run it against `skills/gpt-6-1-sol-router`:

```text
python <skill-creator-directory>/scripts/quick_validate.py skills/gpt-6-1-sol-router
```

The validator requires PyYAML and checks basic skill structure. Passing it does not establish live model availability, correct routing on every task, task quality, or quota savings. Assess those with representative tasks in the target Codex environment.
