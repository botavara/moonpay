# MoonPay DevOps challenge

Take-home exercise for a MoonPay interview (December 2025). The starting point was MoonPay's
[Next.js + Prisma template](https://github.com/moonpay/devops-challenge); the task was to give it a CI pipeline.

Everything I added lives in [.github/workflows/pipeline.yaml](.github/workflows/pipeline.yaml):

- **Feature branches** (`feature/**`): install, lint, Prisma client generation and a production build on every push.
- **Pull requests to `main`**: the same, plus a throwaway Postgres 17 container, `prisma migrate deploy` when
  migrations exist (falling back to `db push`), and a smoke test that starts the built app and curls `/`.
- **Slack notification** with the outcome of every run, through a webhook kept as a repository secret.
- Database credentials come from repository secrets; nothing is hard-coded in the workflow.

## Run locally

```bash
cp .env.example .env
docker compose up -d          # Postgres 17
pnpm install && pnpm db:seed
pnpm dev                      # http://localhost:3000
```
