# dropdown / radio / checkboxGroup / refListStatus

All interactive variants use `editMode: "inherited"` (except `refListStatus`, which is display-only).

---

## dropdown

Two data sources: `'values'` (hardcoded) or `'referenceList'` (Shesha reference list).

### Reference list source

**Always read `referenceListId.module` and `referenceListId.name` from the property metadata — never guess.** The property object returned by `Metadata/GetProperties` has two fields you need:

- `property.referenceListModule` → `referenceListId.module`
- `property.referenceListName` → `referenceListId.name` (this is the fully-qualified list name, e.g. `"PBF.MembershipManagement.AccountType"`)

A wrong module/name pair produces a **silent empty dropdown** with no runtime error — there is no way to detect this without opening the form in a browser. Never infer or guess these values.

```json
{
  "id": "...",
  "type": "dropdown",
  "propertyName": "accountType",
  "label": "Account Type",
  "dataSourceType": "referenceList",
  "referenceListId": {
    "module": "PBF.MembershipManagement",
    "name": "PBF.MembershipManagement.AccountType"
  },
  "referenceListName": "PBF.MembershipManagement.AccountType",
  "valueFormat": "simple",
  "mode": "single",
  "editMode": "inherited"
}
```

**`referenceListId` is NOT a Guid** — it's `{ module, name }`. The framework resolves at runtime.

### Hardcoded values source

Every item must have all three keys (`id`, `label`, `value`):

```json
{
  "id": "...",
  "type": "dropdown",
  "propertyName": "preference",
  "dataSourceType": "values",
  "values": [
    { "id": "1", "label": "Email", "value": "email" },
    { "id": "2", "label": "SMS", "value": "sms" }
  ],
  "editMode": "inherited"
}
```

Validation: when `dataSourceType === "values"`, each item in `values` is `{ id, label, value }`. Missing any of the three is a bug — `clean-form-config` flags this.

`mode` is `single` | `multiple` | `tags`.

---

## radio / checkboxGroup

Same `dataSourceType` rules as `dropdown` (above). `radio` is single-select; `checkboxGroup` is multi-select.

---

## refListStatus

Display-only badge for a reference-list-typed property:

```json
{
  "id": "...",
  "type": "refListStatus",
  "propertyName": "status",
  "module": "PBF.MembershipManagement",
  "referenceListName": "ApplicationStatus",
  "showIcon": true
}
```

Note: `refListStatus` uses `module` + `referenceListName` (flat keys), not the `referenceListId: { module, name }` object that `dropdown` uses. Different shape — don't conflate.

No `editMode` needed (display-only).
