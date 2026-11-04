---
title: Investor follow-up drafter (SAMPLE build)
owner: Ihor Butenko
status: draft
date: 2026-11-04
type: decision
---

# Investor follow-up drafter (SAMPLE build)

This is the technical record for the follow-up drafter. For Ihor's reasoning across all five Virtualio processes, see [virtualio-automation.md](virtualio-automation.md).

## Why

Priya forgets or delays follow-up emails after investor calls. Last week, Harbor Ventures got no follow-up, and its data room was half-empty. Investors also keep asking for MRR by month. This automation writes the follow-up draft into the investor's pipeline row and sets the row's status to `Draft ready`. **It never sends anything.** Priya reviews each draft and sends it herself.

## Decisions

| # | Decision | Why | Rejected alternative |
|---|---|---|---|
| 1 | Built as a Claude Code skill that uses the Notion and Airtable connectors | No keys or tokens to store. The connectors handle sign-in. | A standalone script with API tokens in environment variables. It would mean more secrets to manage. |
| 2 | Tested on **SAMPLE** copies: `SAMPLE Investor Pipeline` (Notion) and `SAMPLE Virtualio MRR` (Airtable) | The real pipeline has no status or draft fields, and there's no real MRR-by-month data. Testing must not touch real investor records. | Adding fields to the real `Fundraising Pipeline` now. |
| 3 | **MRR definition (ASSUMPTION):** net MRR = active subscriptions' monthly value, with annual plans divided by 12 and discounts subtracted | Needed to run the exercise. | — |
| 4 | **Call date = `Last contact`** | It's the only date field in the pipeline. | Adding a separate `Call date` field. |
| 5 | Drafts only for Stage **First call done** or **Diligence** | Without this filter, investors who passed, committed angels and unverified DMs would get follow-ups with MRR attached. | Drafting for every row with an empty status. |
| 6 | Deck and data room links live in `config.md`. They're **SAMPLE** links for now. | No stable real links exist. The onboarding page says the Drive link changes. | Storing links in each investor's row. |
| 7 | "What they asked for" comes from `Notes`. When it's empty, the draft says `MISSING: what they asked for` | Never guess what an investor asked for. | Leaving that section out. |
| 8 | MRR covers the last 6 full months by default. If `Notes` asks for a range, the draft covers that range. The current partial month is always excluded. | A partial month understates MRR. Found in the dry run: Lightpost asked for "MRR since January", but the 6-month default showed only May to October, so the draft contradicted the request. | Always using a fixed 6 months. Always using all months. |

## Assumptions and known limits

- **The MRR definition is an assumption for this exercise. It is not Virtualio's agreed definition.** The only real MRR figure found was "~$19,400, Court's best estimate from Stripe" on the onboarding page, and nobody tracks month-over-month MRR cleanly. Before going live, Priya and Court must agree a definition, and this doc must be updated to match.
- **The SAMPLE MRR numbers are invented.** They were chosen so June ≈ $16,800 and October ≈ $19,400, matching the onboarding page, but every other value is made up.
- **`Last contact` is not only a call date.** It can also change after an email, not only after a call. A row can then look "followed up" when only an email was exchanged, or the date can move without a call. A dedicated `Call date` field would fix this.
- **Notes is empty on every real row.** The real fix is that **Priya writes Notes after each investor call**: what they asked for, in a line or two. Without it, every draft will say `MISSING: what they asked for`.
- Harbor Ventures, the investor that was missed last week, isn't in the real pipeline at all. The real pipeline is incomplete.

## Permissions

- **Notion:** the skill may only fetch, query and update pages. Creating, deleting, moving or duplicating pages and changing a database's structure are all denied in `.claude/settings.json`. The skill updates only `Follow-up draft` and `Follow-up status`.
- **Airtable:** must be re-authorised with **read-only** scope (`data.records:read`, `schema.bases:read`) on the **SAMPLE Virtualio MRR** base only. Until then, all Airtable write tools are denied in `.claude/settings.json`.
- **Gmail and Slack:** all tools are denied, so nothing can be sent.
- The deny rules only apply when Claude Code is started in this project folder.

## Before going live (real data)

1. Priya and Court agree on the MRR definition, and the real monthly data goes into an Airtable base that's shared read-only.
2. Add `Follow-up status` and `Follow-up draft` to the real `Fundraising Pipeline`. Optionally add a `Call date` field.
3. Priya fills in `Notes` after every call.
4. Use real, stable deck and data room links.
5. Point `config.md` at the real database and base, and run a dry run first.
