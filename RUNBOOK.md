# Docker Stacks Runbook

This repo contains three independent Docker Compose stacks. Each lives in its own
folder with its own `docker-compose.yml` and `.env.example`. This doc explains what
each one does and the exact commands to bring it up.

| Stack | Folder | What it runs | Ports |
|---|---|---|---|
| [MySQL + phpMyAdmin](#1-mysql--phpmyadmin-mysql) | `mysql/` | Shared MySQL server + phpMyAdmin UI | 3307, 8080 |
| [Laravel + Next.js + MySQL](#2-laravel--nextjs--mysql-app-stack) | `app-stack/` | Laravel API, Next.js frontend, MySQL, phpMyAdmin | 8001, 3000, 3307, 8080 |
| [Nginx static site](#3-nginx-static-site-nginx) | `nginx/` | Nginx serving a static `html/` folder | 80 |

> Only run one MySQL-based stack at a time (`mysql/` or `app-stack/`) — they both
> default to host port `3307` / `8080` and will conflict if started together.

---

## Prerequisites

- Docker Desktop installed and running (Windows).
- PowerShell (or Git Bash) terminal.

---

## 1. MySQL + phpMyAdmin (`mysql/`)

Shared MySQL server meant to be reused by multiple projects (each project gets its
own database inside the same server).

```powershell
cd mysql
copy .env.example .env
docker compose up -d
```

**Access**
- MySQL: `localhost:3307`
- phpMyAdmin: http://localhost:8080

**Add a new project's database**
1. Open phpMyAdmin and create a database, e.g. `laravel_project_1`.
2. In that project's `.env`, point to this server:
   ```env
   DB_HOST=mysql-db
   DB_PORT=3306
   DB_DATABASE=laravel_project_1
   DB_USERNAME=appuser
   DB_PASSWORD=apppass
   ```

**Stop**
```powershell
docker compose down
```

---

## 2. Laravel + Next.js + MySQL (`app-stack/`)

Full stack: Laravel backend, Next.js frontend, its own MySQL + phpMyAdmin.

```powershell
cd app-stack
copy .env.example .env
docker compose up -d --build
```

**Access**
- Laravel: http://localhost:8001
- Next.js: http://localhost:3000
- phpMyAdmin: http://localhost:8080

**Before starting:** put your actual Laravel project files in `./laravel` and your
Next.js project files in `./nextjs` — the folders are bind-mounted into the
containers, so an empty folder means the app has nothing to run.

**Laravel `.env` DB settings** (inside the Laravel app, not the stack's `.env`):
```env
DB_CONNECTION=mysql
DB_HOST=mysql-db
DB_PORT=3306
DB_DATABASE=app_db
DB_USERNAME=appuser
DB_PASSWORD=apppass
```

**Stop**
```powershell
docker compose down
```

---

## 3. Nginx static site (`nginx/`)

Serves the contents of `nginx/html/` as a static site.

```powershell
cd nginx
copy .env.example .env
docker compose up -d
```

**Access**
- Site: http://localhost (port 80, override with `NGINX_PORT` in `.env`)

**Stop**
```powershell
docker compose down
```

---

## Common commands (any stack)

Run these from inside the stack's folder (`mysql/`, `app-stack/`, or `nginx/`):

| Task | Command |
|---|---|
| Start (background) | `docker compose up -d` |
| Start and rebuild images | `docker compose up -d --build` |
| Stop and remove containers | `docker compose down` |
| View logs | `docker compose logs -f` |
| View running containers | `docker compose ps` |
| Restart one service | `docker compose restart SERVICE_NAME` |

---

## Learning material

For general Docker/Compose/CI-CD concepts and command reference, see
[readme.md](readme.md) in this folder.
