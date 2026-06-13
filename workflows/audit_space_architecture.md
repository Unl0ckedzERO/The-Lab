# Audit Space Architecture

Date created: 2026-05-30  
Updated: 2026-06-13  
Scope: How to separate messy intake, maturing ideas, tool/workflow experiments, and meta-system design inside The Lab.

## Core Principle

Intake spaces should stay messy, but maturing ideas should not stay in intake forever.

The system should allow random inputs to arrive freely while giving validated ideas a cleaner place to develop.

## Problem Observed

The original audit chat began as an Instagram/Reels business opportunity intake space. It then evolved into:

- business idea audit
- tool audit
- API/scraper research
- Apify/Parse/Firecrawl testing
- Indeed job-search workflow design
- Google Sheets implementation planning
- meta discussion about the audit system itself

This was productive, but it created scope blending.

## Was the API / Indeed Research Clutter?

Yes and no.

It was productive clutter because it produced a validated tool workflow with real outputs, costs, and project value.

It was also structural clutter because it shifted the chat away from its original intake purpose and became implementation work for the job-search project.

Conclusion: this type of branch should be allowed to happen temporarily, but once it matures, it should be moved out.

## Final Zone Model

Use four zones:

### 1. Intake

Purpose:

- receive raw reels/posts/tools/ideas
- perform first-pass usefulness audits
- capture weak signals and hype patterns

Properties:

- intentionally messy
- low friction
- no heavy implementation
- no long-term project ownership

Default chat:

`Intake — Reels, Tools, Ideas`

### 2. Incubator

Purpose:

- develop ideas that passed initial audit but do not yet belong to an existing project
- run small tests
- compare tools
- refine a hypothesis

Examples:

- testing a new tool category
- building a repeatable audit method
- exploring a business concept that is promising but not ready for its own project

Default chat:

`Incubator — Validated Ideas + Tool Tests`

### 3. Project Zone

Purpose:

- implement ideas with an existing destination
- maintain durable workflows, sheets, repos, or artifacts

Examples:

- job-search implementation -> Job Search project
- trading strategy testing -> Trading project
- resume workflows -> relevant resume project
- AI course/course mirror work -> AI course project

### 4. Framework

Purpose:

- discuss and improve The Lab itself
- make decisions about chat/project architecture
- define routing rules and thresholds

This prevents framework discussion from cluttering raw intake.

Default chat:

`Framework — Audit System Design`

### 5. Archive

Purpose:

- preserve historical mixed context
- reference prior decisions and tests
- avoid using old mixed chats as clean future intake

Default chat:

`Archive — Audit Lab v0 / Mixed Chat`

## Routing Thresholds

Move an item out of Intake when at least two of these are true:

- repeated signal appears across multiple posts/sources
- real workflow usefulness is identified
- a small test has succeeded
- user interest remains after initial audit
- implementation would take multiple turns or artifacts
- evidence gathering becomes non-trivial
- the idea/tool could affect an existing project
- it needs tracking, scoring, or versioning

Move an item from Incubator to a Project when:

- there is a clear project home
- it needs durable implementation
- it has a sheet/repo/artifact/workflow
- it will be used repeatedly
- future work should not be mixed with unrelated intake

Archive or pattern-bank an item when:

- it is weak alone but may help identify a larger trend
- it is hype-heavy without current actionability
- it duplicates an already-known idea
- it has no clear next test

## Future Random Inputs Affecting Mature Ideas

Random future reels should still be allowed to influence mature ideas, but through routing rather than direct blending.

Use cross-feed notes:

- New item arrives in Intake.
- If it relates to an incubated idea/project, create a short handoff note.
- Add the note to the Incubator or Project space.
- Do not reopen broad implementation work inside Intake unless necessary.

## GitHub's Role

Git helps, but it does not fully solve chat clutter.

Git is good for:

- sanitized evidence logs
- test results
- routing rules
- reusable workflows
- handoff packages
- decisions and revisions

Git is not enough for:

- conversational focus
- reducing cognitive load
- preserving clean project context
- preventing unrelated branches from influencing each other

Therefore, Git should support the architecture, not replace separate chats/projects.

## Naming Standard

Project:

`The Lab`

Repo:

`Unl0ckedzERO/The-Lab`

Core chats:

- `Intake — Reels, Tools, Ideas`
- `Incubator — Validated Ideas + Tool Tests`
- `Framework — Audit System Design`
- `Archive — Audit Lab v0 / Mixed Chat`

## Current Recommendation

Keep the current mixed chat as a historical archive because it already contains useful context.

Create a cleaner future intake space for new reels/posts/tools.

Move the Apify-to-Sheets work into the Job Search project.

Use a separate Framework chat for questions about The Lab architecture itself.

## Practical Operating Rule

Every mature thread should answer:

1. Is this still Intake?
2. Is this an Incubator branch?
3. Does it belong to an existing Project?
4. Does it need its own project/chat?
5. Should it be archived as a pattern?

When the answer changes, move the thread.
