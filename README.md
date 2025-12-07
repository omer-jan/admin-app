# Admin App

A modern web application built with **Laravel**, **Inertia.js**, and **Vue 3**. This project uses **Vite** for asset bundling and provides a reactive, single-page-app experience while keeping a Laravel backend.

---

## Quick summary

- Backend: Laravel 12 (PHP)
- Frontend: Vue 3 + Inertia.js
- Build tooling: Vite, Tailwind (dev + build via npm)
- DB (default/example): SQLite (file: `database/database.sqlite`)

## Requirements

Ensure these are installed and on your PATH before proceeding:

- PHP >= 8.2
- Composer (Composer 2.x recommended)
- Node.js (recommended >= 18)
- npm (comes with Node)

Check installed versions:

```powershell
php -v
composer -V
node -v
npm -v
```

## Key dependencies

From `composer.json` (backend):

- `laravel/framework` ^12.0
- `inertiajs/inertia-laravel` ^2.0
- `laravel/tinker`

From `package.json` (frontend):

- `vue` ^3
- `@inertiajs/inertia` and `@inertiajs/inertia-vue3`
- Dev: `vite`, `@vitejs/plugin-vue`, `tailwindcss`, `laravel-vite-plugin`, `axios`, `concurrently`

## Installation / Initial setup (PowerShell)

You can use the provided Composer setup script which runs PHP & JS installs, copies `.env`, generates the app key, runs migrations and builds frontend assets:

```powershell
composer run setup
```

If you prefer step-by-step manual setup, run these commands from the project root:

```powershell
# 1) Install PHP dependencies
composer install

# 2) Copy environment file
Copy-Item .env.example .env -Force

# 3) Generate application key
php artisan key:generate

# 4) Ensure SQLite database file exists (creates it if needed)
if (!(Test-Path database\database.sqlite)) { New-Item -ItemType File database\database.sqlite | Out-Null }

# 5) Run database migrations
php artisan migrate --force

# 6) Install Node dependencies
npm install

# 7) Build frontend assets for production (optional here)
npm run build
```

Notes:
- The composer `post-create-project-cmd` hook also creates `database/database.sqlite` and runs migrations when a project is created from a template.
- If you're using MySQL/Postgres, update `.env` DB_* values and skip the SQLite creation step.

## Available scripts

NPM scripts (see `package.json`):

- `npm run dev` — start Vite dev server (frontend HMR)
- `npm run build` — build production assets with Vite

Composer scripts (see `composer.json`):

- `composer run setup` — convenience script: installs PHP deps, copies .env, generates key, runs migrations, installs npm deps and builds assets
- `composer run dev` — runs a multi-process development environment (uses `npx concurrently` to run `php artisan serve`, a queue listener, `php artisan pail` logging, and `npm run dev` together)
- `composer run test` — clears config cache and runs the test suite (`php artisan test`)

Run `composer run dev` if you want the full multi-process dev environment. Alternatively run processes individually (see Development section).

## Development

Option A — Full multi-process dev environment (single command):

```powershell
composer run dev
```

This runs (via `concurrently`):
- `php artisan serve` (local PHP server)
- `php artisan queue:listen --tries=1` (queue worker)
- `php artisan pail --timeout=0` (pail logger)
- `npm run dev` (Vite dev server)

Option B — Run pieces individually:

```powershell
# PHP dev server
php artisan serve

# Vite dev server (frontend HMR)
npm run dev

# Optional: queue worker
php artisan queue:listen --tries=1

# Optional: pail logger
php artisan pail --timeout=0
```

## Building for production

```powershell
# Build frontend assets
npm run build

# (Optional) Cache config/routes for performance
php artisan config:cache
php artisan route:cache
```

## Tests

Run the test suite using Composer script or directly with Artisan:

```powershell
composer run test
# or
php artisan test
```

## Database (SQLite)

This repository includes a SQLite-based workflow by default. The database file is expected at `database/database.sqlite`. The composer `post-create-project-cmd` helps create the file when the project is first created.

To manually create the SQLite file on Windows PowerShell:

```powershell
if (!(Test-Path database\database.sqlite)) { New-Item -ItemType File database\database.sqlite | Out-Null }
```

If you prefer a different database (MySQL/Postgres), update your `.env` file accordingly and run migrations.

## Troubleshooting

- Node/Vite problems: ensure Node >= 18. If you see plugin errors, try removing `node_modules` and reinstalling:

```powershell
rm -r node_modules; npm install
```

(Use `Remove-Item -Recurse -Force node_modules` on PowerShell if `rm` isn't aliased.)

- Permission or file-not-found for `.env`: ensure `.env` exists and `APP_KEY` is set (use `php artisan key:generate`).

- Ports in use: `php artisan serve` uses port 8000 by default and Vite typically uses 5173. If ports conflict, stop the occupying process or run servers with explicit ports.

## Further notes

- This README documents a development-oriented setup using SQLite and the convenience composer scripts shipped in the project. Adapt DB and environment variables as needed for production.

- For additional information, consult the Laravel and Vite docs, or inspect `composer.json` and `package.json` in the project root.

---

## License

MIT
