# Parse Indeed Addendum — v3 Filtering Test

Date: 2026-06-25
Zone: Incubator
Tool or Source: Parse `indeed.com` subscribed API variant
Test Type: API optimization follow-up
Raw Data Stored: No — sanitized summary only.

## Question

Can include/exclude term filtering improve usable-row quality for `search_jobs_rows_compact_v3` on a national `heavy equipment operator` query?

## Setup

Same as the prior national `heavy equipment operator` v3 test, with include/exclude terms added.

Key inputs:

- query: `heavy equipment operator`
- location: `United States`
- `jt`: `fulltime`
- `fromage`: `14`
- `start`: `0`
- `limit`: `100`
- `maxPages`: `1`
- `dedupe`: `true`
- include terms: heavy-equipment/operator terms such as `heavy equipment`, `equipment operator`, `excavator`, `loader`, `dozer`, `skid steer`, `grader`, `crane`, `roller`, `asphalt`, `operator`
- exclude terms: drift terms such as `superintendent`, `project manager`, `construction manager`, `delivery driver`, `truck driver`, `machine operator`, `cnc`, `warehouse`, `manufacturing`, `engineer`, `sales`, `recruiter`

## Results

Sanitized summary:

- Cost: 5 credits.
- `totalJobCount`: 2,291.
- `uniqueJobsCount`: 2,282.
- `rowsReturned`: 15.
- `observedRowsBeforeDedupe`: 24.
- `observedRowsAfterDedupe`: 24.
- `observedRowsAfterFiltering`: 15.
- `duplicatesRemoved`: 0.
- `detailCallsMade`: 0.
- `externalSearchCallsMade`: 1.
- `fallbackSearchCallsMade`: 0.
- `pagesRequested`: 1.
- `pagesReturned`: 1.
- `requestedLimit`: 100.
- `limitAppearsHonored`: true.

## Evaluation

The filtering test improved visible relevance but reduced result volume sharply.

Compared with the unfiltered national `heavy equipment operator` test:

- Unfiltered: 73 rows for 8 credits.
- Filtered: 15 rows for 5 credits.

Cost per returned filtered row was worse:

- 5 credits / 15 rows ≈ 0.333 credits per returned row.
- ≈ 333 credits per 1,000 returned filtered rows.

However, many obvious drift categories were removed. The filtered output was much more concentrated around crane operators, heavy equipment operators, excavator operators, construction equipment operators, equipment/line operators, and related equipment operation roles.

Important note: the debug reported only 24 rows before filtering, even though the unfiltered test returned 73 rows on a similar query. This may reflect search-result variability, endpoint behavior when filter terms are supplied, or a debug counting difference. Treat this as a useful quality test but not yet a stable cost benchmark.

## Verdict

Filtering is useful for quality control, not cost reduction.

Best current operating pattern:

- Use unfiltered v3 for broad compact discovery and job-key collection.
- Apply filtering downstream in the sheet or local scoring layer when the goal is ranking/relevance.
- Use endpoint-level filters only when the user specifically wants a smaller, cleaner shortlist from Parse.

## Next Step

Do not over-optimize endpoint filtering until the workflow needs it. The current default should remain unfiltered v3 single-page extraction, followed by downstream scoring/filtering.
