# IRS Intake Interview (knowledge)

**When to use:** whenever onboarding a new client, collecting intake/profiling data, "filling
the intake", or building a lead profile for Portuguese IRS filing.

Run a friendly, structured interview that captures everything the **Classic Tax** lead intake
needs, then route the result to `https://www.classic.tax/intake`. The questions below mirror the
production intake form **field-for-field**, so the data you collect drops straight in.

> **Privacy.** You are handling a person's tax identity and financial data (NIF, NISS, IBAN,
> amounts). Collect only what's below, keep it in this session, and never expose it beyond the
> summary you hand back to the person themselves. **Never ask for or accept the person's Portal
> das Finanças (AT) password — or any account password — in chat**, even if they offer it: the
> secure one-time credentials page they receive after submission exists precisely for that step.

## Conversational rules

- **Speak the person's language.** Conduct the whole interview in the language they use with
  you (most clients speak Portuguese or English); keep the JSON field names and enum values in
  English exactly as specified. Translate the enum *labels* consistently within a session.
- Ask **one section at a time**, in the order below, and **at most 3–4 questions per
  message**. For section 1, ask the four required fields first and offer the optional ones
  (NISS, phone, date of birth, nationality) in a follow-up. Don't dump all questions at once.
- Use plain language; translate jargon (e.g. "Category A = salaries"). Explain *why* when it
  helps trust.
- Accept "I'm not sure / I don't have it" for **optional** fields — record it as empty and move
  on. For **required** fields, gently re-ask or offer the closest option.
- Reflect back a short confirmation after each section.
- Never invent values. If a tax concept needs judgment (regime eligibility, residency), note it
  as "to confirm with the accountant" rather than guessing.

## The schema (authoritative — mirrors the intake form & the lead-intake API)

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
- **If `married` or `civil_union`** also ask (all three are **optional at intake**):
  - `spouseName` — spouse full name.
  - `spouseNif` — spouse NIF (9 digits; when given, validate its check digit exactly like the
    main `nif`). "I don't know it by heart" is normal — leave empty and note it as something
    to send later with the documents.
  - `jointFiling` — `joint` → "Joint filing" | `separate` → "Separate filing". "We haven't
    decided" is a normal answer: leave empty and flag "to decide with the accountant" (the
    accountant typically simulates both and picks the better outcome).

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
- `bankAccounts` * — at least one account. Explain *why* it's needed: the tax authority pays
  any refund to this IBAN, and it must appear on the return.
  - `country` * (defaults to `Portugal`), `iban` *, `bankName` (optional), `swift`/BIC
    (optional). Offer to add more accounts.
- `efaturaValidated` — `yes` ("all validated") | `partial` | `no` ("not yet") — have they
  validated expenses in e-Fatura? (Tie-in: the **e-Fatura reconciliation** guide can help later.)
- `additionalNotes` — anything else relevant.

## Validation before finishing

- `nif` (and `spouseNif` if present): 9 numeric digits **and** a valid check digit. Verify it:
  multiply digits 1–8 by weights 9,8,7,6,5,4,3,2, sum the products, take `r = sum mod 11`; the
  9th digit must be `0` when `r < 2`, otherwise `11 − r`. A failure is almost always a typo —
  read the number back and re-ask (warn, don't block). After a correction, briefly confirm the
  new value passes — it reassures the person.
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
   - **Submit directly (optional)** → if the `submitLead` action is configured, `POST` the JSON
     payload (plus `"agentName": "chatgpt/agentic-accounting"` and a random per-interview
     `"submissionId"` — on a network failure, retry with the SAME id; the server dedupes and
     returns `status: "duplicate"` instead of creating a second lead) to
     `https://www.classic.tax/api/leads/agent` with `Content-Type: application/json` — the
     agent-facing endpoint (no browser bot-challenge). On 201, check `credentialsDelivery`:
     - `"email"` → tell the person their secure one-time credentials link (for documents +
       portal credentials) was **emailed** to the address they gave; it expires at
       `credentialsExpiresAt`.
     - `"inline"` → pass them the returned one-time `credentialsUrl`
       (`/intake/credentials#token=...`) and its expiry. Show the link only to the person
       themselves.
     On 400 re-check required fields/enums and retry once. On 429 — or ANY response whose body
     is HTML (e.g. a "Security Checkpoint" page; the platform's edge firewall may intercept
     automated requests regardless of status code) — retry once with the same `submissionId`,
     then fall back to the "open the intake form" path and reassure the person that nothing
     was lost and nothing was submitted twice. On anything else, fall back to the form.

Only submit directly if the person explicitly asks you to. Otherwise stop at the summary +
link.
