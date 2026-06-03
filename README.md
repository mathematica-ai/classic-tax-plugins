# Classic Tax — Claude Plugins

The Claude Code plugin marketplace for [Classic Tax](https://www.classic.tax) (Classic Maths Lda).

Agentic accounting & tax tooling. The first plugin, **`agentic-accounting`**, runs an
interview-driven Portuguese IRS intake and routes clients to
[classic.tax/intake](https://www.classic.tax/intake), with supporting skills for Modelo 3 *anexo*
selection, e-Fatura reconciliation, and a tax-reviewer agent.

## Install

From a Claude Code session:

```shell
/plugin marketplace add mathematica-ai/classic-tax-plugins
/plugin install agentic-accounting@classic-tax
```

Then try it:

```shell
/intake-interview
```

## What's in the marketplace

| Plugin | Description |
| --- | --- |
| [`agentic-accounting`](./plugins/agentic-accounting) | Interview-driven IRS intake + Modelo 3 filing guidance, e-Fatura reconciliation, and a tax reviewer. |

## Layout

```
.claude-plugin/marketplace.json     # marketplace catalog (name: classic-tax)
plugins/agentic-accounting/         # the plugin (manifest, skills, commands, agent)
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
