# PR #804 — Security Hardening for Help Request Ownership (MergeShip)

MergeShip is an open-source platform that helps developers improve pull requests with AI-powered recommendations, review support, and workflow automation.

**Repository:** [Coder-s-OG-s/MergeShip](https://github.com/Coder-s-OG-s/MergeShip)  
**Pull Request:** [#804](https://github.com/Coder-s-OG-s/MergeShip/pull/804)  
**Status:** Merged (July 27, 2026)  
**Role:** Contributor (Author)  
**Contribution Program:** Contributed under GSSoC '26 (Girlscript Summer of Code) and SSoC Season 5

## Overview
This contribution hardened the backend of MergeShip’s help-request workflow by enforcing recommendation ownership validation before a help request could be created or dispatched. It addressed a security gap in the `sendHelpRequest` flow and improved accountability for downstream support processes.

## Problem
The help-request action accepted a recommendation ID (`recId`) without ensuring that the recommendation belonged to the authenticated user. This created a potential cross-user authorization issue and weakened traceability for support triage and audit workflows.

## What I Implemented
- Added a fail-fast ownership check in `sendHelpRequest`.
- Verified that the recommendation exists before proceeding.
- Returned explicit typed errors:
  - `not_found` when the recommendation does not exist
  - `forbidden` when the recommendation belongs to another user
- Persisted `recommendation_id` in `help_requests` inserts to preserve relational traceability.
- Expanded unit tests to cover missing, foreign, and valid ownership cases.

## Technical Details
- **Files changed:**
  - `src/app/actions/help.ts`
  - `src/app/actions/help.test.ts`
- **Core logic change:** Authorization validation now occurs before cooldown and insert logic.
- **Testing improvement:** Updated mock chains to support `maybeSingle()` and multi-step query sequencing.
- **Systems affected:** Supabase queries and inserts, rate limiting flow, and Inngest dispatch path.

## Impact
This change improves security, prevents unauthorized requests, and strengthens data integrity for support automation. It also makes API behavior clearer and more reliable for both clients and maintainers.

## Before vs After

| Before | After | Value |
|---|---|---|
| No ownership check for `recId` | Ownership validation enforced | Prevents unauthorized requests |
| No explicit not-found guard | `not_found` returned for missing records | Better API correctness |
| Weak linkage in inserted help request | `recommendation_id` persisted | Better auditability and triage |

## Skills Demonstrated
- Backend engineering
- Secure API design
- Database integrity and traceability
- Regression testing and edge-case coverage
- Open-source collaboration

## Collaboration Outcome
- Linked bug issue closed via PR (`Closes #794`)
- Received maintainer approvals
- Merged into main after review

## Why This Matters
This contribution reflects careful debugging, strong ownership of a real security issue, and a focus on making the system safer and easier to maintain for the broader community.