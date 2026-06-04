# CasaNow — Trickiest technical challenges

What is hardest to build, why it matters, and what to validate early. Complements [TECH_PLAN.md](./TECH_PLAN.md) (Phase 3 is longest for a reason).

**Related:** [APP_SKETCHES.md](./APP_SKETCHES.md) · [CRITICAL_REVIEW.md](./CRITICAL_REVIEW.md) · [PLAN.md](./PLAN.md)

---

## Ranked overview

| Rank | Area | Impact if wrong |
|------|------|-----------------|
| 1 | **Stripe Connect + dual payment** | Lost money, chargebacks, helpers never paid |
| 2 | **Job / offer state machine + races** | Double booking, payments stuck in limbo |
| 3 | **Supabase RLS + webhooks** | Data leaks, unauthorized access |
| 4 | **Push + realtime notifications** | Marketplace feels “dead” |
| 5 | **Geo + Marbella geofence** | Wrong jobs, support noise |

**Easier with Cursor (if v1 stays thin):** post-job wizard, auth, categories, i18n, basic chat UI, reviews.

---

## 1. Stripe Connect + job payment flow (hardest)

Longest phase in the build plan. Cursor helps little here—expect real sandbox and device testing.

### What you must implement

- **Helper onboarding:** Connect Express, account links, `account.updated` webhooks, restricted/disabled accounts, incomplete KYC.
- **Customer pay-in:** Payment Sheet (card, Apple Pay, Google Pay)—not App Store IAP (physical services).
- **Platform fee:** `application_fee_amount` or separate fee line; clear split on receipts.
- **When money moves:** Authorize vs **manual capture** vs transfer on `job_completed`—not true escrow; design with lawyer/gestoría.
- **Dual modes:**
  - **In-app:** `PaymentIntent` for job + platform fee → capture/transfer on completion.
  - **Cash job:** No PI for job amount; PI for **platform fee only**; `payment_method = cash_job`; dual confirmation flags.

### Edge cases (easy to miss)

| Case | Handling |
|------|----------|
| SCA / 3DS failure | Retry UX; don’t assign job until payment succeeds (policy choice) |
| Restricted Connect account | Block helper from accepting paid jobs; surface “Complete payout setup” |
| Customer cancels after pay | Refund / partial capture rules in business plan |
| Helper no-show | Refund or capture policy |
| Webhook retry | Idempotent handlers; store `stripe_event_id` |
| Chargeback | Stripe dashboard + internal `disputed` state |

### Validate early (week 1–2 spike)

1. Platform Connect account (Spain test mode).
2. One helper completes Express onboarding.
3. One customer pays → hold/capture → transfer to helper with commission.
4. One cash job: platform fee PI only + both confirm complete.

Do **not** polish all UI before this spike works end-to-end.

---

## 2. Job / offer state machine + concurrency

Looks simple in [APP_SKETCHES.md](./APP_SKETCHES.md); subtle bugs have high impact.

### States

```
draft → open → assigned → in_progress → completed
                    ↘ cancelled
                    ↘ disputed
```

### Hard parts

- **Race:** Two helpers’ offers accepted at once → DB constraint: one `assigned` offer per job; transactional update.
- **Payment sync:** Don’t set `completed` until payment rules satisfied (captured, or cash confirmations).
- **Cash path:** No job-amount PI—completion is policy + flags, not Stripe state alone.
- **Cancellation:** Different rules before/after `assigned` and after capture.

### Implementation hints

- Use Postgres transactions or `UPDATE ... WHERE status = 'open'` with row count check.
- Single source of truth: `jobs.status` driven by Edge Function after webhooks, not only client.

---

## 3. Supabase RLS + Edge Functions (security)

Marketplace data is sensitive: home addresses, photos, chat.

### Rules of thumb

| Actor | Should see |
|-------|------------|
| Customer | Own jobs, offers on own jobs, own payments |
| Helper | Open jobs in radius/categories; assigned jobs; own offers |
| Admin | Moderation via service role only |

- **Photos:** Private bucket; signed URLs; RLS on `job_photos` metadata.
- **Webhooks:** Service role in Edge Functions only; never expose service key to app.
- **Review AI-generated RLS** before production—common leak: helpers reading all `jobs` rows.

---

## 4. Push + realtime (reliability)

Conceptually easy; **bad reliability kills liquidity**.

- Register Expo push tokens; refresh on login.
- Events: new offer, offer accepted, job nearby, payment released, completion reminder.
- **Realtime** on `offers` / `jobs` for in-app updates; **push** when app backgrounded/killed.
- Test on real iOS + Android devices, not only simulator.

---

## 5. Geo + Marbella geofence (medium)

- Polygon or boundary check on job post (reject outside launch area).
- Helper feed: filter by radius + category.
- Pain points: urbanización names vs map pin; gated communities (access notes field).
- Don’t over-build map clustering in v1—list + simple map is enough.

---

## What Cursor accelerates vs not

| Faster (~40–60%) | Barely faster |
|------------------|---------------|
| Expo screens, forms | Connect lifecycle |
| SQL migrations, RLS drafts | Webhook idempotency + payment edge cases |
| i18n strings | SCA / restricted accounts |
| Edge Function stubs | Native push/maps config |
| Category grid UI | Security review of RLS |

---

## Suggested build order

1. **Spike:** Connect + one in-app payment happy path (sandbox).
2. **Core:** Auth, profiles, post job, job list, state machine without pay.
3. **Payments:** Connect onboarding, PI, webhooks, completion payout.
4. **Cash path:** Platform fee PI + dual confirm.
5. **Harden:** RLS audit, push, geofence, disputes/refunds.
6. **Polish:** Reviews, chat, store assets.

Chat and admin can stay minimal in v1. **Payments and state machine cannot.**

---

## Timeline tie-in

From [TECH_PLAN.md](./TECH_PLAN.md):

- **Phase 3** (Connect + pay): **2–2.5 weeks** with Cursor, full-time—still the critical path.
- If the week 1–2 spike fails or slips, add time to the whole **6–8 week** engineering estimate.

---

## Related docs

- [TECH_PLAN.md](./TECH_PLAN.md) — Phases, stack, schema
- [BUSINESS_PLAN.md](./BUSINESS_PLAN.md) — Cash policy, commission, refunds (drives payment logic)
- [APP_SKETCHES.md](./APP_SKETCHES.md) — Screens tied to states above
