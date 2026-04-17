# LeadCatch AI — Changelog v1
# Purpose: Pre-written first 5 changelog updates
# Date: 2026-04-16

---

## v1.0.0 — Launch Day

**Date:** 2026-04-01

LeadCatch AI is live. Here's what's included in the initial release:

**Core Features:**
- Alex AI assistant: responds to missed calls via SMS within 60 seconds
- Lead qualification: automated conversation flow covering damage type, insurance status, urgency, and location
- Appointment booking: direct Google Calendar integration with availability detection
- 7-touch follow-up sequence: automated re-engagement over 14 days for unresponsive leads
- Multi-source support: connects to website forms, Google LSA, Angi, HomeAdvisor, and any webhook-enabled source
- Client dashboard: view conversations, appointments booked, response times, and lead status

**Infrastructure:**
- SMS delivery via Twilio (A2P 10DLC compliant)
- Email delivery via Brevo
- AI conversations powered by Claude API
- Orchestration via Make.com
- Hosted on Vercel

---

## v1.1.0 — Storm Surge

**Date:** Target: 2026-05-01 (not yet built)
<!-- Storm Surge is in development. Do not present as live or available to clients. -->

**New Feature: Storm Surge**
When significant weather events hit your service area, LeadCatch now proactively reaches out to homeowners in affected zip codes. Alex sends a message like: "Hey, this is Alex from [Company] — we noticed some storm activity in your area. If you need a roof inspection, we can get someone out this week."

This puts your business in front of homeowners before they start searching on Google.

**How it works:**
- Weather data monitored via NOAA severe weather alerts
- Zip code targeting based on your service area
- Outreach triggered within 2 hours of a qualifying weather event (hail, high winds, tornado)
- Leads enter the standard qualification flow

**Available on:** Growth and Scale plans only.

---

## v1.2.0 — Review Engine

**Date:** TBD (Target: 2026-05-15)

**New Feature: Automated Google Review Requests**
After a job is marked complete, Alex sends a friendly text asking the homeowner to leave a Google review. The message includes a direct link to your Google Business Profile review page.

**Why this matters:**
- Google reviews directly impact Local Services Ad ranking
- Contractors who ask for reviews within 24 hours of job completion get 3x more reviews
- Alex handles the ask so the contractor doesn't have to remember

**How it works:**
- Contractor marks a job complete in the dashboard (or via webhook from JobNimbus/Jobber)
- Alex waits 4 hours, then sends: "Hey [Name], glad we could help with the roof! If you have a minute, a quick Google review would mean a lot to [Company]. Here's the link: [URL]"
- If no review after 3 days, Alex sends one follow-up

**Available on:** Growth and Scale plans.

---

## v1.3.0 — Multi-Calendar Routing

**Date:** TBD (Target: 2026-06-01)

**New Feature: Route Leads to Multiple Calendars**
For companies with multiple sales reps or service areas, Alex can now route appointments to the right calendar based on:
- Lead's zip code (geographic routing)
- Service type (repair vs. replacement vs. inspection)
- Round-robin (distribute evenly across reps)

**How it works:**
- Configure routing rules in the dashboard
- Alex asks the qualifying questions, then matches the lead to the right rep
- Each rep has their own calendar and gets their own notification

**Available on:** Scale plan only.

---

## v1.4.0 — Dashboard v2

**Date:** TBD (Target: 2026-06-15)

**Improvements:**
- New metrics: lead source breakdown (which source generates the most appointments)
- Conversion funnel: see exactly where leads drop off (contacted > responded > qualified > booked)
- Weekly email digest: automated summary of key metrics sent every Monday morning
- Export to CSV: download all conversation and appointment data
- Mobile-responsive dashboard: full functionality on phone and tablet

**Bug Fixes:**
- Fixed: appointment timezone mismatch for contractors in multiple time zones
- Fixed: duplicate notifications when a lead responds via both SMS and email
- Fixed: Alex occasionally double-booking overlapping time slots
