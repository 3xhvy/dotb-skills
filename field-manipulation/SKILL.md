---
name: dotb-field-manipulation
description: Use this skill whenever a developer needs to add, modify, or remove custom fields on any Dotb module (core or custom). Covers vardef Extensions, dropdown/enum lists, relate fields with id_name/rname, 1:N and N:M relationships. Always ask whether new fields go in custom/Extension/modules/<M>/Ext/Vardefs/ (upgrade-safe) or modules/<M>/vardefs.php (module tree; warn on stock modules). Triggers on keywords "add field", "custom field", "vardef", "relate field", "dropdown field", "enum field", "remove field", "modify field", "relationship". Always use this skill before writing any field-related code.
---

# Dotb Field Manipulation Skill

You are helping a developer add, modify, or remove custom fields in Dotb. **Before generating any code, read:**

- `../shared/references/coding-standards.md`
- `../shared/references/file-structure.md`
- `../shared/references/field-attributes.md`
- `../shared/references/prefix-families.md`
- `../shared/references/repair-and-rebuild.md`
- `../shared/references/module-name-mapping.md` — **read before asking “which module?”** Match UI labels and the **Spoken aliases** section (case-insensitive).

## Clarification protocol (tiered — do not spam menus)

### Step 0 — Resolve the module (mandatory, silent if possible)

1. Open `module-name-mapping.md`. Match the user’s words against the big table **and** the **Spoken aliases** subsection.
2. If **one** module clearly applies (e.g. “create field on student” / “field in student” → **`Contacts`** per aliases; “schedules” → `Meetings`), **state the resolved module key in one sentence** and proceed. **Do not** present a long A/B/C/D list for that case.
3. Ask **only** when two or more modules remain plausible after aliases (e.g. person vs class enrollment). Then **one short question**, not a full form letter.

Then read `$dotb_config['hybrid_modules']` in the consumer `config.php` / `config_override.php` to know if the resolved module is hybrid (for metadata follow-up only; vardef path is still extension-first).

### Step 0a — Where the vardef lives (mandatory — ask; do not assume)

Unless the user **already** chose explicitly, you MUST ask **once** which physical location they want for the field definition:

| Choice | Path | When it is appropriate |
| ------ | ---- | ---------------------- |
| **A — Extension (upgrade-safe, default recommendation)** | `custom/Extension/modules/<ModuleKey>/Ext/Vardefs/<descriptive_name>.php` | **Always** for stock/core modules (`Accounts`, `Contacts`, `Leads`, `Meetings`, …). Also for extending **any** existing module when the team wants merged metadata and safe upgrades. |
| **B — Module-root vardefs** | `modules/<ModuleKey>/vardefs.php` (same `$dictionary['<BeanName>']['fields'][...]` shape) | Primarily for **new custom modules** the team fully owns, where the bean’s dictionary is maintained in-repo under `modules/<ModuleKey>/`. |

**Rules:**

- **Never** silently pick A or B. If the user did not say, ask: *“Put this field in `custom/Extension/.../Ext/Vardefs/` (upgrade-safe) or in `modules/<Module>/vardefs.php` (module tree)?”* and wait for an answer.
- If they choose **B** on a **stock/core** module, **stop and warn in one sentence**: direct edits to `modules/<Core>/vardefs.php` are **not upgrade-safe** and are overwritten by product upgrades; recommend **A** again. Only proceed with **B** if they **confirm in writing** they accept that risk (some teams have a forked core — still document the trade-off).
- If they choose **A**, follow the path template in Workflow Step 1 below.
- If they choose **B** for a custom module, edit only `$dictionary['<BeanName>']['fields'][...]` in `modules/<ModuleKey>/vardefs.php`; never delete unrelated fields; match the style of that file.

### What you must still ask (unless the user already said it)

1. **Prefix / field naming** — do not invent a new `J_`/`C_` family prefix; see `prefix-families.md`. For **core** modules (`Contacts`, `Accounts`, `Leads`), mirror existing `custom/Extension/modules/<Module>/Ext/Vardefs/` naming patterns in that repo; if unclear, ask **one** question.
2. **Field spec** if missing: machine name (or propose from label and ask confirm), type, required/audited as needed.
3. **Enum/multienum:** `options` list name; **both** `en_us` and `vn_vn` list labels when creating new app list strings.
4. **Relate/link:** relationship name, RHS module, id field, cardinality — always confirm if not given.

Use the attribute matrix in `field-attributes.md` for every attribute you set.

## Workflow

### Step 1 — Apply the vardef placement from Step 0a

- **If the user chose Extension (A):** use

  `custom/Extension/modules/<Module>/Ext/Vardefs/[descriptive_name].php`

- **If the user chose module-root (B) and it is allowed (see Step 0a):** edit

  `modules/<Module>/vardefs.php`

In both cases manipulate `$dictionary['<BeanName>']['fields'][...]` only. Never clear existing fields.

### Step 2 — Varchar example (same field array shape for A or B)

For **Extension (A)**, mirror this real pattern (paths differ; dictionary shape is identical for **B** in `modules/<Module>/vardefs.php`):

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

1. One line restating **Step 0a** choice: Extension (`custom/Extension/.../Ext/Vardefs/`) vs module-root (`modules/.../vardefs.php`).
2. File tree.
3. Each file — full path + complete content.
4. Post-install steps from `repair-and-rebuild.md`.

## Examples index

(Add dated files under `examples/` and list them here.)
