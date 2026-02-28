---
name: rfp-lifecycle
description: Manage a full RFP lifecycle from planning through award, including vendor qualification, evaluation scoring, and award recommendation.
argument-hint: "<project ID or description>"
---

# RFP Lifecycle Orchestration

Manage a full RFP lifecycle from planning through award.

## Phase 1: Planning

1. Call `project_info` with action `"details"` and the project ID to load context.
2. Call `spend_analysis` with queryType `"diversity"` to set diversity participation goals.
3. Call `supplier_search` to build an invited vendor list (target 8-15 firms).
4. For each candidate: call `load_relationship` + `supplier_performance`.

## Phase 2: Evaluation

5. Apply 10-dimension scoring:
   - Technical (20%), Past Performance (15%), Cost (15%), Team (10%), Project Approach (15%), Innovation (5%), References (5%), Financial Stability (10%), Commercial Terms (3%), Diversity (2%)
6. Cross-reference: call `supplier_performance` for historical performance data.
7. Shortlist: proposals scoring >= 70 composite.

## Phase 3: Award

8. Generate award recommendation memo with scoring comparison.
9. Call `log_interaction` with event type to record the award decision.
10. If new supplier: initiate vendor onboarding workflow.

## Output Format

- Vendor qualification matrix
- Scoring comparison table (all dimensions)
- Shortlist with recommendation rationale
- Award memo draft

## Detailed Guide

Read MCP resource `stimulus://workflows/rfp-lifecycle` for the full workflow.
Read `stimulus://skills/rfp-workflow` for evaluation methodology and scoring dimensions.
Read `stimulus://skills/supplier-diversity` for diversity goal setting.
