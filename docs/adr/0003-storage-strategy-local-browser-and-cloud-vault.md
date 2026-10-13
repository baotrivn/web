# ADR-0003: Storage Strategy — Local Browser Storage and Online Vaults

- **Status**: Accepted
- **Date**: 2026-10-13 (Evolved from 2026-10-07 R2-only design)
- **Deciders**: User, AI Assistant (Antigravity)
- **References**: AI Discussion on October 13, 2026; `ARCHITECTURE.md` in `kreier/maintenance`

## Context and Problem Statement

A maintenance website should serve two primary usage patterns:
1. **Single-user / Local-only**: A user opens the browser (or offline PWA) and uses the maintenance tool locally on their machine without needing an account, cloud setup, or internet connection.
2. **Multi-user / Synchronized Vault**: Multiple team members or property managers access shared facility data stored in an online vault (cloud storage), authenticating with their respective accounts.

In the initial design from October 7, 2026 (`kreier/maintenance`), all private data was designed to reside directly in a dedicated Cloudflare R2 bucket per installation. However, that required every user to configure Cloudflare credentials and an online worker, presenting a high barrier of entry for single-user local usage.

We need a flexible data storage architecture that supports:
- Running entirely locally in the browser.
- Seamlessly connecting to an online "Vault" (cloud storage) when multi-user collaboration or remote backup is needed.

## Decision Drivers

- **Simplicity for standalone users**: No login or external cloud infrastructure required to try or use the app locally.
- **Data sovereignty & offline capability**: Users retain full control of their data; operational even during internet outages.
- **Multi-user scalability**: Teams can attach an online vault (S3 / Cloudflare R2 / remote API) with authentication and access control.
- **Future-proofing**: Ability to swap storage engines (e.g. SQLite WASM vs. IndexedDB vs. OPFS) without rewriting business logic.

## Considered Options for Local Browser Storage

1. **IndexedDB (via Dexie.js / idb or key-value store)**
   - *Pros*: Native browser support across all browsers, zero binary dependencies, asynchronous, excellent for structured JSON object queries.
   - *Cons*: Limited relational querying capabilities compared to SQL.
2. **SQLite compiled to WebAssembly (WASM via OPFS / sql.js)**
   - *Pros*: Full relational SQL power, standard `.sqlite` file format exportable directly to disk.
   - *Cons*: Larger WASM bundle download, more complex concurrency management in multi-tab scenarios, requires OPFS (Origin Private File System) headers (COOP/COEP) which can be tricky on GitHub Pages.
3. **Local-First Sync Engine (e.g., CRDTs / Yjs / ElectricSQL / RxDB)**
   - *Pros*: Built-in conflict resolution for multi-device sync.
   - *Cons*: High architectural complexity for initial phase.

## Considered Options for Online Vault Storage

1. **Object Storage Vault (Cloudflare R2 / S3-compatible) via API Gateway / Worker**
   - Retains the model established on October 7, 2026: structured JSON objects (`data/objects/`, `data/maintenance/`) + media documents (`documents/manuals/`, `documents/photos/`) in bucket partitions.
2. **Database Backend (PostgreSQL, Supabase, Firebase)**
   - Requires hosting a database server or subscribing to vendor DB services.

## Decision Outcome

We adopt a **Dual-Mode Local-First / Vault Architecture**:

1. **Storage Abstraction Layer (`IVaultStorage`)**:
   The application communicates with a unified storage interface:
   - `getFacility()`, `getObject(id)`, `saveObject(obj)`
   - `listMaintenanceEvents()`, `logMaintenanceEvent(event)`
   - `uploadDocument(file, meta)`, `getDocument(id)`

2. **Local Browser Mode (Default)**:
   - Uses **IndexedDB** as the default lightweight client-side database (ideal for browser-only single user mode without WASM COOP/COEP header hurdles on GitHub Pages).
   - Provides one-click **Import/Export JSON or Zip archive** so local users can back up and transfer data anytime.
   - *Evaluation Note*: If complex relational querying or standalone `.db` file portability is needed later, an adapter for **SQLite WASM (via OPFS)** can be implemented behind `IVaultStorage` without breaking the application.

3. **Online Vault Mode (Multi-User & Remote Sync)**:
   - When a vault URL / account credentials are provided, the app connects to the remote vault endpoint (Cloudflare Worker + R2 or S3 API).
   - User logs in, authenticating their account, and the app reads/writes to the online vault while maintaining an offline cache in IndexedDB.
   - Multiple users can access the same online vault based on their role permissions (`viewer`, `editor`, `reviewer`, `administrator`).

## Consequences

### Positive
- Zero friction for new users: opens instantly in browser at `baotrivn.github.io/web` and works immediately without login.
- Clean architectural migration path: individual users can start locally and connect to an online vault as their facility grows.
- Swappable storage driver: SQLite WASM vs IndexedDB remains an implementation detail of the local driver.

### Negative
- Requires a synchronization or export/import workflow when transitioning from local browser data to an online vault.
- Conflict management needed if multiple users edit the same object concurrently in the online vault.
