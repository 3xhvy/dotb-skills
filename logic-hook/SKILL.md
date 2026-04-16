---
name: dotb-logic-hook
description: Use this skill whenever a developer needs to create, register, or debug a Dotb logic hook — event-driven automation tied to bean save, retrieve, delete, or relationship events. Triggers on keywords "logic hook", "before_save", "after_save", "before_delete", "after_retrieve", "process_record", "hook", "auto-populate", "send email on save", "workflow trigger". Always use this skill before writing any hook code.
---

# Dotb Logic Hook Skill

You are helping a developer implement a Dotb logic hook. **Before generating any code, read:**

- `../shared/references/coding-standards.md`
- `../shared/references/file-structure.md`
- `../shared/references/prefix-families.md`
- `../shared/references/repair-and-rebuild.md`

## Clarification protocol (ask first, always)

1. **Target module** (core or custom, e.g. `J_Class`, `Accounts`).
2. **Hook event** — use exact keys from Dotb’s `LogicHook` docblock (e.g. `before_save`, `after_save`, `after_retrieve`, `process_record`, `before_delete`, `after_delete`, `before_relationship_add`, `after_relationship_add`).
3. **Behavior:** inputs (which fields), outputs (mutations, related saves, notifications).
4. **Guards:** new records only (`$args['isUpdate'] === false` in `after_save`), field-changed checks, etc.
5. **Prefix** for any new custom fields touched — ask; see `prefix-families.md`.

## How hooks load

Hooks merge from `custom/modules/<Module>/logic_hooks.php` and Extension files loaded via the LogicHook loader. Never overwrite another team’s `$hook_array` entries — always append.

Loader reference:

```98:123:include/utils/LogicHook.php
    public function loadHooks($module_dir)
    {
        $hook_array = array();
        if(!empty($module_dir)) {
            $custom = "custom/modules/$module_dir";
        } else {
            $custom = "custom/modules";
        }
        foreach(DotbAutoLoader::existing(
            "$custom/logic_hooks.php",
            DotbAutoLoader::loadExtension("logichooks", empty($module_dir)?"application":$module_dir)
        ) as $file) {
            include $file;
        }
        return $hook_array;
    }
```

## Real Dotb example (`J_Class`)

- `custom/modules/J_Class/logic_hooks.php` — merged registrations
- `custom/modules/J_Class/logicClass.php` — hook implementation class
- `custom/modules/J_Class/logicFilterClass.php` — filter-style hook

Mirror this layout for new hooks on custom modules.

## Workflow A — Extension registration (upgrade-safe)

File: `custom/Extension/modules/<ModuleName>/Ext/LogicHooks/[prefix]_[hookname]_[Module].php`

```php
<?php
/**
 * Registers [hook_event] on [ModuleName]
 *
 * @module     [ModuleName]
 * @author     [Dev name]
 * @date       [YYYY-MM-DD]
 * @upgrade    safe
 */

$hook_array['[hook_event]'][] = [
    10,
    '[Human label]',
    'custom/modules/[ModuleName]/[ClassName].php',
    '[ClassName]',
    '[methodName]',
];
```

Use `$hook_array['event'][] =` — never replace the whole `$hook_array`.

## Workflow B — Class file

File: `custom/modules/<ModuleName>/<ClassName>.php`

```php
<?php
/**
 * Logic hook: [brief]
 *
 * @module     [ModuleName]
 * @upgrade    safe
 */

class [ClassName]
{
    public function [methodName]($bean, $event, $arguments)
    {
        try {
            // logic
        } catch (Exception $e) {
            $GLOBALS['log']->error('[ClassName]::[methodName] - ' . $e->getMessage());
        }
    }
}
```

Never rethrow from a hook — log and return.

## Post-install

Quick Repair & Rebuild. Verify merged hooks in `cache/modules/<Module>/Ext/LogicHooks/logichooks.ext.php`. See `../shared/references/repair-and-rebuild.md`.

## Output format

1. Registration file — path + full content.
2. Class file — path + full content.
3. Test steps (save record; check `./dotbcrm.log`).

## Examples index

(Add dated files under `examples/` and list them here.)
