# e-Fatura Reconciliation (knowledge)

**When to use:** when reviewing e-Fatura data, classifying invoices into deduction categories,
or estimating deductions à coleta before filing Modelo 3.

**e-Fatura** is the AT system where merchant-issued invoices tied to a taxpayer's **NIF**
accumulate. These validated invoices feed the **deduções à coleta** on the IRS return. This
guide reconciles raw e-Fatura data against the expected deduction buckets and surfaces problems
*before* the Modelo 3 is filed.

> **Verify the income year.** Bucket caps, the sector-VAT incentive percentage, and which
> sectors qualify are set annually. Treat all amounts/caps as "confirm for {year}" unless you
> have the current figure.

## Deduction buckets (target classification)

Map each invoice to exactly one bucket (or mark it non-deductible):

1. **Despesas gerais familiares** — general family expenses (the broad default bucket).
2. **Saúde** — health (incl. reduced-VAT / exempt medical).
3. **Educação e formação** — education & training.
4. **Habitação** — rent (*rendas*) or eligible mortgage interest.
5. **Lares** — care homes / dependency.
6. **IVA por setor** — VAT incentive for specific sectors (restaurants & lodging, repairs,
   hairdressers/beauty, veterinary, public transport passes, etc. — confirm the year's list).

## Reconciliation procedure

1. **Load the data**: accept a CSV/JSON export or a pasted table of invoices. Expect, per row:
   merchant, NIF/sector, date, amount, VAT, and the taxpayer's **assigned category / pending
   status** from the e-Fatura portal. For larger exports, use **Data Analysis (code
   interpreter)** to parse and aggregate rather than estimating by eye.
2. **Status triage** — split invoices into:
   - `registered` — classified and counted.
   - `pending` — awaiting taxpayer classification on the portal (**these don't count until
     resolved** — flag them prominently).
   - `awaiting merchant` — merchant hasn't communicated the invoice yet.
   - `unclassified / ambiguous` — sector unclear; needs a decision.
3. **Classify** each registered/pending invoice into a bucket above. Note where the sector code
   forces the bucket (e.g. a pharmacy → health) vs where the taxpayer chooses.
4. **Aggregate per bucket** and compare totals against the (year-verified) caps. Mark buckets
   that are **under cap** (room to add) vs **at/over cap** (extra invoices won't help).
5. **Flag issues**:
   - Pending/awaiting invoices that, if resolved, would change the outcome.
   - Likely misclassifications (e.g. a clearly-medical invoice sitting in general expenses).
   - Duplicates and refunds/credit notes that should net down a bucket.
   - Sector-VAT-eligible invoices not capturing the incentive.
6. **Estimate** the deductible contribution per bucket (apply the year's rules/caps; state the
   assumptions and mark any figure you couldn't verify).

## Output

Produce a concise reconciliation:

- **Summary table**: bucket → invoice count → total → estimated deductible → cap headroom.
- **Action list**: pending invoices to classify, ambiguous ones needing a decision, suspected
  misclassifications, duplicates/refunds to net.
- **Risks/assumptions**: every cap/percentage used, each flagged "verify for {year}".

Hand the result back into the **Modelo 3 filing** workflow (Anexo H / deductions step).
