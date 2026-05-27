# Apify Test Config — borderline/indeed-scraper

Date: 2026-05-26  
Actor: `borderline/indeed-scraper`  
Actor URL: https://apify.com/borderline/indeed-scraper  
Purpose: Compare Apify Indeed extraction against Parse Indeed API.

## Observed Input Options

The actor supports two usage modes:

### Option 1 — Traditional Search Parameters

Required:

- Country
- Query

Visible fields:

- Country
- Query
- Location
- Max rows/listings
- Radius
- Remote
- Job level
- Sort
- Job type
- From days

### Option 2 — Start URLs

The actor can accept multiple Indeed URLs. If URLs are provided, normal search-query/filter parameters are ignored.

Visible fields:

- Start URLs
- Max rows per URL

### Output Settings

Visible toggles:

- Save only unique jobs
- Include "View similar jobs"

### Run Options

Visible defaults:

- Maximum cost per run: $1
- Timeout: 3,600 seconds
- Memory: 512 MB
- Build: latest

## Recommended First Test Configuration

Use traditional search parameters for the first test.

- Country: United States (US) - www.indeed.com
- Query: equipment operator
- Location: Montana
- Max rows/listings: 20
- Radius: blank/default
- Remote: blank/default
- Job level: blank/default
- Sort: relevance/default for first test
- Job type: full-time, if available
- From days: 14, if available
- Save only unique jobs: ON, if available
- Include "View similar jobs": OFF for first test if possible
- Maximum cost per run: $1
- Timeout: 3,600 seconds
- Memory: 512 MB

## Why This Configuration

This mirrors the Parse test as closely as possible while limiting cost and avoiding duplicate/noisy output.

The first goal is not maximum recall. The first goal is to verify whether Apify returns clean job rows in one run.

## Success Criteria

A successful output should include rows with:

- job title
- company
- location
- pay/salary, if available
- posted date or relative age
- job type
- snippet or description
- direct job URL
- apply URL, if available
- source/search context

## Failure Criteria

The actor should be rejected or deprioritized if it:

- returns raw HTML/page state instead of rows
- exceeds the cost cap
- cannot limit rows
- returns mostly irrelevant or duplicate jobs
- omits direct URLs
- times out or crashes at this small scale

## Comparison Baseline

Parse Indeed API:

- `search_jobs` found counts, related queries, and job keys but not clean rows.
- `get_job_details` returned useful details but costs too much if called once per job.

Apify wins if this actor returns clean rows directly at a reasonable cost.
