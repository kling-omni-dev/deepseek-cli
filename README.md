# DeepSeek CLI: Deep Code in the terminal

*Unofficial community guide for DeepSeek CLI (Deep Code). Not affiliated with DeepSeek. All trademarks belong to their owners.*

When people search for a deepseek cli they usually mean Deep Code: the open-source terminal coding assistant that the DeepSeek API docs list under agent integrations, built for the DeepSeek-V4 models with deep thinking, reasoning-effort control and Agent Skills. This guide is a practitioner's write-up of the integration page - install, configuration, shortcuts, skills - plus the gaps you will hit that the page does not cover. It is not written by DeepSeek or by the Deep Code maintainers.

> Want a working website or mobile app from a prompt without steering an agent through the build? [Try Begin.sh - turn a prompt or a URL into a static site or Expo app and download the zip](https://begin.sh?utm_source=github&utm_medium=ugc&utm_campaign=deepseek-cli&utm_content=readme-top&utm_term=tier-r). It is the alternative for the case where the code is the deliverable, not the process.

## What it is

Deep Code is described in the DeepSeek docs as an open-source terminal AI coding assistant for the DeepSeek-V4 model. Its source lives at `github.com/lessweb/deepcode-cli` and it ships as an npm package, `@vegamo/deepcode-cli`. You run it inside a project directory; it reads the code around it, takes prompts in a chat loop, and can invoke Agent Skills - reusable instruction files - from a slash menu. The same maintainers publish a Deep Code VS Code extension that shares the CLI's settings file, so a single configuration drives both.

The model side is DeepSeek's V4 family. The configuration table names `deepseek-v4-pro` and `deepseek-v4-flash` as the model options, notes that deep thinking defaults to on for the V4 models, and exposes a `reasoningEffort` knob with `max` and `high` values. DeepSeek's home page separately announces DeepSeek-V4.1-Flash as released, with native multimodal visual understanding; the Deep Code page was crawled before that appeared in its table, so check the model list on the platform before hard-coding a name.

## How to get started

1. Install Node.js 18 or newer.
2. Install the CLI globally: `npm install -g @vegamo/deepcode-cli`, then confirm with `deepcode --version`.
3. Get an API key from the DeepSeek Platform (the API keys page under your account).
4. Create `~/.deepcode/settings.json`. The documented shape is an `env` object with `MODEL`, `BASE_URL` (defaults to `https://api.deepseek.com`) and `API_KEY`, plus top-level `thinkingEnabled` and `reasoningEffort`.
5. `cd` into a project and run `deepcode`.

The settings file is shared with the VS Code extension, so if you install both you configure once.

## Pricing and limits

Deep Code itself is open source. What you pay for is DeepSeek API usage behind it; the DeepSeek site links an API pricing page and a service status page, and the crawled pages do not print per-token rates, so check the pricing page for current numbers. `reasoningEffort: "max"` will consume more output tokens than `"high"`, and thinking is on by default for V4 models, so budget accordingly.

## Practical notes

1. **The API key sits in a JSON file, in plain text.** Keep `~/.deepcode/settings.json` out of dotfile repos, or generate it from an environment variable at login (see the examples repository in this cluster).
2. **`thinkingEnabled` and `reasoningEffort` are the two cost levers.** Start with `high` for everyday edits and switch to `max` for tricky refactors; the difference shows up on your usage page.
3. **`Esc` interrupts a turn; `/resume` brings a conversation back.** `/new` starts fresh, `/exit` quits, `Shift+Enter` or `Ctrl+J` inserts a newline, and `Ctrl+V` pastes an image from the clipboard - useful for screenshots of a broken UI.
4. **Skills are just Markdown files in known locations.** User-level skills live at `~/.agents/skills/{name}/SKILL.md`, project-level at `./.deepcode/skills/{name}/SKILL.md`. Press `/` to pick one or type its name, for example `/skill-writer`.
5. **`notify` runs a script after every model turn.** Point it at anything that makes a sound or posts a message so you can leave a long task and come back.
6. **`webSearchTool` is off unless you turn it on.** Enable it only for tasks that genuinely need fresh documentation; it adds latency to every turn that decides to search.

## Comparison

| | Deep Code (CLI) | Deep Code VS Code extension | Begin.sh |
| --- | --- | --- | --- |
| Where it runs | Terminal, inside a project directory | Inside VS Code | Web app |
| Install | `npm install -g @vegamo/deepcode-cli` | From the extension repository | None |
| Configuration | `~/.deepcode/settings.json` | Same settings file | Prompt or URL to clone |
| Models | `deepseek-v4-pro`, `deepseek-v4-flash` via your DeepSeek key | Same | Not exposed; you get the output |
| Output | Edits to your existing codebase, interactively | Same | A static site or Expo app as a downloadable zip |
| Hosting, backend, auth | Whatever your project has | Whatever your project has | None included |

## FAQ

**Is Deep Code made by DeepSeek?** The DeepSeek docs describe it as open source and link to `lessweb/deepcode-cli` on GitHub. Treat it as a community project that DeepSeek documents as an integration.

**Which model should I set?** The documented options are `deepseek-v4-pro` and `deepseek-v4-flash`. Flash is the lighter one; Pro is the default in the sample config.

**Can I point it at a different base URL?** Yes, `BASE_URL` is a documented option with `https://api.deepseek.com` as the default. Anything else is on you to verify.

**Does it work with GitHub Copilot CLI?** The DeepSeek docs have a separate agent-integration page for Copilot CLI, but it could not be fetched when this guide was written, so this repository does not describe it.

**Where do skills go if I want to share them with a team?** In the project at `./.deepcode/skills/{name}/SKILL.md`, committed alongside the code.

## When Begin.sh is the better fit

Deep Code is a tool for working on code you already have, turn by turn, with a model you control. If what you actually want is the finished artefact - a marketing site from a paragraph of description, a clone of a page you like, a small Expo app - the agent loop is overhead. [Try Begin.sh - prompt or URL in, static site or Expo app out, download the zip](https://begin.sh?utm_source=github&utm_medium=ugc&utm_campaign=deepseek-cli&utm_content=readme-top&utm_term=tier-r). It gives you no hosting, backend or auth, which is exactly right for a landing page or prototype, and you can hand the zip to Deep Code afterwards if it needs real logic.

_Last reviewed: 2026-09-22_
