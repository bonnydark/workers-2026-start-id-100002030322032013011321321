# NO-NAME

Dark-first company operating system: public site + dashboard + Cloudflare Worker API + D1.

## What is included

- Public company landing page
- Dashboard with Overview, Projects, Clients, Services, Tasks and Finance
- CRUD operations through the API
- Account sign-up / login / logout
- `/api/auth/me` login-state check
- Password hashing with PBKDF2 + per-password salt
- HttpOnly session cookie with 30-day expiry
- Account profile + password change
- Theme presets + custom accent color
- Email verification state in the database, ready for a mail provider later
- Per-account data isolation with `owner_id`
- Animated view/modal transitions
- Responsive mobile UI

## Files

- `index.html` — public landing page
- `dashboard-v2.html` — current control center
- `dashboard.html` — compatibility redirect to v2
- `worker.js` — Cloudflare Workers REST API
- `schema.sql` — complete database schema
- `migrations/0001_init.sql` — company data tables
- `migrations/0002_auth.sql` — users + sessions
- `migrations/0003_owner_scope.sql` — per-account ownership
- `wrangler.toml` — Worker/D1 configuration

## API

Public:
- GET `/api/health`
- POST `/api/auth/signup`
- POST `/api/auth/login`

Authenticated:
- GET `/api/auth/me`
- POST `/api/auth/logout`
- PUT `/api/auth/profile`
- POST `/api/auth/change-password`
- GET `/api/dashboard`
- CRUD `/api/projects`
- CRUD `/api/clients`
- CRUD `/api/services`
- CRUD `/api/tasks`
- CRUD `/api/transactions`

## Cloudflare deployment

1. Create a D1 database named `no-name`.
2. Put its database ID into `wrangler.toml`.
3. Configure the exact frontend origin as Worker environment variable `APP_ORIGIN`.
4. Apply migrations:
   `npx wrangler d1 migrations apply no-name --remote`
5. Deploy:
   `npx wrangler deploy`
6. Open the dashboard and paste the Worker URL on the first-setup screen.

### Security notes

Passwords are never stored directly. The Worker derives a PBKDF2-SHA-256 hash using 120,000 iterations and a random per-password salt. Sessions store only a SHA-256 hash of a random session token. Company records are scoped to the authenticated user's `owner_id`.

For production, keep `APP_ORIGIN` restricted to the real frontend origin and deploy the Worker over HTTPS. Email verification is intentionally not sending mail yet; the `verified` flag is already part of the account model so a mail provider can be added later.

## Current boundary

The repository contains the application code and deployment configuration. Actual Cloudflare account/database creation, DNS, environment values and deployment credentials still have to be supplied by the owner.
