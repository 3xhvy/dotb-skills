# Dotb Cursor Skills

Shared Cursor skill base for Dotb (SuiteCRM/SugarCRM fork) customization. Pulled into consumer repos as a git submodule at `.cursor/skills/`; Cursor auto-discovers every `SKILL.md`.

## Quick start (consuming repo)

```bash
git submodule add git@YOUR_GIT_HOST:YOUR_ORG/dotb-cursor-skills.git .cursor/skills
git submodule update --init --recursive
git add .gitmodules .cursor/skills
git commit -m "Add dotb-cursor-skills submodule"
```

Routine refresh:

```bash
git submodule update --remote .cursor/skills
git add .cursor/skills
git commit -m "Bump dotb-cursor-skills"
```

## What's inside

| Skill | Triggers on |
|-------|-------------|
| `module-onboarding` | Scaffold a new custom module (BWC / Lumia / hybrid) |
| `field-manipulation` | Add, modify, or remove fields on any module |
| `logic-hook` | Logic hooks and event-driven automation |
| `ui-layout` | List / detail / edit / subpanel views (BWC + Sidecar) |
| `api-integration` | Custom REST endpoints and external API calls |
| `reports-dashboards` | Reports, dashlets, scheduled delivery |

Cross-cutting references live in `shared/references/` (including **`module-name-mapping.md`**: English/Vietnamese UI labels → internal module keys). Per-skill recipes live in `<skill>/examples/`.

## Contributing

See `CONTRIBUTING.md`. Low-friction: add a dated file to `<skill>/examples/`. Higher bar: edit `shared/references/`.
