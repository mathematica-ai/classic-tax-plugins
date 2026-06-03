---
name: pt-irs-filing
description: Portuguese personal income tax (IRS) filing via Modelo 3 — who must file, income categories (A–H), the full anexo catalog and how to select them, residency rules, the April–June filing window, deductions à coleta, and special regimes (IRS Jovem, IFICI/ex-NHR, IRS automático). Use whenever preparing, reviewing, or explaining a Portuguese IRS / Modelo 3 declaration, selecting anexos, or reasoning about Portuguese personal income tax.
---

# Portuguese IRS Filing (Modelo 3)

This skill is the working model for assisting with **IRS** — *Imposto sobre o Rendimento das
Pessoas Singulares*, Portugal's personal income tax — filed annually on **Modelo 3**.

> **Not tax advice; verify the income year.** The *structure* below (categories, anexos,
> residency logic, the filing window) is stable year to year. **Rates, brackets, deduction
> caps, and regime eligibility change every year** and are set in the annual *Orçamento do
> Estado*. Always confirm year-specific figures against the **AT** *Portal das Finanças*
> before relying on a number. When unsure of a current figure, say so — never invent a rate.

## 1. Who must file

A taxpayer generally must submit Modelo 3 unless they qualify for *dispensa de declaração* or
fall fully under **IRS automático** and accept the pre-filled return. Common triggers that
**require** a manual Modelo 3:

- Self-employment / business income (Categoria B) outside the narrow auto cases.
- Income obtained abroad (Categoria — any — earned outside Portugal → **Anexo J**).
- Rental income (Categoria F), capital gains (Categoria G), most capital income not at
  final withholding.
- Claiming benefits/regimes that the automatic return doesn't cover (e.g. IFICI, certain
  deductions, transparência fiscal).

Tax residents are taxed on **worldwide income**; non-residents only on **Portuguese-source**
income. Residency (183-day rule / habitual abode) drives the entire return — establish it first.

## 2. Income categories

| Cat. | Income type | Primary anexo |
| --- | --- | --- |
| A | Dependent employment | Anexo A |
| B | Self-employment / business / professional | Anexo B (simplified) or Anexo C (organized accounting) |
| E | Capital (interest, dividends) | Anexo E |
| F | Property / rental | Anexo F |
| G | Capital gains & increments | Anexo G (taxable) / Anexo G1 (exempt or grandfathered) |
| H | Pensions | Anexo A |

(Categories C and D as standalone income types were merged into B in earlier reforms; treat
business/professional income as **Category B**.)

## 3. Anexo catalog — selection logic

Select anexos by the income and circumstances present. The **rosto** (cover/main form) is always
filed; attach anexos as follows:

- **Anexo A** — Category A (employment) and H (pensions).
- **Anexo B** — Category B under the **simplified regime** (*regime simplificado*).
- **Anexo C** — Category B under **organized accounting** (*contabilidade organizada*).
- **Anexo D** — income imputed under **fiscal transparency** (*transparência fiscal*) /
  inheritance undivided estates passing through.
- **Anexo E** — capital income (Category E) not exhausted by final withholding, or where
  *englobamento* (aggregation) is elected.
- **Anexo F** — property income (Category F): rents, less allowable expenses.
- **Anexo G** — capital gains/increments (Category G): real estate, securities, etc.
- **Anexo G1** — non-taxable / exempt gains and grandfathered pre-1989 real estate.
- **Anexo H** — **deductions à coleta** and **tax benefits** (*benefícios fiscais*).
- **Anexo I** — income of an **undivided inheritance** (*herança indivisa*).
- **Anexo J** — **income obtained abroad** and foreign accounts (IBAN/accounts disclosure).
- **Anexo L** — **non-habitual resident (NHR)** / where applicable the successor regime
  reporting. NHR is closed to new entrants; the successor is **IFICI** (see §6).
- **Anexo SS** — social-security annex for self-employed (linked to Category B activity).

**Rule of thumb:** one anexo per income type per holder. In a joint return, income is reported
per holder (sujeito passivo A / B) and dependents are listed on the rosto.

## 4. Household & filing options

- **Married / unidos de facto**: may file **jointly** (*tributação conjunta*) or **separately**
  (*tributação separada*). Default is separate; joint must be elected. Compare both — the better
  option depends on income split and deduction interaction.
- **Dependents**: listed on the rosto; drive per-dependent deductions and the e-Fatura split.
- **Ascendentes** living with the household can add deductions under conditions.

## 5. Filing window & process

- **Window**: the Modelo 3 is filed **1 April – 30 June** of the year *following* the income
  year (e.g. 2025 income → filed Apr–Jun 2026). Confirm the exact dates for the year — they
  are reaffirmed annually.
- **Channel**: submitted electronically on the **Portal das Finanças** (NIF + password).
- **IRS automático**: for simple profiles (mostly Cat. A/H, Portuguese-source) AT pre-fills a
  return; the taxpayer validates/accepts or switches to a manual Modelo 3.
- **Refund / payment**: after assessment (*liquidação*), AT issues a refund or a payment note.

## 6. Deductions à coleta (Anexo H) — buckets

Deductions reduce the *coleta* (tax due), most fed by **e-Fatura**-validated invoices and each
subject to a **per-year cap** (verify current caps):

- General family expenses (*despesas gerais familiares*).
- Health (*saúde*) — incl. reduced-VAT/exempt health.
- Education & training (*educação e formação*).
- Housing — rent (*rendas*) or mortgage interest on pre-cutoff contracts.
- Homes / care (*lares*).
- Dependents and ascendants (fixed per-person amounts).
- VAT incentive by sector (*dedução de IVA* — restaurants, repairs, hairdressers, vet, etc.).
- PPR / retirement-savings and other *benefícios fiscais*.

→ For matching invoices to these buckets, use the **`efatura-reconciliation`** skill.

## 7. Special regimes

- **IRS Jovem**: partial exemption of Category A/B income for qualifying young taxpayers in
  the first years of work, on a declining schedule. Verify age/eligibility/percentages for the
  year — they were revised recently.
- **IFICI** (*Incentivo Fiscal à Investigação Científica e Inovação*): the **successor to NHR**
  for new entrants. Classic **NHR** is closed to new registrations but continues for those
  already enrolled within its term. Don't conflate them.
- **Regime simplificado vs organized accounting** (Cat. B): the simplified regime applies
  coefficients to gross income; organized accounting uses actual books. Threshold-driven.

## 8. Working method (for the agent)

1. **Establish residency** (resident / non-resident; arrivals/departures mid-year).
2. **Inventory income** by category and by holder.
3. **Map categories → anexos** using §3; list every required anexo explicitly.
4. **Decide household option** (joint vs separate) and list dependents/ascendants.
5. **Gather deductions** and reconcile via e-Fatura (`efatura-reconciliation`).
6. **Flag regimes** (IRS Jovem, IFICI, simplified vs organized).
7. **Produce a filing checklist** + open questions, then hand to **`pt-tax-reviewer`** for an
   independent correctness pass before submission.
8. **Surface every year-specific number as "verify against AT for {year}"** rather than
   asserting a figure you are not certain is current.

## Output

When asked to prepare or explain a filing, produce:
- The **residency determination** and worldwide-vs-source scope.
- A **per-holder anexo list** with the reason each is included.
- The **household option** recommendation (with the trade-off).
- A **deductions checklist** tied to e-Fatura buckets.
- A list of **regimes to confirm** and **open questions / missing data**.
- An explicit **"verify for the income year"** note on any rate/cap/threshold.
