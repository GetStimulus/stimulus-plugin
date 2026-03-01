---
name: diversity-compliance
description: "[Stimulus] Audit diversity spend against federal, state, and city targets. Generate compliance reports with gap analysis and corrective actions."
argument-hint: "[jurisdiction or 'all', e.g. 'Philadelphia' or 'Federal']"
---

# Diversity Compliance Audit

Audit diversity spend against compliance targets and generate actionable reports.

## Compliance Targets

Targets vary by jurisdiction (city, state, federal) and are configured per tenant in the platform. Use `spend_analysis` with queryType `"diversity"` to retrieve the current tenant's configured targets. Common program types include MBE, WBE, DBE, MWBE, SDVOB, HUBZone, and LBE.

## Steps

1. Call `spend_analysis` with queryType `"metrics"` for total addressable spend.
2. Call `spend_analysis` with queryType `"diversity"` for spend by certification type.
3. Call `spend_analysis` with queryType `"trends"` (period: `"quarter"`) for trajectory.
4. Call `spend_analysis` with queryType `"industries"` to identify zero-diversity categories.
5. Compare actual % against jurisdiction-specific targets.
6. For each gap category, call `supplier_search` with certification + industry filters.

## Decision Points

- Shortfall > 5% from target: "Red — Action Required"
- Within 2% of target: "Yellow — Monitor"
- At or above target: "Green — On Track"
- Spend category with zero diverse suppliers: run supplier discovery.

## Output Format

- Spend summary table
- Program compliance matrix (Green/Yellow/Red)
- Gap analysis by spend category with recommended suppliers
- Trend chart (past 4 quarters)
- Corrective action plan

## Detailed Guide

Read MCP resource `stimulus://workflows/diversity-compliance` for the full workflow.
Read `stimulus://skills/supplier-diversity` for certification rules and counting.
Read `stimulus://reference/certifications` for certification type definitions.
