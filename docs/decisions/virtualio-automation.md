---
title: Virtualio: what to automate and why
owner: Ihor Butenko
status: draft
date: 2026-11-04
type: decision
---

# Virtualio: what to automate and why

## 1. Context and data

I took six processes from the Virtualio weekly sync with Priya (CEO), Court (sales) and Ren (founding engineer), and kept five. All data here is fictional, because I had no access to Virtualio's real pipeline or MRR. Nothing has run against real investors.

## 2. The five processes

- Court's investor metrics report (3 requests this week, 1.5 h each): automate, same numbers every time.
- Ren's onboarding (4-5 h per customer): simplify, as full self-serve costs a quarter of engineering.
- Priya's follow-up after investor calls: automate the draft, Priya sends, because she forgets to start.
- Priya's Notion pipeline update: simplify to one source of truth and delete the old spreadsheet, since two trackers drift.
- Court's Basecamp activation and churn numbers: not now, activation isn't tracked.

None of these is "leave to a person". Calling churned customers is, with only the trigger automated.

## 3. What I built

A Claude Code skill, `/investor-followup`. It picks SAMPLE Investor Pipeline rows in First call done or Diligence, with a past Last contact and empty follow-up fields. It reads MRR from the SAMPLE MRR base in Airtable, writes Follow-up draft, then sets Follow-up status to Draft ready. It never sends. Gaps become "MISSING: <what>".

The test drafted Lightpost and Basecamp and changed nothing else. With no data room link, both drafts said "MISSING: data room link". A second pass drafted nothing.

## 4. What I chose not to automate

Sending stays with Priya. Onboarding, Basecamp numbers and the churn survey wait. The old spreadsheet gets deleted, not synced. I don't write "what they asked for", because only Priya knows it.

## 5. Permissions per connector

- Notion: the connector sees my whole workspace; the skill only uses the SAMPLE copy with its two added fields. Deletes and schema changes are blocked.
- Airtable: my first authorisation could create in all bases, including the A-Players base with personal data. Now only SAMPLE MRR is visible. Read-only scope: NOT VERIFIED. Writes are blocked.
- Gmail and Slack are blocked. No keys or tokens are in the repo.
- Blocks apply only when Claude Code starts in this folder. My test started elsewhere.

## 6. Assumptions and open questions

- MRR = active subscriptions' monthly value, annual plans / 12, minus discounts. This is my assumption. Priya and Court must agree one, since Court gets a different number each time.
- Last contact also changes after an email. Add a Call date field?
- Are First call done and Diligence the right stages?
- Priya, will you write Notes after each call?
- Where do the real deck and data room links live?

## 7. What I would check in a month

- Every investor call gets a draft within a day.
- Priya sends most drafts with small edits.
- Fewer drafts say "MISSING: what they asked for".
- Draft MRR matches Court's number.
- Deleting or sending from this folder is blocked. NOT VERIFIED.
