---
name: contract-renewal
description: Assess whether to renew, renegotiate, or re-bid an expiring contract based on supplier health and performance data.
argument-hint: "<company name or ID>"
---

# Contract Renewal Decision

Assess whether to renew, renegotiate, or re-bid an expiring contract.

## Steps

1. Call `load_relationship` for the supplier — get health score (0-100).
2. Call `supplier_performance` — get performance trajectory and delivery metrics.
3. Call `spend_analysis` with queryType `"trends"` — understand spend pattern.

## Decision Matrix

| Health | Performance | Recommendation |
|--------|-------------|----------------|
| >= 75  | > 70        | **RENEW** — strong partner |
| 50-74  | > 70        | **RENEGOTIATE** — relationship needs attention |
| >= 75  | 50-70       | **RENEGOTIATE** — performance improvement needed |
| 50-74  | 50-70       | **RENEGOTIATE** — both areas need improvement |
| < 50   | Any         | **RE-BID** — relationship too damaged |
| Any    | < 50        | **RE-BID** — performance unacceptable |

## If RENEGOTIATE or RE-BID

4. Call `supplier_search` to find alternatives for leverage or replacement.
5. Prepare negotiation brief: spend volume, performance gaps, alternatives found.

## Output Format

- Assessment summary (health, performance, utilization scores)
- Recommendation (Renew / Renegotiate / Re-bid) with rationale
- If renegotiate: proposed terms and leverage points
- If re-bid: alternative suppliers and transition timeline

## Detailed Guide

Read MCP resource `stimulus://workflows/contract-renewal` for the full workflow.
Read `stimulus://skills/relationship-intelligence` for health score interpretation.
