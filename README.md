# Maintenance Web (`baotrivn/web`)

An open-source, multilingual maintenance documentation and tracking helper for objects, houses, and appliances.

- **Live Web Application**: [https://baotrivn.github.io/web/](https://baotrivn.github.io/web/)
- **Upstream / Preceding Work**: [kreier/maintenance](https://github.com/kreier/maintenance)

---

## Features & Modes

- **Local Browser Mode (Single-User)**:
  - Runs directly in the browser with no account creation or server configuration needed.
  - Data persisted locally using browser IndexedDB.
  - Portable JSON / Zip import & export.
- **Online Vault Mode (Multi-User & Cloud Sync)**:
  - Connects to an online vault (e.g. Cloudflare Worker + R2 object storage).
  - Multi-user authentication using client-side Web Crypto and secure edge sessions.
  - Role-based permissions (`viewer`, `editor`, `reviewer`, `administrator`).
  - Document attachments (manuals, blueprints, photos).
- **Physical Asset Tracking**:
  - Stable, deterministic IDs (`HVAC-ROOM2-001`, `ELEC-MAIN-001`) ideal for QR codes.
  - Exact age, maintenance intervals, and overdue alerts computed dynamically from raw dates.
- **Multilingual Support**: English (`en`), Vietnamese (`vi`), and Korean (`ko`).

---

## Architectural Decisions & Evolution

To maintain continuity across contributors, forks, and AI collaboration sessions, architectural decisions are systematically documented in **Architecture Decision Records (ADRs)**:

- 📖 **Architecture Overview**: [ARCHITECTURE.md](file:///home/mk/AI-Agents/antigravity/web/ARCHITECTURE.md)
- 🏛️ **Architecture Decision Records**: [docs/adr/](file:///home/mk/AI-Agents/antigravity/web/docs/adr/README.md)
  - [ADR-0001: Record Architecture Decisions](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0001-record-architecture-decisions.md)
  - [ADR-0002: Static-First Client Architecture on GitHub Pages](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0002-static-first-client-architecture-and-hosting.md)
  - [ADR-0003: Storage Strategy — Local Browser Storage and Online Vaults](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0003-storage-strategy-local-browser-and-cloud-vault.md)
  - [ADR-0004: Canonical Schemas and Stable Deterministic Entity IDs](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0004-data-contracts-canonical-schemas-and-stable-ids.md)
  - [ADR-0005: Client-Side Cryptographic Derivation and Serverless Auth](file:///home/mk/AI-Agents/antigravity/web/docs/adr/0005-browser-crypto-and-serverless-worker-auth.md)

---

## Deployment

This website is automatically published to GitHub Pages from the repository via GitHub Actions (`.github/workflows/static.yml`).
