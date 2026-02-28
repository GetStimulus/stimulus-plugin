---
name: supplier-discovery
description: Find, qualify, and shortlist suppliers using AI-powered semantic search with diversity and relationship scoring.
argument-hint: "<search query, e.g. 'HVAC contractors in PA with MBE certification'>"
---

# Supplier Discovery & Qualification

Find, qualify, and shortlist suppliers for a procurement need.

## Steps

1. Parse search intent — extract query terms, filters (state, certification, industry).
2. Call `supplier_search` with extracted filters and `maxResults: 20`.
3. For top 5-10 results:
   - Call `load_relationship` to check for existing relationship and health score.
   - Call `supplier_performance` to review performance trajectory.
4. Score candidates: relevance (40%) + cert match (25%) + health (20%) + performance (15%).
5. Present ranked shortlist with per-supplier recommended action.

## Decision Points

- Existing relationship + health >= 70: "Re-engage for project".
- New supplier + certs active: "Initiate onboarding".
- Cert expired: exclude from diversity count, note for outreach.
- Only 1-2 results: warn about concentration risk, suggest broadening search.

## Output Format

Ranked table: Company | Location | Certifications | Health | Score | Action

Include "Diversity Impact" section estimating how new engagement affects diversity %.

## Detailed Guide

Read MCP resource `stimulus://workflows/supplier-discovery` for the full workflow.
Read `stimulus://skills/supplier-diversity` for certification context.
Read `stimulus://reference/certifications` for certification type definitions.
