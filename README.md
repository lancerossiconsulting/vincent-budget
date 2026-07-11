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

## "Share check-ins with Papa" (status notifications)

The worksheet contains an **opt-in** notification feature: when Vincent & Kayla switch on the
"Share check-ins with Papa" toggle and their month looks stretched, Lance gets an automatic
email. It is dormant until activated:

**One-time setup (Lance):**
1. Go to https://web3forms.com, enter your email address, and copy the **access key** it sends
   you (no account or password — the key is the whole credential).
2. In `worksheet.html`, find `const NOTIFY_KEY = "";` (marked with a `LANCE:` comment) and paste
   the key between the quotes.
3. Commit and push. The consent toggle appears on the worksheet only after the key is set.

**How it behaves:**
- **Off by default.** Nothing is ever sent unless they turn the toggle on; turning it off stops
  everything immediately.
- **Amber/red only.** Statuses: *red* = plan exceeds income or spending has passed income;
  *amber* = under 5% of income unassigned, spending past 95% of income, or savings under 5% of a
  mostly-complete plan. Green months send nothing. Status isn't judged at all until income is
  entered and targets reach at least half of income (so a half-filled worksheet doesn't false-alarm).
- **Percentages only.** The email says things like "plan uses 97% of income" — it never contains
  dollar amounts, category names, or line items. This is stated verbatim on the worksheet so the
  kids know exactly what's shared.
- **Once per month per status.** A given month/status/flag combination sends one email
  (deduplicated in their browser), so edits don't spam you.
- The email arrives from Web3Forms with subject `Budget check-in: AMBER — <Month Year>` and lists
  the pressure points with a suggestion for each (free tier: 250 emails/month — far more than
  this will ever use).

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
