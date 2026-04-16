# Hybrid Modules Reference

Dotb supports three module modes: **BWC** (backward-compatible legacy), **Lumia** (Sidecar-only), and **hybrid** (both). The mode determines which metadata and view files the agent must generate, and whether the module must be registered in `$dotb_config['hybrid_modules']`.

## Decision tree

1. **Does the module need to work in the legacy backwards-compatible UI?** If no, go to step 3.
2. **Does the module need Sidecar/Lumia features (inline editing, modern list view, record view with dashlets)?**
   - Both yes: **hybrid**. Generate BWC metadata + Sidecar clients. Register in `hybrid_modules`.
   - Only BWC: **BWC-only**. Generate BWC metadata only. Do **not** register in `hybrid_modules`.
3. **Sidecar-only module.** Generate Sidecar clients only. Do **not** register in `hybrid_modules` (unless the module is explicitly hybrid per product requirements).

## File matrix

| Mode | BWC metadata | Sidecar clients | `hybrid_modules` entry |
|------|--------------|-----------------|------------------------|
| BWC-only | `modules/<M>/metadata/*.php` | — | No |
| Lumia-only | — | `modules/<M>/clients/base/...` | No |
| Hybrid | `modules/<M>/metadata/*.php` | `modules/<M>/clients/base/...` | Yes |

## Registering a hybrid module

Edit `config_override.php` in the consuming repo (never edit `config.sample.php`):

```php
$dotb_config['hybrid_modules'] = array_merge(
    $dotb_config['hybrid_modules'] ?? [],
    ['<ModuleName>']
);
```

## Reference list (from `config.sample.php`)

Verify at runtime in the consuming instance. This list is taken from `config.sample.php` as a reference:

`J_Kindofcourse`, `J_Class`, `J_Inventory`, `J_Inventorydetail`, `J_PTResult`, `J_StudentSituations`, `J_Targetconfig`, `J_Teachercontract`, `C_Attendance`, `C_Memberships`, `C_Rooms`, `C_Timesheet`, `Meetings`, `J_Invoice`, `Holidays`, `J_ConfigInvoiceNo`, `J_GradebookSettingGroup`, `J_GradebookDetail`, `J_GradebookConfig`, `J_Gradebook`, `Documents`, `M_Activities`, `J_MenuPlanner`, `hed_Balance`.

## Gotchas

- Registering a module in `hybrid_modules` without generating Sidecar clients can cause blank or broken record views.
- Generating Sidecar clients without registering a BWC-capable module in `hybrid_modules` when the product expects hybrid routing can cause inconsistent UI routing.
- After changing `hybrid_modules`, always run Quick Repair & Rebuild (see `repair-and-rebuild.md`).
