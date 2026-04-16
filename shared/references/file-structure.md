# Dotb File Structure Reference

## Custom Directory Map

```
custom/
├── Extension/
│   ├── modules/
│   │   └── [ModuleName]/
│   │       ├── Ext/
│   │       │   ├── Language/          ← Module language strings
│   │       │   ├── Layoutdefs/        ← Layout extensions
│   │       │   ├── LogicHooks/        ← Hook registration (logic_hooks.php files)
│   │       │   ├── Vardefs/           ← Field definitions
│   │       │   ├── WirelessLayoutdefs/← Mobile layouts
│   │       │   └── clients/
│   │       │       └── base/
│   │       │           ├── views/     ← Sidecar view extensions (upgrade-safe)
│   │       │           ├── layouts/
│   │       │           └── filters/
│   └── application/
│       └── Ext/
│           └── Language/              ← App-wide language strings
│
├── modules/
│   └── [ModuleName]/
│       ├── clients/
│       │   └── base/
│       │       ├── views/             ← View metadata (list, detail, record)
│       │       ├── layouts/
│       │       └── filters/
│       ├── language/
│       ├── metadata/
│       └── [HookClass].php            ← Logic hook class files
│
├── include/
│   └── api/                           ← Legacy custom REST API endpoints
│
└── clients/
    └── base/
        └── api/                       ← Modern custom REST API (when used)
```

## Where Each Customization Goes

| Task | Directory |
|------|-----------|
| Add a custom field | `custom/Extension/modules/[Module]/Ext/Vardefs/` |
| Register a logic hook | `custom/Extension/modules/[Module]/Ext/LogicHooks/` |
| Logic hook class file | `custom/modules/[Module]/` |
| Modify list view columns (Sidecar) | `custom/modules/[Module]/clients/base/views/list/` or `custom/Extension/modules/[Module]/Ext/clients/base/views/list/` |
| Modify detail/edit layout (Sidecar) | `custom/modules/[Module]/clients/base/views/detail/` or `custom/Extension/.../clients/base/views/detail/` |
| Add language label | `custom/Extension/modules/[Module]/Ext/Language/` |
| Custom REST API (legacy) | `custom/include/api/` |
| Custom REST API (modern) | `custom/clients/base/api/` (verify in consuming repo) |
| Custom report | `custom/modules/Reports/` or `modules/CustomReports/` |

## BWC vs Lumia vs hybrid paths

| Mode | BWC metadata (`modules/<M>/metadata/...`) | Sidecar clients (`modules/<M>/clients/base/...`) | Registered in `hybrid_modules`? |
|------|-------------------------------------------|--------------------------------------------------|--------------------------------|
| BWC-only | Required | Optional | No |
| Lumia-only | Not required | Required | No |
| Hybrid | Required | Required | Yes (via `$dotb_config['hybrid_modules']`) |

See `hybrid-modules.md` for the full decision tree.

## Module display names vs internal keys

Navigation and Studio often show **translated labels** (e.g. “Students”, “Schedules”, “Học viên”) while code uses **module keys** (`Contacts`, `Meetings`, `J_Class`). When the user’s wording does not match a folder name under `modules/`, read `module-name-mapping.md` or the live files `custom/Extension/application/Ext/Language/en_us.moduleList.php` and `vn_vn.moduleList.php`.

## After Making Changes

See `repair-and-rebuild.md` for Quick Repair & Rebuild, Rebuild Relationships, CLI, and log locations.

## Module Loader Packages

When packaging for deployment, structure as:

```
package/
├── manifest.php
└── Files/
    └── custom/       ← mirrors the custom/ directory above
```
