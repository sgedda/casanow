# CasaNow — Critical review

Honest critique of [BUSINESS_PLAN.md](./BUSINESS_PLAN.md) and [TECH_PLAN.md](./TECH_PLAN.md): what holds up, what is missing, and what will hurt most in Marbella. Index: [PLAN.md](./PLAN.md).

---

## What the plan does well

1. **Correct bottleneck framing** — The pie chart and supply/demand emphasis match how local marketplaces fail (liquidity, not React Native).
2. **Payment reality check** — IAP vs Stripe, cash off-platform with a platform fee, and Connect for helpers are the right direction for Spain.
3. **Legal topics are the right ones** — DAC7, autónomo, waste transport, GDPR, employment-presumption UX (even if rider law is delivery-focused).
4. **Geo-first launch** — Marbella polygon + manual helper vetting is the only sane way to avoid empty maps.
5. **Cursor timelines are more honest than typical** — Phase 3 and store review are not magically shortened; wall-clock beta still 10–14 weeks is credible.

---

## What is lacking (gaps to address before building)

| Gap | Why it matters |
|-----|----------------|
| **Competitive landscape** | Marbella is not greenfield: Habitissimo, Cronoshare, local “manitas” on WhatsApp/Facebook, property-manager networks, and informal cash pool/garden workers. No positioning vs “why app, why fee.” |
| **MVP scope cut** | Eight categories + bidding + chat + dual payment + reviews is a lot. Plan does not define **v1 = 2–3 categories only** (e.g. pool/garden + manitas + small transport). |
| **Unit economics** | No minimum job value, commission on €25 jobs, or Stripe fees eating margin. Small tasks may be **unprofitable** unless booking fee is fixed. |
| **IVA / invoicing** | Spain: platform fee VAT, helper invoices to customer, your SL as intermediary. Not mentioned; gestoría will ask on day one. |
| **Real “escrow” design** | “Hold until completion” is hand-waved. Stripe Connect uses authorize/capture or separate transfer timing—not true escrow; chargebacks and partial completion need a written policy. |
| **Cancellation & refunds** | No-show customer, helper abandons job, weather, wrong scope—no states or money rules in the plan. |
| **Disintermediation** | Users will move to WhatsApp after first match. No tactic: in-app chat only, masked phone, repeat-booking discounts, or accepting leakage and monetizing lead gen. |
| **Bizum** | Very common in Spain for local services; “optional later” may mean **most helpers never onboard to Stripe**. |
| **Helper onboarding friction** | Connect Express + autónomo + bank + app install is heavy for informal workers. No “lite helper” or **you invoice and pay helpers** (more legal load) alternative. |
| **Web / SEO** | Many customers search Google, not App Store. No marketing site, no “post job without app” funnel. |
| **Chat safety & moderation** | `chat` is in the app tree but not in phases: harassment, scams, sharing addresses, UGC store rules. |
| **Operational playbook depth** | “You handle tickets” without SLA, refund authority, or **when you ban users**—solo operator will drown after ~50 jobs. |
| **Insurance as launch blocker** | Mentioned lightly; property managers and urbanizaciones may require **proof of RC** before recommending you. |
| **Injury / property damage** | Helper breaks tile, floods pool, theft allegation—liability cap in terms is not enough; need incident process and possibly category bans. |
| **Fraud & trust** | Fake jobs, stolen photos, helper collusion, chargeback abuse—not in risks table. |
| **Success / kill criteria** | Metrics listed but no “if after 90 days X, stop or pivot.” |
| **AML / payment institution** | Usually fine at MVP with Stripe, but if you **hold** balances creatively, legal review needed. |
| **Testing & security** | No RLS audit plan, pen test, or staging with Stripe test clocks. |
| **Competitive moat** | Why CasaNow wins vs “my guy Carlos on WhatsApp” is unstated. |

---

## Biggest challenges (ranked)

### 1. Marketplace liquidity in a small geography (existential)

Marbella sounds big; operationally you need **density by category and neighborhood**. A customer in Nueva Andalucía waiting 48h for a pool clean will never return. Tiptapp succeeded in cities with scale; you need either brutal category focus or heavy subsidized early jobs. This is harder than the 6–8 week build.

### 2. Informal economy vs digital payments (structural)

The plan correctly offers cash, but the **default path** (Stripe + platform fee on cash jobs) fights local norms. Helpers who live on cash may refuse Connect; customers who trust Carlos won’t pay 12–18% + card fee. Biggest product tension: **compliance/revenue vs adoption**.

### 3. You as solo operator of record (bandwidth)

You’re tech + legal entity + support + helper recruiting + disputes. The plan understates **marketing** (5% in the chart is low for year one; expect **20–30%** of founder time). One bad incident (theft, injury, data leak) consumes weeks.

### 4. Legal and tax complexity beyond a checklist row

SL + marketplace + DAC7 + helper status + consumer law is real, but the plan does not budget **ongoing** gestoría (quarterly models, invoicing flows) or **€8k–15k** if a lawyer drafts platform terms and payment flows properly. “Friends & family beta” with you as operator is riskier than the plan admits.

### 5. Stripe Connect helper activation (technical–business)

Phase 3 is rightly long. In practice, **% of helpers who complete Connect** often dominates payout success. Restricted accounts, SCA failures, and “I don’t have autónomo yet” will be support tickets—not code bugs.

### 6. Trust on first home visit (brand)

Unlike moving a sofa on the street, you send strangers into homes (gated communities, valuables, expats’ second homes). Without insurance, reviews volume, and visible vetting, **customer acquisition cost stays high**.

### 7. Category and regulatory landmines

Waste transport and even some “transport” jobs touch **licences**; pool work touches **chemicals and liability**. Launching “Otros” + broad Tiptapp clone increases legal and quality risk. Easiest fix: **cut categories for v1** (plan mentions restrict waste but not cut list).

### 8. App Store + marketplace policy

Apple scrutinizes peer-to-peer and physical services. You need working moderation, reporting, and clear physical-service payment story. Rejection or metadata churn can add **2–4 weeks** not in engineering estimate.

---

## Weaker assumptions (challenge these)

| Assumption | Reality check |
|------------|----------------|
| **“20–40 vetted helpers”** | Vetting without insurance/background partner is mostly cosmetic; **active** helpers (last job &lt;14 days) might be 5–10 at launch. |
| **Cash + small in-app fee** | Many users resist fee on top of cash; expect **fee evasion** or churn unless cash is deprioritized or fee is customer card-only. |
| **4–6 months “responsible launch”** | Achievable only if legal and helper seeding run **in parallel from week 0** and you accept a **narrow MVP**; full category set + dual payments → **6–9 months** more realistic. |
| **Cursor 6–8 weeks** | Achievable for **thin** MVP; chat, bidding, admin, hardened payments push toward 11–12 weeks. |
| **Commission 12–18%** | May be too high vs informal market; too low on €30 jobs after Stripe fees. Needs a **minimum fee** model. |

---

## Recommended additions to the plans

Most of these are now addressed in **[LAUNCH_PLAYBOOK.md](./LAUNCH_PLAYBOOK.md)** (economics, GTM, money policy, trust & safety, ops, metrics).

1. **One-page positioning**: vs WhatsApp, vs Habitissimo (leads), vs hiring a empresa. → Playbook §1
2. **V1 scope box**: 3 categories, fixed-price or single bid, Marbella-only, no “Otros” at launch. → see §"Suggested v1 scope" below
3. **Payment policy page**: cancel/refund/chargeback, capture timing, cash fee amount (e.g. €2.99). → Playbook §4
4. **Liquidity playbook**: guaranteed response time, founder-filled jobs week 1–4, referral €X helper/customer. → Playbook §3
5. **Helper funnel metrics**: registered → Connect complete → first job → 30-day active. → Playbook §7
6. **Kill/pivot criteria** at 60 and 90 days. → Playbook §7
7. **Marketing line item** and simple landing page in bootstrap list. → Playbook §3 (web/SEO gap)

---

## Suggested v1 scope (tightened)

Use this to cut scope before writing code (update [BUSINESS_PLAN.md](./BUSINESS_PLAN.md) and [TECH_PLAN.md](./TECH_PLAN.md)):

| In v1 | Defer |
|-------|--------|
| Piscina y jardín | Bortforsling / waste (licence risk) |
| Montaje / manitas | Recados / auction delivery |
| Customer sets budget; helper accepts (no multi-bid war) | “Otros” catch-all |
| Card/Apple Pay default; cash with fixed platform fee | Bizum |
| ES + EN | Admin web beyond Supabase + manual |
| Marbella polygon only | Estepona / Málaga |

**Liquidity targets (90 days):**

- Median time to first offer: **&lt; 4 hours** (subsidize if needed).
- Completion rate: **&gt; 70%** of assigned jobs.
- Helpers with ≥1 job in last 14 days: **≥ 15** before marketing spend scales.

**Kill / pivot (example):**

- If **&lt; 30 completed jobs** in first 90 days after beta → narrow categories further or pivot to B2B (property managers only).
- If **&lt; 50%** of helpers complete Stripe Connect → rethink payout model with lawyer/gestoría.

---

## Bottom line

The plans are a **solid technical and compliance sketch** and correctly warns that ops/legal dominate long-term time. It is **not yet a launch playbook**: competition, MVP cuts, money flows (IVA, refunds, minimum fees), disintermediation, and solo-operator ops depth are the main holes.

**One line:** Winning **liquidity and trust** in a cash-heavy, relationship-driven market—while you carry **legal operator liability**—matters more than Expo + Supabase; payments and categories are where that fight is won or lost.

---

## Related docs

- [PLAN.md](./PLAN.md) — Index
- [BUSINESS_PLAN.md](./BUSINESS_PLAN.md) — Product, legal, launch, ops
- [TECH_PLAN.md](./TECH_PLAN.md) — Stack, schema, build phases
