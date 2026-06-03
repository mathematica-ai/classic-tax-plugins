---
description: Guided Portuguese IRS (Modelo 3) filing workflow for one taxpayer or household. Optionally pass a taxpayer NIF or year as argument (e.g. /irs-file 2025).
argument-hint: "[year and/or NIF]"
---

# Prepare a Portuguese IRS (Modelo 3) filing

Drive an end-to-end Modelo 3 preparation for a single taxpayer/household. Load and follow the
**`pt-irs-filing`** skill as the source of truth for categories, anexos, residency, deadlines,
and deductions.

> Not tax advice. Always end with explicit "verify for the income year" notes on any
> rate/cap/threshold, and hand off to a qualified preparer for submission.

Parse "$ARGUMENTS" for an income **year** and/or a **NIF**. If absent, ask for the income year
first (the filing happens the following April–June).

## Steps

1. **Scope & residency** — Determine tax residency for the income year (resident → worldwide;
   non-resident → Portuguese-source only). Note any mid-year arrival/departure.
2. **Household** — Marital/união de facto status, joint vs separate election (compare both),
   dependents and ascendants.
3. **Income inventory** — Collect income by **category (A–H)** and by holder. For each, record
   amounts, withholding, and source (domestic/foreign).
4. **Anexo mapping** — Using the skill's catalog, list **every required anexo** per holder with
   a one-line reason. Always include the rosto; add Anexo J for any foreign income; add Anexo H
   for deductions.
5. **Deductions** — Gather deduction inputs and, if e-Fatura data is available, run
   **`/efatura-reconcile`** (or the `efatura-reconciliation` skill) and fold the result into
   Anexo H.
6. **Regimes** — Flag IRS Jovem, IFICI (NHR successor), and simplified vs organized accounting
   for Category B. Mark eligibility as "confirm for {year}".
7. **Checklist & gaps** — Produce the filing checklist, list missing data and open questions.
8. **Review** — Hand the draft to the **`pt-tax-reviewer`** agent for an independent
   correctness pass before anything is submitted.

## Output

- Residency determination and income scope.
- Per-holder **anexo list** with reasons.
- Household option recommendation + trade-off.
- Deductions summary (linked to e-Fatura buckets).
- Regimes to confirm + open questions / missing data.
- Explicit "verify for {year}" notes on all year-specific figures.
