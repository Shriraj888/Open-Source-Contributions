# PR #126 — Add Toast Notifications for Success and Error Feedback (MindMitra)

MindMitra is an open-source mental wellness platform that combines AI-driven support, journaling, and emergency alerting to help users manage their well-being more effectively.

**Repository:** [1809gaurav/mind-mitra](https://github.com/1809gaurav/mind-mitra)  
**Pull Request:** [#126](https://github.com/1809gaurav/mind-mitra/pull/126)  
**Status:** Merged  
**Role:** Contributor  
**Contribution Program:** Contributed under SSoC Season 5 (Social Summer of Code)

## Overview
This contribution added global toast notifications to the MindMitra frontend to improve user feedback for successful actions and failures. It made the interface more responsive and easier to understand by surfacing feedback for login, registration, journal updates, profile actions, and API errors.

## Problem
The frontend lacked a consistent way to inform users when actions succeeded or failed. Without visible feedback, users could be uncertain whether a request had completed, especially during authentication, data updates, or error conditions.

## What I Implemented
- Integrated `react-hot-toast` into the frontend for global, auto-dismissible notifications.
- Added success toasts for key actions such as login, registration, journal save/update/delete, profile updates, SOS alerts, and password resets.
- Added error toasts for failed API requests, authentication issues, validation errors, rate limiting, and network failures.
- Added a global Axios interceptor fallback to handle unhandled server and connection errors gracefully.
- Included unit test coverage for toast notification behavior.

## Technical Details
- **Core focus:** Frontend user feedback and error handling.
- **Files changed:**
  - `App.tsx` or related frontend app entry
  - `ToastNotifications.test.tsx`
- **Testing:** Added unit tests to validate toast notification behavior.
- **Systems affected:** Frontend UX feedback, API error handling, and user interaction clarity.

## Impact
This change improved the overall user experience by making the application feel more responsive and transparent. Users now receive immediate feedback for important actions and failures, which makes the product more intuitive and reliable.

## Skills Demonstrated
- Frontend engineering
- User experience improvement
- Error handling and resilience
- Testing and quality assurance
- Open-source collaboration

## Why This Matters
This contribution reflects strong product-minded development by improving clarity, trust, and usability in the user interface through thoughtful feedback design.