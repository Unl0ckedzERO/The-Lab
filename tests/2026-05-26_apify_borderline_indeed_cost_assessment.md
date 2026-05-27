# Apify Test — borderline/indeed-scraper Cost Assessment

Date: 2026-05-26  
Tool: Apify  
Actor: `borderline/indeed-scraper`  
Purpose: Record run costs and interpret cost behavior for Indeed job-search extraction.

## Reported Run Costs

User reported three test costs:

| Test | Query / Setup | Cost | Notes |
|---|---|---:|---|
| Test 1 | `equipment operator`, Montana-style controlled run, max rows 20 | $0.17 | Returned 20 structured job records in the uploaded dataset. |
| Test 2 | `grader operator`, Montana, max rows increased to 100 | $0.03 | Few results; increasing max rows did not materially increase price. |
| Test 3 | `grader operator`, blank location, max rows 100, full-time, from days 14, unique jobs ON, view similar jobs ON, max cost $1 | $0.40 | Broader search; higher cost likely due to blank location and including similar jobs. |

## Cost Interpretation

The actor appears cost-efficient for controlled queries. The effective cost is driven more by discovered/scraped result volume and crawl breadth than by the max rows setting alone.

Key observations:

- Raising max rows does not automatically increase cost if few results exist.
- Blank location appears to broaden the search and increase cost.
- Including `View similar jobs` likely expands the crawl/search surface and increases cost.
- A $1 max-cost cap is useful and should stay enabled for tests.

## Comparison to Parse Indeed

Parse Indeed baseline:

- `search_jobs` calls cost 2 credits/tokens and returned search-state/job-key data, not clean rows.
- `get_job_details` calls cost 2 credits/tokens per selected job and returned useful detail.
- A Parse search plus details for many jobs would scale poorly.

Apify result:

- Test 1 produced 20 structured job records for $0.17.
- This is materially better for sheet-ready extraction than Parse Indeed.

## Recommended Default Settings

For controlled job-search extraction:

- Query: specific lane term such as `equipment operator`, `grader operator`, `highway maintenance`, or `safety specialist`
- Location: use a state, city, or region when possible
- Max rows: 100 is acceptable if max cost is capped
- Job type: full-time when relevant
- From days: 14 for fresh searches
- Save only unique jobs: ON
- Include View similar jobs: OFF by default
- Maximum cost per run: $1 during testing

## When to Use View Similar Jobs

Leave `View similar jobs` OFF for normal job-search extraction.

Use it only for discovery-mode runs when the goal is to find adjacent roles and when higher cost/noise is acceptable.

## Updated Verdict

`borderline/indeed-scraper` is currently the best tested Indeed extraction option.

- Output quality: strong
- Cost: acceptable in controlled runs
- Sheet-readiness: strong
- Risk: cost/noise increases when location is blank or similar jobs are included

## Next Step

Define a standard Apify-to-Google-Sheets field mapping and use this actor as the default Indeed extractor for controlled tests, unless a cheaper actor produces similar quality.
