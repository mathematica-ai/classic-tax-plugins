# Changelog

## 0.2.2 — 2026-10-25

Loader fix and a read-only reviewer:

- The commands and the `pt-tax-reviewer` agent didn't load on current Claude Code. The
  manifest listed each file under `commands` and `agents`, and with those lists Claude Code
  2.1.274 loaded only the three skills (`claude plugin details`: 3 skills, 0 agents), so
  `/agentic-accounting:start` and the reviewer were missing. The manifest now relies on the
  default `commands/`, `agents/` and `skills/` directories: 7 skills (commands load as
  skills) and 1 agent.
- `pt-tax-reviewer` is read-only (`tools: Read, Grep, Glob`). It audits a draft; it never
  needs to run commands, write files or reach the network.

## 0.2.1 — 2026-08-07

Interview UX-test fixes, from a full role-played client interview (Portuguese-speaking
persona, invalid-NIF typo, "don't know" answers, live submission attempt):

- Cap questions at 3–4 per message; section 1 asks the required fields first and offers the
  optional ones in a follow-up.
- Explain *why* the IBAN is needed (the tax authority pays refunds to it).
- Married-branch fields (`spouseName`, `spouseNif`, `jointFiling`) are explicitly optional at
  intake; a given spouse NIF gets the same check-digit validation; "we haven't decided" on
  joint filing is a codified normal answer.
- After a NIF correction, confirm the new value passes validation.
- Submission error handling now covers the platform edge firewall: a 429 **or any HTML
  response** (e.g. a "Security Checkpoint" page) → one retry with the same `submissionId`,
  then fall back to the intake form, reassuring the person nothing was lost or duplicated.
  (Removed the previous "no bot-challenge" claim.)
- ChatGPT edition mirrored; its README carries a hand-assembly update checklist.

## 0.2.0 — 2026-08-07

Direct intake from Claude:

- New `/agentic-accounting:start` client entry-point command — welcome + full interview.
- Direct submission targets the agent-facing endpoint `POST /api/leads/agent`: correct 201
  handling, `credentialsDelivery` email/inline branches, `credentialsExpiresAt` surfacing,
  and `submissionId` idempotency (same-id retries are safe).
- NIF check-digit validation (mod-11) during the interview.
- Privacy: never ask for or accept the Portal das Finanças (AT) password in chat.
- Conduct the interview in the person's language (PT/EN).
- ChatGPT Custom GPT edition committed to the repo (instructions, knowledge files, Action
  spec, config) and updated in lockstep.

## 0.1.0

Initial release: `classic-tax` marketplace + `agentic-accounting` plugin (intake interview,
PT IRS filing knowledge, e-Fatura reconciliation, `pt-tax-reviewer` agent).
