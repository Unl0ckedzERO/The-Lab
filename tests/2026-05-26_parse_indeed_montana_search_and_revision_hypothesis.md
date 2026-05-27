# Parse Test — Indeed Marketplace API / Montana Search + Revision Hypothesis

Date: 2026-05-26  
Tool: Parse  
API: indeed.com API  
Endpoint tested: `search_jobs`  
Params:
- query: equipment operator
- location: Montana
- jt: fulltime
- fromage: 14
- start: 0

Credit note: user previously confirmed endpoint calls in this API cost 2 credits/tokens each during this test series.

## Montana Search Result

The Montana search returned a successful response and confirmed that the query localized correctly to Montana.

Useful extracted search-level data:

- Page title: `Equipment Operator Jobs, Employment in Montana | Indeed`
- Search heading: `equipment operator jobs in Montana`
- totalJobCount: 57
- uniqueJobsCount: 56
- pagination detected: 3 pages shown (`start=0`, `start=10`, `start=20`)
- job keys detected: 21 visible/two-pane-eligible job keys in the response
- auto-open job key: `91736baf390ec735`
- related query suggestions captured:
  - heavy equipment operator
  - excavator operator
  - heavy equipment
  - operator
  - loader operator
  - skid steer operator
  - construction
  - dozer operator
  - excavation
  - crane operator

## Evaluation

The Montana `search_jobs` call is useful for broad discovery, market sizing, job-key discovery, and related-query expansion, but it still does not return clean job-card rows.

Missing fields from the search response:

- job title
- company
- salary
- clean snippet
- direct job URL
- posted date

## Revised Pipeline Interpretation

The API may be useful as a discovery layer rather than a direct sheet extractor:

1. Use one `search_jobs` call to estimate opportunity volume and collect job keys.
2. Use related-query suggestions to expand search terms.
3. Use manual/connected Indeed review to inspect visible best results when possible.
4. Use `get_job_details` only for selected high-value job keys.
5. Avoid paginating or detail-calling every job unless the cost is justified.

## Revision Hypothesis

A revision to the Parse Indeed API could potentially improve the workflow if it creates a new endpoint that returns clean job cards directly from search results, or a combined endpoint that:

1. Performs `search_jobs`.
2. Extracts visible job keys.
3. Calls/captures the underlying job detail data for the first N results.
4. Returns a clean array of rows with:
   - job title
   - company
   - location
   - pay
   - posted date
   - snippet/summary
   - direct URL
   - job key
   - source

Potential endpoint name:

`search_jobs_detailed`

Potential inputs:

- query
- location
- jt
- fromage
- start
- max_results

Potential output:

A normalized array of job-card records suitable for a Google Sheet.

## Revision Caution

A private revision may cost credits and may still produce high per-call costs if the endpoint internally performs search plus multiple detail operations. A public/canonical contribution path may be cheaper if Parse allows the revision to flow back to the marketplace, but this must be confirmed in the UI before spending credits.

Revision should not be attempted until Apify Indeed or another marketplace alternative is tested.

## Verdict

Selective discovery candidate.

- Search endpoint: useful for volume/key discovery, not rows
- Detail endpoint: useful but too expensive at scale
- Best current role: one-call market/lead probe
- Next comparison needed: Apify Indeed job actor or another jobs scraper with cleaner row output
