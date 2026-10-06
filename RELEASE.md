# Customer Release 1.0.2

| | |
|---|---|
| Product | Hajj Management System |
| Release version | `v1.0.2` |
| Snapshot generated | 2026-10-06 |
| Allowlist manifest | `customer-release.manifest.json` v1 (Master) |

This repository is a **clean release snapshot**, not a fork or mirror of the
vendor's Master repository. It carries no Master development history.

## What this snapshot contains

- The frontend application source and build configuration.
- The canonical Supabase migrations required to build a fresh customer database.
- The Supabase Edge Functions and `supabase/config.toml`.
- `package-lock.json` and the pinned Supabase CLI version.
- The first-administrator bootstrap script.
- The operational documentation required for this release.

## What it deliberately does not contain

Secrets, real environment values, customer data, Master git history,
internal development/verification/recovery evidence, load-test artifacts,
and vendor-internal GitHub workflows.

## Deployment

Before the first deployment, the repository needs two GitHub secrets:
`SUPABASE_PROJECT_REF` and `SUPABASE_ACCESS_TOKEN` — a Supabase access
token scoped to this project with **Database: read & write**, **Edge Functions:
read & write** and **Secrets: write**. No other secret or password is stored.

1. **Supabase** — Authentication → Users → **Add user**. Enter the
   administrator's Login ID (email form, e.g. `admin@company.local`) and
   password, and tick **Auto Confirm User**.
2. **GitHub** — Actions → **Customer Backend Setup** → Run workflow. Enter the
   Vercel production URL, Vercel project name and owner, a contact
   (`mailto:…`), and the same administrator Login ID with a display name.
   The workflow applies the database, creates the First Administrator with
   every system permission, configures the Edge Function secrets, deploys the
   six Edge Functions and verifies the result. If a run fails, fix the reported
   cause and run it again — it continues safely.
3. **Vercel** — set `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY` and the
   `VITE_VAPID_PUBLIC_KEY` shown in the workflow's summary, then deploy.

Full ownership, release, recovery and handover rules:
`docs/CUSTOMER_TECHNICAL_HANDOVER_MANUAL.md`.
