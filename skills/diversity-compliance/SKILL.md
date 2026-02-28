---
name: diversity-compliance
description: "[Stimulus] Audit diversity spend against federal, state, and city targets. Generate compliance reports with gap analysis and corrective actions."
argument-hint: "[jurisdiction or 'all', e.g. 'Philadelphia' or 'Federal']"
---

# Diversity Compliance Audit

Audit diversity spend against compliance targets and generate actionable reports.

## Compliance Targets by Jurisdiction

- **Philadelphia:** MBE 18%, WBE 10%, LBE tracked, DSBE combined
- **New York:** MWBE 30%, SDVOB 6%
- **Federal:** DBE 10-25% per project, HUBZone 3%, SDVOB 3%
- **Corporate:** Typically 15-25% overall diverse

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
