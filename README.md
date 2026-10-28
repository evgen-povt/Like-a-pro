# Like a pro

**English** | [Русский](README.ru.md)

## Why this exists

AI coding tools make it easier to turn an idea into working software. As a project grows, there are more things to get right: preserving work, protecting data, checking real behavior, choosing sensible infrastructure, and making the project maintainable.

People building without a development background should be able to describe what they want in plain language. They should not have to know which engineering checks to request at every step.

**Like a pro** gives Codex a practical engineering workflow for that situation. It guides the agent to apply checks proportional to the risk, choose simple solutions, and explain important decisions and verification limits. Routine edits stay lightweight; changes involving user data, permissions, money, or production receive more scrutiny.

## Installation

Add the [GitHub repository](https://github.com/evgen-povt/like-a-pro) as a marketplace and install the plugin:

```sh
codex plugin marketplace add evgen-povt/like-a-pro
codex plugin add like-a-pro@like-a-pro
```

Start a new Codex chat after installation. The plugin and skill appear as **Like a pro**.

## Usage

Open your project in Codex and invoke the skill along with your request:

```text
$like-a-pro Help me build a booking form for my website.
```

Use it with an existing project too:

```text
$like-a-pro Fix saving customer requests and check that they persist after reload.
```

Continue describing the outcome you want in ordinary language. Codex can also select the skill automatically for relevant software-development work.

The skill covers source recovery, security and privacy, connected application checks, basic UI usability, simple hosting setups, operating costs, critical-change review, and project handover. It asks about decisions that materially affect the product, cost, compatibility, or risk.

## Technical details

This is a skills-only Codex plugin containing one skill and its references. It has no bundled MCP servers, external connections, hooks, or runtime dependencies.

### Package layout

```text
plugin.json
.agents/plugins/marketplace.json
LICENSE
skills/like-a-pro/
  SKILL.md
  agents/openai.yaml
  references/
```

`plugin.json` declares the plugin identity and Codex display metadata. The marketplace entry points to this repository's root. The skill and all of its references travel together under `skills/like-a-pro/`.

The technical identifier is `like-a-pro`, and the repository includes its own marketplace catalog. Package and marketplace formats follow the [official OpenAI plugin packaging guide](https://developers.openai.com/plugins/build/plugins).

### Local development

From a local checkout, register the repository path and install the plugin:

```sh
codex plugin marketplace add .
codex plugin add like-a-pro@like-a-pro
```

### Maintaining the skill

Behavioral evaluation scenarios are in [skill-validation.md](skills/like-a-pro/references/skill-validation.md). These are for maintainers changing the skill, not an extra workflow for every application task.

Report problems or suggest improvements through [GitHub issues](https://github.com/evgen-povt/like-a-pro/issues).

## License

Licensed under the [MIT License](LICENSE).

Copyright (c) 2026 Evgenii Valeev.
