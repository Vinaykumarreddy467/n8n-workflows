# n8n Workflows

A collection of [n8n](https://n8n.io/) automation workflows. Each workflow lives in its own folder as an n8n JSON export, together with a `README.md` that documents what it does.

**Repository:** <https://github.com/Vinaykumarreddy467/n8n-workflows.git>

## 📁 Repo structure

```
n8n-workflows/
├── README.md                        <- this file (index + change tracker)
└── Google Maps Email Scraper/
    ├── README.md                    <- workflow documentation
    ├── Google Maps Email Scraper.json
    └── Screenshot From 2026-08-11 10-32-59.png   <- workflow screenshot
```

## 📋 Workflows

| # | Workflow | Description | Status |
|---|----------|-------------|--------|
| 1 | [Google Maps Email Scraper](Google%20Maps%20Email%20Scraper/README.md) | Searches Google Maps for businesses (e.g. IT companies in Bangalore), visits their websites, scrapes email addresses, deduplicates/filters them, and appends them to a Google Sheet. | ✅ Active |

## 🚀 How to use

1. **Import a workflow**
   - Open your n8n instance → **Workflows** → **⋯** (top-right) → **Import from File** → select the `.json` file of the workflow you want.
2. **Set up credentials**
   - Each workflow may require credentials (e.g. Google Sheets OAuth2). Open the workflow in the editor and connect your own accounts on the credential nodes.
3. **Configure parameters**
   - Check each workflow's README for the parameters you may want to change (search queries, target sheets, etc.).
4. **Activate & run**
   - Toggle the workflow to *Active* for scheduled/trigger-based runs, or use **Execute Workflow** to run it manually.

## 🗂️ Change tracker / Updates

This section is the master log for everything that changes in this repo — new workflows, updates, fixes, notes, and **any files (including images/screenshots)** added or removed.

### Changelog

| Date | Type | Description |
|------|------|-------------|
| 2026-08-11 | 📝 Documented | Added a Screenshots & images tracking table to the `Google Maps Email Scraper` README (tracks `Screenshot From 2026-08-11 10-32-59.png`). |
| 2026-08-11 | ➕ Added | Initial repo setup: added `Google Maps Email Scraper` workflow, its README, and this master README with a change tracker. |

**Legend:** ➕ Added · ✏️ Updated · 🐛 Fixed · 🔧 Configured · 🗑️ Removed · 📝 Documented

### How to update this tracker

Whenever a change is made to this repo, add a new row at the top of the changelog table:

1. **Date** — today's date (YYYY-MM-DD)
2. **Type** — one of the legend symbols above
3. **Description** — what changed, which workflow, and why

**Images rule:** every image/screenshot in the repo must be listed in the *Screenshots & images* table of its workflow README. When you add or remove an image, update that table **and** add a changelog row here.

Example:

```markdown
| 2026-08-12 | ✏️ Updated | Google Maps Email Scraper: changed search query to `restaurants in hyderabad` |
```

## 🛠️ Workflow status legend

| Status | Meaning |
|--------|---------|
| ✅ Active | Ready to use / in use |
| 🧪 Testing | Work-in-progress, being validated |
| ⏸️ Paused | Disabled / on hold |
| 📦 Archived | Kept for reference, no longer used |

## ⚠️ General notes

- Workflow exports are JSON snapshots — always keep them updated whenever you edit a workflow in n8n.
- Scraping workflows interact with third-party sites; review their terms of service and use responsibly.
- Credentials are **never** stored in these files. If you see credential references in a JSON export, they are only pointers to credentials you must set up yourself.
