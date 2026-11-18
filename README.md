# GPT-6.X Agent Router

A Codex skill for routing GPT-6-series agent work.

- [6.2](6.2/README.md): GPT-6.2 Sol for routine work, GPT-6.2 Astra for specific difficult problems, and GPT-6.1 Terra for routine Computer Use. Sol or Astra can operate the interface when diagnosis requires reasoning over live observations.
- [6.1 baseline](6.1/README.md): the preserved routing rules, with GPT-6.1 Terra as the sole Computer Use operator.
- Global prompt references: [6.2](6.2/global-prompt/global-AGENTS.md) and [6.1 baseline](6.1/global-prompt/global-AGENTS.md). Each version includes its task progress workflow and remains separate from the installable skill.

## Downloads

| Release | Global prompt only | Global prompt + Skill | Skill only |
| --- | --- | --- | --- |
| [6.2](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases/tag/v6.2) | [ZIP](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases/download/v6.2/gpt-6-x-agent-router-v6.2-global-prompt.zip) | [ZIP](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases/download/v6.2/gpt-6-x-agent-router-v6.2-full.zip) | [ZIP](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases/download/v6.2/gpt-6-x-agent-router-v6.2-skill.zip) |
| [6.1 baseline](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases/tag/v6.1) | [ZIP](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases/download/v6.1/gpt-6-x-agent-router-v6.1-global-prompt.zip) | [ZIP](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases/download/v6.1/gpt-6-x-agent-router-v6.1-full.zip) | [ZIP](https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases/download/v6.1/gpt-6-x-agent-router-v6.1-skill.zip) |

## Install from a release

Extract the ZIP to a temporary folder first. Use your Codex home directory (`$CODEX_HOME`, or `~/.codex` when unset) as the base for the copy paths below.

- **Global prompt only:** Copy or merge `AGENTS.md` and `instructions/task-progress.md`, keeping that directory layout. Review existing files first and preserve any local rules or preferences you want to keep. The release files omit personal preferences. If you use `AGENTS.override.md`, review it too, as it takes precedence over `AGENTS.md`.
- **Skill only:** Copy `skills/gpt-6-x-agent-router/` into the Codex home `skills/` directory. This package does not change your global prompt.
- **Global prompt + Skill:** Follow both sets of steps for the components included in the ZIP.

Both versions use the same skill identifier, `gpt-6-x-agent-router`. Install one version per location. Start a new Codex session to use the updated instructions. The version READMEs also show how to install just the Skill from the repository.
