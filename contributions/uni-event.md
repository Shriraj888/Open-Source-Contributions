# PR #427 — Automated Email Template Testing and Preview (Uni-Event)

Uni-Event is an open-source university event management platform built with React Native, Expo, and Firebase, designed to simplify campus event management, attendance tracking, notifications, and engagement.

**Repository:** [roshankumar0036singh/Uni-Event](https://github.com/roshankumar0036singh/Uni-Event)  
**Pull Request:** [#427](https://github.com/roshankumar0036singh/Uni-Event/pull/427)  
**Status:** Merged  
**Role:** Contributor  
**Contribution Program:** Contributed under GSSoC '26 (Girlscript Summer of Code)

## Overview
This contribution added automated email template testing and preview support for the Uni-Event backend. It improved the reliability of the email workflow by introducing a template renderer, preview endpoints, and a dry-run email sending flow.

## Problem
Email templates were difficult to test and preview in a consistent way. Developers had limited visibility into how templates rendered and how placeholders behaved, which made debugging and iteration slower and more error-prone.

## What I Implemented
- Built an email template rendering system with placeholder replacement and fallback sample data.
- Added preview routes to list available templates and render them directly in the browser.
- Implemented a dry-run email sending mode so emails could be previewed without triggering external API calls.
- Added Jest-based unit tests for rendering and template discovery behavior.
- Updated TypeScript and Jest configuration to support better testability and build validation.

## Technical Details
- **Core focus:** Email template rendering, previewing, and safe testing flow.
- **Testing:** Added unit tests covering placeholder injection, fallback data handling, and template resolution.
- **Validation:** Verified through Jest test execution and successful TypeScript build checks.
- **Systems affected:** Cloud Functions email workflow, template rendering pipeline, and backend testing setup.

## Impact
This change improved developer productivity by making email templates easier to test and preview before deployment. It also strengthened maintainability by introducing a more structured and verifiable email workflow.

## Skills Demonstrated
- Backend engineering
- Testing and automation
- API and server-side workflow design
- Open-source collaboration and review responsiveness

## Why This Matters
This contribution reflects strong engineering discipline through feature development, test coverage, and quality-focused iteration during review.