# LeadCatch AI — North Star Strategy
# Purpose: North star metric, postmortem template, v2 feature, launch checklist
# Date: 2026-04-16

---

## North Star Metric

**Appointments Booked Per Client Per Week**

This is the one number that matters. Everything else — MRR, response time, conversation volume, NPS — is either an input to this metric or an output of it.

**Why this metric:**
- It directly ties to the client's ROI (appointments = revenue)
- It's what clients talk about to other contractors ("Alex booked me 4 jobs this week")
- It's controllable (we can improve it through better AI, faster response, better follow-up)
- It drives retention (clients who book 2+ appointments/week don't churn)
- It drives referrals (clients who see results tell their peers)

**Target:** 2-4 appointments booked per client per week (Growth plan)

**How to measure:** Supabase query — count of appointments booked per client_id, grouped by week. Dashboard widget shows this to clients.

---

## Month 1 Postmortem Template

Complete this on Day 31. Be honest. The numbers don't lie.

### Headline Metrics

| Metric | Target | Actual | Delta |
|---|---|---|---|
| Paying clients | 3 | | |
| MRR | $5,000+ | | |
| Total demos completed | 15+ | | |
| Demo-to-close rate | 25%+ | | |
| Appointments booked (all clients) | 30+ | | |
| Avg appointments/client/week | 2-4 | | |
| Client NPS (average) | 8+ | | |
| Churn (cancellations) | 0 | | |
| Inbound demo requests | 5+ | | |

### Channel Performance

| Channel | Outreach Volume | Demos Booked | Clients Closed | Cost |
|---|---|---|---|---|
| Google LSA prospecting | | | | $0 (time only) |
| Facebook groups | | | | $0 (time only) |
| LinkedIn outbound | | | | $0 (time only) |
| Referrals | | | | $500/referral |
| Content/SEO | | | | $0 (time only) |

### What Worked

1. _
2. _
3. _

### What Didn't Work

1. _
2. _
3. _

### What I'd Do Differently

1. _
2. _
3. _

### Biggest Surprise

_

### Biggest Lesson

_

### Decision: Double Down / Pivot / Sunset

Based on the kill-switch framework (see ~/leadcatch/launch/kill-switch.md):

- [ ] Green light (2+ targets hit) — Double down
- [ ] Yellow light (1 target hit) — Pivot on: _
- [ ] Red light (0 targets hit) — Sunset

### Month 2 Priority (one sentence)

_

---

## One Feature to Cut from V1 (Save for V2)

**Cut: Multi-Calendar Routing**

Multi-calendar routing (directing leads to different sales reps based on zip code, service type, or round-robin) is a v2 feature. Here's why:

1. **V1 clients are owner-operators.** They don't have multiple sales reps. They have one calendar — theirs. Routing to multiple calendars is a feature for companies with 15+ employees, which isn't our ICP yet.

2. **It adds complexity to onboarding.** Setting up routing rules requires understanding the client's team structure, territories, and scheduling preferences. This slows down the "one onboarding call" promise.

3. **It increases support burden.** "Alex booked the appointment on the wrong calendar" becomes a support ticket category that doesn't need to exist in month 1.

4. **It's a natural upsell.** When a Starter or Growth client grows to the point where they need routing, they'll upgrade to the Scale plan. This feature literally drives plan upgrades.

**When to ship it:** When 3+ clients ask for it, or when we have 5+ Scale plan clients. Whichever comes first.

---

## Launch Readiness Checklist

### Product

- [ ] Alex AI assistant responding to missed calls in under 60 seconds
- [ ] Lead qualification conversation flow tested with 10+ simulated leads
- [ ] Google Calendar integration working (create event, check availability, send confirmation)
- [ ] 7-touch follow-up sequence configured and tested
- [ ] Client dashboard live with real-time metrics
- [ ] SMS delivery tested across 3+ carriers (AT&T, Verizon, T-Mobile)
- [ ] A2P 10DLC registration complete (required for SMS at scale)
- [ ] Fallback behavior tested (what happens when Claude API is down, Twilio is down, calendar sync fails)

### Website

- [ ] Landing page live at leadcatch.homes
- [ ] Lead magnet (Missed Call Revenue Calculator) functional and capturing emails
- [ ] Pricing page with all 3 tiers
- [ ] Legal pages linked in footer (Terms, Privacy, Refund)
- [ ] Mobile responsive — tested on iPhone and Android
- [ ] Email capture form connected to Brevo
- [ ] Referral waitlist mechanic live

### Sales

- [ ] Stripe configured for all plans (Starter, Growth, Scale, Lifetime Deal)
- [ ] Tripwire offer ($27) checkout page live
- [ ] DFY Installation add-on ($497) checkout page live
- [ ] Demo booking link active (Calendly or Cal.com)
- [ ] Pitches memorized (one-sentence, 30-second, 2-minute)
- [ ] Demo script practiced 3+ times
- [ ] Objection handling scripts reviewed

### Email

- [ ] Onboarding sequence (5 emails) loaded in Brevo
- [ ] Abandoned signup sequence (3 emails) loaded in Brevo
- [ ] Win-back sequence (3 emails) loaded in Brevo
- [ ] All email sequences tested with real email delivery

### Outreach

- [ ] 10 cold DM templates ready
- [ ] LinkedIn profile optimized for roofing contractor outreach
- [ ] Facebook groups joined (5 minimum)
- [ ] First 5 build-in-public posts drafted
- [ ] First 100 users plan reviewed and daily targets set

### Launch

- [ ] Product Hunt listing drafted
- [ ] Launch week plan reviewed (5-day day-by-day)
- [ ] Kill switch metrics defined and tracking sheet created
- [ ] Referral mechanic configured
- [ ] Affiliate programme page drafted

### Legal & Compliance

- [ ] Terms of Service published
- [ ] Privacy Policy published
- [ ] Refund Policy published
- [ ] A2P 10DLC registration submitted
- [ ] TCPA compliance reviewed for all messaging

### Monitoring

- [ ] Error logging configured (Supabase error_log table)
- [ ] Uptime monitoring active (status.leadcatch.homes or UptimeRobot)
- [ ] Dan receives critical error alerts via text
- [ ] Daily error digest email configured

---

**When every box is checked, you're ready to launch. Not before.**
