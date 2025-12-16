# Gmail Invoice Archiver → Google Drive (Vendor-Sorted)

Automated invoice extraction from Gmail into structured Google Drive folders — **no SaaS, no subscriptions, private, fast.**

## What it does

- 📨 Detects invoice emails in Gmail  
- 📎 Saves PDF attachments (Telekom, Vodafone, PayPal, etc.)  
- 🧾 Converts invoice emails **without attachments** to PDF (Apple, Stripe, etc.)  
- 🗂️ Auto-creates vendor folders (amazon, apple, telekom, misc)  
- 🔁 Duplicate-safe  
- ⏱️ Processes in batches (resumes automatically)  
- 📅 Weekly digest email  
- 🔖 Uses a Gmail label (`invoices_exported`) to avoid duplicates & enable re-processing  

> Privacy-first. All processing stays in your Google account.  
> Designed to replace paid invoice-capture SaaS tools without losing control.

---

## Features

| Capability | Status |
|---|---|
PDF attachment extraction | ✅  
Convert email → PDF | ✅  
Auto-vendor folder creation | ✅  
Label-based dedupe | ✅  
Pagination to avoid script timeouts | ✅  
Weekly digest | ✅  
Re-process capability | ✅  
Google Sheets invoice log | 🔜  
UI button panel | 🔜  

---

## Setup (5 minutes)

### 1) Create a Drive folder
- Go to Drive → New → Folder  
- Name it: **Invoices Archive**
- Open it and copy the ID from your browser URL

Example URL (fake):
    https://drive.google.com/drive/folders/1A2B3C4D5E6F7G8H9I0_example_folder_id?usp=sharing

Folder ID (fake):
    1A2B3C4D5E6F7G8H9I0_example_folder_id

Folder location example:

    My Drive
    └─ Invoices Archive
        ├─ amazon/
        ├─ apple/
        ├─ telekom/
        └─ misc/

### 2) Add the script
- Visit: https://script.google.com  
- New project → delete default code → paste `script.gs` content
- At the top of the file, set:

    const ROOT_FOLDER_ID = 'PASTE YOUR DRIVE ID HERE';

### 3) Authorize & test
- Function dropdown → `exportInvoices`
- Click Run ▶
- Allow Gmail + Drive permissions
- Check your Drive folder → you should now see the first exported PDF

### 4) Automation
In Apps Script → Triggers → Add Trigger

| Function | Frequency |
|---|---|
exportInvoices | Hourly or Daily  
weeklyDigest | Weekly (e.g., Mondays 09:00)  

---

## Re-processing invoices

If you want to re-run a time range (example: calendar year 2025):

    unprocessAll('after:2025/01/01 before:2026/01/01');
    exportInvoices();

To re-run **everything** (use with caution):

    unprocessAll();
    exportInvoices();

---

## Output examples

Drive folders created automatically:

    amazon/
    apple/
    telekom/
    misc/

File naming:

    20250115_apple_Your_receipt.pdf  
    20250702_telekom_Rechnung.pdf  

Weekly digest email example:

    Weekly Invoice Digest (Mon, Aug 11 2025)
    • apple: 5
    • amazon: 3
    • telekom: 1

---

## Troubleshooting

- No files?  
  Check: correct folder ID, permissions granted, invoice keywords present

- Folders appear but PDFs don’t  
  Script likely not authorized → run once manually

- Renamed / moved the root Drive folder  
  ✅ Works — ID stays the same (no script update needed)

- Very large inbox  
  Script may need multiple hourly runs to backfill (normal)

---

## Notes

- Runs entirely in Google’s infrastructure (no external servers)
- Uses Gmail search + time-limited batches to avoid hitting quotas
- Designed to be maintainable — no fragile hacks

---

## Roadmap

- Google Sheets invoice metadata log
- UI buttons for non-technical users
- Multi-account setup (central Drive archive)
- Improved HTML→PDF image fidelity

---

## Credits

Built by **Daniel Schnitterbaum**  
Co-piloted by **ChatGPT**  
MIT Licensed

---

## License

See `LICENSE` file in this repo (MIT)
