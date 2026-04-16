---
name: dotb-reports-dashboards
description: Use this skill whenever a developer needs to create a custom report, dashlet, or scheduled report delivery in Dotb. Triggers on keywords "custom report", "dashlet", "dashboard", "scheduled report", "chart dashlet", "report module", "homepage dashlet". Always use this skill before writing any report or dashlet code.
---

# Dotb Reports & Dashboards Skill

You are helping a developer build reports, dashlets, or scheduled report delivery. **Before generating any code, read:**

- `../shared/references/coding-standards.md`
- `../shared/references/file-structure.md`
- `../shared/references/repair-and-rebuild.md`

## Clarification protocol (ask first, always)

1. **Artifact:** saved report in Reports module, list/chart dashlet, custom dashlet PHP class, scheduled export/delivery?
2. **Data source:** which module(s) / bean(s) / filters?
3. **Access:** teams, roles, run-as user for scheduled jobs?
4. **Schedule:** if recurring — frequency, channel (email, file), recipients.

## Where reports live

- Stock Reports module: `modules/Reports/`
- Customizations often under `custom/modules/Reports/` (verify in consuming repo).
- Project also has `modules/CustomReports/` — inspect for team-specific report patterns before inventing a new layout.

```bash
# Discovery commands (run in consuming repo root)
ls modules/Reports
ls custom/modules/Reports 2>/dev/null
ls modules/CustomReports 2>/dev/null
```

## Dashlets

Dashlets typically combine:

- A PHP class under `custom/modules/Home/Dashlets/<Name>/` or module-specific dashlet directories (mirror an existing dashlet in the same repo).
- Metadata / language strings for title and options.

**Do not guess paths** — locate the closest existing dashlet in the repo and copy its directory layout and naming.

## Scheduled reports

Use Admin → Schedulers or the project’s existing scheduler job pattern. Never hardcode credentials for outbound email; use system email config.

## Post-install

Quick Repair & Rebuild after metadata or new PHP classes. See `../shared/references/repair-and-rebuild.md`.

## Output format

1. File tree for all new/changed files.
2. Each file — full path + complete content.
3. Steps to register dashlet or scheduler in Admin UI (if applicable).
4. Post-install from `repair-and-rebuild.md`.

## Examples index

(Add dated files under `examples/` and list them here.)
