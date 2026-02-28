---
name: morning-briefing
description: "[Stimulus] Generate a daily executive procurement briefing with portfolio health, diversity metrics, pipeline status, and urgent actions."
argument-hint: "[date or 'today']"
---

# Morning Briefing

Generate a daily executive briefing for procurement leadership.

## Steps

1. Call `portfolio_health` to get at-risk count, critical suppliers, and overall score.
2. Call `spend_analysis` with queryType `"metrics"` for total spend and diverse spend %.
3. Call `spend_analysis` with queryType `"diversity"` for breakdown by certification type.
4. Compare diversity % against targets: MBE 12%, WBE 6%, DBE 4%, overall 22%.
5. Call `project_info` with action `"list"` to group projects by status phase.
6. For each critical supplier from step 1, call `load_relationship` for context.

## Decision Points

- Diversity % below target: flag as "Compliance Risk" with gap amount.
- Critical suppliers > 0: list each with health score and last interaction date.
- Project stalled > 14 days: flag as "Needs Attention".

## Output Format

Structure the briefing as:
- **Portfolio Health** — score, trend, at-risk count, critical supplier details
- **Spend Pulse** — total, diverse %, gap to each program target
- **Pipeline** — projects by phase, bids due this week
- **Urgent Actions** — prioritized list with recommended next steps

## Detailed Guide

Read MCP resource `stimulus://workflows/morning-briefing` for the full workflow specification.
Read `stimulus://skills/relationship-intelligence` for health score interpretation.
Read `stimulus://skills/supplier-diversity` for compliance target reference.
