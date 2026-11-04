---
name: investor-followup
description: Draft investor follow-up emails into Virtualio's investor pipeline in Notion, with MRR by month from Airtable and the deck and data room links. Drafts only, never sends. Use when asked to draft, prepare or check investor follow-ups. Pass "dry-run" to print drafts without writing anything.
---

# Investor follow-up drafter

Writes a short follow-up draft into each investor row that needs one, so Priya can review it and send it herself.

All settings (database, fields, MRR definition, links) are in `config.md` at the project root. Read it first, and don't hard-code values from it here.

## Hard rules

- **Drafts only.** Never send an email, never delete anything, and never create or remove rows, fields or options.
- **Write only two fields:** `Follow-up draft` and `Follow-up status`, on rows the filter selected. Never change any other field.
- **Never guess.** If a number, a link or the investor's requests are missing, write `MISSING: <what>` in that spot.
- **MRR uses only the definition in `config.md`.** If the definition is missing or unclear, stop and ask.
- **No secrets** in any file. The connectors handle sign-in.

## Modes

- `/investor-followup dry-run`: steps 1 to 4, then print each draft. Write nothing.
- `/investor-followup`: all steps.

## Steps

1. **Find the rows.** Query the pipeline data source with these filters:
   - `Stage` is `First call done` or `Diligence`
   - `Last contact` is before today, in the timezone from `config.md`
   - `Follow-up status` is empty
   - `Follow-up draft` is empty (so a second run never writes a second draft)

   If no rows match, say so and stop.
2. **Read MRR once.** List all records in the Airtable table from `config.md`. Then, for each row:
   - **Choose the months.** If `Notes` asks for a specific range (for example "since January" or "last 12 months"), cover that range, ending with the last full month. Otherwise, use the last N **full** months before the current month. Never include the current, partial month.
   - Compute net MRR per month with the definition in `config.md`, and round to whole dollars.
   - Any month that's missing, or that's missing one of the three inputs, becomes `MISSING: MRR for <Mon YYYY>`.
3. **Read the links** from `config.md`. A blank link becomes `MISSING: deck link` or `MISSING: data room link`.
4. **Build one draft per row** with the template below.
   - "You asked for" is built from `Notes`. Turn each request into a short bullet, using the investor's own words. Don't add requests that aren't in `Notes`.
   - If `Notes` is empty, write `- MISSING: what they asked for (Notes is empty)`.
5. **Write** (skip this in dry-run). For each row, in this order:
   1. Set `Follow-up draft` to the draft text.
   2. Only if step 1 worked, set `Follow-up status` to `Draft ready`.

   Pass only these two properties in each update.
6. **Report.** Show a table with these columns: investor, written or dry-run, every `MISSING` item. Also list the rows that were skipped and why (stage, date, or already drafted).

## Draft template

Plain and short. No filler such as "hope you're well", "just circling back" or "excited to share".

```
Subject: Virtualio: follow-up from our call on <Last contact, e.g. Sep 24, 2026>

Hi <Investor / Fund>,

You asked for:
- <request>
- <request>

MRR by month (net: monthly plans + annual plans / 12, minus discounts):
<Mon YYYY>: $<value>
...

Deck: <deck link | MISSING: deck link>
Data room: <data room link | MISSING: data room link>

Priya
```

## Checks before reporting done

- Every written row has both fields set, and no other field changed.
- Every `MISSING` in the drafts is listed in the report.
- The MRR numbers in the drafts match the definition when recomputed from the Airtable inputs.
