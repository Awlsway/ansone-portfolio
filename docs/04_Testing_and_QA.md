# Testing and Quality Assurance

## Testing Approach

Testing in Ansone is currently mostly manual but systematic. I use UAT-style testing that mirrors how clinic users actually work, focusing on safe stock handling, reliable offline behavior, and trustworthy sync outcomes. Each phase has defined test flows that must pass before the phase is considered complete.

## Test Levels

- **Component/feature testing:** Validate specific UI and logic such as the Receive screen, batch identity validation, and line-item edits.
- **Integration testing:** Confirm the app, Hive storage, and Supabase backend work together as expected, including RPC sync behavior.
- **Workflow testing:** End-to-end scenarios from user action to synced data and audit trail confirmation.

## Key Test Scenarios

**Receive stock offline ? sync later ? verify in Supabase**
- **Preconditions:** Device is offline; lookups already cached; user has access to Receive screen.
- **Steps:** Create a Receive transaction with batch identity fields, save locally, reconnect, run sync.
- **Expected result:** Records appear in `fact_form`, `fact_event`, and `fact_event_line` with correct values; dirty flags clear and `updated_at` reflects server time.

**Edit an existing line and verify sync resolution**
- **Preconditions:** Existing Receive transaction is present locally and in Supabase.
- **Steps:** Edit a line item (quantity or batch fields), save locally, run sync.
- **Expected result:** `row_version` increments, `updated_at` changes, and remote-wins conflict rules resolve cleanly without duplicating lines.

**Lookup sync with corrupted or missing metadata**
- **Preconditions:** `app_table_meta` is incomplete or contains invalid entries.
- **Steps:** Trigger lookup sync on app startup or manual refresh.
- **Expected result:** App handles errors gracefully, logs/flags the issue, and continues running without corrupting local data.

**Connectivity loss mid-operation**
- **Preconditions:** Device starts online with active session.
- **Steps:** Begin a Receive workflow, disable connectivity before saving, then reconnect later.
- **Expected result:** Transaction persists locally, no data loss occurs, and the next sync completes successfully.

## Regression & Phase Testing

When refactoring the Receive flow or sync logic, I re-run critical scenarios to prevent regressions. I use a short checklist before marking a phase as stable:

- Receive flow works fully offline and syncs correctly.
- Sync resolves edits without duplicates or lost updates.
- Lookup sync completes or fails gracefully without breaking the app.
- Core audit trail fields (`row_version`, `updated_at`, soft delete flags) remain consistent.

## Quality Mindset

My testing approach prioritizes data integrity and clinical safety. Reliable stock records reduce the risk of expired or missing medicines, and trustworthy audit trails support accountability across roles. This QA focus aligns with digital health roles that oversee UAT and vendor quality, where structured test scenarios and clear acceptance criteria are essential for safe rollout.
