# Scalbl Team Build

Static landing page for the Client Acquisition Team offer — a 3-question
qualification flow that routes visitors to a Calendly booking widget (if
qualified) or a polite decline message (if not).

## Structure

```
.
├── index.html      # The landing/qualification page (entry point)
├── thank-you.html  # Post-booking confirmation page (Calendly redirect target)
├── assets/         # Static assets (images, extra CSS/JS) as the site grows
└── README.md
```

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
