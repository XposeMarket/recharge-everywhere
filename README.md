# REV — Recharge Everywhere

A production-oriented starter repository for a controlled EV charger marketplace pilot in Washington, D.C. and Northern Virginia.

## What is implemented

- High-fidelity responsive REV homepage based on the supplied purple/green mockups
- Desktop and mobile charger discovery experience
- Charger detail and reservation estimate UI
- Driver bookings and manual check-in/session flow
- Host marketing page with an honest earnings estimator
- Saved-progress-style host onboarding interface
- Host dashboard, earnings visualization, bookings, availability, listings, and support routes
- Internal admin operations dashboard and core admin routes
- Supabase Auth client/server boundaries
- PostGIS schema, private/public property separation, core RLS, radius search function, and database-level overlap exclusion
- Stripe, Mapbox, Resend, and charger-provider integration boundaries
- Unit and Playwright test foundations
- Realistic D.C., Arlington, and Alexandria development listings

## Local setup

1. Install Node.js 20 or newer.
2. Copy `.env.example` to `.env.local`.
3. Run `npm install`.
4. Run `npm run dev`.
5. Open `http://localhost:3000`.

The interface works with seeded in-repository data when third-party credentials are absent. It explicitly labels development mode and never claims a real payment, email, verification, or charger command occurred.

## Supabase

1. Create a Supabase project.
2. Enable PostGIS in the database extensions screen if it is not already available.
3. Run `supabase/migrations/0001_initial.sql` through the CLI or SQL editor.
4. Run `supabase/seed.sql` only for a local or disposable development project.
5. Create storage buckets: `listing-images` public, plus private `profile-images`, `charger-documents`, `verification`, and `incident-evidence`.
6. Configure email, Google Auth, and redirect URLs.

The migration includes public approximate location, private exact location, an approved-charger radius function, booking overlap prevention, and initial RLS. Codex should extend policies table by table before production.

## Stripe test mode

Set `STRIPE_SECRET_KEY`, `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY`, and `STRIPE_WEBHOOK_SECRET`. The starter contains pricing and integration boundaries, not a claim of completed live fund movement. Codex should add Connect account onboarding routes, PaymentIntent idempotency, webhook event persistence, transfers, refunds, and reconciliation against the included payment tables.

## Mapbox

Set `NEXT_PUBLIC_MAPBOX_TOKEN` to replace the CSS pilot map with live Mapbox GL. Public APIs must return only approximate coordinates for residential listings.

## Resend

Set `RESEND_API_KEY` and `RESEND_FROM_EMAIL`. In development, keep email delivery disabled or log previews.

## Vercel

Import the GitHub repository into Vercel, add environment variables, and deploy. Use a Supabase project in the same general region. Keep service-role credentials server-only.

## Security architecture

- Roles are database-owned and never trusted from client input.
- Exact property details are separated into `property_private_details`.
- Residential address access is limited to hosts, authorized staff, and eligible confirmed booking drivers.
- Active bookings use a PostgreSQL exclusion constraint to prevent overlap.
- Sensitive admin actions are designed to append to `admin_audit_logs`.
- Private uploads should use signed URLs and server authorization.
- Production payment and status transitions must remain server-side and idempotent.

## Testing

- `npm run typecheck`
- `npm run lint`
- `npm test`
- `npm run test:e2e`
- `npm run build`

## Known limitations / Codex handoff

The execution environment that generated this starter could create and visually inspect files but could not download npm packages from its registry, so it could not truthfully run the Next.js build, TypeScript checker, Vitest, or Playwright. Codex should install dependencies first and repair any package-version or framework API drift.

Before a real pilot, Codex must complete and verify: Supabase auth actions/callbacks, all RLS policies, server-side booking transactions, Stripe Connect webhooks and payouts, Mapbox geocoding, secure upload routes, Resend templates, rate limiting, admin action audit writers, identity-verification provider selection, legal copy, cancellation terms, incident SOPs, and final accessibility testing.

## Design references

The original supplied mockups are preserved in `docs/mockups/`. The application styling deliberately follows their light off-white surfaces, strong purple actions, green availability cues, rounded white cards, soft shadows, large friendly type, mobile bottom navigation, and marketplace/host dashboard layouts.
