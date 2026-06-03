# Agentic Accounting

An agentic toolkit for accounting & tax workflows, by [Classic Tax](https://www.classic.tax)
(Classic Maths Lda).

The first offered service is **Portuguese personal income tax (IRS) onboarding**. The headline
capability is an **interview-driven intake**: the agent profiles a prospective client through a
friendly, structured conversation, then hands them a paste-ready summary and routes them to
[classic.tax/intake](https://www.classic.tax/intake). From there the toolkit supports Modelo 3
*anexo* selection, e-Fatura reconciliation, and a final correctness review before submission.

> **Not tax advice.** This plugin encodes process knowledge to make an agent a competent
> assistant for a qualified preparer. Tax law and year-specific figures (brackets, deduction
> caps, regime rules) change every year — always verify against the **Autoridade Tributária e
> Aduaneira (AT)** *Portal das Finanças* for the relevant income year before filing.

## What's inside

| Type | Name | Purpose |
| --- | --- | --- |
| **Skill** | **`irs-intake-interview`** | **Interview a client to capture their IRS profile + income data (mirrors the classic.tax intake schema), then hand over a summary + the intake link or submit it.** |
| **Command** | **`/intake-interview`** | **Run the intake interview end-to-end and route to classic.tax/intake.** |
| Skill | `pt-irs-filing` | The Portuguese IRS (Modelo 3) filing model: categories, *anexos*, residency, deadlines, deductions, regimes (IRS Jovem, IFICI/NHR). |
| Skill | `efatura-reconciliation` | Reconcile e-Fatura invoices against declared deductions and flag gaps. |
| Command | `/irs-file` | Guided, end-to-end Modelo 3 filing workflow for one taxpayer/household. |
| Command | `/efatura-reconcile` | Reconcile a taxpayer's e-Fatura export against expected deduction buckets. |
| Agent | `pt-tax-reviewer` | Independent reviewer that audits a draft Modelo 3 for missing *anexos* and common errors. |

The intake interview mirrors the live `LeadIntakeForm` / `POST /api/leads` schema field-for-field
(personal info, residency & marital status, income types, per-income details, bank accounts &
e-Fatura), so the captured data drops straight into the form. By default the agent stops at a
paste-ready summary + the [intake link](https://www.classic.tax/intake); direct API submission
is opt-in (the endpoint is bot-protected).

## Install

From a Claude Code session:

```
/plugin marketplace add mathematica-ai/classic-tax-plugins
/plugin install agentic-accounting@classic-tax
```

To test before publishing, point the marketplace at a local checkout (the directory that
contains `.claude-plugin/marketplace.json`):

```
/plugin marketplace add ./classic-tax-plugins
/plugin install agentic-accounting@classic-tax
```

## Roadmap

- Additional jurisdictions and services (corporate IRC, IVA periodic returns, SAF-T parsing).
- Bookkeeping & month-end close workflows.
- Bank/ledger reconciliation skills.
