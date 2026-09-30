# Facebook Pages Scraper

Extract public data from Facebook Pages at scale — posts, engagement metrics, reviews, and page metadata — without logging in or managing proxies.

## What does Facebook Pages Scraper do?

This Actor collects publicly available information from Facebook Pages and returns it as structured JSON, CSV, or Excel. Give it one or more page URLs and it walks the page, expands the post feed, and extracts every field below.

Typical uses:

- Track competitor posting frequency and engagement over time
- Collect reviews and ratings for reputation monitoring
- Build a lead list of local businesses from their Pages
- Feed social content into a dashboard or LLM pipeline

## Features

- Scrapes posts, comments counts, reactions, shares, and timestamps
- Extracts page metadata: category, follower count, address, phone, website, opening hours
- Optional review scraping with rating and reviewer name
- Handles infinite scroll and lazy-loaded content
- Automatic proxy rotation and retry on rate limits
- Incremental runs — set `onlyPostsNewerThan` to fetch just what changed

## Input

The Actor accepts a JSON input object. All fields except `startUrls` are optional.

```json
{
  "startUrls": [
    { "url": "https://www.facebook.com/nasa" },
    { "url": "https://www.facebook.com/natgeo" }
  ],
  "maxPosts": 100,
  "scrapeReviews": true,
  "onlyPostsNewerThan": "2025-01-01",
  "proxyConfiguration": { "useApifyProxy": true }
}

┌────────────────────┬─────────┬─────────────┬─────────────────────────────────────────────────────────┐
│       Field        │  Type   │   Default   │                       Description                       │
────────────────────────────────────────────┤
│ maxPosts           │ integer │ 50          │ Maximum posts per page. Set 0 for page metadata only.   │
├────────────────────┼─────────┼─────────────┼─────────────────────────────────────────────────────────┤
│ scrapeReviews      │ boolean │ false       │ Also collect the page's public reviews.                 │
├────────────────────┼─────────┼─────────────┼─────────────────────────────────────────────────────────┤
│ onlyPostsNewerThan │ string  │ —           │ ISO date. Skips posts older than this.                  │
├────────────────────┼─────────┼─────────────┼─────────────────────────────────────────────────────────┤
│ proxyConfiguration │ object  │ Apify Proxy │ Proxy settings. Residential recommended for large runs. │
└────────────────────┴─────────┴─────────────┴─────────────────────────────────────────────────────────┘

Output

Each post becomes one Dataset item:

{
  "pageName": "NASA",
  "pageUrl": "https://www.facebook.com/nasa",
  "pageCategory": "Government organization",
  "followers": 24100000,
  "postId": "pfbid02x9kLm",
  "postUrl": "https://www.facebook.com/nasa/posts/pfbid02x9kLm",
  "text": "Our Europa Clipper spacecraft has completed its Mars flyby…",
  "publishedAt": "2025-11-14T16:32:00.000Z",
  "reactions": 48213,

}

Results are stored in the Actor's default Dataset and can be exported as JSON, CSV, XML, or Excel from the Apify Console or the API.

How much does it cost?

The Actor runs on pay-per-result. Scraping 1,000 posts typically costs around $2 and takes 3–5 minutes on 2 GB of memory. Runs with scrapeReviews enabled are slower because review sections paginate separately.

Limitations

- Only public Pages are supported. Private groups, personal profiles, and login-gated content are out of scope.
- Facebook caps how far back a public feed can be scrolled; very old posts may be unreachable.
- Reaction breakdowns by type (love, haha, etc.) are not exposed publicly and are not returned.

Is it legal to scrape Facebook?

This Actor only collects data that is publicly visible without authentication. It does not access private content or personal data behind a login. You are responsible for how you use the output — in particular, review GDPR and CCPA obligations before processing anything that identifies an individual.

Support

Found a bug or need a field that isn't extracted? Open an issue on the Actor's Issues tab and it'll be picked up.
