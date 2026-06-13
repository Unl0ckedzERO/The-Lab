# Project Instruction With Git Reference

Date created: 2026-05-30  
Updated: 2026-06-13  
Context: The project and repo are now standardized around `The Lab`.

## Recommendation

The project instructions should include a short Git reference line.

The instruction should not over-specify the full repo tree, because the Git structure may evolve. Instead, it should tell the assistant to use The Lab GitHub repo as the durable ledger for sanitized workflows, decisions, tool tests, routing rules, naming rules, and handoff notes.

## Why

Including Git in the project instructions helps ensure that future chats inside the project know:

- a GitHub repo exists
- the repo is a source of durable workflow memory
- logs should be sanitized, not raw dumps
- mature tool tests and decisions should be preserved
- implementation handoffs can reference Git files

## Recommended Instruction Block

This project is an audit lab for evaluating incoming ideas, tools, Reels/posts, workflow opportunities, and data-source/scraper experiments. Keep early intake lightweight and avoid over-building weak ideas. Use routing stages: Inbox, Triage, Evidence Pass, Audit, Route. When an idea passes initial usefulness proof and belongs to an existing project, create a handoff and move implementation there. When an idea is promising but has no project home, move it from Intake to Incubator. Use the GitHub repo `Unl0ckedzERO/The-Lab` as the durable ledger for sanitized workflows, tool tests, decisions, routing rules, naming rules, and project handoff notes. Do not commit raw datasets, signed/private links, API keys, or sensitive metadata; summarize and sanitize before logging. Avoid HVAC/work-realism bias unless explicitly requested. Treat clout as a signal, not validation.

## Shorter Version

Use `Unl0ckedzERO/The-Lab` as the project’s durable Git ledger for sanitized workflows, test summaries, decisions, routing rules, naming rules, and handoff notes. Do not commit raw datasets, signed/private links, API keys, or sensitive metadata.

## Git Reference Rule

Reference GitHub when:

- a tool test produces a reusable finding
- a workflow/routing rule is created or changed
- a naming/project-structure decision is made
- a project handoff is prepared
- an audit framework decision is made
- a cost/output comparison is completed

Do not use GitHub for:

- raw scraper output
- unredacted API responses
- signed URLs
- private credentials
- temporary low-value chat notes

## Historical Note

This repository was originally named `Scraper-Audit`. Older references may use that name, but the current durable repository name is `Unl0ckedzERO/The-Lab`.
