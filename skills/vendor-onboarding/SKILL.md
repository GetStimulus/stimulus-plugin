---
name: vendor-onboarding
description: Onboard a new supplier with duplicate checking, certification verification, risk screening, and activation tracking.
argument-hint: "<company name>"
---

# Vendor Onboarding

Onboard a new supplier with verification, approvals, and activation.

## Phase 1: Intake & Verification (Day 0-3)

1. **Duplicate check:** Call `supplier_search` with `companyName` to verify not already in system.
2. If found: call `load_relationship` to check if relationship is active or inactive.
3. **Certification check:** Verify claimed certifications (MBE, WBE, DBE, etc.).
4. **Risk screening:** Assess financial stability, sanctions, compliance.

## Phase 2: Approval & Setup (Day 3-7)

5. Route for approval: procurement, legal, finance.
6. Contract execution from template + agreed terms.
7. System provisioning (vendor master, banking, payment terms).

## Phase 3: Activation (Day 7-14)

8. Welcome package and kickoff meeting.
9. Call `supplier_performance` to set baseline metrics.
10. Call `log_interaction` with event type `"other"` and notes `"onboarding_complete"`.
11. Schedule: 30-day check-in and 90-day performance review.

## Onboarding Checklist

- [ ] Duplicate check complete
- [ ] Profile created/updated
- [ ] Certifications verified
- [ ] Risk screening passed
- [ ] Documents collected (W-9, COI, banking)
- [ ] Internal approvals obtained
- [ ] Contract executed
- [ ] Kickoff meeting completed
- [ ] Performance baseline set

## Detailed Guide

Read MCP resource `stimulus://workflows/vendor-onboarding` for the full workflow.
