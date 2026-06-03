---
name: irs-intake-interview
description: Conversationally interview a prospective Portuguese IRS client to capture their full profile and income data, validate it against the Classic Tax lead-intake schema, and finish by either handing them a paste-ready summary plus the https://www.classic.tax/intake link, or submitting directly to the intake API. Use whenever onboarding a new client, collecting intake/profiling data, "filling the intake", or building a lead profile for IRS filing.
---

# IRS Intake Interview

Run a friendly, structured interview that captures everything the **Classic Tax** lead intake
needs, then route the result to `https://www.classic.tax/intake`. The questions below mirror the
production intake form **field-for-field**, so the data you collect drops straight in.

> **Privacy.** You are handling a person's tax identity and financial data (NIF, NISS, IBAN,
> amounts). Collect only what's below, keep it in this session, and never expose it beyond the
> summary you hand back to the person themselves.

## Conversational rules

- Ask **one section at a time**, in the order below. Don't dump all questions at once.
- Use plain language; translate jargon (e.g. "Category A = salaries"). Explain *why* when it
  helps trust.
- Accept "I'm not sure / I don't have it" for **optional** fields — record it as empty and move
  on. For **required** fields, gently re-ask or offer the closest option.
- Reflect back a short confirmation after each section.
- Never invent values. If a tax concept needs judgment (regime eligibility, residency), note it
  as "to confirm with the accountant" rather than guessing.

## The schema (authoritative — mirrors the intake form & `/api/leads`)

`*` = required. Enums must use the **exact values** shown (left of the `→`).

### 1. Personal information
- `fullName` * — full legal name as on ID.
- `nif` * — Portuguese tax number (9 digits).
- `email` * — valid email.
- `driveConnector` * — `google_drive` → "Google Drive" | `dropbox` → "Dropbox" (where they'll
  share documents).
- `niss` — Social Security number (optional).
- `phone` — optional (e.g. `+351 ...`).
- `dateOfBirth` — optional, `YYYY-MM-DD`.
- `nationality` — optional, country name.

### 2. Residency & marital status
- `residency` * — `resident` → "Tax resident in Portugal" | `non_resident` → "Non-resident".
- `residencyRegime` — **only if resident**, recommended: `standard` | `nhr` (NHR) |
  `young_irs` (IRS Jovem) | `ex_resident` (Returning emigrant / ex-resident). If unsure → leave
  empty and flag "to confirm".
- `maritalStatus` * — `single` | `married` | `civil_union` | `divorced` | `widowed`.
- **If `married` or `civil_union`** also ask:
  - `spouseName` — spouse full name.
  - `spouseNif` — spouse NIF (9 digits).
  - `jointFiling` — `joint` → "Joint filing" | `separate` → "Separate filing" (explain the
    choice can be optimized later).

### 3. Income types
- `selectedIncomeTypes` * — one or more of:
  `salaries` (Category A) · `self_employment` (Category B / sole trader) · `pensions` ·
  `dividends` · `interest` · `crypto` · `rental` · `capital_gains` · `other`.

### 4. Income details — **one entry per selected type** (`incomes[]`)
For each selected income type, collect an object:
- `type` * — the income-type key from section 3.
- `source` * — `portugal` | `foreign`.
- `grossAmount` * — gross annual amount in EUR (digits; the person can estimate, note if so).
- `taxWithheld` — tax already withheld in Portugal (EUR), optional.
- **If `source` = `foreign`:**
  - `country` * — country of origin.
  - `taxWithheldAbroad` — tax withheld abroad (EUR), optional.
- **If `type` = `self_employment`:**
  - `activityCodes` — IRS activity code(s) (e.g. `1519`).
  - `caiCodes` — CAI/CAE code(s), optional.
  - `hasStableEstablishment` — `no` | `portugal` | `abroad`.
    - If `abroad` → `stableEstablishmentCountry`.
- **If `type` = `other`:**
  - `description` — what this income is.

### 5. Bank accounts & expenses
- `bankAccounts` * — at least one account:
  - `country` * (defaults to `Portugal`), `iban` *, `bankName` (optional), `swift`/BIC
    (optional). Offer to add more accounts.
- `efaturaValidated` — `yes` ("all validated") | `partial` | `no` ("not yet") — have they
  validated expenses in e-Fatura? (Tie-in: the `efatura-reconciliation` skill can help later.)
- `additionalNotes` — anything else relevant.

## Validation before finishing

- `nif` (and `spouseNif` if present): 9 numeric digits — warn if it isn't.
- `email`: looks like an email.
- At least **one** income type **and** a matching `incomes[]` entry with a `grossAmount`.
- At least **one** bank account with an `iban`.
- All enum fields use the exact allowed values above.
- List anything still missing/uncertain instead of fabricating it.

## Finishing — route to the intake

When the interview is complete, produce **all three** of the following:

1. **Human-readable summary**, grouped by the five sections, for the person to confirm.

2. **Paste-ready payload** — a JSON object using the exact field names above (include
   `"website": ""` — it's an anti-spam honeypot that must stay empty). Shape:

   ```json
   {
     "fullName": "", "nif": "", "email": "", "driveConnector": "google_drive",
     "niss": "", "phone": "", "dateOfBirth": "", "nationality": "",
     "residency": "resident", "residencyRegime": "", 
     "maritalStatus": "single", "spouseName": "", "spouseNif": "", "jointFiling": "",
     "selectedIncomeTypes": ["salaries"],
     "incomes": [
       { "type": "salaries", "source": "portugal", "grossAmount": "",
         "taxWithheld": "", "taxWithheldAbroad": "", "country": "",
         "activityCodes": "", "caiCodes": "", "hasStableEstablishment": "",
         "stableEstablishmentCountry": "", "description": "" }
     ],
     "bankAccounts": [ { "country": "Portugal", "bankName": "", "iban": "", "swift": "" } ],
     "efaturaValidated": "", "additionalNotes": "", "website": ""
   }
   ```

3. **The submission options** (default to the first):
   - **Open the intake form** → send them to **https://www.classic.tax/intake** and tell them
     the summary above maps section-by-section to the form. (The public form does not accept
     pre-filled URL parameters, so they enter the values — the summary makes it a copy job.)
   - **Submit directly (optional)** → `POST https://www.classic.tax/api/leads` with the JSON
     payload and `Content-Type: application/json`. On success it returns
     `status: "success"` and a one-time `credentialsUrl` (an `/intake/credentials#token=...`
     link) for the secure document/credentials step — pass that link to the person.
     ⚠️ The endpoint is **bot-protected**; automated/non-browser requests may be rejected. If a
     submission is blocked, fall back to the "open the intake form" path.

Only submit directly if the person explicitly asks you to. Otherwise stop at the summary +
link.
