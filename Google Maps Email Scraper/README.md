# Google Maps Email Scraper

An [n8n](https://n8n.io/) workflow that searches Google Maps for businesses, visits their websites, extracts email addresses, deduplicates and filters them, and saves the results to a Google Sheet.

> Workflow file: `Google Maps Email Scraper.json` — import it into n8n via **Workflows → ⋯ → Import from File**.

## What it does

1. Searches Google Maps for a query (default: `it companies bangalore`) and fetches the results HTML.
2. Extracts the base domain URLs of the businesses found in the results.
3. Filters out irrelevant/junk domains (Google-owned, tracking, placeholder domains, etc.).
4. Removes duplicate URLs.
5. Visits each business website (2 URLs at a time, in a batch loop) and fetches the page HTML.
6. Extracts email addresses from each page with a regex that skips image-like extensions (`.png`, `.jpg`, `.gif`, `.jpeg`).
7. Aggregates all collected emails, splits them out into individual items, and removes duplicates.
8. Filters out emails belonging to known junk/irrelevant domains.
9. Appends the final list to a Google Sheet.

## Workflow flow

```
Starts scraper workflow (Execute Workflow Trigger)
        │
        ▼
Search Google Maps with query  ──►  Scrape URLs from results  ──►  Filter irrelevant URLs
        (HTTP GET)                    (Code – extract domains)        (remove Google/junk domains)
                                                                              │
                                                                              ▼
                                                                      Remove Duplicate URLs
                                                                              │
                                                                              ▼
                                                                   Loop over URLs (batch = 2)
                                                                              │
                                                    ┌─────────────────────────┴──────────────────────────┐
                                                    ▼                                                    │
                                       Request web page for URL ─────────────────────────────────────────┘
                                            (HTTP GET per site)                     (loop back)
                                                    │
                                                    ▼
                                                 Loop over pages
                                                    │
                                    ┌───────────────┴────────────────┐
                                    ▼                                │
                          Scrape emails from page ───────────────────┘
                             (Code – email regex)        (loop back)
                                    │
                                    ▼
                        Aggregate arrays of emails
                                    │
                                    ▼
                    Split out into default data structure
                                    │
                                    ▼
                          Remove duplicate emails
                                    │
                                    ▼
                          Filter irrelevant emails
                                    │
                                    ▼
                    Save emails to Google Sheet (append)
```

## Nodes

| # | Node | Type | Purpose |
|---|------|------|---------|
| 1 | Starts scraper workflow | Execute Workflow Trigger | Manual / parent-workflow trigger |
| 2 | Search Google Maps with query | HTTP Request | GET the Google Maps search URL |
| 3 | Scrape URLs from results | Code | Regex-extract base domains from the HTML |
| 4 | Filter irrelevant URLs | Filter | Drop Google/junk/placeholder domains |
| 5 | Remove Duplicate URLs | Remove Duplicates | Deduplicate the URL list |
| 6 | Loop over URLs | Split In Batches | Outer loop, batch size 2 |
| 7 | Request web page for URL | HTTP Request | Fetch each business website |
| 8 | Loop over pages | Split In Batches | Inner loop over fetched pages |
| 9 | Scrape emails from page | Code | Extract emails (skips image extensions) |
| 10 | Aggregate arrays of emails | Aggregate | Merge all email lists (`mergeLists`) |
| 11 | Split out into default data structure | Split Out | One item per email (`emails` field) |
| 12 | Remove duplicate emails | Remove Duplicates | Deduplicate on `emails` |
| 13 | Filter irrelevant emails | Filter | Drop known junk email domains |
| 14 | Save emails to Google Sheet | Google Sheets | Append to target sheet |

## Configuration you may need to change

| Item | Node | Notes |
|------|------|-------|
| **Search query** | Search Google Maps with query | URL is hardcoded: `https://www.google.com/maps/search/it+companies+bangalore` — edit the `url` parameter to search something else |
| **Google Sheet** | Save emails to Google Sheet | Target spreadsheet ID and sheet (`Sheet6`) are hardcoded — point these at your own sheet |
| **Google Sheets credentials** | Save emails to Google Sheet | Uses an OAuth2 credential named *"Google Sheets account"* — you must have your own Google account connected in n8n |

## Requirements

- n8n instance (self-hosted or cloud)
- A connected **Google Sheets OAuth2** credential
- Outbound internet access (Google Maps + business websites)

## Notes & limitations

- This uses **unofficial scraping of Google Maps** search results. Google may block, rate-limit, or change the HTML structure at any time; use responsibly and check the [terms of service](https://policies.google.com/terms).
- The email regex captures only `mailto`-style / plain-text email patterns present in the raw page HTML; it does not click through links or run JavaScript.
- Businesses without a website, or with the email loaded dynamically via JS, will not be captured.
- The `Filter irrelevant URLs` / `Filter irrelevant emails` steps keep out domains like `google.com`, `gstatic.com`, `ggpht.com`, `schema.org`, `example.com`, and Sentry error-tracking domains.
- Error outputs are set to `continue` on several nodes, so a failed page fetch or empty scrape does not stop the whole run.

## Screenshots & images

| File | Description | Added |
|------|-------------|-------|
| `Screenshot From 2026-08-11 10-32-59.png` | Screenshot of the workflow (likely the n8n canvas) | 2026-08-11 |

> Keep this table updated whenever a screenshot or image is added or removed — every image in this folder should be listed here.

## Change log

| Date | Change |
|------|--------|
| 2026-08-11 | Initial import and documentation of the workflow |
