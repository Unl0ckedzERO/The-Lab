# Framework Rule — Pattern Synthesis Trigger and Cross-Lane Sweep

Date: 2026-10-12
Type: Framework / Intake Rule
Status: Adopted

## Purpose

This rule makes explicit a behavior that The Lab had already been using implicitly: when multiple incoming ideas, tools, Reels, repos, or workflows point toward the same underlying concept, Intake should pause and synthesize the lane rather than only auditing the newest item in isolation.

The rule has two parts:

1. **In-chat Pattern Synthesis Trigger** — the primary mechanism, used while Intake is actively evaluating a related item.
2. **Scheduled Cross-Lane Synthesis Sweep** — a backup and creative recombination pass for non-obvious connections that may not surface during normal Intake.

This is a Framework rule. It does not create a new operating chat and does not replace the normal Intake → Triage → Evidence Pass → Audit → Route flow.

## Primary Mechanism: In-Chat Pattern Synthesis Trigger

### Trigger

When Intake receives a new item that is clearly the **third or later example** of a recurring concept, perform a short Pattern Synthesis Pass before final routing.

Use this trigger flexibly. A Pattern Synthesis Pass may also be appropriate when:

- A new item strongly resembles a lane seen at least twice before.
- The user explicitly says an item reminds them of prior Lab work.
- A tool or idea appears under a new label but uses the same underlying structure as prior items.
- Several weak ideas become stronger when viewed as a repeated pattern.
- A lane has accumulated enough evidence that Watchlist, Incubator, Archive, Pattern Bank, or Existing Project Handoff should be reconsidered.

### What the Pass Should Answer

Keep the pass lightweight unless the lane is ready for a handoff or Incubator test.

Minimum questions:

```text
What repeats across the related items?
What differs?
What is becoming more credible?
What still looks like hype, clout, or funneling?
What proof would validate or disprove the lane?
Does this remain Pattern Bank / Watchlist, move to Incubator, archive, or hand off to an existing project?
What is the next concrete action?
```

Optional questions when useful:

```text
Does this lane need a better name?
Should two lanes be merged?
Should one broad lane be split into narrower sub-lanes?
Is a tool from this lane useful in a different Lab or user project?
Is the strongest value actually a workflow, data source, distribution pattern, or safety lesson rather than the visible product claim?
```

### Expected Output

A concise synthesis that includes:

- Lane name or candidate name
- Evidence count or rough basis for recurrence
- Repeating pattern
- Important differences
- Confidence level
- Key risks or missing proof
- Routing recommendation
- One next action

### Guardrails

- Do not treat repetition as validation by itself.
- Do not create new projects automatically.
- Do not commit raw Reels, raw datasets, private links, API keys, signed URLs, screenshots with sensitive metadata, or private dashboard contents.
- Keep early synthesis lightweight; avoid over-building weak ideas.
- Preserve the Lab's routing stages: Inbox, Triage, Evidence Pass, Audit, Route.
- Treat clout as a signal, not proof.

## Secondary Mechanism: Scheduled Cross-Lane Synthesis Sweep

### Role

The scheduled sweep is not the main Intake interface. Its job is to catch non-obvious connections that may be missed during first-pass audits.

Examples:

- A tool first evaluated for trading may become useful for clipping or creator workflows.
- A video-generation API may become valuable when paired with AI-assisted clipping.
- A security-testing repo may become relevant to a user-facing app project.
- A memory or organization tool may improve Intake itself.
- Several unrelated Reels may share the same funnel, distribution, or monetization structure.
- A repo becomes more valuable when paired with another repo or workflow.

### Sweep Questions

The sweep should review recent Lab materials and ask:

```text
What cross-lane connections are emerging?
Which tools may be useful outside their original lane?
Which weak ideas become stronger when paired with another tool, source, or workflow?
Which concepts are duplicated under different names?
Which lanes should be merged, split, renamed, downgraded, promoted, or handed off?
What is the highest-leverage next action?
```

### Output Format

Keep the report short:

```text
1. New cross-lane connections noticed
2. Why each connection matters
3. Confidence level
4. Risk or missing proof
5. One recommended next action
```

The sweep may recommend a new handoff, Incubator test, Framework rule, or Archive/Watchlist change, but it should not create those automatically unless the user asks.

### Guardrails

- Clearly label speculation.
- Do not treat a possible connection as validation.
- Prefer repo-backed Lab files, handoffs, tests, and manifest entries when possible.
- Do not surface or log raw private data.
- Do not produce a giant report; focus on the few most useful connections.
- Keep implementation in the appropriate project when an idea belongs outside The Lab.

## Example Lanes Where This Applies

- AI-assisted clipping / Content Rewards / Whop
- AI trading agents and trading research workflows
- App testing / agentic pentesting tools
- Memory and organization tools
- AI-generated digital products
- Prompt/system-prompt collections
- Creator funnels and comment-to-lead magnets
- Video/image generation APIs used for content workflows

## Relationship to Scheduled Task

A weekly scheduled task should run the Cross-Lane Synthesis Sweep as a backup and creative review layer.

Recommended cadence: weekly, Friday evening.

Recommended task title: `Lab Cross-Lane Sweep`

Recommended task prompt:

```text
Review The Lab’s recent Intake, handoffs, tests, manifest entries, and active lanes for cross-lane synthesis opportunities. Look for non-obvious connections between tools, Reels, repos, workflows, and project lanes that may not have been obvious during first-pass Intake. Identify repeated structures, tools that may be useful outside their original lane, weak ideas that become stronger when paired, overlapping Incubator candidates, and concepts that should be merged, split, renamed, downgraded, or promoted. Keep the output concise: new connections, why they matter, confidence level, risk, and one recommended next action. Do not treat speculation as validation, do not create new projects automatically, and do not log raw private data.
```

## Decision

Adopt both mechanisms:

1. Use the **in-chat Pattern Synthesis Trigger** as the primary, contextual workflow.
2. Use the **weekly Cross-Lane Synthesis Sweep** as a secondary backup for creative, non-obvious recombination.
