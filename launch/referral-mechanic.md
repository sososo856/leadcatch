# LeadCatch AI — Referral Mechanic
# Purpose: Full referral programme design with tracking and triggers
# Date: 2026-04-16

---

## What the Referrer Gets

| Reward | Condition |
|---|---|
| $500 cash (via Stripe payout or Venmo) | Referred contractor signs up and pays first month |
| OR 1 free month of their current plan | Referrer chooses cash or credit at time of payout |
| Bonus: $250 extra | If the referred client stays for 3+ months |

**Cap:** No cap on referrals. A client who refers 10 contractors earns $5,000+.

**Leaderboard bonus:** Top referrer each quarter gets an additional $1,000 or a free month of Scale plan.

---

## What the Referee Gets

| Reward | Condition |
|---|---|
| $300 off first month | Applied automatically when signing up via referral link |
| Priority onboarding | Scheduled within 24 hours instead of standard 48-hour window |
| Extended guarantee | 45-day money-back guarantee instead of 30 (extra trust for a warm lead) |

---

## Referral Link Mechanics

**URL format:** `leadcatch.homes/ref/{client_id}`

**How it works:**
1. Each paying client gets a unique referral link in their dashboard and welcome email
2. When someone visits the referral link, a cookie is set (90-day attribution window)
3. If the visitor signs up, the referral is attributed to the referring client
4. Attribution is tracked in Supabase: `referrals` table with fields: `referrer_id`, `referee_email`, `signup_date`, `first_payment_date`, `status`, `payout_status`

**Referral statuses:**
- `pending` — referee visited the link but hasn't signed up
- `signed_up` — referee created an account but hasn't paid
- `converted` — referee paid first month (triggers $500 payout)
- `retained` — referee stayed 3+ months (triggers $250 bonus)
- `churned` — referee cancelled before 3 months

---

## Waitlist Referral Mechanic (Pre-Launch)

Before the product is live, the referral mechanic is used to gamify the waitlist:

**How it works:**
1. User signs up on the landing page with their email
2. They receive a unique referral link: `leadcatch.homes?ref={waitlist_id}`
3. For every person they refer who also joins the waitlist, they move up in line
4. The landing page shows their position: "You're #47 in line. Share your link to skip ahead."

**Rewards by referral count:**
| Referrals | Reward |
|---|---|
| 1 | Move up 10 spots |
| 3 | Move up 50 spots + get the lead magnet early |
| 5 | Skip to top 20 + get a free DFY Installation ($497 value) |
| 10 | Founding member status + Lifetime Deal at 60% off |

**Tracking:** Waitlist referrals tracked in Supabase `waitlist` table: `email`, `referral_code`, `referred_by`, `referral_count`, `position`, `created_at`

---

## Trigger Points (When to Ask for Referrals)

| Trigger | When | How |
|---|---|---|
| First booked appointment | Day 3-7 | In-app notification: "Alex just booked your first appointment. Know another contractor who could use this?" |
| NPS score 8+ | Day 7 (onboarding email 4) | Email reply: "Glad it's working! We pay $500 for every contractor you refer." |
| 10 appointments booked | Variable (usually week 2-3) | Dashboard banner: "You've booked 10 appointments with LeadCatch. Your referral link is below." |
| Monthly review email | Day 30, then monthly | Section at bottom of performance email: "Share LeadCatch. Earn $500 per referral." |
| After positive support interaction | Anytime | Support follow-up email: "Glad we could help! By the way — referrals earn you $500 cash." |

---

## Anti-Gaming Rules

- Self-referrals are not eligible (same email domain or same Stripe account)
- Referee must be a new LeadCatch customer (never previously signed up)
- Referee must complete first payment to trigger the referral reward
- Referral payouts are processed within 7 business days of the referee's first payment
- If a referee requests a refund within 30 days, the referral reward is clawed back
- LeadCatch reserves the right to modify or cancel the programme with 30 days notice
