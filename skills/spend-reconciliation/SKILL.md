---
name: spend-reconciliation
description: "[Stimulus] Reconcile procurement spend against budgets, contracts, and compliance targets with anomaly detection and concentration analysis."
argument-hint: "[period, e.g. 'last month' or 'Q4 2025']"
---

# Spend Reconciliation

Reconcile procurement spend against budgets and compliance targets.

## Steps

1. Call `spend_analysis` with queryType `"metrics"` for total spend for period.
2. Call `spend_analysis` with queryType `"trends"` (period: `"month"`) for MoM change.
3. Call `spend_analysis` with queryType `"industries"` for category breakdown. Flag categories with >15% MoM change as anomalies.
4. Call `spend_analysis` with queryType `"topSuppliers"` (limit: 20) for concentration analysis. Calculate top-3 %, top-5 %, top-10 % of total spend. Warn if top-3 > 40%.
5. Call `spend_analysis` with queryType `"diversity"` for diversity reconciliation. Compare against program targets.

## Key Metrics

| Metric | Healthy Range |
|--------|---------------|
| Spend concentration (top 3) | < 40% |
| Budget variance by category | +/- 10% |
| Diversity spend % | Per program targets |

## Decision Points

- Category spend anomaly >15% MoM: investigate driver (which supplier, what POs).
- Concentration >40% with top 3: flag single-source risk.
- Diversity declining MoM: trigger diversity compliance audit.
- Budget overrun >10%: require explanation and CPO approval.

## Output Format

- Spend overview (total, trend, change from prior period)
- Category breakdown with variance indicators
- Concentration analysis with risk flags
- Diversity reconciliation (actual vs. target, trend direction)
- Anomalies requiring investigation
- Recommended actions (prioritized)

## Detailed Guide

Read MCP resource `stimulus://workflows/spend-reconciliation` for the full workflow.
