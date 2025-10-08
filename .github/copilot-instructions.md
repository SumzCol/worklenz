# .github/copilot-instructions.md

## Worklenz AI Coding Agent Guidelines

### Project Overview

Worklenz is a modular project management platform with a TypeScript/Express backend and a React/TypeScript frontend. It uses PostgreSQL for data storage and supports Docker-based and manual local development.

### Architecture

- **Backend (`worklenz-backend/`)**: Express.js app in TypeScript. Organized by feature: `controllers/`, `models/`, `services/`, `routes/`, `middlewares/`, etc.
  - Database migrations managed via `node-pg-migrate` in `database/pg-migrations/`.
  - SQL initialization scripts in `database/sql/` must be run in a specific order (see `database/README.md`).
  - Validators are implemented as Express middlewares (`src/middlewares/validators/`).
- **Frontend (`worklenz-frontend/`)**: React app using Vite, Ant Design, and TypeScript.
  - Entry point: `src/`
  - Static assets: `public/`
- **Email Templates**: Located in `worklenz-backend/worklenz-email-templates/`.

### Developer Workflows

- **Backend**
  - Install dependencies: `npm install`
  - Start dev server: `npm run dev`
  - Build: `npm run build`
  - Database setup: Use `database/00-init-db.sh` or run SQL files in order.
  - Migrations: Use `npm run migrate:create`, `npm run migrate:up`, `npm run migrate:down` (see `pg-migrations/README.md`).
- **Frontend**
  - Install dependencies: `npm install`
  - Start dev server: `npm run dev`
  - Build: `npm run build`
  - Preview production build: `npm run preview`

### Conventions & Patterns

- **TypeScript**: Strict typing enforced throughout backend and frontend.
- **Express Middlewares**: Validators are always implemented as middlewares for request body validation.
- **Database Migrations**: Always use `IF EXISTS/IF NOT EXISTS` for idempotency. Each migration must have both `up` and `down` functions.
- **Environment Variables**: Use `.env` files, based on `.env.template`.
- **Docker**: Recommended for local development; see `docker-compose.yml` and shell scripts.
- **File Naming**: Migration files use timestamp prefixes for ordering.

### Integration Points

- **Authentication**: Supports Google OAuth (see `.env` and backend config).
- **Storage**: S3 or Azure Blob, configured via environment variables.
- **Rich Text Editor**: Uses TinyMCE (see `src/public/tinymce/`).

### Key Files & Directories

- `worklenz-backend/src/` — Main backend source code
- `worklenz-backend/database/` — DB setup and migrations
- `worklenz-frontend/src/` — Main frontend source code
- `worklenz-backend/worklenz-email-templates/` — Email HTML templates

### Examples

- To add a new API route: create a controller in `controllers/`, define the route in `routes/`, and add any needed validators in `middlewares/validators/`.
- To create a migration: `npm run migrate:create -- migration-name`, then implement `exports.up` and `exports.down`.
