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

### Calendly prefill setup

The last step of `index.html` collects name, email, and phone, then loads
the Calendly widget with `Calendly.initInlineWidget(...)`, prefilling
name and email directly and passing phone via `customAnswers`, keyed by
the custom question's position on the event.

On `s-io-client-acquisition-team-build`, Invitee Questions are currently
ordered:

1. `a1` — "Please share anything that will help prepare for our meeting."
2. `a2` — "Phone Number"

So phone is sent as `customAnswers.a2` in `loadCalendly()`. **If the
question order on this event ever changes in Calendly**, update that key
in `index.html` to match the new position (`a1`, `a3`, etc.) — otherwise
the phone number will silently land in the wrong field.

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
