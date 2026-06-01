# Trustcore — Insurance Agency Salesforce Configuration

Salesforce DX source for the Insurance Agency setup on the Trustcore org
(`orgfarm-174eb5b501-dev-ed.develop.my.salesforce.com`).

Plan designed by Traycer AI; implemented as metadata-as-code and validated
against the org with `sf project deploy start --dry-run` (0 errors).

## Contents (`force-app/main/default/`)

| Step | Component | Type |
|---|---|---|
| 1 | `OpportunityStage` adds **Pending** (Open, prob 10) | StandardValueSet |
| 2 | `SSN__c`, `Policy_Number__c`, `Date_of_Birth__c`, `Insurance_Company__c`, `Application_Status__c` | Opportunity CustomFields |
| 3 | `Task_Type__c`, `Requirement__c`, `Worker_Response__c` | Activity CustomFields (surface on Task) |
| 4 | Field-Level Security for all new fields | Admin & Standard Profiles |
| 5 | **Insurance Agency Layout** + profile assignment | Opportunity Layout |
| 6 | Custom fields added | Task Layout |
| 7 | **Pending Opportunities - Insurance Agency** in public folder | Report + ReportFolder |
| 8 | **Insurance Agency - Pending Prospects** (table from the report) | Dashboard + DashboardFolder |

## Deviations from the original plan (forced by platform limits)

1. **`SSN__c` is `Text(20)`**, not `EncryptedText` — org has no Classic/Shield
   Encryption. Protected via Field-Level Security. (Was the plan's fallback.)
2. **`Requirement__c` / `Worker_Response__c` are `Text Area (255)`**, not
   `LongTextArea(32768)` — Salesforce does **not** allow Long Text Area on
   Activities (Task/Event).
3. **Opportunity layout** also includes `AccountId` and `Probability` — the org
   requires these fields on the layout.
4. **Dashboard** uses `autoselectColumnsFromReport`; CloseDate-ascending order is
   noted in the component footer.

## Deploy

```bash
sf org login web --alias trustcore        # or your preferred auth
sf project deploy start --target-org trustcore
```

Validate without deploying:

```bash
sf project deploy start --dry-run --target-org trustcore
```
