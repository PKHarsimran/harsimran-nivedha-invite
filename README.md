# Harsimran & Nivedha — Introduction & Lunch

Single-page invite site. No build step — `index.html` is the entire site, rendered client-side by a small custom component runtime (`support.js`, generated — see the "do not edit" note at the top of that file). Editing is just editing `index.html` directly.

**Live:** https://harsim.ca/harsimran-nivedha-invite/
**Repo:** https://github.com/PKHarsimran/harsimran-nivedha-invite

## How it's deployed

- Hosted on GitHub Pages from the `main` branch (Settings → Pages). Push to `main` and the live site updates automatically within a minute or two — no CI/build step involved.
- The `harsim.ca` custom domain is **not** owned by this repo. It belongs to `PKHarsimran/PKHarsimran.github.io` (that repo has the actual `CNAME` file). This repo is served at `harsim.ca/harsimran-nivedha-invite/` as a **subpath** of that already-verified domain, via GitHub's own multi-repo verified-domains mechanism. **Don't add a `CNAME` file to this repo** — it would conflict with the domain's real owner rather than reinforce it.
- The `github.io` URL (`https://pkharsimran.github.io/harsimran-nivedha-invite/`) also works and redirects to the `harsim.ca` path.

## Editing content

Everything lives in `index.html`:
- **Date/time/venue**: the countdown target is set in the `<script type="text/x-dc" data-dc-script>` block as `target = Date.UTC(2026, 7, 9, 5, 0, 0)` — this is a UTC instant (10:30 AM IST), not a local time, so the countdown and local-timezone hint stay correct for guests anywhere. The `.ics` calendar data and the Google Calendar link both hardcode the same UTC timestamps separately — if the date/time ever changes, update all three in the same place (search for `20260809`).
- **"Our little story" timeline, menu, venue text**: plain HTML in the template, edit directly.
- **RSVP**: submits to Formspree at the endpoint hardcoded as `FORMSPREE_ENDPOINT` near the top of the script block. Formspree's own dashboard (not just the email notifications) shows a full table of every submission — check there instead of digging through email. **Formspree's free tier caps submissions per month** — if the guest list is large, check the plan's limit before sending the invite out widely.

## Analytics (GA4)

Property ID `G-KCWX2XJD28`, installed via `gtag.js` in the `<head>`. Custom events fired from the page (see `track()` calls in the script block):

| Event | When | Params |
|---|---|---|
| `open_invite` | seal tapped | — |
| `view_section` | a section scrolls into view (first time only) | `section_name` |
| `add_calendar` | calendar link clicked | `calendar_type` (ics/google), `position` (primary/secondary) |
| `click_maps` / `click_photos` | respective link clicked | — |
| `rsvp` | RSVP submitted successfully | `response`, `party_size` |
| `rsvp_error` | validation failed (empty name) | `reason` |
| `rsvp_failed` | Formspree submission actually failed | `response` |
| `change_rsvp` | guest changes their answer | `previous_response` |
| `share_invite` | share button used | `method` (native/clipboard/clipboard_failed) |

No event ever includes the guest's name or other identifying info — intentional, to keep GA4 free of PII.

## Assets

- `og-image.jpg`, `icon-512.png`, `icon-192.png`, `apple-touch-icon.png` — all generated in-browser via `<canvas>` + `toDataURL()`, not hand-designed. If the design needs to change, easiest to regenerate the same way rather than hand-edit the PNGs.
- `qr-album.png` — QR code for the shared Google Drive photo folder, self-hosted so it doesn't depend on a third-party QR-generation API at request time.

## Local preview

No server needed for a quick look — any static file server works, e.g. `python -m http.server` from this directory, then open `http://localhost:8000/index.html`. (`.claude/launch.json` has this pre-configured if using Claude Code's browser preview.)
