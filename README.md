# Web Title Scraper

A lightweight Apify Actor that crawls one or more websites and collects the **page title** and **URL** of every page it visits. Built with Crawlee and Cheerio for fast, efficient HTML scraping — no headless browser required.

## What it does

Starting from the URLs you provide, the actor:

1. Fetches each page using an HTTP request (no browser overhead).
2. Parses the HTML with Cheerio and extracts the `<title>` tag.
3. Follows links and continues crawling up to the configured limit.
4. Saves every `{ url, title }` pair to a structured Dataset.

The resulting dataset can be exported as JSON, CSV, Excel, or XML directly from the Apify Console.

## Input

| Field | Type | Default | Description |
|---|---|---|---|
| `startUrls` | array | `https://apify.com` | One or more URLs to start crawling from |
| `maxRequestsPerCrawl` | integer | `100` | Hard cap on the total number of pages visited |

### Example input

```json
{
  "startUrls": [
    { "url": "https://example.com" }
  ],
  "maxRequestsPerCrawl": 50
}
```

## Output

Each crawled page produces one record in the dataset:

```json
{
  "url": "https://example.com/about",
  "title": "About Us – Example"
}
```

## Running locally

Make sure you have Node.js 18+ and the Apify CLI installed.

```bash
# Install dependencies
npm install

# Run the actor locally
apify run
```

Results are saved to `./storage/datasets/default/`.

## Deploying to Apify

### Option A — push from your machine

```bash
apify login   # enter your Apify API token when prompted
apify push    # builds and deploys the actor to Apify Platform
```

### Option B — link a Git repository

1. Open Actor creation in the Apify Console.
2. Click **Link Git Repository** and point it at this repo.
3. Every push to the main branch triggers an automatic build.

## Tech stack

| Library | Purpose |
|---|---|
| Apify SDK | Actor lifecycle, input/output, proxy |
| Crawlee | Request queue, crawl orchestration |
| Cheerio | Fast server-side HTML parsing |

## Resources

- Apify Platform docs
- CheerioCrawler API reference
- Web scraping with Cheerio — Apify Blog
- Join the Apify developer community on Discord
