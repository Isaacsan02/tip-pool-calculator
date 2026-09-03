# Tip Tracker Calculator

A client-side calculator for restaurant servers who tip out part of their
tips to support staff (food runners, bartenders, bussers, etc.) under
different house rules — plus a shift log, so you can see what you actually
kept over a week, a month, or a pay period.

## The problem it solves

Tip-out policies vary by restaurant and are usually a mix of flat
percentages of total tips and percentages of a specific sales category
(e.g. "6% of alcohol sales to the bar"). Doing that math by hand every
shift is error-prone. This tool lets a server set up their restaurant's
rules once, then just enter the shift's numbers to see exactly what gets
tipped out and what they keep — and keeps a history so they can estimate
a pay period before it lands.

## Screens

**Shift** — tap a field row and type on the keypad; the breakdown updates
live. Sales-category rows are generated from your rules. "Save shift to
log" records the shift under today's date.

**History** — a Week / Month / Year switcher, a kept-vs-tipped-out donut, a
month calendar with a dot on every logged shift, and a pay-period estimate
you set by tapping a start and an end date (or with presets that follow
your pay schedule). Below that, averages and your recent shifts.

**Rules** — the tip-out rule editor (recipient, rate, % of total tips or %
of a sales category), your pay schedule, and a button to delete all history.

## Tech stack

- Vanilla HTML, CSS, and JavaScript — no framework, no build step
- Browser `localStorage` for rules, shift history and preferences
- A service worker and web manifest, so it installs to a phone home screen
  and opens offline
- No backend, no database, no external dependencies

The whole app is still a single `index.html`. That's a deliberate choice:
the problem doesn't need a server, a database, or a JS framework, so it
doesn't have one. It's meant to be genuinely deployable as-is, not an
over-engineered demo.

## Responsive layout

One codebase, two layouts. Under 900px it's a phone app: bottom tab bar,
keypad pinned above it. At 900px and up the tabs move to a top nav bar,
content centers at 1000px, and each screen splits into two columns; the
on-screen keypad is hidden and your physical keyboard types into the
selected row. Same colors, type and components either way.

## Setup

Just open `index.html` in a browser — no install step required.

To serve it locally (needed to test the service worker, and useful for
testing storage behavior over `http://` instead of `file://`):

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Persistence

| Key | Holds |
| --- | --- |
| `tip-calc-rules` | your tip-out rules |
| `tip-calc-history` | logged shifts, keyed `YYYY-MM-DD` |
| `tip-calc-prefs` | pay schedule |
| `tip-calc-seeded` | marks that first-run example shifts were added |

Rules are auto-saved on every change (debounced) and reloaded on page load.
Shifts are saved when you press "Save shift to log". If storage is
unavailable (e.g. private browsing with storage disabled, or quota
exceeded), the app detects this and keeps working in-memory for the
session, with a status message letting the user know changes won't persist.
Data loaded from storage is sanitized, so corrupted or hand-edited values
can't crash the renderer.

Nothing leaves the device: no accounts, no sync, no analytics.

## Before a real launch

1. Add `icon-192.png` and `icon-512.png` (referenced by the manifest)
2. Remove the first-run seed block in `index.html` — it adds ten example
   shifts so the History tab isn't empty on a fresh install
3. Bump `CACHE` in `sw.js` on each deploy so clients pick up changes

## Deploying to Netlify

1. Push this repo to GitHub.
2. In Netlify, click **Add new site → Import an existing project** and
   connect your GitHub account.
3. Select this repository.
4. Build settings: leave the build command empty and set the publish
   directory to `.` (the repo root) — there's nothing to build.
5. Click **Deploy**. Netlify will auto-deploy again on every push to the
   default branch.

## Known gaps

- No way yet to edit or delete a single logged shift — tapping a calendar
  day selects a date range rather than opening that shift
- The estimate covers tips only; hourly wages and tax withholding aren't
  included
- History is per-device. Surviving a lost phone means accounts and a backend.

## Design notes

This project is intentionally small: one HTML file, no dependencies, no
backend. It's built to actually run in production rather than to
demonstrate tooling it doesn't need.
