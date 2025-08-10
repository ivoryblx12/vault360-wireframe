# Vault360 — Day 1 Starter Pack

This pack gives you a **today-only** path to ship a working skeleton:
- Web-first app (Next.js) with Supabase (Auth + Postgres)
- S3-compatible storage (Wasabi/B2 S3) via presigned URLs
- Stripe-ready subscription placeholders
- Referrals, Gifting, Storage Allocation, Funding Pool/Noticeboard schema
- “Close Forever (no re-join)” suppression list design

## Quick Start (Today)
1) **Create Supabase project** → copy URL + anon + service role keys.
2) **Create Stripe Product/Price** (e.g., _Vault360 100GB monthly_). Keep the price ID.
3) **Create a Wasabi/B2 S3 bucket** (e.g., `v360-user-content`) and note endpoint, region, keys.
4) Locally run:
```bash
# Node 18+ and pnpm recommended
npx create-next-app@latest web --ts --eslint --tailwind --src-dir --app

cd web
pnpm add @supabase/supabase-js zod zustand lucide-react stripe aws-sdk @aws-sdk/client-s3 @aws-sdk/s3-request-presigner
pnpm add -D @types/node

# Copy the server examples in ../server into /src/app/api/...
# Copy .env.example to .env.local and fill values
# Create a /src/lib/supabase.ts that reads SUPABASE_URL & SUPABASE_ANON_KEY
```

5) **Database** → open Supabase SQL editor and run `db/schema.sql` (and `db/policies.sql` if needed).
6) **Deploy** the web app to Vercel (or Railway/Fly). Add the same env vars there.
7) **Wire UI**: Use the wireframe we designed; routes to add first:
   - `/login` (Supabase OTP)
   - `/app/home`, `/app/vault`, `/app/hub`, `/app/funding`, `/app/noticeboard`, `/app/linked`, `/app/account`

## Env Variables (.env.local)
See `.env.example` and fill:
- Supabase: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`
- Stripe: `STRIPE_SECRET_KEY`, `STRIPE_PRICE_100GB`
- Storage (S3-compatible): `S3_ENDPOINT`, `S3_REGION`, `S3_ACCESS_KEY_ID`, `S3_SECRET_ACCESS_KEY`, `S3_BUCKET`
- Security: `SUPPRESSION_PEPPER` (random 64+ chars)

## Notes
- **Passkeys** (FIDO2) can be added after OTP is live.
- **No re-join** is enforced via hashed suppressions; you keep deletion promises.
- **Referrals** capped at 10/mo; gifting only from Vault360 in 5GB increments.
