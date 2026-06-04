# CasaNow — Technical plan

**Stack:** React Native (Expo) + Supabase + Stripe Connect. Marbella geofence, dual payment (in-app + cash workflow), marketplace job lifecycle.

**Related:** [BUSINESS_PLAN.md](./BUSINESS_PLAN.md) · [APP_SKETCHES.md](./APP_SKETCHES.md) · [CRITICAL_REVIEW.md](./CRITICAL_REVIEW.md) · [PLAN.md](./PLAN.md) (index)

---

## Checklist (technical)

- [ ] Open Stripe Connect platform (ES); commission + capture/payout flow + Apple/Google Pay
- [ ] Create Supabase EU project: schema, RLS, Storage, Edge Functions (webhooks)
- [ ] Scaffold Expo app: auth, roles, post job, photos, geofence, offers, job state machine
- [ ] Stripe Connect helper onboarding in app
- [ ] In-app pay + cash path (platform fee PI + dual confirmation)
- [ ] Push notifications (offers, assignment, completion)
- [ ] Reviews, report user, minimal admin paths
- [ ] TestFlight + Play internal; GDPR screens; Sentry
- [ ] RLS audit; Stripe webhook idempotency; staging test clocks

---

## Stack

| Layer | Choice | Why |
|-------|--------|-----|
| Mobile | **Expo (SDK 52+) + TypeScript + Expo Router** | iOS/Android, OTA, camera/location |
| Backend | **Supabase** (Postgres, Auth, Storage, Realtime, Edge Functions) | RLS, EU region |
| Payments | **Stripe Connect** (Express accounts for helpers) | ES; Payment Sheet + Apple/Google Pay |
| Maps / geo | **Google Maps** or Mapbox | Marbella polygon; distance sort |
| Push | **Expo Notifications** + FCM/APNs | Job/offer alerts |
| Analytics | PostHog or Mixpanel + **Sentry** | Funnels, crashes |
| Admin | Supabase Studio + Edge Functions; Retool optional | Moderation, refunds |

### Payments — not App Store IAP

Apple/Google **IAP** is for digital goods only. Physical services use **Stripe Payment Sheet** (card, Apple Pay, Google Pay). Bizum → later via local PSP if needed.

### Cash + in-app (implementation)

| Mode | Behavior |
|------|----------|
| **In-app** | `PaymentIntent` for job + platform fee; **manual capture** or transfer on `job_completed`; Connect transfer to helper minus application fee |
| **Cash job** | No PI for job amount; PI for **platform fee only**; `payment_method = cash_job`; both parties confirm completion flags |

Edge cases to implement: SCA failures, restricted Connect accounts, partial refund, cancel before/after assign, idempotent webhooks.

---

## Data model (Supabase)

| Table / bucket | Purpose |
|----------------|---------|
| `profiles` | `customer` \| `helper` \| `admin` |
| `helper_profiles` | radius, categories, `stripe_account_id`, `autonomo_declared` |
| `jobs` | status: `draft` → `open` → `assigned` → `in_progress` → `completed` \| `disputed` \| `cancelled` |
| `job_photos` | Storage bucket; private; signed URLs |
| `offers` | helper_id, amount, message |
| `payments` | `stripe_payment_intent_id`, method: `card` \| `apple_pay` \| `cash_job` |
| `reviews`, `reports`, `service_categories` | |
| `geo_municipalities` | Marbella launch polygon (PostGIS or lat/lng check) |

**RLS** on all tables.

**Edge Functions:**

- Stripe webhooks (`payment_intent.succeeded`, `account.updated`, …)
- `create-payment-intent`, `create-connect-account-link`
- `check-geofence` (job post validation)
- Optional: `notify-offer` via push provider

**Feature flags (env or table):** `cash_payments`, `category_waste`

---

## App structure (Expo Router)

```
app/
  (auth)/ login, register
  (customer)/ post-job, my-jobs, job/[id]
  (helper)/ browse, offers, earnings
  (shared)/ chat, profile, payments-onboarding
```

**Libraries:** `expo-image-picker`, `expo-location`, `@stripe/stripe-react-native`, `i18next` (es, en), maps (Google or `react-native-maps`).

**Auth:** Supabase Auth (email magic link or phone OTP for ES market—decide in phase 0).

**Realtime:** Supabase Realtime on `offers` / `jobs` for offer notifications (fallback: poll + push).

---

## Build phases

One experienced developer; reviews all Cursor/AI output.

| Phase | Scope | Without Cursor | With Cursor |
|-------|--------|----------------|-------------|
| **0** | Expo scaffold, Supabase EU, auth, profiles | 1 week | **2–4 days** |
| **1** | Post job, photos, categories, geofence | 2 weeks | **1–1.5 weeks** |
| **2** | Offers, assign, state machine, push | 2 weeks | **1–1.5 weeks** |
| **3** | Connect onboarding, pay, payout | 3 weeks | **2–2.5 weeks** |
| **4** | Cash flow, platform fee PI, confirmations | 1 week | **3–5 days** |
| **5** | Reviews, report user, admin hooks | 1 week | **3–5 days** |
| **6** | TestFlight/Play, GDPR UI, polish | 1 week | **~1 week** |

| Summary | |
|---------|--|
| Without Cursor | **~11–12 weeks** focused dev |
| With Cursor | **~6–8 weeks** focused dev |
| Wall-clock to beta | **10–14 weeks** incl. Stripe verification, devices, store review |

**Cursor helps (~40–60%):** screens, forms, SQL/RLS, i18n, Edge Function stubs.

**Cursor helps little:** Connect lifecycle, webhooks, SCA, maps/push native config, RLS security review.

**Part-time (15–20 h/week) + Cursor:** ~8–10 calendar weeks for MVP build.

---

## Engineering risks (technical)

1. **Stripe Connect** — onboarding drop-off, restricted accounts, payout timing
2. **Job state machine** — double-accept race (`assigned` lock + DB constraint)
3. **Geo** — urbanización addresses; gated communities
4. **Storage + RLS** — photo access only for job parties + admin
5. **Push reliability** — token refresh, background delivery
6. **Chat** (if in v1) — moderation hooks for store compliance

---

## App store & compliance (technical deliverables)

- Report user flow, block user, terms/privacy links in app
- Marketplace metadata: physical services, external payments
- Stripe platform business verification (days–weeks)
- GDPR: export/delete account → Supabase + Storage cleanup job

---

## Repo bootstrap

1. `git init`; Expo app in repo root or `apps/mobile`
2. Supabase CLI migrations in `supabase/migrations`
3. `.env.example` — no secrets committed
4. Stripe CLI for local webhooks
5. `docs/` — ops playbook referenced from business plan

---

## Environment & secrets

| Secret | Where |
|--------|--------|
| Supabase URL, anon key, service role | EAS secrets / server only for service role |
| Stripe publishable | App |
| Stripe secret, webhook secret | Edge Functions only |
| Google Maps API key | App + restrictions |

---

## Immediate next steps (technical)

1. Create Supabase project (EU) + Stripe Connect platform (test mode).
2. Phase 0 scaffold: Expo + Auth + `profiles` RLS.
3. Spike: PaymentIntent + Connect Express onboarding in sandbox.
4. Document capture/refund rules aligned with [BUSINESS_PLAN.md](./BUSINESS_PLAN.md).
