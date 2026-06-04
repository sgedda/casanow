# CasaNow — Launch playbook (economics, GTM, operations)

Turns the gaps flagged in [CRITICAL_REVIEW.md](./CRITICAL_REVIEW.md) into actionable content: **unit economics, go-to-market/liquidity, money policy, trust & safety, operations, and metrics**. This is the "how to run and grow it" layer that complements the product and tech plans.

**Related:** [BUSINESS_PLAN.md](./BUSINESS_PLAN.md) · [TECH_PLAN.md](./TECH_PLAN.md) · [TECH_CHALLENGES.md](./TECH_CHALLENGES.md) · [CRITICAL_REVIEW.md](./CRITICAL_REVIEW.md) · [PLAN.md](./PLAN.md)

> Not legal, tax, or financial advice. Numbers below are **illustrative models** to pressure-test the business—replace with real quotes from your gestoría and Stripe pricing.

---

## 1. Positioning (why CasaNow vs the alternatives)

| Alternative | Their edge | CasaNow angle |
|-------------|-----------|---------------|
| **"My guy Carlos" (WhatsApp)** | Trusted, cash, free | For when you *don't* have a guy: fast match, reviews, no awkward price haggling |
| **Habitissimo / Cronoshare** | SEO, many pros | Lead-gen, not booking; pros pay for leads, slow. CasaNow = instant small jobs, fixed flow |
| **Hiring a company (empresa)** | Insured, invoiced | Overkill/expensive for a €40 pool skim or shelf mount |
| **Facebook groups** | Free, local | Unstructured, no payment/trust layer |

**One-liner:** *"Post a small home job, get it done today by a vetted local—pay by card or cash."*

**Wedge:** Start with **pool/garden + handyman** (high frequency, expat second-homes, clear value), not a broad Tiptapp clone.

---

## 2. Unit economics (illustrative)

**Assumptions (replace with real figures):**
- Stripe card/wallet fee (EEA cards): **~1.5% + €0.25** (non-EEA higher). Connect/payout costs extra.
- Commission v1: **15%**, with a **minimum platform fee** so tiny jobs aren't loss-making.

### In-app paid job

| | Small job | Typical job | Bigger job |
|--|----------|-------------|------------|
| Job price | €30 | €75 | €150 |
| Customer pays (price + 15% fee) | €34.50 | €86.25 | €172.50 |
| Gross platform fee (15%) | €4.50 | €11.25 | €22.50 |
| Stripe (~1.5% + €0.25 on total) | ~€0.77 | ~€1.54 | ~€2.84 |
| **Net platform margin** | **~€3.73** | **~€9.71** | **~€19.66** |
| Helper receives | €30 | €75 | €150 |

> On a **€10–15 micro-job**, a 15% fee (€1.50–2.25) is mostly eaten by Stripe's €0.25 + percentage → **set a minimum fee of ~€2.99** or a minimum job price (e.g. €20).

### Cash job (fee-only revenue)

| | Cash job |
|--|----------|
| Job price (cash, off-platform) | €75 |
| Platform fee charged in-app | €2.99 |
| Stripe on €2.99 | ~€0.29 |
| **Net platform margin** | **~€2.70** |

**Implication:** Cash jobs earn **~3.6×–7× less** per job than in-app. Either (a) keep cash fee flat and accept it as a retention/trust tool, or (b) raise cash fee, or (c) restrict cash to a perk for verified repeat users. **Decide before launch.**

### Break-even sketch (monthly)

Fixed-ish costs (low volume): infra €300 + accounting €150 + misc €200 ≈ **€650/mo** (excludes your salary, legal setup, marketing).

- At **~€9.70 net/typical in-app job** → break-even ≈ **~67 in-app jobs/month** (~2–3/day).
- Mixed with cash jobs (lower margin) → realistically **80–120 jobs/month** to cover costs.

**Takeaway:** This is a **volume + repeat-rate** business. The model only works if helpers and customers come back; one-off jobs barely cover Stripe + ops.

**Open decisions:** commission %, minimum fee, minimum job price, cash fee, who pays Stripe fee (customer vs split).

---

## 3. Go-to-market & liquidity (cold start)

Marketplaces die from **empty supply or empty demand**. Solve supply first, demand in a tight geo.

### Phase 0 — Pre-launch supply (weeks -4 to 0)

- **Recruit 15–25 helpers** by hand: pool/garden freelancers, handymen, students; WhatsApp groups, "Manitas Marbella" FB groups, urbanización maintenance contacts, gym/co-working noticeboards.
- **Vet manually** (see §5). Onboard their Stripe Connect *before* launch so payouts work day one.
- **Helper pitch:** "Free leads near you, you set your price, get paid fast, no monthly fee."

### Phase 1 — Concentrated demand (weeks 0–6, private beta)

- **Single-neighborhood focus** (e.g. Nueva Andalucía / Golden Mile) to get **density**, not city-wide thin coverage.
- Channels: expat Facebook groups, local property managers, holiday-rental cleaners, real-estate agents (move-in jobs), urbanización admin newsletters.
- **Founder-filled gaps:** for the first weeks, *guarantee* a response—if no helper bites in 2h, you call one directly. Subsidize a few first jobs (e.g. "first pool clean €10 off") to seed reviews.

### Phase 2 — Referral loops

- **Customer referral:** give €X credit for inviting a neighbor (urbanizaciones are dense referral networks).
- **Helper referral:** small bonus for bringing another vetted helper.
- **Repeat nudges:** "Book Ana again" one-tap; seasonal reminders (pool season, pre-summer garden).

### Channels ranked (Marbella)

| Channel | Why | Effort |
|---------|-----|--------|
| Expat Facebook groups | Large, English-speaking, high intent | Low |
| Property / rental managers | Recurring jobs, B2B-ish volume | Medium |
| Urbanización admins | Trusted distribution, dense | Medium |
| Local SEO + simple landing page | "pool cleaning Marbella app" | Medium (ongoing) |
| Flyers in gated communities | Old-school but works locally | Low |

> **Web presence is missing from current plans.** Even a one-page site (post-a-job lead form + app links) captures Google demand that never opens an App Store.

---

## 4. Money policy (cancellation, refunds, payouts)

The tech docs say "design with the business plan"—here are the rules to lock. Put the final version in **Terms** (lawyer-reviewed).

### Capture & payout timing

- **In-app:** authorize on booking; **capture on customer "Confirm completed"** (or auto-capture 24–48h after helper marks done if no dispute).
- **Payout to helper:** after capture, minus commission, via Connect (Stripe payout schedule, e.g. daily/weekly).

### Cancellation

| When | Customer cancels | Helper cancels |
|------|------------------|----------------|
| Before `assigned` | Free, no charge | n/a |
| After `assigned`, before start | Free or small fee (grace window, e.g. 1h) | Warning; repeat → lower ranking |
| After helper started / en route | Charge cancellation fee (e.g. 25–50% or flat €X) | Refund customer; flag helper |
| No-show (either side) | Document via app timestamps; manual review | Same |

### Refunds & disputes

- **Customer "Report a problem"** → job to `disputed`, payout held.
- Resolution paths: full refund, partial, or release to helper—**you decide within SLA** (e.g. 48h).
- **Chargebacks:** respond in Stripe with in-app evidence (photos, timestamps, chat, confirmations).
- **Cash jobs:** platform is **not** party to the cash; fee is non-refundable except platform error. State clearly.

### Caps

- Optional **max job value** at launch (e.g. €300) to limit fraud/liability exposure.

---

## 5. Trust & safety (vetting, insurance, incidents)

### Helper vetting (v1, manual)

- Verify identity (DNI/NIE + selfie or Stripe Identity).
- Confirm category competence (photos of past work, short call).
- Collect **autónomo status** / intent; surface earnings info (DAC7).
- Optional: **RC profesional** (liability insurance) declaration—required for higher-risk categories (pool chemicals, electrical).

### Insurance reality

- **Platform does not insure home visits.** Decide v1 stance:
  - (a) Helpers must hold RC (limits supply, raises trust), or
  - (b) Disclaim liability in Terms + push customers to choose insured helpers (badge), or
  - (c) Explore a group/on-demand liability product (hard/manual in ES).
- Property managers and urbanizaciones may **require proof of insurance** before recommending you—this can be a launch blocker for the best channel.

### Incident response (write before launch)

| Incident | First action | Then |
|----------|-------------|------|
| Property damage | Freeze payout; gather photos/chat | Mediate; helper RC if any; possible category ban |
| Theft allegation | Suspend helper; preserve records | Advise police report; cooperate; legal |
| Injury | Document; do not admit liability | Insurance/legal; review category |
| Harassment / abuse in chat | Suspend; preserve logs | Ban policy; report flow |

### Anti-fraud

- Fake jobs / stolen photos → image checks + manual review of new accounts.
- Collusion (fake jobs to cycle money) → watch new-account + cash patterns.
- New-helper limits (e.g. cap concurrent jobs until N completed).

---

## 6. Operations playbook (solo operator)

You will be support + disputes + recruiting. Define limits so you don't drown.

- **Support channels:** in-app report + email/WhatsApp; **target first response < 12h** in beta.
- **Triage:** payment-stuck and safety issues first; cosmetic later.
- **Refund authority:** you decide up to €X instantly; document reason.
- **Ban policy:** written thresholds (e.g. 2 valid complaints, any safety incident).
- **Daily ritual (beta):** check unmatched jobs (fill gaps), new helper approvals, disputes, payout failures.
- **Scaling trigger:** when > ~50 active jobs/week consistently → hire part-time ops/community person.
- **Runbooks:** keep short docs for "payout failed," "Connect account restricted," "chargeback received," "disputed job."

---

## 7. Metrics & instrumentation

**North star:** **completed jobs / week** (proxy for real value exchanged).

### Funnels to instrument (PostHog/Mixpanel + Sentry)

- **Customer:** open → category → post job → receives offer → accept → pay → complete → repeat.
- **Helper:** install → register → **Connect complete** → first offer → first job → 30-day active.
- **Liquidity:** time-to-first-offer, % jobs receiving an offer < 2h, fill rate, completion rate.

### KPI dashboard (weekly)

| KPI | Target (90 days) |
|-----|------------------|
| Completed jobs/week | trending up; ~15–25 to feel alive |
| Time to first offer (median) | < 4h (ideally < 1h in core neighborhood) |
| Offer-within-2h rate | > 60% |
| Completion rate | > 70% |
| Helper Connect completion | > 50% of registered |
| Helpers active (≥1 job/14d) | ≥ 15 |
| Repeat customer rate (30d) | > 20% |
| Dispute rate | < 5% of jobs |

### Kill / pivot

- < 30 completed jobs in first 90 days → narrow categories or pivot **B2B (property managers only)**.
- < 50% helper Connect completion → rethink payout/onboarding with gestoría.

---

## 8. GDPR / data protection (operational specifics)

Beyond "have a privacy policy":

- **Data inventory (ROPA):** identities, location, photos (may show interiors), chat, payment metadata.
- **DPAs** with Supabase and Stripe; keep data in **EU region**.
- **Retention:** define (e.g. job photos deleted N months after completion; chat retained for dispute window).
- **Rights:** in-app **export** and **delete account** → cascade Supabase rows + Storage objects (see [TECH_PLAN.md](./TECH_PLAN.md) GDPR job).
- **Minimize:** mask helper/customer phone until assigned; precise location only to assigned helper.

---

## 9. Consolidated open decisions (decide before/at launch)

| # | Decision | Owner | Blocks |
|---|----------|-------|--------|
| 1 | Commission %, minimum fee, minimum job price | You | Economics, payments code |
| 2 | Cash fee amount + cash strategy (perk vs revenue) | You | Payments, Terms |
| 3 | Who pays Stripe fee (customer vs split) | You | Pricing UI |
| 4 | Capture timing + payout schedule | You + Stripe | Payment flow |
| 5 | Cancellation/refund rules | You + lawyer | Terms, code |
| 6 | Insurance stance (require RC vs disclaim vs group) | You + lawyer | Trust, channels |
| 7 | Helper status / earnings cap (autónomo, DAC7) | Gestoría | Onboarding |
| 8 | v1 categories (recommend pool/garden + handyman) | You | Scope, legal |
| 9 | Max job value cap | You | Fraud/liability |
| 10 | Launch neighborhood (density-first) | You | GTM |
| 11 | Web/landing presence in v1? | You | Demand capture |

---

## 10. What this doc adds vs the others

| Theme | Was it covered? | Now |
|-------|-----------------|-----|
| Unit economics (numbers) | No | §2 worked examples + break-even |
| GTM / liquidity playbook | Flagged only | §3 channels, cold start, referrals |
| Money policy (refund/cancel/payout) | "TBD" in tech docs | §4 concrete rules |
| Trust & safety / incidents | Mentioned | §5 vetting + incident table |
| Solo-operator ops | "you handle tickets" | §6 runbooks, SLAs, scaling trigger |
| Metrics & instrumentation | KPIs named loosely | §7 funnels + dashboard |
| GDPR operational detail | Light | §8 ROPA, retention, rights |
| Positioning | Flagged only | §1 |
| Open decisions | Scattered | §9 single list |
