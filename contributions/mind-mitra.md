# PR #125 — Add SOSAlert Pydantic Models and Resolve Startup Crash (MindMitra)

MindMitra is an open-source mental wellness platform that combines AI-powered emotion analysis, journaling, chatbot support, and emergency alerting to support users in need.

**Repository:** [1809gaurav/mind-mitra](https://github.com/1809gaurav/mind-mitra)  
**Pull Request:** [#125](https://github.com/1809gaurav/mind-mitra/pull/125)  
**Status:** Merged  
**Role:** Contributor  
**Contribution Program:** Contributed under SSoC Season 5 (Social Summer of Code)

## Overview
This contribution fixed a backend startup issue in MindMitra by adding proper Pydantic models for SOS alerts and hardening the SOS SMS helper against runtime errors. The change stabilized the application and improved the reliability of the emergency alert flow.

## Problem
The SOS-related backend models were missing or incomplete, and the SMS helper was not safely handling both dictionary-based and model-based user/contact data. This caused startup issues and potential runtime crashes in the emergency alert path.

## What I Implemented
- Added and refined Pydantic V2 schema models for SOS alerts, including base, create, response, and list models.
- Improved the `send_sos_sms` helper to safely access user and emergency contact fields without raising `AttributeError` or `KeyError`.
- Preserved the existing Twilio SMS notification behavior while making the flow more robust.
- Removed stray merge-conflict artifacts that were interfering with normal startup.

## Technical Details
- **Core focus:** Backend model correctness and startup stability.
- **Files changed:**
  - `app/models/sos.py`
  - `app/api/v1/endpoints/journal.py`
- **Validation:** The change was reviewed and merged after addressing backend reliability concerns.
- **Systems affected:** SOS model definitions, emergency notification logic, and application startup flow.

## Impact
This change resolved a critical backend issue and improved the robustness of the SOS feature. It made the application more stable, reduced the chance of runtime failures, and strengthened the reliability of emergency communication logic.

## Skills Demonstrated
- Backend engineering
- API and data model design
- Bug fixing and runtime stability improvements
- Open-source collaboration and review responsiveness

## Why This Matters
This contribution reflects careful debugging, resilience in handling real backend failures, and a strong focus on making the system more dependable for users.