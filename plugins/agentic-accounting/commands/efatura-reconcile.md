---
description: Reconcile a taxpayer's e-Fatura invoices against the IRS deduction buckets and flag pending/misclassified invoices before filing. Provide the e-Fatura export (CSV/JSON/table) or a path to it.
argument-hint: "[path to e-Fatura export, or paste the data]"
---

# Reconcile e-Fatura against IRS deduction buckets

Load and follow the **`efatura-reconciliation`** skill. Reconcile the taxpayer's e-Fatura
invoices into the deduction buckets and surface problems before the Modelo 3 is filed.

Resolve the input from "$ARGUMENTS":
- If it's a **file path**, read that file (CSV/JSON/table of invoices).
- If it's **pasted data**, use it directly.
- If **nothing** is provided, ask the user to paste the e-Fatura export or give a path, and ask
  for the **income year** (caps and the sector-VAT list are year-specific).

## Do

1. Triage invoice **status**: registered / pending / awaiting merchant / unclassified.
   Pending & awaiting **don't count** until resolved — flag them.
2. **Classify** each invoice into one bucket: despesas gerais, saúde, educação, habitação,
   lares, or IVA por setor (or mark non-deductible).
3. **Aggregate** per bucket; compare to the year-verified caps (under cap vs at/over).
4. **Flag** misclassifications, duplicates, refunds/credit notes, and unclaimed sector-VAT.
5. **Estimate** the deductible per bucket, stating every cap/percentage assumption and marking
   any unverified figure as "verify for {year}".

## Output

- **Summary table**: bucket → count → total → estimated deductible → cap headroom.
- **Action list**: pending to classify, ambiguous to decide, suspected misclassifications,
  duplicates/refunds to net.
- **Assumptions/risks**, each flagged "verify for {year}".

Then offer to fold the result into the Anexo H / deductions step of **`/irs-file`**.
