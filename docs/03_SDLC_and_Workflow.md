# SDLC and Workflow

## Approach to SDLC

I use an iterative, phase-based approach tailored to real clinic workflows. Each phase starts with understanding the problem and mapping roles to user stories, then moves through implementation, testing, and refinement. This keeps the scope realistic while ensuring the system stays aligned with how clinics actually operate.

## Project Phases

**Phase 1: Core data model and backend foundation**
- **Goals:** Establish the minimum schema needed for inventory and clinical transactions.
- **Major activities:** Designed and created Supabase tables for `item`, `fact_form`, `fact_event`, `fact_event_line`, and initial metadata tables.
- **Delivered:** A stable data model and backend foundation to support future workflows.

**Phase 2: Offline-first and sync logic**
- **Goals:** Make the app reliable without internet and safe to sync later.
- **Major activities:** Implemented Hive local storage, dirty flags, RPC push/pull, and conflict resolution logic.
- **Delivered:** Offline-first data capture with controlled synchronization.

**Phase 3: Receive UI refactor and workflow polish**
- **Goals:** Improve usability and reduce errors during stock intake.
- **Major activities:** Refactored the Receive page, introduced reusable widgets, and separated services (validator, repository, save service, lookup services).
- **Delivered:** Cleaner UX, clearer validation, and a maintainable code structure for growth.

**Phase 4: Planned expansion**
- **Goals:** Extend coverage to more clinical and logistics workflows.
- **Major activities (planned):** Add issue/consumption flows, reports, dashboards, and role-based workflows.
- **Expected outcome:** Broader operational coverage without losing the offline-first reliability.

## From Clinic Workflows to User Stories

I translate clinic roles (nurse, doctor, cashier, admin) into concrete user stories that guide development. This keeps the backlog grounded in real work and reduces feature drift.

Example user stories:
- “As a nurse, I want to receive stock quickly with expiry validation so I don’t accidentally accept expired medicines.”
- “As an admin, I want a clear history of transactions so I can audit stock movement.”
- “As a cashier, I want to record payments linked to services so financial records align with clinical events.”
- “As a doctor, I want to record clinical transactions accurately so stock use matches patient care.”

## Day-to-Day Workflow

- Create GitHub issues or a personal backlog for each workflow or feature.
- Implement in small vertical slices to keep progress testable and visible.
- Review changes, run manual testing, and document outcomes before moving on.
- Apply UAT-style testing before marking a phase complete.

## Alignment with Digital Health Product Roles

This way of working mirrors roles like a Digital Initiatives Support Officer, which require coordinating between programme teams, developers, and vendors. I emphasize clear documentation, traceable decisions, and consistent testing, ensuring the product reflects real clinical processes while remaining technically sound.
