# Parse Indeed Addendum — v3 Heavy Equipment Operator Test

Date: 2026-06-25
Zone: Incubator
Tool or Source: Parse `indeed.com` subscribed API variant
Test Type: API optimization follow-up
Raw Data Stored: No — sanitized summary only.

## Question

Does narrowing the national query from `equipment operator` to `heavy equipment operator` improve relevance while preserving the high-yield `search_jobs_rows_compact_v3` behavior?

## Setup

Same as the national capacity test, except the query was changed.

Key inputs:

- query: `heavy equipment operator`
- location: `United States`
- `jt`: `fulltime`
- `fromage`: `14`
- `start`: `0`
- `limit`: `100`
- `maxPages`: `1`
- `dedupe`: `true`

## Results

Sanitized summary:

- Cost: 8 credits.
- `totalJobCount`: 2,306.
- `uniqueJobsCount`: 2,301.
- `rowsReturned`: 73.
- `observedRowsBeforeDedupe`: 73.
- `observedRowsAfterDedupe`: 73.
- `observedRowsAfterFiltering`: 73.
- `duplicatesRemoved`: 0.
- `detailCallsMade`: 0.
- `externalSearchCallsMade`: 1.
- `fallbackSearchCallsMade`: 0.
- `pagesRequested`: 1.
- `pagesReturned`: 1.
- `requestedLimit`: 100.
- `limitAppearsHonored`: true.

## Evaluation

Narrowing the query improved both yield and relevance compared with the broader national `equipment operator` test:

- Broader national `equipment operator`: 59 rows for 7 credits.
- Narrower national `heavy equipment operator`: 73 rows for 8 credits.

Cost efficiency was essentially unchanged:

- 8 credits / 73 rows ≈ 0.110 credits per compact row.
- ≈ 110 credits per 1,000 compact rows.
- 200 free credits/month would imply roughly 1,825 compact rows/month if this behavior holds.

The narrower query still produced some drift, including construction superintendent, construction manager, truck driver, and adjacent equipment/operations roles, but the first result set was more concentrated around heavy equipment, crane, excavator, skid steer, utility, farm equipment, and equipment operator roles.

## Verdict

`search_jobs_rows_compact_v3` performs well on a large national query when the query is specific enough. The best current pattern is:

- Use specific query terms rather than broad ones.
- Use `limit=100` for national capacity/relevance probes.
- Use `maxPages=1`.
- Keep `detailCallsMade=0`.
- Use optional filters only to improve usable-row quality, not to reduce runtime cost.

## Next Step

Recommended next useful test: apply include/exclude filters to the same query to measure relevant-row quality improvement, or test a non-equipment query to confirm the endpoint generalizes beyond one job family.
