# ADR-0004: Canonical Schemas and Stable Deterministic Entity IDs

- **Status**: Accepted
- **Date**: 2026-10-07 (Established in `kreier/maintenance` Phase 1 & 2), 2026-10-13 (Confirmed in `baotrivn/web`)
- **Deciders**: Matthias Kreier, AI Assistant
- **Traceability**: Commits `34ba243`, `b5146c8`, `d2f50a2` in `kreier/maintenance`

## Context and Problem Statement

A maintenance tracking system manages physical equipment across rooms and buildings (air conditioners, electrical panels, pumps, fire extinguishers).
Without consistent schema contracts and stable identifier schemes:
- Data becomes corrupted or inconsistent across different versions and storage backends.
- Deep links (e.g. scanning a QR code on an air conditioner to view its service manual) break if IDs are random database autoincrement IDs.
- Computed fields (such as equipment age, next service due date) can be stored as ambiguous prose (e.g., "installed about 3 years ago") rather than deterministic values.

## Decision Drivers

- Physical items must be easily referenceable via physical QR labels and deep links (`#/objects/{ID}`).
- Equipment and maintenance records must be interoperable between local browser storage and online vaults.
- Raw facts (dates, intervals) must be strictly separated from presentation and computed metrics.

## Considered Options

1. **Deterministic Structured Identifiers + Canonical Zod/JSON Schemas**
2. **UUIDs / Auto-increment database IDs without strict schemas**
3. **Freeform key-value notes or Wiki-style markdown files**

## Decision Outcome

Chosen option: **Deterministic Structured Identifiers + Canonical Schemas**.

1. **Stable ID Scheme**:
   - Equipment: `[CATEGORY]-[LOCATION]-[SEQUENCE]` (e.g., `HVAC-ROOM2-001`, `ELEC-MAIN-001`, `FIRE-AUD-001`).
   - Rooms: `ROOM-[SEQUENCE]` (e.g., `ROOM-001`).
   - Events: `EVT-[YEAR]-[SEQUENCE]` (e.g., `EVT-2026-0001`).
   - Documents: `DOC-[CATEGORY]-[SEQUENCE]` (e.g., `DOC-MAN-001`).

2. **Core Principle: Facts are Raw Data, Never Prose**:
   - Dates are stored as ISO 8601 strings (`installed: "2024-06-15"`).
   - Maintenance intervals are numbers (`interval_months: 6`).
   - The application computes age, days overdue, and upcoming maintenance dynamically.

3. **Schema Validation**:
   - Zod and JSON Schema definitions ensure data conforms to the specification before saving, whether in local browser storage or online vaults.

## Consequences

### Positive
- Predictable URLs suitable for physical QR codes.
- High data integrity across storage backends.
- Calculations (overdue alerts, calendars) remain accurate over time without stale computed text.

### Negative
- Requires user or generator to follow structured naming conventions for entity IDs.
