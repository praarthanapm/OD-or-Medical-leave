# Attendance Dashboard

A browser-first student attendance companion that visualizes attendance health, models OD and medical leave scenarios, and answers personalized what-if questions.

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

- `artifacts/attendance-dashboard/src/App.tsx` — dashboard UI, timetable profiles from the uploaded SRM sheets, class editor, simulator math, and Attendance Advisor conversation logic.
- `artifacts/attendance-dashboard/src/index.css` — product theme, responsive shell, chart styling, print rules, and dark mode tokens.
- `artifacts/attendance-dashboard/src/main.tsx` — React entry point and error boundary.

## Architecture decisions

- Phase 2 is client-only and uses seeded in-memory attendance data so the simulator and advisor are immediately interactive without requiring account setup or a backend.
- OD days are modeled as credited attendance; medical leave is modeled as an excused absence that adds to held classes. Both projections are labeled as assumptions in the UI.
- The advisor uses the same projection math as the simulator and parses leave duration, subject, and leave type from natural-language questions.
- The class editor uses local timetable profiles transcribed from the uploaded SRM timetable sheets. Selecting a year/section replaces the active subject set, while individual class toggles allow a student to match their enrolled classes.
- Dashboard refresh controls are local UX state only; automatic refresh options never poll more frequently than every 5 minutes.

## Product

- Overall attendance health with a semester trend and 75% threshold.
- Subject-level attendance bars and health cards.
- Timetable-based class switching for I/II/III/IV year ECE, ECE-DS, and BME sections, with per-class selection.
- OD / medical leave simulator with instant projected overall and subject percentages.
- Floating Attendance Advisor with natural-language what-if answers.
- Dark mode, print/PDF action, CSV chart exports, responsive navigation, and auto-refresh controls.

## User preferences

No additional preferences recorded.

## Gotchas

The leave projection is planning guidance, not an institutional policy decision; the UI intentionally tells students to confirm how their department credits OD and medical leave.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
