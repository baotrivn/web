# Architecture Overview — baotrivn/web

**Target Deployment**: [https://baotrivn.github.io/web/](https://baotrivn.github.io/web/)  
**Preceding Repository**: [https://github.com/kreier/maintenance](https://github.com/kreier/maintenance)  
**Status**: Evolving / Active (October 2026)

---

## 1. System Vision & Modality

`baotrivn/web` is an open-source, multilingual maintenance documentation and tracking system for objects, buildings, and appliances.
It is architected around two flexible operating modes:

```text
               ┌────────────────────────────────────────────────────────┐
               │           Static Web Application (GitHub Pages)        │
               │             https://baotrivn.github.io/web/            │
               └───────────────────────────┬────────────────────────────┘
                                           │
                    ┌──────────────────────┴──────────────────────┐
                    │                                             │
                    ▼                                             ▼
       ┌─────────────────────────┐                  ┌───────────────────────────┐
       │     MODE A: LOCAL       │                  │     MODE B: ONLINE VAULT  │
       │    (Single User)        │                  │      (Multi-User / Team)  │
       ├─────────────────────────┤                  ├───────────────────────────┤
       │ • Browser-only          │                  │ • User Account Login      │
       │ • IndexedDB storage     │                  │ • Serverless Worker API   │
       │ • No account needed     │                  │ • Cloudflare R2 / S3      │
       │ • JSON/Zip Export/Import│                  │ • Roles & Audit Log       │
       │ • Zero server cost      │                  │ • Real-time synchronization│
       └─────────────────────────┘                  └───────────────────────────┘
```

1. **Local Mode (Default)**: Opens immediately in any browser with zero setup. Data is stored in the browser's persistent IndexedDB storage. Users can export their entire database as a portable file at any time.
2. **Online Vault Mode**: When attached to an online storage vault (e.g. Cloudflare Worker + R2), team members authenticate to access shared records, upload documentation, and synchronize across devices.

---

## 2. Architecture Decision Records (ADRs)

Key architectural and technical decisions made during the evolution of this project are documented in the [ADR Directory](file:///home/mk/AI-Agents/antigravity/web/docs/adr/README.md).

| ADR | Title | Decision Date | Status |
|---|---|---|---|
| [ADR-0001](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0001-record-architecture-decisions.md) | Record Architecture Decisions | 2026-10-13 | Accepted |
| [ADR-0002](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0002-static-first-client-architecture-and-hosting.md) | Static-First Client Architecture on GitHub Pages | 2026-10-07 / 2026-10-13 | Accepted |
| [ADR-0003](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0003-storage-strategy-local-browser-and-cloud-vault.md) | Storage Strategy: Local Browser Storage and Online Vaults | 2026-10-13 | Accepted |
| [ADR-0004](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0004-data-contracts-canonical-schemas-and-stable-ids.md) | Canonical Schemas and Stable Deterministic Entity IDs | 2026-10-07 | Accepted |
| [ADR-0005](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0005-browser-crypto-and-serverless-worker-auth.md) | Client-Side Cryptographic Derivation and Serverless Auth | 2026-10-07 | Accepted |

---

## 3. Timeline & Provenance

### Phase 0: Conceptual Foundation (October 7, 2026)
- **Repository**: `https://github.com/kreier/maintenance`
- **Contributor**: Matthias Kreier
- **Key Milestones**:
  - `34ba243` (21:14): Initial architecture, AGENTS guidelines, and roadmap documentation.
  - `b5146c8` (22:16): Canonical Zod schemas (`facility`, `room`, `object`, `maintenance-event`), validation script.
  - `d2f50a2` (22:43): Static web app with catalog search, room drill-down, deep links.
  - `677ae05` (22:53): Maintenance dashboard, service intervals, age calculation, overdue items, SVG analytics.
  - `2f48538` (23:24): Cloudflare Worker, PBKDF2 Web Crypto auth, session management, R2 storage endpoints.

### Phase 1: Web Deployment & Vault Decoupling (October 13, 2026)
- **Repository**: `https://github.com/baotrivn/web`
- **Live URL**: `https://baotrivn.github.io/web/`
- **Contributor**: baotrivn & AI Pair Programming
- **Key Milestones**:
  - `6ee53f1` – `fb9f4d5`: Setup GitHub Pages static deployment pipeline.
  - Establishment of ADR (Architecture Decision Record) workflow.
  - Expansion to support dual-mode: **Local Browser-Only (Single User)** alongside **Online Cloud Vault (Multi-User)**.
  - Adoption of flexible storage driver abstraction (`IVaultStorage`).

---

## 4. Storage Architecture

```text
               Unified Application Interface (IVaultStorage)
                                     │
                 ┌───────────────────┴───────────────────┐
                 │                                       │
                 ▼                                       ▼
     IndexedDB Storage Driver                Remote Vault Driver
   (Browser Local Persistence)           (HTTP / Web Crypto Session)
                 │                                       │
                 ▼                                       ▼
       Browser IndexedDB Store                 Cloudflare Worker API
       (Offline, Single-User)                            │
                                                         ▼
                                                Cloudflare R2 Bucket
                                                • data/ (Current JSONs)
                                                • history/ (Archival versions)
                                                • documents/ (PDFs, Images)
```

For detailed storage decisions and comparisons (such as IndexedDB vs. SQLite WASM vs. OPFS), see [ADR-0003](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0003-storage-strategy-local-browser-and-cloud-vault.md).
