---
name: dotb-api-integration
description: Use this skill whenever a developer needs to create a custom REST endpoint in Dotb or integrate with an external API from server-side PHP. Covers DotbApi subclasses under custom/clients/base/api, route registration, auth context, config-backed credentials. Triggers on keywords "REST API", "custom endpoint", "external API", "webhook", "API call", "integration", "DotbApi", "registerApiRest". Always use this skill before writing any API code.
---

# Dotb API Integration Skill

You are helping a developer build a custom REST endpoint or outbound integration. **Before generating any code, read:**

- `../shared/references/coding-standards.md`
- `../shared/references/file-structure.md`

## Clarification protocol (ask first, always)

1. **Inbound vs outbound:** new REST route exposed by Dotb, or Dotb calling a third party?
2. **Inbound:** HTTP method(s), URL path segments after `/rest/v10/` (or version in use), public vs session-authenticated vs admin-only.
3. **Outbound:** target service, timeouts, retry policy, where credentials live (`config_override.php` / env — never hardcode secrets).
4. **Prefix** for any new beans/fields used for integration state — ask.

## Inbound — custom REST (Dotb pattern)

**Location:** `custom/clients/base/api/<Name>Api.php`

Dotb uses classes extending `DotbApi` with `registerApiRest()` returning a map of route definitions.

**Real example** — `MeetingApi.php`:

```1:22:custom/clients/base/api/MeetingApi.php
<?php

class MeetingApi extends DotbApi {
    function registerApiRest() {
        return array(
            'meeting-get-type' => array(
                'reqType' => 'PUT',
                'path' => array('Meetings', 'get-type'),
                'pathVars' => array(''),
                'method' => 'getType',
                'shortHelp' => '',
                'longHelp' => '',
            ),
```

**Skeleton for a new endpoint:**

```php
<?php

class [Prefix][Feature]Api extends DotbApi
{
    public function registerApiRest()
    {
        return array(
            '[route-key]' => array(
                'reqType' => 'GET',
                'path' => array('[ModuleOrNamespace]', '[action]'),
                'pathVars' => array(''),
                'method' => '[methodName]',
                'shortHelp' => '[short]',
                'longHelp' => '',
            ),
        );
    }

    public function [methodName](ServiceBase $api, array $args)
    {
        // Use BeanFactory, $GLOBALS['current_user'], etc.
        return array('success' => true);
    }
}
```

After adding the file, run **Quick Repair & Rebuild** so REST metadata is rebuilt.

## Outbound — calling external HTTP

- Prefer project HTTP utilities if they exist; otherwise PHP `curl` with timeouts.
- Log failures with `$GLOBALS['log']`.
- Read credentials from `$GLOBALS['dotb_config']` keys defined in `config_override.php` (never commit secrets).

## Error handling

Follow `coding-standards.md`: log exceptions; return structured error payloads for REST methods instead of uncaught exceptions when the API layer expects it.

## Post-install

See `../shared/references/repair-and-rebuild.md`.

## Output format

1. API class file(s) — full path + complete content.
2. Optional `config_override.php` snippet (no real secrets).
3. Example `curl` to hit the new route (with placeholder host/token).
4. Post-install: Quick Repair & Rebuild.

## Examples index

(Add dated files under `examples/` and list them here.)
