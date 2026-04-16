# Contributing

How to extend the Dotb Cursor skill base: recipes in `<skill>/examples/`, stable knowledge in `shared/references/`, and quality expectations on every pull request.

## Add a recipe (`examples/`) — low friction

Fork or branch `dotb-cursor-skills`, then add a dated recipe file at `<skill>/examples/YYYY-MM-DD-<slug>.md`. Use this template:

- State **what it demonstrates** in one line.
- Give **context**: the module involved, and a real path in the Dotb repo when it applies.
- Show **code**: prefer a Cursor code reference in the form `startLine:endLine:filepath` instead of pasting large blocks.
- Capture **why it matters / gotchas** — the lesson someone should take away.

Append one line to the **Examples index** in that skill’s `SKILL.md` so the new file is discoverable.

Open a pull request. You need **one team reviewer** and the PR must satisfy the checklist: template sections present, code references resolve, and no secrets. After merge, consuming projects refresh the submodule with `git submodule update --remote .cursor/skills`.

## Edit a reference (`shared/references/`) — higher bar

Open an issue or discussion first: references are the source of truth for cross-cutting behavior.

Submit a PR that includes your rationale and cross-references to motivating examples. **Two reviewers** are required for reference changes.

When you ship a reference change, update `CHANGELOG.md` under `## <date>` (or the project’s agreed heading for that release).

## Promote an example into a reference

Do this when three examples across any skill repeat the same insight, or when a reviewer flags content as evergreen.

In one PR: add the new section to the relevant `shared/references/*.md`. Slim or remove the source examples and replace them with a link into the reference. Log the promotion in `CHANGELOG.md`.

## Meet the quality bar on every PR

- Do not ship generic SugarCRM advice without a Dotb anchor: a real file path, real module name, or real config key from Dotb.
- Do not put secrets, tokens, customer IDs, or PII in code blocks.
- Do not use the `crm_` prefix anywhere. Use Dotb’s real prefix families or explicit `[ASK_PREFIX]` placeholders.
- Prefer code references in the `startLine:endLine:filepath` form over pasted blocks when the pattern already exists in a consumer repo.

## Smoke-test before merging a `SKILL.md` change

In a Cursor workspace that mounts this submodule, start a fresh chat.

Prompt with a trigger phrase from the skill’s `description` frontmatter.

Confirm Cursor loads the skill and that generated output follows the workflow: correct paths, correct conventions, and the agent asks before inventing details it should not assume.

If the skill does not trigger, tune the `description` keywords before you merge.
