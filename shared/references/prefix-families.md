# Prefix Families Reference

Dotb’s customization prefixes map loosely to domains. There is **no default prefix** for new custom work. Always ask the developer which family a new module or field belongs to; **never invent a prefix silently**.

## Known families

| Prefix | Domain | Example modules |
|--------|--------|-----------------|
| `J_*` | Academic / school business (classes, inventory, invoicing, gradebook) | `J_Class`, `J_Inventory`, `J_Invoice`, `J_Gradebook`, `J_BankTrans` |
| `C_*` | Classroom / community (attendance, rooms, siblings, parents, news) | `C_Attendance`, `C_Rooms`, `C_Siblings`, `C_Parents`, `C_News` |
| `hed_*` | Higher-ed balance / fees | `hed_Balance` |
| `CJ_*` | Customer journey (forms, webhooks) | `CJ_Forms`, `CJ_WebHooks` |
| `DRI_*` | Workflow / sub-workflow templates | `DRI_Workflow_Templates`, `DRI_SubWorkflows` |
| `M_*` | Mobile-oriented activities | `M_Activities` |

## Rule the agent must enforce

When a developer asks to create a new module or custom field, the agent MUST:

1. Ask which family this belongs to (offer the table above as options).
2. If none fit, ask the developer to propose a new prefix — never generate one.
3. Never use `crm_` (legacy fictional convention; not used in Dotb).
4. Custom fields on core modules (`Accounts`, `Contacts`, `Leads`, etc.) still follow the chosen naming convention plus `_c` for custom DB columns where Sugar/Dotb applies it.

## Discovery

To inspect current prefix usage in a consuming repo:

```bash
ls modules/ | awk -F_ '{print $1"_"}' | sort -u
ls custom/modules/ | awk -F_ '{print $1"_"}' | sort -u
```
