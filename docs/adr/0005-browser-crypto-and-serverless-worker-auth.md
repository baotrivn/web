# ADR-0005: Client-Side Cryptographic Derivation and Serverless Session Management

- **Status**: Accepted
- **Date**: 2026-10-07 (Established in `kreier/maintenance` Phase 4), 2026-10-13 (Adopted in `baotrivn/web`)
- **Deciders**: Matthias Kreier, AI Assistant
- **Traceability**: Commit `2f48538` in `kreier/maintenance`

## Context and Problem Statement

When users connect to an online vault (such as a shared Cloudflare Worker + R2 deployment), they must authenticate securely to establish their permissions (Administrator, Editor, Reviewer, Viewer).
However, serverless execution platforms (like Cloudflare Worker free tier) impose tight CPU execution limits (typically ~10ms CPU per request).
Running heavy password hashing algorithms (e.g. standard multi-iteration Argon2 or PBKDF2 with 100,000+ rounds) on the serverless edge worker risks hitting CPU limits and failing requests.

## Decision Drivers

- Secure authentication and password verification without compromising security.
- Stay well within serverless edge CPU limits (~10ms).
- Zero plaintext passwords in transit.
- Secure, cookie-based session tokens with role-based access control.

## Considered Options

1. **Client-Side Key Derivation (Web Crypto PBKDF2) + Server HMAC Verification**
2. **Traditional Server-Side PBKDF2/Bcrypt on Edge Worker** (High risk of CPU timeout on free tier)
3. **Third-Party Auth Provider (e.g. Auth0, Clerk, Firebase Auth)** (Adds external vendor dependency, monthly quotas, and complex setup for forks)

## Decision Outcome

Chosen option: **Client-Side Key Derivation + Server HMAC Verification**:
1. When logging in, the browser computes a derived cryptographic key using standard Web Crypto API (`crypto.subtle.deriveBits` using PBKDF2 with salt).
2. The browser sends the derived key over HTTPS.
3. The serverless worker performs a constant-time HMAC check against the stored hash in minimal CPU time (< 1ms).
4. On verification, the server issues a secure, signed `HttpOnly`, `SameSite=Strict` session cookie.

For single-user **Local Browser Mode**, no server or login is needed; data remains stored unencrypted or optionally encrypted in local IndexedDB.

## Consequences

### Positive
- Negligible CPU consumption on serverless functions.
- Avoids external auth vendor vendor-lock-in.
- Uses native standard Web Crypto API available in all modern browsers without extra libraries.

### Negative
- Client devices must support Web Crypto API (standard in all modern browsers).
