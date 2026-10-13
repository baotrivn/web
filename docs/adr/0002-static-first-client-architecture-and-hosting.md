# ADR-0002: Static-First Client Architecture and GitHub Pages Hosting

- **Status**: Accepted
- **Date**: 2026-10-07 (Originated in `kreier/maintenance`), 2026-10-13 (Adopted in `baotrivn/web`)
- **Deciders**: Matthias Kreier, baotrivn, AI Assistant
- **Traceability**: Commit `34ba243` & `be5423c` in `kreier/maintenance`, Commits `208cb46` & `622e537` in `baotrivn/web`

## Context and Problem Statement

The maintenance tracking tool needs to be globally accessible, cost-effective (ideally zero fixed hosting cost), simple to deploy, and resilient.
The target deployment for this application is:
https://baotrivn.github.io/web/

Early prototyping began in repository `https://github.com/kreier/maintenance` on October 7, 2026, and established that the client application should be statically hosted without requiring a dedicated traditional web application server (e.g., node/express, python/django).

## Decision Drivers

- Zero or near-zero operating costs for the base application.
- Anyone can fork the GitHub repository and have a working website deployed within minutes via GitHub Actions.
- Client browser should be able to run offline or standalone without server dependencies for basic usage.
- Standard continuous deployment pipeline using GitHub Pages.

## Considered Options

1. **Static Web Application on GitHub Pages (`public/` artifact deployment)**
2. **Full-stack Node.js / SSR application (e.g. Next.js on Vercel/Node server)**
3. **Containerized service (Docker / VPS)**

## Decision Outcome

Chosen option: **Option 1: Static Web Application on GitHub Pages**.
- All user interface rendering, dashboard computations, and business logic run client-side in the browser.
- Assets are deployed from the repository (`public/` directory or Vite build output) via GitHub Actions using `actions/deploy-pages`.
- Avoids server maintenance overhead, vulnerabilities, and recurring server costs.

## Consequences

### Positive
- High availability, zero server maintenance, fast CDN edge delivery worldwide.
- Free hosting via GitHub Pages.
- Clean separation between presentation/client logic and storage backend.

### Negative
- All dynamic data persistence and synchronization must be handled either by client-side browser storage or an external API / storage vault.
