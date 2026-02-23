# Aromatheca

Orchestration repository for the Aromatheca platform. Contains Docker Compose configuration for local development and production deployment.

## Repository Structure

```
aromatheca/
  core/                  # Submodule → aromatheca-api-core (Spring Boot)
  web/                   # Submodule → aromatheca-web (Next.js)
  docker-compose.yml     # Local development stack
  docker-compose.prod.yml  # Production stack (coming soon)
  .env.example           # Environment variable template
```

## Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Git](https://git-scm.com/)

## Local Setup

### 1. Clone with submodules

```bash
git clone --recurse-submodules git@github.com:yourorg/aromatheca.git
cd aromatheca
```

If you already cloned without submodules:

```bash
git submodule update --init --recursive
```

### 2. Configure environment

```bash
cp .env.example .env
```

Edit `.env` and set a secure `POSTGRES_PASSWORD`.

### 3. Start the stack

**Full stack** (requires app code in submodules):

```bash
docker compose up
```

**Database only** (for early development):

```bash
docker compose up postgres
```

### 4. Verify services

| Service  | URL                          |
|----------|------------------------------|
| Frontend | http://localhost:3000        |
| Backend  | http://localhost:8080        |
| Database | localhost:5432               |

### 5. Stop the stack

```bash
docker compose down
```

To also remove the database volume:

```bash
docker compose down -v
```

## Updating Submodules

Pull latest from all submodules:

```bash
git submodule update --remote --merge
```

Update only the backend:

```bash
git submodule update --remote --merge core
```

After updating, commit the new pointer:

```bash
git add core
git commit -m "chore: update core to latest"
```

## Environment Variables

See `.env.example` for all available variables and their defaults.

| Variable            | Description                  | Default         |
|---------------------|------------------------------|-----------------|
| `POSTGRES_DB`       | Database name                | aromatheca      |
| `POSTGRES_USER`     | Database user                | aromatheca_user |
| `POSTGRES_PASSWORD` | Database password            | **change this** |
| `POSTGRES_PORT`     | Database port                | 5432            |
| `CORE_PORT`         | Backend port                 | 8080            |
| `WEB_PORT`          | Frontend port                | 3000            |