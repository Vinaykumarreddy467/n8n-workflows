# Integration Google Sheets, Groq, Linked API

A [Make.com](https://www.make.com/) (formerly Integromat) **scenario blueprint** that turns a Google Sheet of blog post links into social media content using the **Groq** AI (llama-3.3-70b-versatile) — it summarizes blog posts, publishes a **LinkedIn post**, and generates an **Instagram caption** that gets saved back to the sheet.

> ⚠️ **Important:** this is a **Make.com blueprint**, not an n8n export. Import it into Make.com (Create a new scenario → ⋯ → Import Blueprint) and re-connect the accounts.

> Blueprint file: `Integration Google Sheets, Groq, Linked API.blueprint.json`

## What it does

1. **Reads rows** from a Google Sheet (`Sheet4` of the "email" spreadsheet — the same spreadsheet used by workflow `01 - Google Maps Email Scraper`, different tab). Each row has a **Blog Post URL (column A)** and a **Summary (column B)**.
2. **Groq summarizes the blog** — a chat-completion prompt (llama-3.3-70b-versatile) is told to *"go to this link and get the content from the url and summarize the blog"*.
3. A **BasicRouter** splits the flow into two branches:
   - **Route 1 – LinkedIn:** Groq generates a LinkedIn post from the summary (max ~3000 chars, no emojis, hook → key insights → question → 3 hashtags → closing) and **publishes it** via the LinkedIn API (`visibility: PUBLIC`, `MAIN_FEED`).
   - **Route 2 – Instagram:** Groq generates an Instagram caption from the summary (no emojis, scroll-stopping hook, key points → question → 3 hashtags → reflective close) and **writes it back** into the **Summary (B)** column of the same row in the sheet.

## Scenario flow

```
Google Sheets – filterRows (Sheet4, A1:CZ1, ascending)
        │   Blog Post URL (A) + row number
        ▼
Groq chatCompletion (llama-3.3-70b-versatile)
   "go to this link and summarize the blog"
        │   summary
        ▼
BasicRouter ──┬─► Route 1 – LinkedIn ─────────────────────────────┐
              │      Groq: LinkedIn post prompt (<3000 chars)      │
              │        │  post text                                │
              │        ▼                                           │
              │      LinkedIn: CreatePost (PUBLIC, MAIN_FEED)      │
              │                                                   │
              └─► Route 2 – Instagram ────────────────────────────┤
                     Groq: Instagram caption prompt (no emojis)    │
                       │  caption                                  │
                       ▼                                           │
                     Google Sheets: updateRow → Summary (B) ───────┘
```

## Modules

| # | Module | Type | Purpose |
|---|--------|------|---------|
| 1 | Read rows | `google-sheets:filterRows` | Read Blog Post URL + Summary columns from Sheet4 (range A1:CZ1, headers included, ascending) |
| 2 | Summarize blog | `groq:chatCompletion` | Prompt: fetch URL content & summarize the blog (model: llama-3.3-70b-versatile) |
| 3 | Router | `builtin:BasicRouter` | Branch: LinkedIn publish vs Instagram caption generation |
| 4 | LinkedIn post prompt | `groq:chatCompletion` | Generates the LinkedIn post text (professional tone, insights, hashtags) |
| 5 | Publish to LinkedIn | `linkedin:CreatePost` | Publishes the post (PUBLIC, MAIN_FEED, reshare disabled) |
| 6 | Instagram caption prompt | `groq:chatCompletion` | Generates the Instagram caption (hook, key points, hashtags) |
| 7 | Save caption | `google-sheets:updateRow` | Writes the Instagram caption into Summary (B) of the current row |

## Configuration you may need to change

| Item | Module | Notes |
|------|--------|-------|
| **Spreadsheet / tab** | Read rows + Save caption | Hardcoded to the "email" spreadsheet (`1Ijn7R34abJP5dzLOpqHD5Jy6yo6HPTAxYVh4m7n_HeM`), `Sheet4`, Blog Post (A) / Summary (B) columns |
| **Connections** | All | Blueprint references *Vinay's* connections: Google (`vinaykumarreddy467@gmail.com`), Groq, Groq@2, LinkedIn — re-connect your own accounts after import |
| **AI model** | Groq modules | `llama-3.3-70b-versatile` on all three chat-completion modules |
| **Router logic** | BasicRouter | Which rows go to LinkedIn vs Instagram — check the router rule in Make (not visible in the export) |
| **LinkedIn post visibility** | CreatePost | Set to `PUBLIC`; change to `CONNECTIONS` / `LOGGED_IN` if you prefer |

## Requirements

- A Make.com account (blueprint zone: `eu1.make.com`)
- Connected accounts: **Google**, **Groq** (×2 connections), **LinkedIn**
- A Google Sheet with a **Blog Post** column containing article URLs

## Known issues / things to check

- 🔴 **The "summarize" step cannot actually fetch the URL.** The system prompt tells Groq to *"go to this link and get the content"*, but there is no web-fetch/HTTP module in the scenario and Groq's chat completion cannot browse — it only receives the URL string. Add an HTTP/web-scraper module before the Groq call, or the model may hallucinate or return a generic response.
- 🟡 **Router condition is not in the export** — verify the BasicRouter rules in Make to confirm which rows go to the LinkedIn branch and which to the Instagram branch.
- 🟡 **Scenario settings:** `autoCommit: true`, 1 roundtrip, `maxErrors: 3`, non-sequential execution — check these match your intent (auto-commit means changes are saved automatically).
- 🟡 Same spreadsheet as workflow `01` (Google Maps Email Scraper) — "email" spreadsheet, but a different tab (Sheet4 here vs Sheet6 there); make sure they don't collide.
- 🟡 Name says "Linked API" — assumed to mean "LinkedIn API".

## Screenshots & images

| File | Description | Added |
|------|-------------|-------|
| `Screenshot From 2026-08-13 16-31-35.png` | Screenshot of the scenario flow (Make.com canvas) | 2026-08-13 |

> Keep this table updated whenever a screenshot or image is added or removed — every image in this folder should be listed here.

## Change log

| Date | Change |
|------|--------|
| 2026-08-13 | Initial import and documentation of the scenario blueprint |
