# Scalbl Team Build

Static landing page for the Client Acquisition Team offer — a 3-question
qualification flow, then a name/email/phone step, that routes visitors to
a prefilled Calendly booking widget (if qualified) or a polite decline
message (if not).

## Structure

```
.
├── index.html      # The landing/qualification page (entry point)
├── thank-you.html  # Post-booking confirmation page (Calendly redirect target)
├── assets/         # Static assets (images, extra CSS/JS) as the site grows
└── README.md
```

### Calendly prefill setup (one-time)

The last step of `index.html` collects name, email, and phone, then loads
the Calendly widget with `Calendly.initInlineWidget(...)`, prefilling
name and email directly and passing phone as the event's **first custom
question** (`customAnswers.a1`). For the phone number to actually land in
that field on Calendly's side:

1. In Calendly, open the event type (`s-io-client-acquisition-team-build`)
   → **Invitee Questions**.
2. Make sure a free-text question (e.g. "Phone number") exists as
   **question 1** in that list — that's what `a1` maps to.
3. If it's in a different position, update the key in `loadCalendly()` in
   `index.html` (`a1` → `a2`, etc.) to match.

### thank-you.html

Shown after a visitor books a call. If linked to from Calendly with
"Add params to confirmation page redirect" enabled, it reads
`event_start_time` / `event_end_time` from the URL to show the booked
time and generate working "Add to Calendar" (Google + .ics) links.
Without those params it still renders correctly, just without a
specific time.

## Local preview

This is a single self-contained static HTML file — no build step required.
Open `index.html` directly in a browser, or serve the directory locally:

```
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

## Deploying

Any static host (GitHub Pages, Netlify, Vercel, S3, etc.) can serve this
repo as-is by pointing at `index.html`.
