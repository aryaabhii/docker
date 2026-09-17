# MySQL Docker Stack

This folder is for the shared MySQL + phpMyAdmin setup that can be reused by Laravel, Node, or Nginx projects.

## Start the stack

1. Copy `.env.example` to `.env`
2. Update the values if needed
3. Run:

```bash
docker compose up -d
```

## Access

- MySQL: `localhost:3307`
- phpMyAdmin: `http://localhost:8080`

## Create database for a project

Open phpMyAdmin and create databases like:

- `laravel_project_1`
- `laravel_project_2`
- `laravel_project_3`

Then in each app's `.env` file, point to the same MySQL host and a different database name.

Example:

```env
DB_HOST=mysql-db
DB_PORT=3306
DB_DATABASE=laravel_project_1
DB_USERNAME=appuser
DB_PASSWORD=apppass
```

This keeps one MySQL server and allows multiple projects to share it with different databases.
