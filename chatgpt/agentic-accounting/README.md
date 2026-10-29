# Agentic Accounting — ChatGPT variant (Custom GPT)

The **ChatGPT** edition of the `agentic-accounting` Claude Code plugin, by
[Classic Tax](https://www.classic.tax) (Classic Maths Lda). Same service, same domain knowledge,
delivered as a **Custom GPT** — ChatGPT's no-code, GPT-Store-distributable analog of a plugin.

The first service is **Portuguese personal income tax (IRS) onboarding & preparation support**:
an interview-driven intake that profiles a client and routes them to
[classic.tax/intake](https://www.classic.tax/intake), plus Modelo 3 *anexo* selection, e-Fatura
reconciliation, and an independent review pass.

> **Not tax advice.** Process knowledge to make the assistant a competent aide to a qualified
> preparer. Year-specific figures (brackets, caps, regime rules) change annually — always verify
> against the **Autoridade Tributária e Aduaneira (AT)** *Portal das Finanças* for the income
> year before filing.

> ⚠️ **This edition is hand-assembled in the GPT builder** — editing these files does NOT
> update the live GPT. After any change here, re-paste into the builder: `instructions.md` →
> Instructions, the four `knowledge/*.md` files → Knowledge (replace all), and
> `actions/leads-action.openapi.yaml` → the `submitLead` Action schema. Last content update:
> 2026-08-07 v0.2.1 (UX-test fixes: 3–4 questions/message cap, IBAN "why", married-branch
> fields optional with spouse-NIF checksum, edge security-checkpoint fallback handling — on
> top of the v0.2.0 agent-endpoint/submissionId/language-mirroring changes).

## Why a Custom GPT (and not an Apps-SDK app)

The Claude plugin is entirely **prompt/process knowledge** — skills, commands, and an agent, no
code. The faithful ChatGPT equivalent is therefore a **Custom GPT** (instructions + knowledge
files + optional Action), which is no-code and publishable to the GPT Store today.

A **ChatGPT App (Apps SDK / MCP)** would mean building a coded MCP server with a sandboxed UI —
worth it later if we want rich widgets (interactive intake forms, reconciliation dashboards) and
cross-host portability (the same MCP app runs in ChatGPT, Claude, VS Code). That's the roadmap
path, not needed for parity with today's plugin.

## How the plugin maps to the GPT

| Claude plugin piece | ChatGPT variant |
| --- | --- |
| `skills/*` (3 skills) + `agents/pt-tax-reviewer` | `knowledge/*.md` (4 retrieval docs) |
| `commands/*` (intake-interview, irs-file, efatura-reconcile) | Workflows A–C in `instructions.md` + conversation starters |
| `agents/pt-tax-reviewer` | Workflow D in `instructions.md` |
| `plugin.json` metadata | `gpt-config.json` |
| Direct `POST /api/leads` (opt-in) | `actions/leads-action.openapi.yaml` (optional Action) |

## Files

```
instructions.md                  # the GPT's system instructions (orchestrates Workflows A–D)
gpt-config.json                  # name, description, starters, capability + action settings
avatar.png                       # 512×512 GPT avatar (Classic Tax mark, rendered from brand/logo.svg)
knowledge/
  irs-intake-interview.md        # intake script + exact lead-intake schema
  pt-irs-filing.md               # Modelo 3 model: categories, anexos, residency, deductions
  efatura-reconciliation.md      # invoice → deduction-bucket reconciliation
  pt-tax-reviewer.md             # independent review checklist
actions/
  leads-action.openapi.yaml      # OPTIONAL Action for direct intake submission
```

## Publish it (GPT Builder)

1. Go to **ChatGPT → Explore GPTs → + Create** (or **My GPTs → Create a GPT**).
2. Open the **Configure** tab and fill in from [`gpt-config.json`](./gpt-config.json):
   - **Avatar** — click the profile circle → **Upload photo** → [`avatar.png`](./avatar.png)
     (the Classic Tax mark, 512×512, framed for the circular crop).
   - **Name** and **Description** — copy the `name` / `description`.
   - **Instructions** — paste the full contents of [`instructions.md`](./instructions.md).
     The builder caps Instructions at **8,000 characters**; keep `instructions.md` under it
     (detail belongs in the knowledge files). Check with
     `python3 -c "print(len(open('instructions.md', encoding='utf-8').read()))"`.
   - **Conversation starters** — the four `conversation_starters`.
   - **Capabilities** — enable **Web Browsing** and **Code Interpreter & Data Analysis**;
     leave **DALL·E** off (see `capability_rationale`).
   - **Knowledge** — upload all four files from [`knowledge/`](./knowledge).
3. **(Optional) Action** — under **Actions → Create new action**, import
   [`actions/leads-action.openapi.yaml`](./actions/leads-action.openapi.yaml). Note: the
   `/api/leads` endpoint is **bot-protected** and may reject GPT-Action calls; the default flow
   routes the user to `classic.tax/intake`, so the Action is genuinely optional.
4. **Save** → choose visibility (**Anyone with a link** recommended for a controlled rollout;
   **GPT Store** for public discovery).

## Keeping parity with the Claude plugin

The knowledge docs are faithful ports of the plugin's skills/agent; cross-references were
reworded (Claude command names → workflow names). When the plugin's skills change, re-port the
edited sections here so both editions stay in lock-step. Bump the `version` in `gpt-config.json`
alongside the plugin's `plugin.json`.

## License

UNLICENSED — © Classic Maths Lda. All rights reserved.
