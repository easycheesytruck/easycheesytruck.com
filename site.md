# Easy Cheesy Site

## Overview

- **Live URL:** easycheesytruck.com
- **Stack:** Static HTML
- **Hosting:** GitHub Pages
- **Repo:** https://github.com/easycheesytruck/easycheesytruck.com

## Branches

- **`main`** — Production. Powers the live site at easycheesytruck.com. Running the redesigned site as of 2026-03-09.
- **`redesign`** — Dev branch. Merged to `main` on 2026-03-09. All future dev work goes here before merging.

## Local Files

- **Local path:** `G:\Shared drives\Easy Cheesy\site-design\`
- This directory corresponds to the `redesign` branch.

## Directory Structure

```
site-design/
├── index.html              - Main landing page
├── menu.html               - Menu page
├── book-us.html            - Booking request page
├── _redirects.html         - Redirect rules
├── CNAME                   - GitHub Pages custom domain config
├── seasonal.csv            - Seasonal schedule (month -> item_id)
├── seasonal_items.csv      - Seasonal item library (item details)
└── assets/
    ├── logo/
    │   ├── toast-logo.png  - Primary nav/brand logo
    │   └── logo.png        - Alternate logo asset
    └── menu/               - Food photography (empty until Mariah delivers shots)
```

## Seasonal Menu System

The seasonal sandwich on `menu.html` is driven by two CSVs — no HTML edits required to rotate it.

- **`seasonal.csv`** — Maps each month (3-letter abbreviation: `jan`-`dec`) to an `item_id`
- **`seasonal_items.csv`** — Item library keyed by alphanumeric slug (e.g. `ciao-bella`), with `name`, `subtitle`, and `description` columns

To rotate the seasonal item:
1. Add a new row to `seasonal_items.csv` with a unique slug
2. Update the relevant month row(s) in `seasonal.csv` to point to the new slug

The page JS fetches both files, looks up the current month, joins on `item_id`, and renders the featured section. Falls back to Ciao Bella if fetch fails (e.g. local `file://` viewing).

## Smoke Test

**Trigger:** Type `site: test form` to run this test.

Uses Playwright (Node.js) to load `https://easycheesytruck.com/book-us.html` in a headless browser and submit the booking form end-to-end.

**What it checks:**
- Page loads (HTTP 200)
- Nav logo (`assets/logo/toast-logo.png`) is present and visible
- All 10 form fields are present: `name`, `email`, `phone`, `eventType`, `headcount`, `budgetRange`, `dateWindow`, `venueAddress`, `notes`, `referrer`
- Form submits and Formspree responds with success (`https://formspree.io/thanks`)

**Test data used:**
- Name: Smoke Test Bot
- Email: chris@easycheesytruck.com
- Phone: 6562170375
- Event type: Corporate lunch
- Headcount: 50
- Budget: $1,000 – $2,000
- Date window: "SMOKE TEST - please ignore"
- Venue: "SMOKE TEST - please ignore"
- Notes: "This is an automated smoke test. Please ignore."
- Referrer: Smoke test

**Note:** This sends a real Formspree submission. Chris will receive the email — it can be ignored/deleted.

**Dependencies:** Node.js + Playwright installed in `/tmp`. If Playwright is missing, run:
```
cd /tmp && npm init -y && npm install playwright && npx playwright install chromium
```

**Last passing:** 2026-03-11

## History

- 2026-03-09: Merged `redesign` → `main`. Deployed redesigned site to production. Fixed broken nav logo (was referencing `logo.png`, changed to `toast-logo.png`). Full mobile-responsive layout pass.
