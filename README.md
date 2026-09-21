# Aromatheca

[![Publish Docs](https://github.com/HigumaSoft/aromatheca/actions/workflows/publish-docs.yml/badge.svg)](https://github.com/HigumaSoft/aromatheca/actions/workflows/publish-docs.yml)

Open-source fragrance data platform: a structured database of fragrances, brands, notes, perfumers and community ratings, with a public read-only API.

**Links**

- [Project site](https://higumasoft.github.io/aromatheca/)
- [Public API reference](https://higumasoft.github.io/aromatheca/api/)

> **September 2026 — relaunch in progress.** The project is being rebuilt documentation-first. The application code repos ([api-core](https://github.com/HigumaSoft/aromatheca-api-core), [web](https://github.com/HigumaSoft/aromatheca-web)) are archived history and no longer submodules; their future is decided as part of the relaunch.

## Repository structure

```
aromatheca/
  docs/                 # Submodule -> aromatheca-docs (private): specs, ADRs, plans, decisions, AI rules
  pages/                # GitHub Pages: project site + API reference (Scalar)
  docker-compose.yml    # Local data services only: PostgreSQL + MinIO
  .env.example          # Environment template for the data services
  AGENTS.md, CLAUDE.md  # Pointers to the rules in docs/ for AI agents
  .github/workflows/    # publish-docs.yml — deploys pages/ + public API spec
```

## Local data services

```bash
git clone --recurse-submodules git@github.com:HigumaSoft/aromatheca.git   # docs/ needs private-repo access
cd aromatheca
cp .env.example .env            # set POSTGRES_PASSWORD
docker compose up -d            # postgres :5432, minio :9000 (console :9001)
docker compose down             # add -v to drop volumes
```

| Variable | Default | Purpose |
| --- | --- | --- |
| `POSTGRES_DB` / `POSTGRES_USER` / `POSTGRES_PASSWORD` / `POSTGRES_PORT` | `aromatheca` / `aromatheca_user` / — / `5432` | PostgreSQL |
| `MINIO_ROOT_USER` / `MINIO_ROOT_PASSWORD` / `MINIO_PORT` / `MINIO_CONSOLE_PORT` | `minioadmin` / `minioadmin` / `9000` / `9001` | MinIO (S3-compatible local store, bucket `aromatheca-assets`) |

## GitHub Pages

`pages/` is deployed by [`publish-docs.yml`](.github/workflows/publish-docs.yml) on every push to `master` that touches `pages/` or `docs/api/`. The workflow checks out the private `docs` submodule with a deploy key and copies `docs/api/public-api.yaml` next to `pages/api/index.html`.

## Updating the docs pointer

```bash
git submodule update --remote --merge docs
git add docs
git commit -m "chore: bump docs to $(git -C docs rev-parse --short HEAD)"
```

## How we work

Every document and feature goes through a plan → critique → revise → tickets → execute → review → merge+document loop run by AI agents with the owner approving at each gate. The rules, roles, templates and prompts are in the private docs repo (`docs/AGENTS.md`, `docs/ai/pipeline.md`). Drafts live in a local, gitignored `.codex/` workspace and never reach GitHub.
