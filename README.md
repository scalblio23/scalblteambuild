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

Calendly's phone question renders as a country picker (defaults to
**+61**) plus a national-number field, so the prefill has to be the bare
national number — no leading `0`, no country code — or it ends up trying
to combine `+61` with a number that already starts with `0`/`61`.
`toAuNationalNumber()` in `index.html` strips those before sending, e.g.
`0403 214 320` → `403214320`. This assumes Australian numbers; if this
event ever needs to support other countries, that function (and the
picker's default country) will need to change.

### thank-you.html

Shown after a visitor books a call. It reads `event_start_time` /
`event_end_time` off the URL to show the actual booked date/time and
generate working "Add to Calendar" (Google + .ics) links. Without those
params it falls back to a plain "check your email" message and hides the
calendar buttons, rather than showing a broken/guessed time.

**This requires a one-time setting in Calendly** (not available via the
public API — checked; the event type's confirmation-page redirect isn't
an exposed field, so this can't be scripted):

1. In Calendly, open the event type `S.IO - Client Acquisition Team
   Build` → **Confirmation Page** (under event settings).
2. Enable **"Redirect to an external site after scheduled"**, and set the
   URL to `https://scale.scalbl.io/thank-you.html`.
3. Enable **"Add parameters to the confirmation page redirect"** — this
   is what appends `event_start_time`, `event_end_time`,
   `invitee_full_name`, `invitee_email`, etc. as query params.

This redirect fires for the embedded widget on `index.html` too (Calendly
navigates the top-level page, not just the iframe), so the flow stays
booking → automatic redirect → `thank-you.html` with the real time filled
in. Until this is turned on, visitors will see the fallback message
instead of a specific time.

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
