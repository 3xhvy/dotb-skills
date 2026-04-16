---
name: dotb-module-onboarding
description: Use this skill whenever a developer needs to scaffold a brand-new custom module in Dotb (bean class, vardefs, metadata, language files, BWC vs Lumia vs hybrid registration). Triggers on keywords "new module", "create custom module", "scaffold module", "hybrid module", "register module", "module builder". Always use this skill before writing any module scaffolding code.
---

# Dotb Module Onboarding Skill

You are helping a developer scaffold a new custom module in Dotb. **Before generating any code, read:**

- `../shared/references/coding-standards.md`
- `../shared/references/file-structure.md`
- `../shared/references/hybrid-modules.md`
- `../shared/references/prefix-families.md`
- `../shared/references/repair-and-rebuild.md`

## Clarification protocol (ask first, always)

1. **Module internal name and display label.** Which prefix family (`J_*`, `C_*`, `hed_*`, `CJ_*`, `DRI_*`, `M_*`)? If none fit, ask the developer to propose one — never invent a prefix.
2. **Mode:** BWC-only, Lumia-only, or hybrid? See `hybrid-modules.md`.
3. **Initial fields:** at minimum a name field; list `(name, type, required?)`.
4. **Relationships** to existing modules and cardinality (1:1, 1:N, N:M).
5. **Vietnamese labels:** you MUST generate **both** `en_us` and `vn_vn` language files. Ask for the Vietnamese string for every user-facing label. Do not machine-translate or guess.

## Workflow

### Step 1 — Directory layout (new module)

Under `modules/<ModuleName>/`:

- `vardefs.php` — bean dictionary; mirror `modules/J_BankTrans/vardefs.php` or similar custom module.
- `metadata/detailviewdefs.php`, `editviewdefs.php`, `listviewdefs.php`, `searchdefs.php`, `SearchFields.php`, `subpaneldefs.php` (if needed).
- `metadata/metafiles.php` — mirror `modules/J_Class/metadata/metafiles.php`:

```3:11:modules/J_Class/metadata/metafiles.php
$module_name = 'J_Class';
$metafiles[$module_name] = array(
    'detailviewdefs'  => 'modules/' . $module_name . '/metadata/detailviewdefs.php',
    'editviewdefs'    => 'modules/' . $module_name . '/metadata/editviewdefs.php',
    'listviewdefs'    => 'modules/' . $module_name . '/metadata/listviewdefs.php',
    'searchdefs'      => 'modules/' . $module_name . '/metadata/searchdefs.php',
    'popupdefs'       => 'modules/' . $module_name . '/metadata/popupdefs.php',
    'searchfields'    => 'modules/' . $module_name . '/metadata/SearchFields.php',
);
```

- Bean class file `<ModuleName>.php` at module root (follow existing custom beans).

Register `VardefManager::createVardef` like `modules/Leads/vardefs.php` (tail of file) for traits (`default`, `assignable`, `team_security`, etc.) appropriate to the bean.

### Step 2 — Lumia / Sidecar (when Lumia or hybrid)

Add under `modules/<ModuleName>/clients/base/` as needed:

- `views/record/record.php`
- `views/recordlist/recordlist.php`
- `layouts/subpanels/subpanels.php`

Post-onboarding overrides prefer `custom/Extension/modules/<M>/Ext/clients/base/...`.

### Step 3 — Language files (mandatory bilingual)

Create **both**:

- `custom/Extension/modules/<ModuleName>/Ext/Language/en_us.lang.php`
- `custom/Extension/modules/<ModuleName>/Ext/Language/vn_vn.lang.php`

Pattern:

```1:8:custom/Extension/modules/Leads/Ext/Language/en_us.lang.php
$mod_strings['LBL_TENANT_FIELD'] = 'tenant field';
$mod_strings['LBL_SCHEDULE_CALL_STATUS'] = 'Schedule Call Status';
```

Mirror every `$mod_strings` key in `vn_vn.lang.php` with developer-supplied Vietnamese text.

### Step 4 — Hybrid registration

If hybrid, append to `$dotb_config['hybrid_modules']` in `config_override.php` (never `config.sample.php`). See `hybrid-modules.md`.

### Step 5 — Relationships and subpanels

Use **dotb-field-manipulation** for `link` / `relate` vardefs and relationship arrays. Use **dotb-ui-layout** for subpanels (BWC `subpaneldefs` or Sidecar `layouts/subpanels`).

### Step 6 — Manifest (optional)

If packaging externally, describe `manifest.php` + `installdefs` at a high level only; keep checked-in work under `modules/` and `custom/`.

### Step 7 — Post-install

Quick Repair & Rebuild; Rebuild Relationships if relationships added. See `../shared/references/repair-and-rebuild.md`.

## Output format

1. Directory tree of every file to create.
2. Each file — full path + complete content (no truncation).
3. `hybrid_modules` snippet if hybrid.
4. Post-install checklist.

## Examples index

(Add dated files under `examples/` and list them here.)
