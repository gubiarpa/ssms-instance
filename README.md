# SSMS Instance with Docker Compose

This repository provides a ready-to-run Microsoft SQL Server instance using Docker Compose.

## Requirements

- Docker Desktop or Docker Engine with Docker Compose v2.
- A container-capable OS (Windows, macOS, or Linux).

## Environment Variables

Before starting, create a `.env` file at the project root with the variable required by `docker-compose.yaml`:

```
MSSQL_SA_PASSWORD=YourStrong!Passw0rd
```

Notes:
- SQL Server requires a strong password (≥ 8 characters, mix of uppercase, lowercase, numbers, and symbols).
- Do not commit your `.env` to the repository. It is already ignored via `.gitignore`.

## Usage

1) (Optional) Create the `shared/` directory to store local backups mounted into the container:

```
mkdir shared
```

2) Start the services defined in `docker-compose.yaml`:

```
docker compose up -d
```

3) Check service status and health:

```
docker compose ps
docker compose logs -f mssql
```

When the health status becomes "healthy", the server is ready.

## Connect from SSMS or other clients

- Host: `localhost`
- Port: `1433`
- User: `sa`
- Password: the `MSSQL_SA_PASSWORD` value from your `.env`

## Persistence and Backups

- Data persists in the `mssql-data` volume declared in `docker-compose.yaml`.
- Local folder `./shared` is mounted to `/var/opt/mssql/backups` inside the container. Use it for backup/restore files.

## Stop and Clean Up

- Stop containers (preserves data):

```
docker compose down
```

- Also remove volumes (destroys data):

```
docker compose down -v
```

## Reference

- Compose file: `docker-compose.yaml`
  - Image: `mcr.microsoft.com/mssql/server:2022-latest`
  - Service: `mssql`
  - Exposed port: `1433`
  - Environment: `ACCEPT_EULA`, `MSSQL_SA_PASSWORD`, `MSSQL_PID`
  - Healthcheck: runs `SELECT 1` using `sqlcmd`

## Troubleshooting

- View logs: `docker compose logs -f mssql`
- Verify environment: ensure `MSSQL_SA_PASSWORD` exists in `.env` and meets complexity requirements.
- If you changed the password and it doesn’t apply, remove the previous container: `docker compose down` and start again.

