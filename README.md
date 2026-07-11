# Vincent & Kayla's Budget Toolkit

Static GitHub Pages site hosting budgeting tools for Vincent and Kayla. No build step, no server,
no framework — every page is a self-contained HTML file.

**Live site:** https://lancerossiconsulting.github.io/vincent-budget/

## What's here

| File | What it is |
|---|---|
| `index.html` | Landing page — note to Vincent, links to the tools, starter-family money tips |
| `worksheet.html` | The budget worksheet (targets vs actuals, live totals, "Copy summary" export, plus Download Excel / Download PDF buttons that generate a formula-driven `.xlsx` or a styled PDF of the current month entirely in the browser) |
| `calculator.html` | Finance-it-vs-save-for-it comparison + income-based spending guide |
| `vincent-budget.xlsx` | Excel version of the worksheet, offered as a download |

## Privacy model

The site is public (anyone with the URL can open the pages), but **no personal financial
data lives in this repo or on the site**. Everything Vincent types is stored in his own
browser via `localStorage` (keys `vbud:data:v1` and `vbud:calc:v1`) and never uploaded.
Numbers only leave his device when he taps **Copy summary** and sends the text himself.

## How to update the site

1. Edit the file you want to change (any text editor works — the pages are plain HTML).
2. Commit and push:
   ```
   git add -A
   git commit -m "Describe the change"
   git push
   ```
3. GitHub Pages redeploys automatically — changes appear at the live URL within a minute or two.

### Common edits

- **The note to Vincent** — in `index.html`, look for the comment block
  `LANCE: YOUR NOTE TO VINCENT GOES HERE`. Replace the placeholder paragraphs, delete the comment.
- **Update the Excel file** — overwrite `vincent-budget.xlsx` with the new version, push.
- **Tweak worksheet categories** — the suggested starting rows live in the `DEFAULT` object
  inside `worksheet.html`. (Vincent's saved data overrides these once he's typed anything.)
- **Adjust spending-guide percentages** — the `GUIDE` array inside `calculator.html`.

## "Papa's status light" (Command Deck indicator)

The worksheet contains an **opt-in** status light: when Vincent & Kayla switch on the
"Turn on Papa's status light" toggle, the worksheet publishes their current status —
green / amber / red plus percentages — to a small shared status slot, and Lance's Command
Deck shows it as an indicator in the finance rail. No emails, no messages.

**The plumbing:** the status slot is an anonymous JSON bin
(`https://extendsclass.com/api/json-storage/bin/bfaebdd`) — no account, no verification,
nothing to maintain. The worksheet PUTs a JSON payload there; the deck GETs it. If the bin
ever disappears (free service, kept alive by the deck's regular reads), the deck simply shows
"no report"; recreate with `curl -X POST https://extendsclass.com/api/json-storage/bin -H
"Content-Type: application/json" -d '{}'` and update the URL here and in the deck.

**How it behaves:**
- **Off by default.** Nothing is published unless they turn the toggle on; turning it off stops
  publishing immediately (the deck will show the last report as stale, then "no report").
- **Statuses:** *red* = plan exceeds income or spending has passed income; *amber* = under 5% of
  income unassigned, spending past 95% of income, or savings under 5% of a mostly-complete plan;
  *green* = everything else, and green **is** published so the light recovers after a tight month.
  Status isn't judged at all until income is entered and targets reach at least half of income
  (so a half-filled worksheet doesn't false-alarm).
- **Percentages only.** The payload carries things like "plan uses 97% of income" — never dollar
  amounts, category names, or line items. This is stated verbatim on the worksheet so the kids
  know exactly what's shared.
- **Publish-on-change.** The worksheet only writes when the status payload actually differs from
  the last one it sent (tracked in their browser), so edits don't hammer the slot.
- **Caveat:** the slot's URL is visible in this public repo, so treat the status as
  semi-public — which is why it carries percentages and nothing else.

## Adding the reconciliation tool later

The landing page already has a "Coming soon — Your Reconciliation Tool" card. When the tool
exists:

1. Drop it in as `reconcile.html` (keep it self-contained like the other pages).
2. In `index.html`, convert the coming-soon `<div class="tool soon">` into a link:
   `<a class="tool" href="reconcile.html">`, change its tag class from `soon` to `wks`,
   and update the copy.
3. Push.

## One caution

If Vincent clears his browser data (or uses private browsing), his saved worksheet entries
are gone — `localStorage` is per-browser, per-device. Encourage him to use **Download Excel**
or **Download PDF** at the end of each month; those files are the durable, portable records
(the Excel/PDF libraries load from CDNs on first use, so the export buttons need an internet
connection — everything else works offline once loaded).
