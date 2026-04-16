# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

All commands run from the `web/` directory:

```bash
npm run dev      # Development server (Next.js + Turbopack) → http://localhost:3000
npm run build    # Production build
npm run lint     # ESLint check
```

No test suite is configured. Before committing, run `lint` and `build` to catch errors.

## Architecture Overview

**Gestor360** is a multitenant HR and attendance management system for Grupo CT. Stack: Next.js 16 (App Router), React 19, TypeScript 5, Tailwind CSS v4, Supabase (PostgreSQL + Auth + Realtime + Storage).

### Directory Layout

```
web/
  app/                    # Next.js App Router routes + Server Actions
    actions/              # Server Actions (one file per domain)
    (protected)/          # Auth-gated routes
    api/v1/               # REST API routes
  src/
    components/           # React components (UI + domain-specific)
    lib/                  # Utilities: supabase clients, shift-resolver, date-utils
    hooks/                # Custom hooks (useAttendanceRealtime, useDirtyState)
    types/                # TypeScript types per domain (monitor, security, kiosk)
  supabase/
    functions/            # Edge Functions
    permissions-manifest.json  # SSOT for role→permission matrix
```

### Multitenant Model

Every data-access query must filter by `company_id`. Users belong to one or more companies via `company_memberships` (roles: `owner`, `admin`, `rrhh`, `supervisor`, `viewer`). Supabase RLS enforces company isolation using `is_member_of()` and `is_company_admin()` SECURITY DEFINER helpers to avoid recursive policy lookups.

### Server Actions Pattern

- Live in `app/actions/` — one file per domain.
- Return type: `ActionState = { error: string } | null`.
- Use `createClient()` from `@/lib/supabase/server` (never the browser client).
- Call `revalidatePath()` after mutations.
- **Do not call `redirect()` inside a try/catch** — it throws internally and will be caught as an error. Return the path and let the client call `router.push()`.

### Server Components First

Pages are Server Components by default. Add `'use client'` only when state, hooks, or browser APIs are needed. Fetch data server-side at request time; avoid client-side data fetching unless required for realtime.

### Shift Inheritance (4 Levels)

Employee schedules cascade through: **Override → Manual → Global → Branch**. Always resolve via `resolveShift()` / `resolveShiftInMemory()` in `src/lib/shift-resolver.ts`. Never inline schedule logic.

### Security / Permissions

- `AdminShell` component receives a `permissions` object and filters what reaches the client (binary: has it or not, no metadata).
- Never expose `profile_id` or internal user metadata to client-rendered components.
- `supabase/permissions-manifest.json` is the SSOT for role permissions.

### Realtime (Monitor module)

The Monitor shows live employee status. Realtime subscriptions live in `useAttendanceRealtime()`. When merging realtime updates into state, preserve JOIN fields (don't overwrite with partial objects). Always clean up subscriptions on unmount.

### Kiosk

The public root route `/` requires no auth. PIN lookup is a single query with JOIN via `verifyKioskPin()`. Attendance is marked via the `rpc_mark_attendance_action` RPC — never write directly to `attendance_logs` from the client.

### Styling

Global CSS variables for theming in `app/globals.css`. Tailwind v4 for layout/responsive. Use the `.app-surface` utility for dark-mode containers. The system uses a Premium Dark Theme with glassmorphism (do not add light-mode assumptions).

### Key RPC Functions

| RPC | Purpose |
|-----|---------|
| `rpc_mark_attendance_action` | Official kiosk clock-in/out with validation |
| `rpc_monitor_mark_attendance` | Supervisor manual marking from Monitor |
| `get_weekly_attendance_counts` | Server-side aggregation for dashboard |
| `get_monthly_top_delays` | Top delays for dashboard |
| `create_company_with_owner` | Multitenant setup |

### PIN Generation Rules

4-digit numeric (1000–9999), unique per company. Rejects sequential patterns (1234) and all-same-digit patterns (1111). Auto-generated on employee creation.

## Additional Documentation

Detailed references in `web/docs/`:
- `DATABASE.md` — Full schema, columns, RLS policies
- `BUSINESS_RULES.md` — Attendance and shift business logic
- `SECURITY_MAP.md` — RLS policies and role authorization model
- `SSOT_LOGIC.md` — Security governance approach
- `REPOSITORY_MAP.md` — File structure overview
