# Apify Actor Triage Workflow

Date created: 2026-05-26

## Purpose

Use this workflow to sort large Apify Store search results before spending credits or running an Actor.

This was created after searching `indeed` in Apify returned 981 results, which is too broad to evaluate manually without a triage process.

## Rule

Do not run an Actor first. Triage candidates first.

Preferred order:

1. Share the Apify search URL or specific Actor links.
2. Evaluate visible marketplace metadata.
3. Shortlist 3-5 candidate Actors.
4. Inspect pricing, input schema, output example, last update, reviews, and limits.
5. Run only one small test on the best candidate.

## Candidate Scoring Criteria

Score each Actor from 1-5 on:

- Relevance to the target site/workflow
- Clean job-card output
- Cost predictability
- Recent maintenance / last update
- Run count / user trust signal
- Input simplicity
- Output fields available
- Ability to limit results for small tests
- Support for location/title/date filters
- Evidence of direct job URLs

## Red Flags

Avoid or deprioritize Actors that:

- have no sample output
- have unclear pricing
- require broad scraping with no max-results limit
- are stale or abandoned
- return raw HTML instead of structured data
- require credentials unnecessarily
- focus on unrelated Indeed pages, resumes, or company reviews when the goal is job listings
- require expensive proxy/browser runs before proving value

## Ideal Actor Output

For job-search workflows, ideal output should include:

- job_title
- company
- location
- pay / salary
- posted_date or age
- job_type
- snippet / summary
- direct_job_url
- apply_url, if available
- source
- search_query
- search_location

## First Test Query

Use the same query already tested in Parse for comparison:

- query: equipment operator
- location: Montana
- job type: full-time
- posted within: 14 days, if supported
- max results: 10-20

## Comparison Target

Compare Apify against Parse Indeed:

- Parse search_jobs returned useful discovery signals but not clean rows.
- Parse get_job_details returned clean details but costs 2 credits per detail call.
- Apify is worth adopting only if it returns clean rows at a better cost or with better workflow simplicity.

## Verdict Template

- Actor name:
- Actor URL:
- Test run date:
- Input used:
- Cost:
- Rows returned:
- Clean row quality:
- Missing fields:
- Would use again:
- Verdict:
