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

## Team onboarding

Every teammate needs (a) the skills submodule mounted at `.cursor/skills/` and (b) the always-on rule at `.cursor/skills/dotb.mdc` activated via a symlink into `.cursor/rules/`. Do it once per clone.

### Step 1 — One-time Git config (per machine)

Only required if your organization hosts this submodule via a `file://` URL:

```bash
git config --global protocol.file.allow always
```

### Step 2 — Pull the skills submodule

Fresh clone of a consuming repo:

```bash
git clone <repo-url> && cd <repo>
git submodule update --init --recursive
```

Adding into a repo that does not have it yet:

```bash
git submodule add git@<host>:<org>/dotb-cursor-skills.git .cursor/skills
git submodule update --init --recursive
git add .gitmodules .cursor/skills
git commit -m "chore(cursor): add dotb-cursor-skills submodule"
```

### Step 3 — Activate the rule in Cursor

Cursor only auto-loads rules from `.cursor/rules/` at the workspace root. The rule lives inside the submodule at `.cursor/skills/rules/dotb.mdc`, so link it in (tracked in the consuming repo):

```bash
mkdir -p .cursor/rules
ln -s ../skills/rules/dotb.mdc .cursor/rules/dotb.mdc
git add .cursor/rules/dotb.mdc
git commit -m "chore(cursor): activate dotb rule via submodule symlink"
```

Windows teammates: symlinks require either Developer Mode + `ln` from Git Bash, `mklink /H .cursor\rules\dotb.mdc .cursor\skills\dotb.mdc` from an elevated shell, or committing a real copy and manually re-syncing after each submodule bump. Pick one convention team-wide.

### Step 4 — Verify

- Cursor → Settings → Rules: `dotb` appears as an active project rule (description "Dotb (SuiteCRM/SugarCRM fork) team conventions").
- Start a new chat and prompt "add a custom field on Contacts". The `dotb-field-manipulation` skill should auto-load and the generated code should follow the rule (prefix family question, bilingual `en_us` + `vn_vn` labels, Quick Repair & Rebuild reminder).
- After a test save, tail `./dotbcrm.log` and confirm log lines match the `ClassName::method - ...` convention.

### Step 5 — Keep skills + rule up to date

```bash
git submodule update --remote .cursor/skills
git add .cursor/skills
git commit -m "chore(cursor): bump dotb-cursor-skills"
```

Run monthly or whenever the skills repo announces a new entry in `CHANGELOG.md`.

### Step 6 — Contribute back

- **Low-friction:** add a dated recipe at `<skill>/examples/YYYY-MM-DD-<slug>.md`; one team reviewer.
- **Higher bar:** edits to `shared/references/*.md` or `dotb.mdc` itself; two reviewers + `CHANGELOG.md` entry. See `CONTRIBUTING.md`.

### Step 7 — Roll it out to the team

- Post Steps 1–4 in the team channel the day the submodule lands in the product repo.
- Add a one-liner in the product repo's root `README.md` pointing at its `AGENTS.md` Cursor skills section.
- Optional follow-up: a pre-commit or CI check that fails when `.cursor/rules/dotb.mdc` is missing after `git submodule update`, to catch teammates who skipped Step 3.

## Contributing

See `CONTRIBUTING.md`. Low-friction: add a dated file to `<skill>/examples/`. Higher bar: edit `shared/references/`.
