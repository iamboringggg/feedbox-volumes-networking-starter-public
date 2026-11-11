# FeedBox volumes and networking starter

FeedBox is a minimal feedback collector with PostgreSQL (`db`), Express (`api`), and an Nginx-served frontend (`web`).

## Project structure

```text
├── server.js
├── Dockerfile
├── package.json
├── package-lock.json
├── docker-compose.yml
├── .env.example
└── client/
    ├── Dockerfile
    └── index.html
```

## Run the starter

```bash
cp .env.example .env
docker compose up --build
```

- API: http://localhost:5000/api/messages
- Frontend: http://localhost:3000

## Production compose

The main `docker-compose.yml` keeps the app in a clean, production-oriented topology:

- `db` uses a named Docker volume to persist PostgreSQL data.
- `db` is only connected to the private `backend` network.
- `api` is connected to both the `backend` and `app` networks so it can reach the database and serve the browser.
- `web` is connected only to the `app` network.
- `db` is not published to the host, so external clients cannot connect directly to PostgreSQL.

## Development hot-reload

Use the development override file when working locally:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build
```

This mounts the host working tree into the API container and starts the app with `nodemon`, while keeping the container's `node_modules` protected from host binaries.

Do not commit `.env` or `node_modules`.
