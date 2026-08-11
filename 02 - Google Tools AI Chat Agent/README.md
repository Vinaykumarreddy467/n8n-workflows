# Google Tools AI Chat Agent

An [n8n](https://n8n.io/) AI Agent (LangChain) workflow that turns a chat conversation into a Google Workspace assistant — it can **send emails via Gmail**, **manage Google Calendar**, and **read/write Google Sheets** based on natural-language requests.

> Workflow file: `Google Tools AI Chat Agent.json` — import it into n8n via **Workflows → ⋯ → Import from File**.

## What it does

1. Listens for incoming chat messages through a webhook (Chat Trigger, streaming responses).
2. Sends the conversation to an AI agent powered by **Groq** (`llama-3.3-70b-versatile`) with a short-term **window memory** (last 10 messages).
3. The agent decides which Google tool to call and fills in the parameters from the conversation:
   - ✉️ **send_email** — send a Gmail message (recipient, subject, body)
   - 📅 **create_calendar_event** — calendar operations on a fixed Google Calendar
   - 📊 **append_sheet_row** — append/update a row in the **Contacts** spreadsheet (`Name` tab), matched on the `email` column
   - 🆕 **Create spreadsheet in Google Sheets** — create a brand-new spreadsheet
4. The agent follows strict rules in its system prompt: never invent values, ask the user for missing details first, use ISO 8601 datetimes, and confirm what it did.

## Workflow flow

```
When chat message received (Chat Trigger, streaming)
        │
        ▼
Google Assistant Agent ───────────────────────────────────┐
   │  language model: Groq Chat Model (llama-3.3-70b)      │
   │  memory:        Simple Memory (window = 10)           │
   │  tools:                                             │
   │     send_email  ───────── Gmail OAuth2  ────────────┐  │
   │     create_calendar_event ── Calendar OAuth2 ──────┐│  │
   │     append_sheet_row ───── Sheets OAuth2 ─────────┐││  │
   │     Create spreadsheet ──── Sheets OAuth2 ────────┘││  │
   └────────────────────────────────────────────────────┴┴──┘
```

## Nodes

| # | Node | Type | Purpose |
|---|------|------|---------|
| 1 | When chat message received | Chat Trigger | Webhook entry point, streaming responses |
| 2 | Google Assistant Agent | AI Agent | Orchestrates the conversation, decides tool calls |
| 3 | Groq Chat Model | Chat Model (Groq) | LLM: `llama-3.3-70b-versatile` |
| 4 | Simple Memory | Memory Buffer Window | Keeps last 10 messages as context |
| 5 | send_email | Gmail Tool | Sends email (to, subject, message) |
| 6 | create_calendar_event | Google Calendar Tool | Calendar operations (see known issues) |
| 7 | append_sheet_row | Google Sheets Tool | Append/update row in Contacts → `Name` tab (match on `email`) |
| 8 | Create spreadsheet in Google Sheets | Google Sheets Tool | Creates a new spreadsheet with one tab |

## Configuration you may need to change

| Item | Node | Notes |
|------|------|-------|
| **System prompt / agent rules** | Google Assistant Agent | Edit the rules, tool list, or assistant personality here |
| **Chat model** | Groq Chat Model | Model name or provider; needs a **Groq API key** credential |
| **Gmail** | send_email | Needs **Gmail OAuth2** credential |
| **Calendar** | create_calendar_event | Needs **Google Calendar OAuth2** credential; calendar ID is hardcoded to a Google group calendar |
| **Sheets** | append_sheet_row / Create spreadsheet | Needs **Google Sheets OAuth2** credential; Contacts spreadsheet ID and `Name` tab are hardcoded |
| **Memory** | Simple Memory | Adjust `contextWindowLength` for longer/shorter conversation context |

## Requirements

- n8n instance with the **LangChain (AI) nodes** enabled
- Credentials: **Groq API**, **Gmail OAuth2**, **Google Calendar OAuth2**, **Google Sheets OAuth2**
- The workflow must be set to **Active** for the chat webhook to receive messages

## Known issues / things to check

- 🔴 **`search_drive` is mentioned in the system prompt but no Google Drive tool node exists** in this workflow. The agent will believe it can search Drive and may hallucinate or fail. Fix: add a Drive search tool node, or remove that line from the prompt.
- 🔴 **`create_calendar_event` parameters look like a calendar *query* (time range: `timeMin`/`timeMax`), not an event *creation*.** Verify the operation in the n8n editor — as exported, it may list events in a time range instead of creating one.
- 🟡 **Workflow is exported as inactive** (`active: false`) — toggle it on in n8n before use.
- 🟡 **Hardcoded targets** — group calendar ID, Contacts spreadsheet ID + `Name` tab, and `email` matching column. Verify these still point where you want.
- 🟡 Credentials in the JSON are only pointers — the importing n8n instance needs its own Google/Groq accounts connected.
- Minor: the "Create spreadsheet" node has no `id` field (n8n regenerates it on import).

## Screenshots & images

| File | Description | Added |
|------|-------------|-------|
| `Screenshot From 2026-08-11 17-00-36.png` | Screenshot of the workflow flow (n8n canvas) | 2026-08-11 |

> Keep this table updated whenever a screenshot or image is added or removed — every image in this folder should be listed here.

## Change log

| Date | Change |
|------|--------|
| 2026-08-11 | Initial import and documentation of the workflow |
