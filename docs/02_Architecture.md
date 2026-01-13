# Architecture Overview

## High-Level Design

Ansone is built as an offline-first Flutter application with a local data layer (Hive) and a cloud backend (Supabase Postgres). The app assumes connectivity can be unreliable, so core workflows write locally first and sync later.

The Flutter UI interacts with Hive for immediate reads and writes. Supabase provides centralized storage, security, and synchronization via RPCs. This design allows multiple devices and roles to work independently while maintaining a shared, auditable record once sync completes.

## Frontend (Flutter + Hive)

- **UI layout:** The current MVP centers on the Receive screen for stock intake. The layout is focused on fast data entry and validation of batch identity fields.
- **Reusable building blocks:** Shared widgets and services handle transactions and validation, such as `TxnLineVM`, `validator`, `repository`, `save_service`, and lookup services.
- **Hive usage:**
  - Local-first writes for fact data to keep the app responsive and offline-capable.
  - Boxes for lookups, fact tables, settings, sync state, and device identity.
  - Dirty flags and client metadata stored with rows to mark pending sync.
  - Lookup caches to avoid hard-coded lists and enable dynamic reference data.

## Backend (Supabase Postgres)

- **Fact tables:**
  - `item` stores the core inventory items.
  - `fact_form` stores higher-level transaction groupings.
  - `fact_event` stores transaction events.
  - `fact_event_line` stores line-level details per event.
- **Lookup and metadata tables:**
  - `app_table_meta` defines which lookup tables exist and how they are discovered.
  - `app_settings` and `user_profile` hold configuration and user-related metadata.
- **Triggers and data rules:**
  - Automatic timestamp handling for `updated_at`.
  - `row_version` increments to support sync resolution.
  - Batch identity derived from `des_id`, `expire_date`, `lot_no`, `supply_org`, and `location_id`.
  - Soft delete flags for safe removal without losing history.
  - `origin_device_id` and `client_uuid` to track data provenance.
- **Security:**
  - Row-Level Security (RLS) policies enforce access boundaries across users and roles.

## Sync Architecture

- **Unified fact sync:**
  - Push and pull are consolidated via RPCs: `rpc_fact_push_rows` and `rpc_fact_pull_changes`.
  - Local dirty rows are pushed to the backend; remote changes are pulled back.
- **Conflict resolution:**
  - Remote-wins logic based on `updated_at` and `row_version`.
  - Each row carries `client_uuid` and `origin_device_id` to support traceability.
- **Sync state tracking:**
  - A `sync_state_box` (or equivalent) tracks last sync checkpoints per table.
- **Lookup sync:**
  - Lookup tables are pulled at startup and can be refreshed on demand.

## Key Design Decisions & Trade-offs

- **Offline-first:** Prioritized to ensure usability in low-connectivity clinics, even if it adds sync complexity.
- **Metadata-driven lookups:** Chosen to avoid hard-coded reference data and enable flexible updates without app redeploys.
- **Unified RPC sync:** Simplifies client logic and makes conflict handling consistent across fact tables.
- **Trade-offs:** More backend complexity and careful sync resolution logic in exchange for resilience and multi-device support.

## Component Diagram (Text)

```
Flutter UI
  |  local writes/reads
  v
Hive Boxes (facts, lookups, settings, sync state, device identity)
  |  push/pull via RPC
  v
Supabase Postgres
  - fact tables: item, fact_form, fact_event, fact_event_line
  - lookups + metadata: app_table_meta, app_settings, user_profile
  - RLS, triggers, row_version, updated_at
```
