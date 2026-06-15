# Apify Indeed Test — Paid Export Layer Evaluation

Date: 2026-06-14  
Zone: Incubator  
Tool or Source: Apify `borderline/indeed-scraper`  
Test Type: Scraper / API / Job Search Export  
Raw Data Stored: No — only sanitized summary and evaluation are committed.

## Question

Can Apify's Indeed scraper serve as the paid bulk-export layer for job-search workflows when the ChatGPT Indeed connector is useful for interactive review but exposes only a limited visible result set at a time?

## Setup

Evidence reviewed:

- Apify actor documentation / UI tabs supplied by user.
- Prior small smoke test: 5 returned jobs, observed cost about `$0.025`, rounded in discussion to about `$0.03`.
- Larger scrape: 100 returned jobs, observed cost `$0.50`.
- Controlled Montana scrape: 25 returned jobs from a focused operator-style search.
- Export UI options: JSON, CSV, XML, Excel, HTML Table, RSS, JSONL; view options included Jobs overview, Company overview, and All fields.

Sanitization:

- Raw datasets were not committed.
- Raw Indeed response payloads, private/signed links, API keys, credentials, and sensitive metadata are excluded.
- This note preserves only cost model, output coverage, workflow implications, and routing decisions.

## Actor Capability Notes

The actor supports two useful input modes:

1. Search configuration mode
   - `country`
   - `query`
   - `location`
   - `maxRows`
   - `radius`
   - `jobType`
   - `level`
   - `sort`
   - `fromDays`
   - `remote`
   - `enableUniqueJobs`
   - `includeSimilarJobs`

2. URL-based mode
   - `urls`
   - `maxRowsPerUrl`

URL mode may be useful later for carefully constructed Indeed search URLs or company job pages, but it has not yet been tested as a replacement for normal search configuration.

## Pricing / Cost Model

The observed pricing matches the advertised model:

| Run Type | Rows Returned | Observed / Expected Cost | Notes |
|---|---:|---:|---|
| Smoke test | 5 | `$0.025` observed, rounded to `$0.03` | Cheap proof that the actor works. |
| Larger scrape | 100 | `$0.50` observed | Confirms `$0.005` per returned job. |
| Controlled Montana scrape | 25 | about `$0.125` expected | Useful low-cost lane test. |
| Projected export | 1,000 | `$5.00` | Standard advertised pricing. |
| Large sweep | 10,000 | `$50.00` | Requires strong justification and filtering first. |

Conclusion: Apify is not unexpectedly expensive, but it is a paid scaling decision. The cost is predictable and linear enough for controlled exports.

## 100-Row Dataset Coverage

The 100-row scrape returned structured job records with strong field coverage:

| Field / Output | Result | Notes |
|---|---:|---|
| Unique job keys | 100 / 100 | Strong for dedupe and URL construction. |
| Job title | 100 / 100 | Present for every record. |
| Company name | 98 / 100 | Strong, but not perfect. |
| Location object | 100 / 100 | Included city/state/geodata-style structure. |
| Description text | 100 / 100 | Strong advantage over search-only snippets. |
| Date published / age | 100 / 100 | Useful for freshness filters. |
| Job URL | 100 / 100 | Direct Indeed job URL available. |
| Apply URL | 98 / 100 | Strong application-link coverage. |
| Salary object | 72 / 100 | Good but incomplete; salary still needs null handling. |
| Benefits | 71 / 100 | Useful for scoring but incomplete. |
| Occupation tags | 100 / 100 | Useful for grouping and relevance checks. |
| Attributes | 100 / 100 | Useful for skill/requirement extraction. |
| Requirements | 28 / 100 | Sparse; do not rely on this alone. |
| Rating | 98 / 100 | Useful but should not dominate scoring. |

## 25-Row Controlled Montana Dataset Coverage

The 25-row focused scrape returned 25 unique structured records.

| Field / Output | Result | Notes |
|---|---:|---|
| Unique job keys | 25 / 25 | Strong. |
| Job title | 25 / 25 | Strong. |
| Location object | 25 / 25 | Strong. |
| Description text | 25 / 25 | Strong. |
| Date published / age | 25 / 25 | Strong. |
| Job URL | 25 / 25 | Strong. |
| Apply URL | 24 / 25 | Strong. |
| Salary object | 21 / 25 | Strong for this run. |
| Benefits | 16 / 25 | Partial. |
| Company name | 22 / 25 | Partial; use `source` as fallback if necessary. |
| Occupation tags | 24 / 25 | Strong. |
| Attributes | 25 / 25 | Strong. |
| Requirements | 12 / 25 | Partial. |
| Rating | 22 / 25 | Partial. |

## Quality Notes

What worked:

- Returned rich row-level records in bulk.
- Included full `descriptionText`, not just snippets.
- Included job keys and direct Indeed job URLs.
- Included apply URLs for most rows.
- Included salary data for many rows.
- Included useful metadata for scoring: benefits, attributes, occupations, requirements, company info, date published, age, expired status, and remote status.
- Cost scaled predictably at `$0.005` per returned job.

Cautions:

- Search relevance drift still occurs. Example drift categories include adjacent truck driving, warehouse, general labor, ski-lift construction, plant/mill operator, foreman, and public works maintenance roles.
- Company names can be missing; fallback to source may be needed.
- Salary is not universal.
- Requirements are sparse and should not be treated as complete.
- Very broad state/national sweeps can become expensive quickly.
- Actor output is useful enough to require a scoring/filtering pass after export.

## Export Format Recommendation

For ongoing job-search review:

- Use `Excel` or `CSV` with `Jobs overview` for manual sorting and Google Sheets workflows.
- Use `JSON` with `All fields` for deep audit, field mapping, and parser design.
- Use `JSONL` with `All fields` if building a repeatable parser or line-by-line processing pipeline later.
- Use `Company overview` only for company prospecting, not job-row scoring.

Suggested clean sheet fields:

- `jobKey`
- `title`
- `companyName`
- `location.formattedAddressShort`
- `salary.salaryText`
- `jobType`
- `datePublished`
- `age`
- `jobUrl`
- `applyUrl`
- `descriptionText`
- `benefits`
- `occupation`
- `attributes`
- `requirements`
- `rating.rating`
- `rating.count`
- `source`
- `isRemote`
- `expired`

Fields to omit from routine review sheets unless specifically needed:

- `descriptionHtml`
- logo/header image URLs
- CEO photo URLs
- contacts
- shifts
- socialInsurance
- workingSystem
- large company-link objects
- raw emails

## Comparison to ChatGPT Indeed Connector

The ChatGPT Indeed connector remains the best no-extra-cost interactive job-search tool:

- useful for live searches,
- fit review,
- conversational triage,
- company/job comparison,
- application preparation.

However, connector-visible batches are limited. In prior testing, the connector could report hundreds of available jobs for a broad query but expose a smaller visible set at once. Apify solves the row-export problem when a search lane has proven worth paying for.

## Comparison to Parse Indeed

Parse remains useful for discovery, counts, related queries, pagination, and job-key discovery.

Parse is not currently cost-competitive for bulk detail extraction because the working fork appears to charge as:

```text
search cost + per-job detail cost
```

Apify is therefore the stronger current paid export layer, while Parse is better treated as a discovery or selective-enrichment probe unless a lower-cost batch detail path is proven.

## Verdict

Route: Tool Stack / Incubator-proven paid export layer.

Apify `borderline/indeed-scraper` should be treated as the current best paid bulk row extraction option for Indeed jobs. It should not replace the Indeed connector for normal interactive job search, but it is useful when a lane needs structured export beyond the connector's visible result window.

Adoption rule:

```text
Use the Indeed connector first for interactive search and fit review.
Use Parse for cheap-ish discovery/counts/query expansion when useful.
Use Apify only after a lane is worth exporting or comparing at scale.
```

## Next Step

Create a lightweight job-search export mapping or scoring template only when an active job-search project needs it. Do not run large Apify sweeps casually; use smaller 25-100 row tests to validate lane quality first.
