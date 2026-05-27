# Parse Test — Indeed Marketplace API Pipeline Assessment

Date: 2026-05-26  
Tool: Parse  
API: indeed.com API  
Endpoints tested: `search_jobs`, `get_job_details`  
Credit note: user confirmed each call cost 2 credits/tokens in this test.

## Summary

The Parse Indeed marketplace API is not useful as a simple one-call search-to-sheet extractor, but it may still be useful as a selective lead-discovery layer.

`search_jobs` returned broad search-state data, result counts, related queries, pagination/context, and job keys, but did not return clean job-card rows such as title, company, salary, snippet, and direct job URL.

`get_job_details` returned a usable job record when supplied with a specific job key. The detail response included title, company, location, description, duties, job type, pay, attributes, and posting age.

## Cost Finding

Each endpoint call cost 2 credits/tokens in this test.

Implication:

- 1 `search_jobs` call = 2 credits
- 1 `get_job_details` call = 2 credits
- A search plus details for 10 jobs would cost roughly 22 credits before cleanup

This makes the API too expensive for broad exhaustive extraction unless the search endpoint itself provides enough discovery value.

## Possible Pipeline Use

The API may still be useful as a broad lead-discovery probe:

1. Run one broad or semi-broad `search_jobs` call for a job title, state, or national search.
2. Use result counts, related queries, and job keys as a discovery map.
3. Use the connected Indeed tool or manual Indeed browsing to inspect the best visible results.
4. Call `get_job_details` only for high-priority jobs where the detail record is worth the extra 2-credit cost.

This is especially relevant because a broad typo/location test returned a very large result set, suggesting the API may expose or reveal broader Indeed search surfaces than the current ChatGPT Indeed connector workflow.

## Best Use Cases

Potentially useful for:

- broad market sizing by job title or region
- discovering related search terms
- identifying job keys for selective detail calls
- comparing broad Indeed availability against official/connector-based workflows
- supplementing Mark-style national job searches when used sparingly

Not useful for:

- direct one-call sheet creation
- exhaustive extraction of all jobs
- detail-calling every search result
- low-credit workflows where every call must produce a clean row

## Current Verdict

Selective use candidate, not a primary extraction tool.

- `search_jobs`: weak for clean rows, possibly useful for broad discovery
- `get_job_details`: useful but credit-expensive at scale
- Overall: keep as a research probe, but compare against Apify Indeed and any official/connected Indeed tools before adopting

## Next Tests

1. Test Apify Indeed job actor or equivalent without committing to large spend.
2. Run one controlled broad Parse `search_jobs` query at national or state level and compare discovery quality against the ChatGPT Indeed connector.
3. Determine whether Parse search results expose meaningfully more leads than existing workflows.
4. Only use `get_job_details` on manually selected high-value job keys.
