# Architecture Decision Records (ADRs)

Architecture Decision Records (ADRs) capture important architectural, structural, and design decisions made over the course of the project, including their context, rationale, tradeoffs, and timeframe.

## Decision Log

| ID | Title | Date | Status | Context / Origin |
|---|---|---|---|---|
| [ADR-0001](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0001-record-architecture-decisions.md) | Record Architecture Decisions | 2026-10-13 | Accepted | Process & repository standard |
| [ADR-0002](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0002-static-first-client-architecture-and-hosting.md) | Static-First Client Architecture Deployed to GitHub Pages | 2026-10-07 / 2026-10-13 | Accepted | Originated in `kreier/maintenance`, transitioned to `baotrivn.github.io/web` |
| [ADR-0003](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0003-storage-strategy-local-browser-and-cloud-vault.md) | Storage Strategy: Local Browser (IndexedDB/OPFS) and Online Vault Storage | 2026-10-13 | Accepted | Evolution from single-bucket R2 to local-first / dual-mode vault |
| [ADR-0004](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0004-data-contracts-canonical-schemas-and-stable-ids.md) | Canonical Schemas and Stable Deterministic Entity IDs | 2026-10-07 | Accepted | Inherited from `kreier/maintenance` |
| [ADR-0005](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0005-browser-crypto-and-serverless-worker-auth.md) | Client-Side Cryptographic Derivation with Serverless Session Management | 2026-10-07 | Accepted | Inherited from `kreier/maintenance` Phase 4 |

## How to Propose or Amend an ADR

1. Create a new markdown file named `docs/adr/NNNN-kebab-case-title.md` (sequentially numbered).
2. Follow the MADR (Markdown Architectural Decision Records) structure:
   - **Title**: `ADR-NNNN: Short title`
   - **Status**: Proposed / Accepted / Deprecated / Superseded by [ADR-XXXX]
   - **Date**: YYYY-MM-DD
   - **Decision-Makers / Participants**: User, AI Assistant (include reference/session timestamp)
   - **Context & Problem Statement**: What problem are we solving?
   - **Decision Drivers**: What matters most (simplicity, offline usability, multi-user, costs)?
   - **Considered Options**: Alternative 1, Alternative 2, Alternative 3.
   - **Decision Outcome**: Selected option with rationale.
   - **Pros and Cons of the Options**: Tradeoffs and consequences.
   - **Traceability & Timeline**: References to git commits, previous ADRs, or AI discussion sessions.
3. Update this `README.md` index table.
