# Classic Tax — Claude Plugins

The Claude Code plugin marketplace for [Classic Tax](https://www.classic.tax) (Classic Maths Lda).

Agentic accounting & tax tooling. The first plugin, **`agentic-accounting`**, lets a prospective
client **start their Portuguese IRS intake directly from Claude** — a guided interview that
captures their full profile and either submits it to classic.tax or hands them a paste-ready
summary for [classic.tax/intake](https://www.classic.tax/intake) — with supporting skills for
Modelo 3 *anexo* selection, e-Fatura reconciliation, and a tax-reviewer agent.

## Start your intake from Claude

Three commands in any Claude Code session:

```shell
/plugin marketplace add mathematica-ai/classic-tax-plugins
/plugin install agentic-accounting@classic-tax
/agentic-accounting:start
```

Claude interviews you section by section (personal info → residency → income → bank accounts),
validates everything, and — on your say-so — submits it straight to Classic Tax. You then
receive a secure one-time link for documents and portal credentials, and an accountant takes it
from there. Saying things like *"help me file my Portuguese IRS"* or *"start my Classic Tax
intake"* also triggers the interview once the plugin is installed.

## What's in the marketplace

| Plugin | Description |
| --- | --- |
| [`agentic-accounting`](./plugins/agentic-accounting) | Interview-driven IRS intake + Modelo 3 filing guidance, e-Fatura reconciliation, and a tax reviewer. |

## Other editions

The same service is also packaged for **ChatGPT** as a Custom GPT (no-code, GPT-Store
distributable) — see [`chatgpt/agentic-accounting`](./chatgpt/agentic-accounting). It mirrors the
plugin's skills/agent as instructions + knowledge files, with an optional Action for direct
intake submission.

## Layout

```
.claude-plugin/marketplace.json     # marketplace catalog (name: classic-tax)
plugins/agentic-accounting/         # the Claude Code plugin (manifest, skills, commands, agent)
chatgpt/agentic-accounting/         # the ChatGPT Custom GPT edition (instructions, knowledge, action)
```

## Develop & validate

```shell
# Validate the marketplace catalog
claude plugin validate .

# Validate a plugin (manifest + skill/command/agent frontmatter)
claude plugin validate ./plugins/agentic-accounting

# Test locally before publishing
/plugin marketplace add ./classic-tax-plugins
/plugin install agentic-accounting@classic-tax
```

## License

UNLICENSED — © Classic Maths Lda. All rights reserved.
