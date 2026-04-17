# LeadCatch AI — Feedback System
# Purpose: In-app prompts, survey questions, database schema, auto-tagging
# Date: 2026-04-16

---

## In-App Prompt Design

### Prompt 1: NPS Score (Day 7)
**Trigger:** 7 days after account activation, shown once in the dashboard
**Display:** Bottom-right slide-up card, non-blocking

**Copy:**
"Quick question — on a scale of 0-10, how likely are you to recommend LeadCatch to another contractor?"

[0] [1] [2] [3] [4] [5] [6] [7] [8] [9] [10]

**After selection:**
- 0-6 (Detractor): "Thanks for your honesty. What's the biggest thing we could improve?" [Text input] [Submit]
- 7-8 (Passive): "Thanks! What would make LeadCatch a 10 for you?" [Text input] [Submit]
- 9-10 (Promoter): "Glad to hear it! Would you be willing to leave a quick testimonial?" [Yes, I'd be happy to] [Not right now]

### Prompt 2: Feature Request (Day 14)
**Trigger:** 14 days after activation, shown once in the dashboard
**Display:** Center modal, dismissible

**Copy:**
"You've been using LeadCatch for 2 weeks. If you could add one feature, what would it be?"

[Text input — placeholder: "e.g., CRM integration, Spanish language support..."]
[Submit] [Skip]

### Prompt 3: Post-Support Interaction
**Trigger:** Immediately after a support ticket is resolved
**Display:** Email (not in-app)

**Copy:**
"How was your support experience?"
[Great] [Okay] [Not great]

If "Not great": "Sorry about that. What could we have done better?" [Text input] [Submit]

### Prompt 4: Monthly Check-In (Day 30, then monthly)
**Trigger:** 30 days after activation, then every 30 days
**Display:** Dashboard banner, persistent until dismissed

**Copy:**
"Monthly check-in: Is LeadCatch meeting your expectations?"
[Exceeding expectations] [Meeting expectations] [Falling short]

If "Falling short": Route to a personal email from Dan within 24 hours.

---

## Typeform Survey (Sent via Email at Day 30)

**Title:** LeadCatch 30-Day Feedback
**Estimated time:** 2 minutes

**Q1:** How many extra appointments has LeadCatch booked for you this month? (Approximate is fine.)
- 0
- 1-5
- 6-10
- 11-20
- 20+

**Q2:** How would you rate Alex's conversations with your leads?
- Excellent — sounds like a real team member
- Good — gets the job done
- Okay — sometimes misses the mark
- Poor — leads can tell it's automated

**Q3:** What's been the most valuable part of LeadCatch so far?
- Speed of response (60-second text back)
- Follow-up persistence (7-touch sequence)
- Appointment booking
- Storm Surge
- The fact that I don't have to do anything
- Other: [text input]

**Q4:** What's one thing you wish LeadCatch did better or differently?
[Open text — required]

**Q5:** Would you recommend LeadCatch to another contractor?
- Definitely yes
- Probably yes
- Not sure
- Probably not
- Definitely not

**Q6:** Any other comments, concerns, or ideas?
[Open text — optional]

**Q7:** Can we use your feedback as a testimonial (anonymized or with your name)?
- Yes, use my name
- Yes, but keep it anonymous
- No

---

## Notion Database Schema

**Database name:** LeadCatch Feedback

| Property | Type | Options/Notes |
|---|---|---|
| ID | Auto-increment | Primary key |
| Client Name | Title | From client record |
| Client ID | Text | Links to Supabase client record |
| Feedback Type | Select | NPS, Feature Request, Support Rating, Monthly Check-In, Typeform Survey |
| Source | Select | In-App Prompt, Email, Typeform, Support Ticket, Direct Message |
| Date | Date | Auto-populated |
| NPS Score | Number | 0-10 (only for NPS type) |
| Sentiment | Select | Positive, Neutral, Negative (auto-tagged) |
| Category | Multi-Select | Product Quality, Speed, Pricing, Support, Alex Quality, Feature Gap, Onboarding, Integration |
| Raw Feedback | Long Text | Exact words from the client |
| Summary | Text | 1-sentence summary (auto-generated or manual) |
| Action Required | Checkbox | True if sentiment is Negative or NPS < 7 |
| Action Taken | Long Text | What was done in response |
| Resolved | Checkbox | True when follow-up is complete |
| Resolved Date | Date | When the issue was addressed |
| Priority | Select | Low, Medium, High, Critical |
| Plan | Select | Starter, Growth, Scale, Lifetime |
| MRR | Number | Client's monthly payment (for prioritization) |

---

## Auto-Tagging Rules (Sentiment Detection)

### Sentiment Classification

**Positive (auto-tag when any of these are true):**
- NPS score 9-10
- Contains words: "love," "great," "amazing," "perfect," "thank," "awesome," "excellent," "impressed"
- Monthly check-in response: "Exceeding expectations"
- Typeform Q5: "Definitely yes"

**Neutral (auto-tag when any of these are true):**
- NPS score 7-8
- Contains words: "okay," "fine," "decent," "good enough," "works"
- Monthly check-in response: "Meeting expectations"
- Typeform Q5: "Probably yes" or "Not sure"

**Negative (auto-tag when any of these are true):**
- NPS score 0-6
- Contains words: "terrible," "waste," "cancel," "not working," "disappointed," "frustrated," "broken," "scam"
- Monthly check-in response: "Falling short"
- Typeform Q5: "Probably not" or "Definitely not"
- Support rating: "Not great"

### Category Auto-Tagging

| If feedback contains... | Auto-tag category |
|---|---|
| "Alex," "conversation," "text," "message," "sounds like a bot" | Alex Quality |
| "price," "expensive," "cost," "worth," "ROI," "value" | Pricing |
| "fast," "slow," "response time," "60 seconds," "speed" | Speed |
| "feature," "wish," "should," "missing," "need," "want" | Feature Gap |
| "setup," "onboarding," "install," "connect," "integration" | Onboarding |
| "support," "help," "Dan," "response," "ticket" | Support |
| "booking," "calendar," "appointment," "dashboard," "metrics" | Product Quality |

### Auto-Actions

| Condition | Auto-Action |
|---|---|
| NPS 0-6 | Create high-priority task, notify Dan via text, schedule personal outreach within 24 hours |
| NPS 9-10 | Send referral programme invite + testimonial request email |
| Feature request matches existing roadmap item | Auto-reply: "Great news — this is already on our roadmap for [version]" |
| "Cancel" mentioned in any feedback | Escalate to Dan immediately, trigger win-back pre-emptively |
| Support rating "Not great" | Flag for review, Dan personally follows up |

---

## Feedback Review Cadence

- **Daily:** Dan reviews all new negative feedback and takes action
- **Weekly (Sunday):** Review all feedback from the week, identify patterns, update roadmap priorities
- **Monthly:** Aggregate NPS score, publish internal report, share highlights with clients ("You asked, we built")
