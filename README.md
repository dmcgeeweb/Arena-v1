# National Basketball Arena — website

Static site. No build step. Cloudflare Pages deploys automatically on push to `main`.

## Layout

    index.html            Home — spaces, floor plan, technical spec, enquiry form
    whats-on.html         Public events with ticket links
    calendar.html         Availability calendar, links into the enquiry form
    redevelopment.html    The €38m redevelopment
    404.html              Not found
    data/events.json      SINGLE SOURCE OF TRUTH for all bookings
    images/               Photography and renders
    _headers              Cloudflare security headers and cache rules

## Events

`data/events.json` is the only place bookings are recorded. It is written by
the n8n workflow "National Basketball Arena - publish events", which reads the
bookings Google Sheet every 15 minutes and commits the file if anything changed.

Do not hand-edit it — the next sync will overwrite you. Edit the sheet.

Private bookings are stripped of all detail before the file is written: the
date is published, nothing else. Never point anything at the raw sheet.

The chat assistant reads this same file, so it cannot contradict the calendar.

## Before going live

- [ ] Replace `YOUR-N8N-HOST` in `_headers` (CSP `connect-src`) or the form and chat will be blocked
- [ ] Replace `ARENA-DOMAIN.ie` in `robots.txt` and `sitemap.xml`
- [ ] Set `ENDPOINT` in `index.html` to the enquiry webhook — currently demo mode
- [ ] Replace the placeholder reviews above the enquiry form with real ones
- [ ] Confirm image and render licensing with Basketball Ireland
- [ ] Confirm published capacity: current pages say 2,500; redevelopment takes it to 3,272

## Caching

`/images/*` is cached for a year — change a photo by changing its filename.
`/data/*` is cached for 60 seconds so new bookings appear quickly.
