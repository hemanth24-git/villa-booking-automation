 # Villa Booking Automation

An automated booking system for boutique villas, built with n8n. Two workflows share
one Google Calendar and one Google Sheet, so a booking made through either channel
instantly blocks that date on the other — no double-bookings.

## What it does

**WhatsApp Booking Bot ("Maya")**
A conversational AI agent (GPT-4o-mini) that guests message directly on WhatsApp.
It collects guest name, check-in/check-out dates, and guest count one question at a
time, checks real-time availability against the shared calendar, and confirms or
declines the booking — no human needed for standard bookings.

**Website Booking Engine**
Powers the booking flow on the villa's website: shows live availability with booked
dates greyed out, and handles the booking submission with a server-side conflict
check (if two people try to book the same date at once, the second one is safely
rejected instead of double-booking).

**Review Automation**
Scrapes the villa's Google reviews (via Apify), uses GPT to draft tone-matched reply
suggestions (warm for positive reviews, careful/empathetic for negative ones), and
saves them to a sheet for the owner to review and approve before sending — nothing
sends automatically.

**Missed Call Recovery**
Calls to the villa's number ring the owner's phone through Twilio Voice call
forwarding. If it goes unanswered, busy, or fails, the caller automatically gets a
WhatsApp message from the same number inviting them to continue the conversation
with Maya — so a missed call never means a lost lead.

## Tech stack
- **n8n** — workflow orchestration
- **Google Calendar & Sheets API** — shared source of truth for bookings
- **Twilio (WhatsApp Business API + Voice)** — guest messaging and call forwarding
- **OpenAI GPT-4o-mini** — conversational AI agent + review reply drafting
- **Apify** — Google review scraping

## Files
- `Villa_Booking_System__WhatsApp___Website_.json` — the WhatsApp bot + website
  booking workflow (30 nodes)
- `Villa_Review_Automation.json` — the review scraping + AI reply drafting workflow
- `Villa_Missed_Call_Recovery.json` — the missed-call-to-WhatsApp recovery workflow
  (6 nodes)

## Setup
1. Import both JSON files into your n8n instance
2. Reconnect credentials on every node: Google Calendar OAuth2, Google Sheets OAuth2,
   OpenAI, and Twilio (Basic Auth for HTTP nodes)
3. Point the webhook URLs in your frontend to your n8n instance's webhook addresses
4. Replace placeholder API tokens (Apify) with your own

## Live demo
Website: https://villa-booking-website-xi.vercel.app/