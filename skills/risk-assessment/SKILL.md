---
name: risk-assessment
description: Assess supplier risk across financial, operational, compliance, and reputational dimensions with mitigation planning.
argument-hint: "<company name or ID>"
---

# Supplier Risk Assessment

Assess supplier risk across multiple dimensions and generate mitigation plans.

## Steps

1. Call `load_relationship` for the supplier — health score, last interaction, journey phase.
2. Call `supplier_performance` — performance trajectory, delivery rates.
3. Call `spend_analysis` with queryType `"topSuppliers"` — spend concentration %.

## Risk Scoring (0-100 composite)

| Dimension | Weight | Source |
|-----------|--------|--------|
| Financial Stability | 30% | Years of relationship, spend stability |
| Operational Reliability | 25% | Performance scores, delivery, quality |
| Compliance | 20% | Certification status, regulatory flags |
| Reputational | 15% | Available information, issues flagged |
| Concentration Risk | 10% | % of total spend, single-source dependency |

## Risk Tiers

| Score | Tier | Action |
|-------|------|--------|
| 75-100 | LOW | Annual review, monitor |
| 50-74 | MEDIUM | Quarterly monitoring, watch |
| 25-49 | HIGH | Monthly monitoring, mitigation plan, find alternatives |
| < 25 | CRITICAL | Executive escalation, freeze new commitments |

## If HIGH or CRITICAL

4. Call `supplier_search` to find alternative suppliers for mitigation.
5. Develop mitigation plan: performance bonds, SLA clauses, spend diversification.

## Output Format

- Composite score and tier
- Dimension breakdown with individual scores
- Trend vs. previous assessment
- Mitigation plan (if HIGH/CRITICAL)
- Alternative suppliers identified

## Detailed Guide

Read MCP resource `stimulus://workflows/risk-assessment` for the full workflow.
Read `stimulus://skills/relationship-intelligence` for health score interpretation.
