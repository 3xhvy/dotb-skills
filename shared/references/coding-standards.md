# Dotb Team Coding Standards

## PHP Style

- PHP version: 7.4+ compatible (confirm against the consuming repo’s `composer.json` or server if newer is required)
- Always use `<?php` (never short tags)
- Class names: `PascalCase`
- Method/function names: `camelCase`
- Variables: `snake_case`
- Constants: `UPPER_SNAKE_CASE`
- Indentation: 4 spaces (no tabs)
- Always include a file-level docblock

## File Header Template

```php
<?php
/**
 * [Brief description of what this file does]
 *
 * @module     [ModuleName]
 * @author     [Dev name]
 * @date       [YYYY-MM-DD]
 * @upgrade    safe
 */
```

## Naming Conventions

- **Module and field prefix:** Dotb uses several prefix families (`J_*`, `C_*`, `hed_*`, `CJ_*`, `DRI_*`, `M_*`). There is no single default. **Always ask the developer which prefix to use** before naming a new module or custom field. See `prefix-families.md` for domain hints.
- **Hook files:** `[HookName]_[ModuleName].php` e.g. `BeforeSave_Accounts.php`
- **Logic hook class:** `[HookName]_[ModuleName]` e.g. `class BeforeSave_Accounts`
- **API endpoint files:** `[ModuleName][Action]Api.php` e.g. `AccountsSyncApi.php`

## Upgrade-Safe Rules (CRITICAL)

- NEVER edit stock Sugar/Dotb core files under `include/`, `data/`, `src/`, or stock modules you do not own
- For **extensions** to existing modules, ALWAYS use `custom/Extension/...` (never patch `modules/<Existing>/` directly unless the team explicitly overrides that rule)
- New **custom modules** may live under `modules/<NewModule>/` per team onboarding; that is not the same as patching core
- NEVER use `mysql_*` functions — use `$db->query()` or `DBManagerFactory`
- NEVER hardcode user IDs or team IDs
- ALWAYS use `sugar_cache_*` instead of APC/memcache directly

## Error Handling

```php
// Always log errors, never silent fails
$GLOBALS['log']->error('[ModuleName] ClassName::methodName - ' . $e->getMessage());

// For hooks: never throw uncaught exceptions (breaks Sugar's flow)
try {
    // your logic
} catch (Exception $e) {
    $GLOBALS['log']->fatal('[HookName] Failed: ' . $e->getMessage());
}
```

## Database Queries

```php
// CORRECT - use SugarQuery or $db->query with quoting
$db = DBManagerFactory::getInstance();
$safe_value = $db->quote($raw_input);
$result = $db->query("SELECT id FROM accounts WHERE name = '$safe_value'");

// NEVER do raw string concat with user input
```

## Sugar-Specific Helpers

- Retrieve bean: `BeanFactory::getBean('ModuleName', $id)`
- New bean: `BeanFactory::newBean('ModuleName')`
- Current user: `$GLOBALS['current_user']`
- App strings: `$GLOBALS['app_strings']`, `$GLOBALS['mod_strings']`
- Date utils: `TimeDate::getInstance()`
