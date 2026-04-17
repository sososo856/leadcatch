# LeadCatch AI — Error Handling UX
# Purpose: Every failure state, what the user sees, and what happens next
# Date: 2026-04-16

---

## Principle: No Dead Ends

Every error state must:
1. Tell the user what happened in plain language
2. Tell them what to do next (or do it for them)
3. Provide a fallback if the primary action fails

---

## Onboarding Errors

### Lead source connection fails
**What happened:** Webhook URL, API key, or OAuth token for a lead source (Google LSA, Angi, HomeAdvisor, website form) failed validation.
**What user sees:** "We couldn't connect to [Source Name]. This usually means the API key has changed or the permissions need updating."
**What happens next:** Show a step-by-step reconnection guide specific to that source. Offer a "Connect manually" option with a webhook URL they can paste into their lead source. If it still fails, auto-open a support chat with Dan pre-populated with the error details.

### Calendar sync fails
**What happened:** Google Calendar OAuth returned an error or the calendar can't be accessed.
**What user sees:** "We couldn't access your Google Calendar. This usually means the permission expired or was revoked."
**What happens next:** Show a "Reconnect Calendar" button that re-triggers OAuth. If it fails again: "Try logging out of Google in your browser first, then reconnect." Fallback: Alex stops trying to book appointments and instead tells leads "I'll have [Owner Name] call you to schedule" — and sends an instant notification to the contractor.

### Payment fails at signup
**What happened:** Stripe returned a card decline or processing error.
**What user sees:** "Your payment didn't go through. This can happen with new cards or bank restrictions."
**What happens next:** Show "Try Again" button with the card form pre-filled (minus CVV). Suggest using a different card. If payment fails 3 times: "No worries — email dan@leadcatch.homes and we'll get you set up manually." Do not lose the lead.

---

## Messaging Errors

### SMS delivery fails
**What happened:** Twilio returned a delivery failure (invalid number, carrier block, or opt-out).
**What user sees:** Nothing — this is handled silently.
**What happens behind the scenes:**
- If invalid number: mark the lead as "undeliverable" in the dashboard, show it with a warning icon
- If carrier block: retry once via email. If email exists, Alex continues conversation via email
- If opt-out: mark lead as "opted out," stop all messaging, log the opt-out for compliance
- Notify the contractor: "A lead couldn't be reached via text. Here's their number — you may want to try calling directly: [number]"

### AI response generation fails
**What happened:** Claude API returned an error or timed out.
**What user sees:** Nothing — the lead doesn't know.
**What happens behind the scenes:**
- Retry the API call twice with 5-second delays
- If still failing: send a generic fallback response from a pre-written template: "Hey [Name], thanks for reaching out! Let me grab someone on the team to help you out. What's the best time to call?"
- Alert Dan via Slack/text: "Claude API is down for [client]. Fallback messages active."
- Log the failure in the dashboard with timestamp

### Lead responds with something Alex can't handle
**What happened:** Lead sends a message that's outside Alex's training (legal threat, extreme profanity, request for something unrelated to roofing).
**What user sees:** The contractor sees a "Needs Attention" flag on the conversation in their dashboard.
**What happens next:** Alex sends: "I want to make sure I get this right — let me have [Owner Name] reach out to you directly. They'll be in touch shortly." Instant notification sent to the contractor with the full conversation thread. Conversation paused until the contractor takes over or instructs Alex to resume.

---

## Dashboard Errors

### Dashboard won't load
**What happened:** Vercel deployment issue, API timeout, or database connection failure.
**What user sees:** "The dashboard is temporarily unavailable. Your system is still running — Alex is still responding to leads."
**What happens next:** Show a status page link (status.leadcatch.homes or a simple uptime widget). Auto-retry loading every 30 seconds. If down for more than 15 minutes: auto-send an email to all affected clients: "Dashboard is temporarily down. Alex is still working. We're fixing it."

### Metrics show zero when they shouldn't
**What happened:** Data sync lag between Make.com and the dashboard.
**What user sees:** "Your data is syncing. Metrics may take up to 10 minutes to update. Your conversations are not affected."
**What happens next:** Show the last-known metrics with a "Last updated: [timestamp]" label. Auto-refresh every 60 seconds until data populates.

---

## Billing Errors

### Recurring payment fails
**What happened:** Monthly charge was declined (expired card, insufficient funds, bank hold).
**What user sees (email):** "Your LeadCatch payment didn't go through. Your system is still active — we'll retry in 48 hours. Update your card here: [link]"
**What happens next:**
- Retry 1: 48 hours later
- Retry 2: 96 hours later (send second email: "Second notice — update your card to avoid service interruption")
- Retry 3: 7 days later (send final email: "Last attempt. If payment isn't updated by [date], Alex will be paused.")
- Day 10: Pause Alex. Send email: "Alex is paused. Update your card to reactivate instantly: [link]"
- Day 30: Account suspended. Data retained for 90 days per policy.

At no point is the account deleted without explicit action.

### Client disputes a charge
**What happened:** Client initiated a chargeback through their bank.
**What user sees (email):** "We noticed a dispute on your account. We'd love to resolve this directly — please reach out to dan@leadcatch.homes before we respond to your bank."
**What happens next:** Account flagged but not suspended (give benefit of the doubt). Dan personally reaches out within 24 hours. If resolved: drop the dispute. If not: respond to Stripe dispute with usage data.

---

## System-Wide Errors

### Twilio outage
**What happens:** All SMS delivery stops.
**Mitigation:** Alex switches to email-only mode. All clients notified: "SMS delivery is temporarily disrupted due to a carrier issue. Alex is continuing conversations via email. We're working to restore SMS."
**Fallback:** Pre-built Make.com scenario triggers email versions of all active follow-up sequences.

### Make.com outage
**What happens:** Automation orchestration stops — Alex can't receive triggers or send responses.
**Mitigation:** Vercel-hosted webhook queue stores all incoming leads. When Make.com recovers, the queue processes in order. No leads are lost.
**Client notification:** "Our automation system is experiencing delays. All leads are being captured and will receive a response as soon as service is restored (typically within 1-2 hours)."

### Database outage (Supabase)
**What happens:** Dashboard is down, conversation history inaccessible, but Alex can still function in stateless mode.
**Mitigation:** Alex continues operating using Twilio conversation context (last 10 messages). New conversations are logged to a backup flat file until database recovers.
**Client notification:** "Dashboard is temporarily unavailable. Alex is still responding to your leads. No data will be lost."

---

## Error Logging

All errors are logged in Supabase `error_log` table:
- `id` (auto-increment)
- `client_id`
- `error_type` (sms_failure, api_failure, payment_failure, etc.)
- `error_message`
- `severity` (low, medium, high, critical)
- `resolved` (boolean)
- `resolved_at` (timestamp)
- `created_at` (timestamp)

Dan receives a daily error digest email at 7 AM CT. Critical errors trigger an immediate Slack/text notification.
