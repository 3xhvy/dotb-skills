# Field Attributes Reference

Exhaustive field attribute reference used by the `field-manipulation` skill. For anything not covered here, check a real vardef in the Dotb repo (for example `modules/J_Class/vardefs.php`, `modules/Leads/vardefs.php`).

## Purpose

Use this reference to decide **which vardef attributes are valid for a new field type** and **which attributes require explicit developer confirmation** before you add them.

## Prerequisite Checks

1. Read `config.php` in the consuming repo and load the `$dotb_config` array.
2. Extract `hybrid_modules` from `$dotb_config['hybrid_modules']`:
   - Treat every module in this list as **hybrid/BWC-routed** for the purpose of field behavior and layout decisions.
   - For hybrid modules, use BWC-style PHP metadata for legacy layouts and allow Lumia customizations under `clients/base` when they already exist.
3. Determine the target module name (for example `Leads`, `Contacts`, `J_Class`).
4. Decide if the module is:
   - **Pure BWC**: `!in_array($module, $dotb_config['hybrid_modules'])` and only legacy PHP metadata exists under `modules/<Module>/metadata`.
   - **Hybrid**: `in_array($module, $dotb_config['hybrid_modules'])`.
   - **Lumia-first**: non-hybrid and uses `clients/base` definitions.

## Allowed Attribute Matrix

You MUST use this table before adding or modifying any vardef. Never insert attributes outside the allowed set for the field type.

### Core columns

- **Allowed**: you may set this attribute for the field type.
- **Confirm?**: you MUST ask the developer explicitly before setting this attribute.
- **Default behavior**: how to behave if the developer does not specify a value.

### Varchar

| Attribute          | Allowed | Confirm? | Default behavior                                                                 |
|--------------------|---------|----------|----------------------------------------------------------------------------------|
| `name`             | yes     | yes      | Required; must match requested field name.                                      |
| `vname`            | yes     | yes      | Required; always ensure a language label exists.                                |
| `type` = `varchar` | yes     | no       | Fixed for varchar fields.                                                       |
| `len`              | yes     | yes      | Default to `255` if developer does not specify.                                 |
| `required`         | yes     | yes      | Default to `false` if not specified.                                            |
| `default`          | yes     | yes      | Do not set if developer does not explicitly provide one.                        |
| `massupdate`       | yes     | yes      | Default to `false` to match most existing patterns.                             |
| `importable`       | yes     | yes      | Default to `true` for normal varchar unless developer says otherwise.          |
| `unified_search`   | yes     | optional | Only set when developer asks for search behavior.                               |
| `full_text_search` | yes     | optional | Only set when developer asks for FTS; copy pattern from similar fields.        |
| `comment`          | yes     | optional | Optional; never overwrite existing comments.                                    |
| `source`           | yes     | yes      | Only set when field is `non-db`; confirm DB vs non-DB with developer.          |

### Enum

| Attribute          | Allowed | Confirm? | Default behavior                                                                 |
|--------------------|---------|----------|----------------------------------------------------------------------------------|
| `name`             | yes     | yes      | Required.                                                                        |
| `vname`            | yes     | yes      | Required; language label must be created.                                       |
| `type` = `enum`    | yes     | no       | Fixed for enum fields.                                                           |
| `options`          | yes     | yes      | Required; ask for existing dropdown list name (for example `lead_status_dom`).  |
| `len`              | yes     | optional | Default to `100` if not specified, following core patterns.                     |
| `required`         | yes     | yes      | Default to `false` unless developer says required.                              |
| `default`          | yes     | yes      | Do not set if developer does not explicitly provide one.                        |
| `massupdate`       | yes     | yes      | Default to `false` unless developer wants mass update.                          |
| `importable`       | yes     | yes      | Default to `true` unless developer says otherwise.                              |
| `isMultiSelect`    | no      | n/a      | Not allowed on simple enum (only for multienum).                                |

### Multienum

| Attribute               | Allowed | Confirm? | Default behavior                                                                 |
|-------------------------|---------|----------|----------------------------------------------------------------------------------|
| `name`                  | yes     | yes      | Required.                                                                        |
| `vname`                 | yes     | yes      | Required; language label must be created.                                       |
| `type` = `multienum`    | yes     | no       | Fixed for multienum.                                                             |
| `isMultiSelect` = true  | yes     | no       | Always set to `true` for multienum.                                             |
| `options`               | yes     | yes      | Required; ask for existing dropdown list name.                                  |
| `len`                   | yes     | optional | Default to `255` if not specified.                                              |
| `required`              | yes     | yes      | Default to `false` unless developer says required.                              |
| `default`               | yes     | yes      | Do not set if developer does not explicitly provide one (default is empty).     |
| `massupdate`            | yes     | yes      | Ask explicitly; default to `false`.                                             |
| `importable`            | yes     | yes      | Default to `true` unless developer says otherwise.                              |
| `dbType` = `varchar`    | yes     | no       | Only when project pattern uses it (check existing multienum fields first).      |

### Relationship / link fields

These are **non-db** fields that represent relationships; they often come in pairs (link + relationship array) and may be accompanied by `relate` fields.

For a `link` field:

| Attribute        | Allowed | Confirm? | Default behavior                                                                 |
|------------------|---------|----------|----------------------------------------------------------------------------------|
| `name`           | yes     | yes      | Required.                                                                        |
| `type` = `link`  | yes     | no       | Fixed for link fields.                                                           |
| `relationship`   | yes     | yes      | Required; ask for relationship name or generate from modules.                   |
| `module`         | yes     | yes      | Required; ask for RHS module.                                                   |
| `bean_name`      | yes     | optional | Default to module bean if known; confirm if unclear.                            |
| `source`         | yes     | no       | Always `non-db`.                                                                |
| `vname`          | yes     | yes      | Required; label must exist.                                                     |
| `reportable`     | yes     | optional | Default to `false` for pure link fields unless developer wants reporting.       |

For a `relate` field:

| Attribute        | Allowed | Confirm? | Default behavior                                                                 |
|------------------|---------|----------|----------------------------------------------------------------------------------|
| `name`           | yes     | yes      | Required.                                                                        |
| `type` = `relate`| yes     | no       | Fixed.                                                                           |
| `vname`          | yes     | yes      | Required.                                                                        |
| `rname`          | yes     | yes      | Usually `name`; confirm if different.                                           |
| `id_name`        | yes     | yes      | Required; id field name must be provided or generated.                          |
| `module`         | yes     | yes      | Required; target module.                                                        |
| `table`          | yes     | optional | Use when project patterns include it; copy from similar existing relate field.  |
| `link`           | yes     | optional | Use when tying to a `link` field; copy pattern from existing examples.         |
| `dbType`         | yes     | optional | Only if project pattern sets it explicitly (for example `varchar`).            |
| `source`         | yes     | optional | Only `non-db` when field is not stored in DB.                                   |
| `massupdate`     | yes     | yes      | Default to `false`.                                                             |
| `importable`     | yes     | yes      | Default to `false` for non-db relate fields unless developer wants import.      |

## Attribute Confirmation Rules

Before writing any vardef, you MUST ask the developer the following, based on field type:

- **All field types**:
  - Confirm: field name, label key, target module (for relationships).
- **Varchar**:
  - Confirm: `len`, `default`, `importable`, `massupdate`.
- **Enum**:
  - Confirm: `options` dropdown list name, `len` (if not standard), `default`, `importable`, `massupdate`.
- **Multienum**:
  - Confirm: `options`, `len` (if not standard), `default`, `importable`, `massupdate`.
- **Relate / link**:
  - Confirm: relationship name, RHS module, whether the field is DB-backed or `non-db`, and whether it should appear in mass update or import.

When the developer answers ambiguously, follow this rule:

1. Prefer existing patterns in the same module:
   - Example: if similar enums in `Leads` use `len` `100` and `massupdate` `false`, mirror that unless told otherwise.
2. If still ambiguous:
   - Use conservative defaults from the tables above.
   - Do not set `default` values unless explicitly requested.

## Clarification Protocol

When preparing to add a new field, ask targeted questions using this template:

1. Identify type:
   - “What is the field type? (`varchar`, `enum`, `multienum`, `relate`, or `link` relationship)”
2. Ask type-specific attribute questions:
   - **Varchar**:
     - “What maximum length (`len`) do you want? Default is 255.”
     - “Do you want a default value? If yes, specify it exactly.”
     - “Should this field be importable? (yes/no)”
     - “Should this field support mass update? (yes/no)”
   - **Enum / Multienum**:
     - “Which existing dropdown list (`options`) should this field use?”
     - “Do you want a default value from that list? If yes, give the key.”
     - “Should this field be importable and/or mass-updateable?”
   - **Relate / link**:
     - “What is the relationship name, or should I generate one from `<lhs>_<rhs>`?”
     - “What is the RHS module?”
     - “Should the relate field be `non-db` helper only, or stored in DB as an `id` field?”

Never proceed to write vardefs until you have the answers required by the matrix above.

## Example references from Dotb

Use these as reference when deciding patterns:

- **Enum in core vardef (`Leads`)**:

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

- **Multienum in core vardef (`Leads`)**:

```217:228:modules/Leads/vardefs.php
            'dp_business_purpose' => array (
                'name' => 'dp_business_purpose',
                'vname' => 'LBL_DATAPRIVACY_BUSINESS_PURPOSE',
                'type' => 'multienum',
                'isMultiSelect' => true,
                'audited' => true,
                'options' => 'dataprivacy_business_purpose_dom',
                'default' => '',
                'len' => 255,
                'comment' => 'Business purposes consented for',
                'importable' => false,
            ),
```

- **Enum and multienum in custom vardef extension (`Contacts`)**:

```200:220:custom/Extension/modules/Contacts/Ext/Vardefs/vardefs.php
$dictionary['Contact']['fields']['prefer_level'] = array(
    'name' => 'prefer_level',
    'vname' => 'LBL_PREFER_LEVEL',
    'type' => 'multienum',
    'dbType' => 'varchar',
    'default' => '',
    'comments' => 'comment',
    'importable' => 'true',
    'duplicate_merge' => 'enabled',
    'duplicate_merge_dom_value' => '1',
    'audited' => true,
    'reportable' => true,
    'len' => 255,
    'size' => '20',
    'options' => 'kind_of_course_list',
    'studio' => 'visible',
    'dependency' => false,
    'massupdate' => true,
    'isMultiSelect' => true,
);
```

These examples define the idiomatic patterns you should mirror when generating new fields.
