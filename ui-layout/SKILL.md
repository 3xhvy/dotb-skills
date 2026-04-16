---
name: dotb-ui-layout
description: Use this skill whenever a developer needs to modify list, detail, edit, or subpanel layouts (BWC) or Sidecar record/recordlist views (Lumia) in Dotb. Covers detailviewdefs, editviewdefs, listviewdefs, subpaneldefs, record.js/hbs, buttons, panels. Triggers on keywords "list view", "detail view", "edit view", "edit layout", "add column", "subpanel", "record view", "sidecar view", "button", "panel". Always use this skill before changing any view or layout metadata.
---

# Dotb UI Layout Skill

You are helping a developer modify a view or layout. **Before generating any code, read:**

- `../shared/references/coding-standards.md`
- `../shared/references/file-structure.md`
- `../shared/references/hybrid-modules.md`
- `../shared/references/repair-and-rebuild.md`
- `../shared/references/module-name-mapping.md` — resolve UI labels to module keys for layout/view paths.

## Clarification protocol (ask first, always)

1. **Target module** and **view type:** detail, edit, list, subpanel, Sidecar `record`, `recordlist`, filter, dashboard layout.
2. **BWC vs Sidecar:** which layer changes? For hybrid modules, ask if both layers must stay in sync.
3. **Concrete change:** add/remove/reorder field, new panel, new button, new subpanel from parent.
4. **Prefix** for any new field keys — ask.

## BWC metadata

Place files under:

- `custom/modules/<Module>/metadata/detailviewdefs.php` (or edit/list/search)
- Or Extension: `custom/Extension/modules/<Module>/Ext/Layoutdefs/*.php`

Follow existing PHP array structure from the same module’s stock `modules/<Module>/metadata/*.php` as a template. Never strip unrelated panels or fields.

## Sidecar (Lumia)

Prefer Extension overlays:

`custom/Extension/modules/<Module>/Ext/clients/base/views/<view>/<view>.php`

Direct overrides when Extension is insufficient:

`custom/modules/<Module>/clients/base/views/<view>/`

**Canonical example module:** `J_Class` — inspect:

- `custom/Extension/modules/J_Class/Ext/clients/base/`
- `custom/modules/J_Class/clients/base/views/`

## Subpanels

- BWC: parent `metadata/subpaneldefs.php` or Extension `Layoutdefs`.
- Sidecar: `clients/base/layouts/subpanels/subpanels.php` under module or Extension `clients/base/layouts/subpanels/`.

## Post-install

Quick Repair & Rebuild; hard-refresh browser for Sidecar. See `../shared/references/repair-and-rebuild.md`.

## Output format

1. File tree.
2. Each file — full path + complete content.
3. Note if only one of BWC / Sidecar was updated for a hybrid module.

## Examples index

(Add dated files under `examples/` and list them here.)
