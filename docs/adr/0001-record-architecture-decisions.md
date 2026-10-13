# ADR-0001: Record Architecture Decisions

- **Status**: Accepted
- **Date**: 2026-10-13
- **Deciders**: User, AI Assistant (Antigravity)
- **Consulted**: Community standards (Michael Nygard ADR format / MADR)

## Context and Problem Statement

The architecture, storage layer, and features of the maintenance web application are continually evolving through iterative collaboration between the human developer and AI agents.
Without a structured, persistent, and chronological record of architectural design decisions, future developers (or anyone forking the repository) cannot understand *why* certain trade-offs were chosen (for example, why a particular database or storage engine was selected over another on a specific date).

## Decision Drivers

- Need a lightweight, plain-markdown documentation format that lives inside the git repository.
- Decisions must be traceable back in time, referencing specific conversation dates and git commits.
- Independent forks should be able to amend, supersede, or branch decisions clearly.
- Documents must be easy for both humans and AI assistants to read and update.

## Considered Options

1. **Architecture Decision Records (ADRs) in `docs/adr/`**
2. **Monolithic `ARCHITECTURE.md` file updated in place**
3. **GitHub Issues / Discussions / Wiki**

## Decision Outcome

Chosen option: **Option 1: ADRs in `docs/adr/`**, because:
- ADRs are immutable, numbered, and dated chronological records. When a previous decision changes (e.g. changing database from SQLite to IndexedDB or vice-versa), we do not rewrite history; we create a new ADR that explicitly supersedes the older ADR, citing the date, reasoning, and context.
- Storing them in `docs/adr/` keeps architecture decisions versioned alongside the code.
- A central `docs/adr/README.md` index provides quick discovery.
- `ARCHITECTURE.md` at the repository root will serve as the synthesized "current state" high-level architecture, pointing to individual ADRs for historical rationale.

## Consequences

### Positive
- Clear audit trail of design evolution.
- Future AI conversations have explicit historical context to reference.
- Explicit timeframe and provenance recorded for all decisions.

### Negative
- Requires maintaining discipline to write or update an ADR whenever a key architectural pivot occurs.
