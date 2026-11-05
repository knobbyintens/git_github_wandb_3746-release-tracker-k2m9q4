# Vendor Review Policy (v4.1)

Owner: Vendor Management Office
Effective: 2026-10-01

This policy governs the quarterly review of every vendor listed in `vendors/registry.json`.
The current cycle is **2026-Q4** and the review date is **2026-11-05**.
Per-vendor assessment notes live in `vendors/notes.md`.

## 1. Data sensitivity tiers

Every vendor record declares exactly one data sensitivity tier: `high`, `medium` or `low`.

## 2. Decision rules

Apply the first matching rule to each vendor and record exactly one decision.

### R1 - High sensitivity

- **approved** when `SOC 2 Type II` appears in the vendor's `certifications`.
- **conditional** when `SOC 2 Type II` is absent but `ISO 27001` appears in `certifications`.
  The condition text must be exactly: `Provide SOC 2 Type II evidence within 60 days`
- **rejected** when neither certification is present.

### R2 - Medium sensitivity

- **approved** when `open_findings` is 2 or fewer.
- **conditional** when `open_findings` is greater than 2.
  The condition text must be exactly: `Remediate open findings before contract renewal`

### R3 - Low sensitivity

- **approved** unless `last_audit` is more than 24 months before the review date.
- **conditional** when `last_audit` is more than 24 months before the review date.
  The condition text must be exactly: `Complete a fresh security audit within 90 days`

## 3. Escalation

A vendor is marked `escalated: true` when either condition holds:

- `annual_spend_usd` is 300000 or more, or
- `criticality` is `critical`.

Escalated vendors are reported to the Head of Vendor Management regardless of the decision.
Vendors that are not escalated are marked `escalated: false`.

## 4. Reviewer routing

Reviewer names are not listed in this document. Assign the reviewer from the roster kept in
institutional memory under the entity `Vendor Review Board`, matched by the vendor's data
sensitivity tier.

## 5. Next review quarter

Quarter offsets are derived from the review date (2026-11-05, which falls in 2026-Q4):

- approved -> `2027-Q4`
- conditional -> `2027-Q2`
- rejected -> `2027-Q1`

## 6. Required outputs

Every cycle publishes the following files inside this repository:

1. `reviews/2026-Q4-vendor-decisions.json` - one decision record for every vendor, in the
   structure described by `templates/decision-record.json`.
2. An updated `vendors/registry.json` - every record keeps all of its existing fields and
   gains `review_status`, `last_reviewed` (the review date) and `review_quarter` (the cycle).
3. `reviews/2026-Q4-summary.md` - a memo that reports the totals for approved, conditional
   and rejected vendors, lists every escalated vendor, and lists the primary risk stated for
   each vendor in `vendors/notes.md` that is not approved.
