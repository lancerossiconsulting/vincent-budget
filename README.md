# Vincent's Budget Toolkit

Static GitHub Pages site hosting budgeting tools for Vincent. No build step, no server,
no framework — every page is a self-contained HTML file.

**Live site:** https://lancerossiconsulting.github.io/vincent-budget/

## What's here

| File | What it is |
|---|---|
| `index.html` | Landing page — note to Vincent, links to the tools, starter-family money tips |
| `worksheet.html` | The budget worksheet (targets vs actuals, live totals, "Copy summary" export) |
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
are gone — `localStorage` is per-browser, per-device. Encourage him to send the summary back
once it's filled in; that text is the durable copy.
