---
name: weekly-intelligence
description: "[Stimulus] Generate a comprehensive weekly procurement intelligence report with health trends, diversity compliance, spend analysis, and pipeline status."
argument-hint: "[week ending date or 'this week']"
---

# Weekly Portfolio Intelligence Report

Comprehensive weekly report for Monday morning procurement standups.

## Steps

1. **Relationship Health:** Call `portfolio_health`. For each at-risk/critical supplier (up to 10), call `load_relationship` and `supplier_performance`.
2. **Diversity Compliance:** Call `spend_analysis` with `"metrics"` and `"diversity"`. Compare: MBE vs 12%, WBE vs 6%, DBE vs 4%, SDVOB vs 3%, overall vs 22%. Traffic-light each.
3. **Spend Trends:** Call `spend_analysis` with `"trends"` (monthly), `"topSuppliers"` (limit 10), `"industries"`. Flag categories with >20% WoW change.
4. **Pipeline Status:** Call `project_info` with action `"list"`. Group by phase. Highlight advances, stalls, bids due.
5. **Compile Report** with: Executive Summary, Relationship Health, Diversity Compliance, Spend Analysis, Pipeline, Recommended Actions.

## Output Format

- Executive Summary (3-5 bullets)
- Relationship Health (distribution table + critical supplier details)
- Diversity Compliance (traffic-light matrix: Green/Yellow/Red)
- Spend Analysis (trends, anomalies, concentration)
- Pipeline (phase counts, movement)
- Recommended Actions (prioritized, owner-assigned)

## Detailed Guide

Read MCP resource `stimulus://workflows/weekly-intelligence` for the full workflow.
Read `stimulus://skills/relationship-intelligence` for health interpretation.
Read `stimulus://skills/supplier-diversity` for compliance targets.
