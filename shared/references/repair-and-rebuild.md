# Repair & Rebuild Reference

After any change under `custom/` — new module, new field, new hook, new layout — Dotb must regenerate compiled metadata. Skipping this step is the most common reason changes “do not show up.”

## Quick Repair & Rebuild (UI)

Admin → Repair → Quick Repair and Rebuild.

If Dotb shows SQL statements at the bottom, run them (they alter tables for new fields).

## Repair via CLI

```bash
cd <dotb-root>
php -r "
if (!defined('dotbEntry')) define('dotbEntry', true);
chdir(__DIR__);
require 'include/entryPoint.php';
\$repair = new RepairAndClear();
\$repair->repairAndClearAll(['clearAll'], [translate('LBL_ALL_MODULES')], false, false);
echo \"done\\n\";
"
```

Use the same entry constant as other CLI scripts in the consuming repo (Dotb uses `dotbEntry`, see e.g. `quick_repair.php` in this codebase).

## Rebuild Relationships

Admin → Repair → Rebuild Relationships. Required after:

- Adding a new relationship vardef.
- Changing an existing `link` or `relate` field definition.
- Adding or removing a subpanel that depends on a relationship.

## When to run which

| Change | Quick Repair | Rebuild Relationships |
|--------|-------------|----------------------|
| New custom field | Yes | No |
| New logic hook | Yes | No |
| New layout (detailview/editview/listview) | Yes | No |
| New relationship | Yes | Yes |
| Hybrid module registration (`hybrid_modules` change) | Yes | No |
| Sidecar view file added | Yes (also clear browser cache) | No |

## Log locations

- Application log: `./dotbcrm.log` (or the path set in `config.php` under `log_dir`).
- PHP errors: wherever `error_log` points (`php.ini`).
- Sidecar JS errors: browser DevTools console.

## Gotchas

- After editing metadata, hard-reload the browser — Sidecar bundles are cached aggressively.
- If a new field does not appear in Studio: verify the Extension file path and name, then run Quick Repair. Studio reads from compiled `cache/modules/.../Ext/Vardefs/vardefs.ext.php`.
- If a hook does not fire: check `cache/modules/<Module>/Ext/LogicHooks/logichooks.ext.php` for merged registration. If missing, re-run Quick Repair.
