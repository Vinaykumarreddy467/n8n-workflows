# n8n Workflows

A collection of [n8n](https://n8n.io/) automation workflows. Each workflow lives in its own folder as an n8n JSON export, together with a `README.md` that documents what it does.

**Repository:** <https://github.com/Vinaykumarreddy467/n8n-workflows.git>

## 📁 Repo structure

```
n8n-workflows/
├── README.md                        <- this file (index + change tracker)
├── 01 - Google Maps Email Scraper/
│   ├── README.md                    <- workflow documentation
│   ├── Google Maps Email Scraper.json
│   └── Screenshot From 2026-08-11 10-32-59.png   <- workflow screenshot
├── 02 - Google Tools AI Chat Agent/
│   ├── README.md                    <- workflow documentation
│   ├── Google Tools AI Chat Agent.json
│   └── Screenshot From 2026-08-11 17-00-36.png   <- workflow screenshot
└── 03 - Integration Google Sheets, Groq, Linked API/
    ├── README.md                    <- workflow documentation
    ├── Integration Google Sheets, Groq, Linked API.blueprint.json
    └── Screenshot From 2026-08-13 16-31-35.png   <- workflow screenshot
```

> **Numbering:** folder names are prefixed with `01 -`, `02 -`, … so the workflow order is visible on GitHub. Add new workflows with the next number in sequence.

## 📋 Workflows

| # | Workflow | Description | Status |
|---|----------|-------------|--------|
| 1 | [01 - Google Maps Email Scraper](01%20-%20Google%20Maps%20Email%20Scraper/README.md) | Searches Google Maps for businesses (e.g. IT companies in Bangalore), visits their websites, scrapes email addresses, deduplicates/filters them, and appends them to a Google Sheet. | ✅ Active |
| 2 | [02 - Google Tools AI Chat Agent](02%20-%20Google%20Tools%20AI%20Chat%20Agent/README.md) | AI chat agent powered by Groq (llama-3.3-70b) that can send emails via Gmail, work with Google Calendar, and read/write Google Sheets — all from natural-language chat. | 🧪 Testing |
| 3 | [03 - Integration Google Sheets, Groq, Linked API](03%20-%20Integration%20Google%20Sheets%2C%20Groq%2C%20Linked%20API/README.md) | Make.com scenario blueprint: reads blog post URLs from a Google Sheet, summarizes them with Groq (llama-3.3-70b), publishes a LinkedIn post, and generates an Instagram caption saved back to the sheet. | 🧪 Testing |

## 📸 Workflow previews

### 01 - Google Maps Email Scraper

![Google Maps Email Scraper](01%20-%20Google%20Maps%20Email%20Scraper/Screenshot%20From%202026-08-11%2010-32-59.png)

### 02 - Google Tools AI Chat Agent 

![Google Tools AI Chat Agent](02%20-%20Google%20Tools%20AI%20Chat%20Agent/Screenshot%20From%202026-08-11%2017-00-36.png)

### 03 - Integration Google Sheets, Groq, Linked API

![Integration Google Sheets, Groq, Linked API](03%20-%20Integration%20Google%20Sheets%2C%20Groq%2C%20Linked%20API/Screenshot%20From%202026-08-13%2016-31-35.png)

> New workflows get their screenshot added here too, right below their index entry.

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
| 2026-08-13 | ➕ Added | Added `Integration Google Sheets, Groq, Linked API` — a Make.com scenario blueprint (Google Sheets → Groq summary → LinkedIn post publish + Instagram caption saved back to sheet), its README, and a workflow screenshot. |
| 2026-08-11 | ✏️ Updated | Renumbered workflow folders to `01 - …` / `02 - …` for easy ordering on GitHub, and added a **Workflow previews** section to this README so screenshots render inline. |
| 2026-08-11 | ➕ Added | Added `Google Tools AI Chat Agent` workflow (Groq-powered chat agent with Gmail / Calendar / Sheets tools), its README, and a workflow screenshot. |
| 2026-08-11 | 📝 Documented | Added a Screenshots & images tracking table to the `Google Maps Email Scraper` README (tracks `Screenshot From 2026-08-11 10-32-59.png`). |
| 2026-08-11 | ➕ Added | Initial repo setup: added `Google Maps Email Scraper` workflow, its README, and this master README with a change tracker. |

**Legend:** ➕ Added · ✏️ Updated · 🐛 Fixed · 🔧 Configured · 🗑️ Removed · 📝 Documented

### How to update this tracker

Whenever a change is made to this repo, add a new row at the top of the changelog table:

1. **Date** — today's date (YYYY-MM-DD)
2. **Type** — one of the legend symbols above
3. **Description** — what changed, which workflow, and why

**Images rule:** every image/screenshot in the repo must be listed in the *Screenshots & images* table of its workflow README **and** previewed in the *Workflow previews* section of this file. When you add or remove an image, update both **and** add a changelog row here.

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
