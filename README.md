# Scalbl Team Build

Static landing page for the Client Acquisition Team offer — a 3-question
qualification flow that routes visitors to a Calendly booking widget (if
qualified) or a polite decline message (if not).

## Structure

```
.
├── index.html      # The landing/qualification page (entry point)
├── assets/         # Static assets (images, extra CSS/JS) as the site grows
└── README.md
```

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
