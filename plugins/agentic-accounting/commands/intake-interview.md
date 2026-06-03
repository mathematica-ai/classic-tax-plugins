---
description: Interview a prospective client to capture their IRS profile + income data, then hand them a paste-ready summary and the https://www.classic.tax/intake link (or submit directly on request). Optionally pass the client's name as argument.
argument-hint: "[client name, or 'submit' to auto-post]"
---

# Classic Tax intake interview

Run the **`irs-intake-interview`** skill to onboard a prospective Portuguese IRS client. Conduct
a friendly, section-by-section interview that captures exactly the data the Classic Tax lead
intake needs, validate it, and route the result to `https://www.classic.tax/intake`.

Parse "$ARGUMENTS":
- A **name** → greet the person by name and pre-fill `fullName`.
- The word **`submit`** → after confirmation, offer to POST directly to the intake API.
- Otherwise → default to handing over a summary + the intake link (no auto-submit).

## Flow

1. Briefly explain what you'll collect and why (to prepare their IRS / Modelo 3 filing) and note
   their data stays in this conversation.
2. Walk the five sections from the skill **one at a time**: Personal → Residency & marital →
   Income types → Income details → Bank accounts & expenses. Confirm after each.
3. **Validate** (NIF/spouse NIF 9 digits, valid email, ≥1 income type with amount, ≥1 IBAN,
   correct enum values). Re-ask required fields; mark optional/uncertain ones as empty + flag.
4. Produce the three deliverables from the skill:
   - a section-by-section **summary** to confirm,
   - a **paste-ready JSON payload** (exact intake field names, `website: ""`),
   - the **submission options**.
5. **Default**: point them to **https://www.classic.tax/intake** with the summary as a copy
   guide. **Only if** the user asked to submit (or chose it now): POST the payload to
   `https://www.classic.tax/api/leads`; on success pass back the returned one-time
   `credentialsUrl`. If the bot-protection blocks it, fall back to the intake link.

## Output

- The confirmed summary, the JSON payload, and a clear next step (link or submission result).
- A short list of anything still missing or "to confirm with the accountant".
