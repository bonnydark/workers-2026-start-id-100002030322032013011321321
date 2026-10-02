# NO-NAME

Dark-first company website + operating dashboard.

## Structure
- `index.html` — public landing page
- `dashboard.html` — operating dashboard
- `worker.js` — REST API for Cloudflare Workers
- `schema.sql` / `migrations/` — D1 schema
- `api-client.js` — browser API client

## API
GET /api/health
GET /api/dashboard
CRUD /api/projects
CRUD /api/clients
CRUD /api/services
CRUD /api/tasks
CRUD /api/transactions

## Deploy API
1. Create a Cloudflare D1 database named `no-name`.
2. Put its ID into `wrangler.toml`.
3. Run `npx wrangler d1 migrations apply no-name --remote`.
4. Run `npx wrangler deploy`.
5. Set `localStorage.no_name_api` to the deployed Worker URL, or set `window.NO_NAME_API` before loading the dashboard.

The repository contains the complete application code, but database creation, domain/DNS, secrets and deployment credentials must be supplied by the owner.