# Customer Release 1.1.0

| | |
|---|---|
| Product | Hajj Management System |
| Release version | `v1.1.0` |
| Snapshot generated | 2026-10-26 |
| Allowlist manifest | `customer-release.manifest.json` v1 (Master) |

This repository is a **clean release snapshot**, not a fork or mirror of the
vendor's Master repository. It carries no Master development history.

## What this snapshot contains

- The frontend application source and build configuration.
- The canonical Supabase migrations required to build a fresh customer database.
- The Supabase Edge Functions and `supabase/config.toml`.
- `package-lock.json` and the pinned Supabase CLI version.
- The **Customer Backend Setup** workflow, which creates the first administrator
  (Deployment, step 3).
- The operational documentation required for this release.

## What it deliberately does not contain

Secrets, real environment values, customer data, Master git history,
internal development/verification/recovery evidence, load-test artifacts,
and vendor-internal GitHub workflows.

## Deployment

Before the first deployment, the repository needs two GitHub secrets:
`SUPABASE_PROJECT_REF` and `SUPABASE_ACCESS_TOKEN` — a Supabase access
token scoped to this project with **Database**, **Migrations**, **Edge
Functions** and **Edge Function Secrets**, all **Read-Write**, and **Auth
Config: Read**. No other permission, secret or password is stored.

1. **Supabase** — set what the workflow verifies before it changes anything
   (it reads these; it cannot change them):
   - Authentication → Sign In / Providers → **Allow new users to sign up** →
     Off, and **Allow anonymous sign-ins** → Off → **Save changes**.
   - Authentication → URL Configuration → **Site URL** → the Vercel production
     URL (the first URL entered in step 3) → **Save**.
   - Edge Functions → Secrets → add **`ANTHROPIC_API_KEY`** with the
     customer's own Anthropic API key (passport scanning). The workflow checks
     only that the name exists; the key never goes to GitHub or Vercel.
2. **Supabase** — Authentication → Users → **Add user**. Enter the
   administrator's Login ID (email form, e.g. `admin@company.local`) and
   password, and tick **Auto Confirm User**.
3. **GitHub** — Actions → **Customer Backend Setup** → Run workflow. Enter the
   Vercel production URL, Vercel project name and owner, a contact
   (`mailto:…`), the same administrator Login ID with a display name, and
   the Hijri year of the first Hajj season (e.g. `1448`). The workflow first
   verifies that public sign-up and anonymous sign-in are off, that the Site
   URL is the first Production URL and that `ANTHROPIC_API_KEY` exists, and
   stops without changing anything if not.
   It then applies the database, creates the First Administrator with every system permission,
   creates the first open season (an existing open season is kept unchanged),
   configures the Edge Function secrets, deploys the seven Edge Functions,
   activates scheduled push notifications for this project (its own Vault
   dispatch URL and token, never copied from elsewhere) and verifies the
   result — including a probe that proves scheduled push reaches its function.
   If a run fails, fix the reported
   cause and run it again — it continues safely.
4. **Vercel** — set `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY` and the
   `VITE_VAPID_PUBLIC_KEY` shown in the workflow's summary, then deploy.
5. **Application** — sign in and scan a test/specimen passport image (never
   real customer or pilgrim data). Confirm the extracted fields are populated,
   then close the form without saving the test pilgrim. The workflow proves
   only that the key exists; this proves passport scanning works.

Full ownership, release, recovery and handover rules:
`docs/CUSTOMER_TECHNICAL_HANDOVER_MANUAL.md`.
