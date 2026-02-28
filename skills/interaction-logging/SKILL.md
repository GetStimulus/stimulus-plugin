---
name: interaction-logging
description: Log a supplier interaction (call, meeting, email, site visit) with structured notes, outcome, and follow-up scheduling.
argument-hint: "<interaction description, e.g. 'call with Acme Corp about contract renewal'>"
---

# Interaction Logging & Follow-up

Log supplier interactions and schedule follow-up actions.

## Steps

1. Parse interaction details from the user's description:
   - Company name or ID, event type, date, notes, outcome, next steps, follow-up date.
   - Event types: `call`, `meeting`, `email`, `site_visit`, `video_call`, `event`, `other`.
2. If company name given (not UUID), call `supplier_search` with `companyName` to resolve the ID.
3. Call `log_interaction` with the extracted fields.
4. Call `supplier_performance` to show updated engagement status.

## Decision Points

- Outcome contains "escalation" or "risk": recommend notifying manager.
- Follow-up date specified: recommend scheduling a calendar event.
- No follow-up date but action items exist: suggest timeline based on type (call: 3-5 days, meeting: 1-2 weeks, site visit: 2-4 weeks).

## Output Format

Confirmation with: company, type, date, summary, outcome, follow-up date, updated health score and interaction count.

## Detailed Guide

Read MCP resource `stimulus://workflows/interaction-logging` for the full workflow.
