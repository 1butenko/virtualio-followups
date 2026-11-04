# Config: investor follow-up drafter

No secrets in this file. The Notion and Airtable connectors handle sign-in.
Everything marked **SAMPLE** is invented test data, not Virtualio's real data.

## Notion: investor pipeline (SAMPLE)

| Setting | Value |
|---|---|
| Database | SAMPLE Investor Pipeline: https://app.notion.com/p/a0d1067d4f0d4217a23510212da8dc38 |
| Data source | `collection://7f6f12a9-9290-4f8a-a8d8-de03cf3a309c` |
| Investor name field | `Investor / Fund` (title) |
| Call date field | `Last contact` (date). See the decision doc: it can also change after an email. |
| Stage field | `Stage` (select) |
| Stages that get a draft | `First call done`, `Diligence` |
| "What they asked for" field | `Notes` (text) |
| Status field (write) | `Follow-up status` (select). The only value the drafter sets is `Draft ready`. |
| Draft field (write) | `Follow-up draft` (text) |

The real pipeline (`Fundraising Pipeline`, `collection://c4a76b64-1a55-839d-9b79-87501ac35a86`) is **not** used. Don't point the drafter at it until the decision doc's "Before going live" list is done.

## Airtable: MRR (SAMPLE, read-only)

| Setting | Value |
|---|---|
| Base | SAMPLE Virtualio MRR (`appAbWPNJzFeoC0Ms`) |
| Table | SAMPLE Monthly Revenue (`tbl8vhCClbUBteY1R`) |
| Month | `Month` (`fldWBo6IylpLZwDFH`), text `YYYY-MM` |
| Monthly plans gross | `fldTxb3Ey42TXlXz4` |
| Annual plans contract value | `fldeKsbd3DkQW95EN` |
| Discounts per month | `fldVlWJiXsbaD1JNf` |
| Months in the draft | Last **6 full months** by default. If `Notes` asks for a range (for example "since January"), cover that range instead. The current, partial month is never included. |

## MRR definition (ASSUMPTION for this exercise)

```
Net MRR = Monthly plans gross + (Annual plans contract value / 12) - Discounts per month
```

This is an assumption, **not Virtualio's agreed definition**. See the decision doc.

## Links (SAMPLE)

| Link | Value |
|---|---|
| Deck | SAMPLE: https://example.com/SAMPLE-virtualio-deck-v3 |
| Data room | *(empty: test run 1 must produce `MISSING: data room link`)* |
<!-- After test run 1, set data room to: SAMPLE: https://example.com/SAMPLE-virtualio-data-room -->

## Draft

| Setting | Value |
|---|---|
| Sign-off | Priya |
| Timezone for "before today" | America/New_York (Priya is in NYC) |
