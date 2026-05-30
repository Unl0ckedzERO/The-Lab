# Apify to Google Sheets Field Mapping

Date created: 2026-05-26  
Primary tested actor: `borderline/indeed-scraper`  
Purpose: Standardize how Apify Indeed JSON outputs should be converted into job-search spreadsheet rows.

## Why This Exists

The tested Apify actor returns rich structured JSON that is much more useful than the Parse Indeed marketplace API for sheet-ready job extraction. To make the output reusable across searches, every imported row should preserve both the original job fields and the search/run context.

## Required Traceability Columns

These should be added manually or by import script for every row:

| Sheet Column | Source / Rule |
|---|---|
| Run ID | User-assigned run label, e.g. `APIFY-2026-05-26-01` |
| Run Date | Date the actor was run |
| Source Tool | `Apify` |
| Actor | `borderline/indeed-scraper` |
| Search Mode | `precision` or `discovery` |
| Search Query | Query used in Apify input |
| Search Location | Location used in Apify input; blank if national/discovery |
| From Days | Apify `from days` value |
| Max Rows | Apify max rows setting |
| Similar Jobs Included | Yes/No |
| Save Unique Jobs | Yes/No |
| Run Cost | Actual Apify run cost |

## Core Job Columns

| Sheet Column | JSON Source |
|---|---|
| Job Key | `jobKey` |
| Title | `title` |
| Company | `companyName` if present, else `source` |
| Source / Poster | `source` |
| City | `location.city` |
| State | parse from `location.formattedAddressShort` or `location.fullAddress` |
| Full Location | `location.fullAddress` or `location.formattedAddressLong` |
| Job Type | join `jobType` array |
| Salary Text | `salary.salaryText` |
| Salary Min | `salary.salaryMin` |
| Salary Max | `salary.salaryMax` |
| Salary Type | `salary.salaryType` |
| Date Published | `datePublished` |
| Age | `age` |
| Expired | `expired` |
| Remote | `isRemote` |
| Job URL | `jobUrl` |
| Apply URL | `applyUrl` |
| Company URL | `companyUrl` |
| Company Rating | `rating.rating` |
| Company Rating Count | `rating.count` |

## Enrichment Columns

| Sheet Column | JSON Source / Rule |
|---|---|
| Benefits | join `benefits` array |
| Occupations | join `occupation` array |
| Attributes | join `attributes` array |
| Requirements | join each `requirements[].label` with severity if available |
| Shift / Schedule | join `shiftAndSchedule` array |
| Emails | join `emails` array |
| Corporate Website | `companyLinks.corporateWebsite` |
| Company Description | `companyDescription` or `companyBriefDescription` |
| Description Text | `descriptionText` |

## Recommended Scoring Columns

These are model/user-added fields after import:

| Sheet Column | Purpose |
|---|---|
| Duplicate? | Mark if `jobKey` already exists in sheet |
| Relevance Score | 1-10 fit against the lane/search goal |
| Pay Score | 1-10 based on compensation and pay clarity |
| Location Score | 1-10 based on user/candidate location preference |
| Experience Fit | 1-10 based on qualifications and candidate background |
| Risk / Concern | Short concern note |
| Apply Priority | High / Medium / Low / Reject |
| Lane | e.g. grader, highway maintenance, equipment operator, safety, mechanic, public works |
| Notes | Human/model notes |
| Status | New / Reviewed / Applied / Saved / Rejected |

## Deduplication Rule

Primary dedupe key:

`jobKey`

Secondary duplicate warning:

- Same normalized title
- Same normalized company
- Same city/state
- Similar or identical `jobUrl`

Do not rely on title alone because similar roles may be reposted by the same company in different locations.

## Import Modes

### Precision Mode

Use when importing search results into a real job-search sheet.

Recommended Apify settings:

- Specific state/city/region location
- View similar jobs: OFF
- Max rows: 100
- Save only unique jobs: ON
- Max cost: $1

### Discovery Mode

Use when mapping a national market or finding adjacent lanes.

Recommended Apify settings:

- Blank or broad location
- View similar jobs: ON
- Max rows: 100
- Save only unique jobs: ON
- Max cost: $1
- Always add relevance scoring after import

## Minimum Viable Sheet Columns

If using a smaller sheet, keep at least:

- Run ID
- Search Query
- Search Location
- Run Cost
- Job Key
- Title
- Company
- City
- State
- Salary Text
- Date Published
- Age
- Job Type
- Job URL
- Apply URL
- Description Text
- Attributes
- Relevance Score
- Apply Priority
- Status

## Next Implementation Step

Create a repeatable conversion process:

1. Export Apify results as JSON or CSV.
2. Import into Google Sheets or upload here for transformation.
3. Add traceability columns.
4. Deduplicate by `jobKey`.
5. Score relevance and priority.
6. Preserve raw JSON outside GitHub unless deliberately sanitized.
