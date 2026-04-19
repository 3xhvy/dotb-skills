# Changelog

All notable changes to `dotb-cursor-skills` are logged here. Append-only. Newest at top.

## Unreleased

## 2026-04-19

- Rename `cursorrules` → `dotb.mdc` and rewrite as a Dotb-specific, always-on project rule (MDC frontmatter `alwaysApply: true`). Removes SugarCRM 6.x / `pfx_` / `sugarcrm.log` / `v4_1` legacy content; aligns with Sidecar + BWC + hybrid architecture, `DotbApi`, `$dotb_config`, `./dotbcrm.log`, `dotbEntry`, prefix families, bilingual `en_us` + `vn_vn` labels, and PHP 7.4+. Delegates to the six SKILLs instead of restating them.
- `README.md`: add **Team onboarding** (7 steps) covering submodule pull, `.cursor/rules/dotb.mdc` symlink, verification, keeping up to date, contribution bar, and team rollout. Windows symlink caveat included.

## Older unreleased

- Field skill: tiered clarification — resolve vague “student” etc. via `module-name-mapping` **Spoken aliases** before multi-choice module prompts.
- Module mapping: add **Spoken aliases** (default `Contacts` for student/students/học viên unless enrollment/situation context).
- Add `shared/references/module-name-mapping.md` (UI labels EN/VI → module keys from `en_us.moduleList.php` / `vn_vn.moduleList.php`).
- Link module mapping from all SKILLs and `file-structure.md`.

## 0.1.0

- Initial scaffolding, six skills, shared references.
