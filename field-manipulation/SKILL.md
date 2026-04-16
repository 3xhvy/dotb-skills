---
name: dotb-field-manipulation
description: Use this skill whenever a developer needs to add, modify, or remove custom fields on any Dotb module (core or custom). Covers vardef Extensions, dropdown/enum lists, relate fields with id_name/rname, 1:N and N:M relationships. Triggers on keywords "add field", "custom field", "vardef", "relate field", "dropdown field", "enum field", "remove field", "modify field", "relationship". Always use this skill before writing any field-related code.
---

# Dotb Field Manipulation Skill

You are helping a developer add, modify, or remove custom fields in Dotb. **Before generating any code, read:**

- `../shared/references/coding-standards.md`
- `../shared/references/file-structure.md`
- `../shared/references/field-attributes.md`
- `../shared/references/prefix-families.md`
- `../shared/references/repair-and-rebuild.md`

## Clarification protocol (ask first, always)

1. **Target module** and whether it is BWC, Lumia, or hybrid (`$dotb_config['hybrid_modules']`).
2. **Prefix** for the new field name — ask; do not invent. See `prefix-families.md`.
3. **Field spec:** machine name, label key, type (`varchar`, `enum`, `multienum`, `relate`, `link`, etc.), DB vs `non-db`, required, audited.
4. For **enum/multienum:** `options` list name; confirm keys and **both** `en_us` and `vn_vn` list labels in `custom/Extension/application/Ext/Language/` when creating new lists.
5. For **relate/link:** relationship name, RHS module, id field name, cardinality.

Use the attribute matrix in `field-attributes.md` for every attribute you set.

## Workflow

### Step 1 — Prefer Extension vardefs

Path template:

`custom/Extension/modules/<Module>/Ext/Vardefs/[descriptive_name].php`

Manipulate `$dictionary['<BeanName>']['fields'][...]` only. Never clear existing fields.

### Step 2 — Varchar example (extension)

Mirror real pattern:

```1:20:custom/Extension/modules/Contacts/Ext/Vardefs/vardefs.php
$dictionary['Contact']["fields"]["grade"] = array(
    'required' => false,
    'name' => 'grade',
    'vname' => 'LBL_GRADE',
    'type' => 'varchar',
    'massupdate' => 0,
    'no_default' => false,
    'importable' => true,
    'duplicate_merge' => 'disabled',
    'duplicate_merge_dom_value' => '0',
    'audited' => false,
    'reportable' => true,
    'unified_search' => false,
    'merge_filter' => 'disabled',
    'calculated' => false,
    'len' => '255',
    'size' => '20',
    'studio' => 'visible',
);
```

Add matching `$mod_strings` in **both** `en_us` and `vn_vn` under `custom/Extension/modules/<Module>/Ext/Language/`.

### Step 3 — Enum / multienum

Follow core enum style:

```14:23:modules/Leads/vardefs.php
            'last_call_status' => array(
                'name' => 'last_call_status',
                'vname' => 'LBL_LAST_CALL_STATUS',
                'type' => 'enum',
                'options'=>'call_status_dom',
                'len' => '100',
                'massupdate' => false,
                'no_default' => true,
                'importable' => false,
            ),
```

Register new dropdowns application-wide:

- `custom/Extension/application/Ext/Language/en_us.[prefix]_lists.php`
- `custom/Extension/application/Ext/Language/vn_vn.[prefix]_lists.php`

Ask the developer for Vietnamese option labels.

### Step 4 — Relate / link / relationships

Define `link` + `relate` + relationship arrays per existing module patterns. After relationship changes, run **Rebuild Relationships** (see `repair-and-rebuild.md`).

### Step 5 — Post-install

Quick Repair & Rebuild; Rebuild Relationships when relationships changed.

## Output format

1. File tree.
2. Each file — full path + complete content.
3. Post-install steps from `repair-and-rebuild.md`.

## Examples index

(Add dated files under `examples/` and list them here.)
