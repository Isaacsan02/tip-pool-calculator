# Tip Pool Calculator

A client-side calculator for restaurant servers who tip out part of their
tips to support staff (food runners, bartenders, bussers, etc.) under
different house rules.

## The problem it solves

Tip-out policies vary by restaurant and are usually a mix of flat
percentages of total tips and percentages of a specific sales category
(e.g. "6% of alcohol sales to the bar"). Doing that math by hand every
shift is error-prone. This tool lets a server set up their restaurant's
rules once, then just enter the shift's numbers to see exactly what gets
tipped out and what they keep.

## Tech stack

- Vanilla HTML, CSS, and JavaScript — no framework, no build step
- Browser `localStorage` for saving tip-out rules between visits
- No backend, no database, no external dependencies

The whole app is a single `index.html` file. That's a deliberate choice:
the problem doesn't need a server, a database, or a JS framework, so it
doesn't have one. It's meant to be genuinely deployable as-is, not an
over-engineered demo.

## Setup

Just open `index.html` in a browser — no install step required.

To serve it locally (useful for testing storage behavior over `http://`
instead of `file://`):

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Persistence

Tip-out rules are auto-saved to the browser's `localStorage` on every
change (debounced) and reloaded automatically on page load. If storage is
unavailable (e.g. private browsing with storage disabled, or quota
exceeded), the app detects this and keeps working in-memory for the
session, with a status message letting the user know changes won't
persist.

## Deploying to Netlify

1. Push this repo to GitHub.
2. In Netlify, click **Add new site → Import an existing project** and
   connect your GitHub account.
3. Select this repository.
4. Build settings: leave the build command empty and set the publish
   directory to `.` (the repo root) — there's nothing to build.
5. Click **Deploy**. Netlify will auto-deploy again on every push to the
   default branch.

## Design notes

This project is intentionally small: one HTML file, no dependencies, no
backend. It's built to actually run in production rather than to
demonstrate tooling it doesn't need.
