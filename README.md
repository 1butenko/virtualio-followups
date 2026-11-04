# Virtualio investor follow-up drafter

This drafts investor follow-up emails into the Notion pipeline. It never sends them.

- **Run it:** start Claude Code in this folder, then type `/investor-followup dry-run` (prints the drafts and writes nothing) or `/investor-followup`.
- **Settings:** `config.md`. It contains no secrets.
- **Rules and flow:** `.claude/skills/investor-followup/SKILL.md`.
- **Permissions:** `.claude/settings.json`. Gmail, Slack, deleting and schema changes are all blocked.
- **Why it's built this way:** `docs/decisions/2026-11-04-investor-followup-drafter.md`.

It currently runs against **SAMPLE** data only.
