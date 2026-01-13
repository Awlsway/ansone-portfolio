# Ansone – Project Overview

Ansone is an offline-first inventory and clinical transaction app for low-resource clinics. It replaces paper and Excel stock ledgers with a reliable digital workflow that keeps working during internet outages while preserving auditability and safe stock tracking across multiple devices and roles.

## Problem Context

- Paper and spreadsheet-based logistics create errors, delays, and inconsistent records.
- Clinics often lack a single, trusted source of truth for stock and transactions.
- Unreliable connectivity makes typical cloud-only tools impractical in low-resource settings.

## Objectives

- Provide reliable stock tracking and clinical transaction capture.
- Ensure offline-first behavior with safe sync when connectivity returns.
- Support multi-device, multi-role teams in real clinic workflows.
- Maintain auditability and data consistency over time.

## Users & Use Cases

**User roles**
- **Nurse:** receives stock at the facility and confirms batch identity rules.
- **Doctor:** records clinical transactions linked to available stock.
- **Cashier:** captures payment-related transactions aligned to services.
- **Admin:** monitors high-level stock status and ensures compliance with workflows.

**Example flows**
- A nurse receives a delivery, records batch details, and the app validates identity rules before confirming stock.
- A doctor logs a clinical transaction; the system checks stock availability and records a traceable event.
- An admin reviews the latest stock picture across devices after sync to spot gaps or anomalies.

## Scope & Non-Scope (MVP)

**In scope now**
- Receive flows with batch identity rules for safe stock intake.
- Offline-first local storage using Hive with a usable basic UI.
- Sync to Supabase Postgres via a unified fact sync approach.
- Metadata-driven lookups that keep reference data flexible.

**Out of scope for MVP**
- Issue/consumption flows.
- Advanced reporting and dashboards.
- Full EMR or patient record management.
- Complex role-based access control beyond the current workflow needs.

## Why This Project Matters for Digital Health

Ansone demonstrates how offline-first design improves the quality of logistics data in low-resource clinics, where paper and spreadsheets often fail to provide reliable visibility. The project reflects a digital product mindset with clear scope definition, iterative delivery, and an emphasis on data integrity. It is directly relevant to roles such as a “Digital Initiatives Support Officer,” where understanding SDLC, coordinating between health workflows and technical systems, and driving practical digital transformation are essential.
