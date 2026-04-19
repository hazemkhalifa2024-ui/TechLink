# TechLink Yemen — Production Build

A self-contained, Railway-ready bundle of the TechLink Yemen platform.
The Express API and the React frontend are served from a single Node process.

## Stack

- **Runtime:** Node.js 20+
- **Server:** Express 5 (bundled with esbuild — all deps inlined)
- **Frontend:** React + Vite (pre-built static files in `./public`)
- **Database:** PostgreSQL (driver: `pg`, used via Drizzle ORM)
- **Auth:** JWT

## Required Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `PORT` | yes | HTTP port the server binds to |
| `DATABASE_URL` | yes | PostgreSQL connection URL |
| `JWT_SECRET` | yes | Secret used to sign JSON Web Tokens |
| `NODE_ENV` | no  | Defaults to `development`. Set `production` on Railway. |
| `PUBLIC_DIR` | no  | Defaults to `./public`. Path to the built frontend. |

Copy `.env.example` to `.env` and fill in the values.

## Local Run

```bash
npm install
npm start
# Server listens on http://localhost:$PORT (UI + /api/* on the same origin)
```

## Deploying to Railway

1. Push this folder to a Git repository.
2. Create a new Railway project from that repo.
3. Add a PostgreSQL plugin — Railway will inject `DATABASE_URL` automatically.
4. Add `JWT_SECRET` in the project Variables (use a long random string).
5. Railway sets `PORT` automatically.
6. Deploy. Railway runs `npm start` by default.

## Project Layout

```
.
├── server.js                    # Single Express entry (bundled, all deps inlined)
├── pino-file.mjs                # Pino logging worker
├── pino-pretty.mjs              # Pino pretty-printer worker
├── pino-worker.mjs              # Pino worker thread
├── thread-stream-worker.mjs     # Pino dependency
├── public/                      # Built React frontend (Vite output)
│   ├── index.html
│   ├── favicon.svg
│   └── assets/
├── package.json
├── .env.example
└── .gitignore
```

## Routes

- `GET /api/*` — REST API (auth, users, jobs, offers, contracts, payments, admin, etc.)
- `GET /*` — Serves the React SPA (index.html with client-side routing)

## Test Accounts (seed data — change in production)

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@techlink.ye | admin123 |
| Shop Owner | ahmad2@shop.ye | shop123 |
| Technician | ali@tech.ye | tech123 |
