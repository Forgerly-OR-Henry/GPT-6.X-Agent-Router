# GPT-6.X Agent Router

A Codex skill for routing GPT-6-series agent work.

- [6.2](6.2/README.md): GPT-6.2 Sol for routine work, GPT-6.2 Astra for specific difficult problems, and GPT-6.1 Terra for routine Computer Use. Sol or Astra can operate the interface when diagnosis requires reasoning over live observations.
- [6.1 baseline](6.1/README.md): the preserved routing rules, with GPT-6.1 Terra as the sole Computer Use operator.
- Global prompt references: [6.2](6.2/global-prompt/global-AGENTS.md) and [6.1 baseline](6.1/global-prompt/global-AGENTS.md). Each version includes its task progress workflow and remains separate from the installable skill.

## Download with Codex

Copy one of these prompts into Codex. Each uses the release that GitHub marks as latest. To download a specific version, replace `latest` on the `Release` line with a tag such as `v6.1`.

### Global prompt + Skill

```text
Use the releases at https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases
Release: latest

Use the release GitHub marks as latest, or the exact tag above if specified.
Download the attached ZIP ending in -full.zip to my current workspace.
Tell me the release tag and the full path to the ZIP. If the attachment is missing, report that.
Do not install the package or change my Codex configuration.
```

### Global prompt only

```text
Use the releases at https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases
Release: latest

Use the release GitHub marks as latest, or the exact tag above if specified.
Download the attached ZIP ending in -global-prompt.zip to my current workspace.
Tell me the release tag and the full path to the ZIP. If the attachment is missing, report that.
Do not install the package or change my Codex configuration.
```

### Skill only

```text
Use the releases at https://github.com/Forgerly-OR-Henry/GPT-6.X-Agent-Router/releases
Release: latest

Use the release GitHub marks as latest, or the exact tag above if specified.
Download the attached ZIP ending in -skill.zip to my current workspace.
Tell me the release tag and the full path to the ZIP. If the attachment is missing, report that.
Do not install the package or change my Codex configuration.
```

## Install from a release

Extract the ZIP to a temporary folder first. Use your Codex home directory (`$CODEX_HOME`, or `~/.codex` when unset) as the base for the copy paths below.

- **Global prompt only:** Copy or merge `AGENTS.md` and `instructions/task-progress.md`, keeping that directory layout. Review existing files first and preserve any local rules or preferences you want to keep. The release files omit personal preferences. If you use `AGENTS.override.md`, review it too, as it takes precedence over `AGENTS.md`.
- **Skill only:** Copy `skills/gpt-6-x-agent-router/` into the Codex home `skills/` directory. This package does not change your global prompt.
- **Global prompt + Skill:** Follow both sets of steps for the components included in the ZIP.

Both versions use the same skill identifier, `gpt-6-x-agent-router`. Install one version per location. Start a new Codex session to use the updated instructions. The version READMEs also show how to install just the Skill from the repository.
