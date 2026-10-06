# Customer Release 1.0.1

| | |
|---|---|
| Product | Hajj Management System |
| Release version | `v1.0.1` |
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

1. Set every value in `.env.example` for this customer — frontend values in
   the customer's Vercel project, Edge Function values in the customer's
   Supabase project secrets. The four per-customer values
   (`ALLOWED_ORIGINS`, `VERCEL_PROJECT_SLUG`, `VERCEL_OWNER_SLUG`,
   `VAPID_SUBJECT`) have no defaults: the functions fail closed without them.
2. `npm ci` then `npm run build`.
3. Apply the database: `npm run supabase -- db push` against the customer's
   own Supabase project, following `docs/runbooks/MIGRATION_WORKFLOW.md`.
4. Deploy the Edge Functions from this release source.
5. Create the first administrator:
   `node supabase/scripts/seed_first_admin.mjs`.

Full ownership, release, recovery and handover rules:
`docs/CUSTOMER_TECHNICAL_HANDOVER_MANUAL.md`.
