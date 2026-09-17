# Laravel + Next.js + MySQL Stack

This stack runs:

- Laravel backend on port 8001
- Next.js frontend on port 3000
- MySQL on port 3307
- phpMyAdmin on port 8080

## Start the stack

1. Copy `.env.example` to `.env`
2. Run:

```bash
docker compose up -d --build
```

## Access

- Laravel: http://localhost:8001
- Next.js: http://localhost:3000
- phpMyAdmin: http://localhost:8080

## Database config for Laravel

In your Laravel `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=mysql-db
DB_PORT=3306
DB_DATABASE=app_db
DB_USERNAME=appuser
DB_PASSWORD=apppass
```

## Notes

- The `laravel` and `nextjs` folders are mounted into the containers.
- Put your actual Laravel project in `./laravel` and your Next.js project in `./nextjs`.
- If the app folder is empty, Docker still builds the image but the project files must be there for the app to run.
