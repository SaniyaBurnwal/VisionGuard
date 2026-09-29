# VisionGuard Traffic Monitor

VisionGuard is a live traffic-operations dashboard for monitoring camera analysis, tracked objects, traffic flow, and safety alerts.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/visionguard-dashboard/src/pages/monitoring-dashboard.tsx` — primary monitoring surface
- `artifacts/visionguard-dashboard/src/index.css` — dashboard theme and responsive layout
- `artifacts/api-server/src/routes/monitoring.ts` — monitoring overview and analysis controls
- `lib/api-spec/openapi.yaml` — source of truth for the monitoring API

## Architecture decisions

- The first operational release uses an in-memory monitoring session because camera analysis is an active runtime stream rather than durable user-owned records.
- The frontend uses generated API hooks from the shared OpenAPI contract and polls the overview while analysis is active.
- The original standalone VisionGuard dashboard remains the visual reference for the control-room identity; the production UI is implemented as React components.

## Product

- Shows camera/session readiness, analysis progress, detection metrics, traffic flow, tracked objects, and recent security alerts.
- Starts and stops an analysis session from the operator dashboard.
- Filters tracked objects by confidence and handles ready, analyzing, completed, stopped, loading, empty, and error states.

## User preferences

- The user asked to run the attached VisionGuard code as a durable web app.

## Gotchas

- Frontend builds require workflow-provided `PORT` and `BASE_PATH`; for a shell build use `PORT=4173 BASE_PATH=/`.
- API routes are mounted under `/api` and frontend requests should use the generated client hooks.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
