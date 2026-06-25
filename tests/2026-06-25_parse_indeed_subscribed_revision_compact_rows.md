# Parse Indeed Test — Subscribed Revision and Compact Search Rows

Date: 2026-06-25
Zone: Incubator
Tool or Source: Parse `indeed.com` API, subscribed/public variant; Apify `borderline/indeed-scraper` comparison
Test Type: API / Platform Workflow / Cost Comparison
Raw Data Stored: No — only sanitized summary and evaluation are committed.

## Question

Did the Parse `Subscribe` path allow free revisions to the public/subscribed `indeed.com` API variant, and can a revised endpoint return compact Indeed search-result rows without per-job detail calls?

Secondary question: does the result change the Parse vs Apify cost/usefulness decision for Indeed job extraction?

## Setup

- Parse free plan: 200 monthly credits.
- Apify free plan: $5 monthly free usage credit.
- Earlier Parse result: public `search_jobs` v5 returned page-state/search metadata rather than sheet-ready rows.
- A previous revision submitted while an old private fork existed attached to the private fork and consumed credits during build/testing.
- Support clarified: clicking `Subscribe` is the public fork path.
- User deleted earlier Indeed API subscriptions/forks, resubscribed, and confirmed the revision bar appeared on the new subscribed variant.

Controlled revision test:

1. Add static `ping_parse_revision_test` endpoint to subscribed variant.
2. Confirm whether credits changed.
3. Confirm whether private fork was untouched.
4. Add `search_jobs_rows_compact` endpoint to subscribed variant.
5. Run typical test query:
   - query: `equipment operator`
   - location: `Montana`
   - `jt`: `fulltime`
   - `fromage`: `14`
   - `start`: `0`

## Results

### Revision routing / billing

- Static endpoint revision succeeded.
- Credits did not change.
- New endpoint appeared on the subscribed API variant.
- Private fork was unchanged.
- `search_jobs_rows_compact` revision also succeeded and was free.

Interpretation:

- `Subscribe` is Parse's public/subscribed fork path.
- The revision bar is safe when used from the subscribed variant.
- Existing private forks can confuse routing; verify the target variant before revising.
- Revision/build can be free, but endpoint execution still costs credits.

### Compact-row endpoint output

The subscribed `search_jobs_rows_compact` test cost 6 credits and returned compact search-card rows.

Sanitized output summary:

- `totalJobCount`: 56
- `uniqueJobsCount`: 55
- `rowsReturned`: 23
- `jobKeysFound`: 23
- `detailCallsMade`: 0
- `extractionMode`: `search_page_only`
- Useful paths reported:
  - `mosaic-provider-jobcards`
  - `spa.body`
  - `relatedQueries`
  - `totalJobCount`
  - `uniqueJobsCount`
  - `metaData.mosaicProviderJobCardsModel.results`

Returned compact fields included:

| Field | Result | Notes |
|---|---|---|
| `jobkey` | Present | Useful for dedupe and selective enrichment. |
| `title` | Present | Search-card title. |
| `company` | Present | Search-card employer. |
| `location` | Present | Search-card location string. |
| `salary` | Partial | Present when Indeed search card exposes it. |
| `snippet` | Present | Search-card snippet, not full job description. |
| `postedAge` | Present | Relative posting age. |
| `jobUrl` | Present | Indeed viewjob URL built from job key. |
| `source` | Present | `Indeed`. |
| Full description | Missing | Requires richer scrape/detail layer. |
| Apply URL | Missing | Use Apify or detail endpoint when needed. |
| Benefits / attributes / requirements / occupation tags | Missing | Use Apify when needed. |

### Cost estimate

Observed Parse compact-row tests around this endpoint:

| Test | Credits | Rows | Credits / row |
|---|---:|---:|---:|
| Private-fork compact page 1 | 5 | 23 | 0.217 |
| Private-fork compact page 2 | 6 | 15 | 0.400 |
| Private-fork `loader operator` | 6 | 24 | 0.250 |
| Subscribed compact typical test | 6 | 23 | 0.261 |

Rough estimate:

- Observed range: ~0.22–0.40 credits/job.
- Observed average from first three compact tests: ~0.274 credits/job, or ~274 credits per 1,000 compact rows.
- Subscribed typical test: ~0.261 credits/job, or ~261 credits per 1,000 compact rows.

Parse free-tier implication:

- 200 free credits/month ÷ ~0.261–0.274 credits/job ≈ ~730–765 compact rows/month.
- Broad observed range ≈ ~500–920 compact rows/month depending on rows/page.

Apify comparison:

- Apify `borderline/indeed-scraper` verified pricing: ~$0.005/job, or ~$5/1,000 jobs.
- Apify free $5 monthly credit therefore covers roughly 1,000 richer Indeed rows/month.
- Apify records are richer than Parse compact rows.

## Evaluation

What worked:

- Free subscribed/public-variant revision path is confirmed.
- A static endpoint can be added without credit cost.
- `search_jobs_rows_compact` can be migrated to the subscribed variant for free.
- Endpoint execution returns compact rows from search page state without per-job detail calls.
- The endpoint is useful for cheap query testing, job-key collection, dedupe, related-query discovery, and first-pass row previews.

What did not change:

- Running endpoints still costs Parse credits.
- Parse compact rows are lighter than Apify exports.
- Parse paid economics are worse than Apify for bulk Indeed extraction under observed tests.
- The endpoint has some relevance drift because it extracts what Indeed search cards expose.

New possibilities opened by free revisions:

- Maintain a lightweight subscribed Parse API variant with custom helper endpoints.
- Add cheap post-processing endpoints without paying revision/build cost.
- Add endpoint-specific debug fields before spending runtime credits.
- Create source-specific compact row extractors for other marketplace APIs.
- Prototype small workflow endpoints before deciding whether a tool deserves Incubator/Tool Stack adoption.
- Use static `ping` endpoints as safety checks before larger revisions.

Guardrails:

- Always verify the target API variant says subscribed/public, not private fork.
- Start with a static no-external-call endpoint to confirm revision routing.
- Do not commit raw API responses, tokens, internal page state, or unredacted output.
- Treat free revisions as iteration savings, not free runtime.
- Keep Apify as the richer export layer unless Parse becomes materially cheaper or richer.

## Verdict

Tool Stack / Incubator decision:

- Adopt Parse subscribed `search_jobs_rows_compact` as a lightweight compact preview and query-testing layer.
- Keep Apify as the stronger free-tier and paid bulk/rich export layer for Indeed job data.
- Keep the ChatGPT Indeed connector as the default interactive review layer.
- Keep Parse platform behavior on a watchlist because the marketplace, versioning, and revision model are evolving quickly.

## Next Step

Log this result in the manifest and stop further Indeed API testing unless one of these changes occurs:

- Parse support clarifies a cheaper runtime path.
- Parse marketplace exposes a richer Indeed rows endpoint.
- The active Job Search project needs a compact preview export.
- A separate Parse Platform Incubator chat is created for broader marketplace/API tracking.
