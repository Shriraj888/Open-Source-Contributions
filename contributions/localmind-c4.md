# PR #1033 — Add Citation Merging Guidance to Build and Deploy Docs (LocalMind)

LocalMind is an open-source offline AI assistant that helps users chat with documents locally using Python, FastAPI, React, Ollama, and RAG-based workflows.

**Repository:** [imDarshanGK/localmind](https://github.com/imDarshanGK/localmind)  
**Pull Request:** [#1033](https://github.com/imDarshanGK/localmind/pull/1033)  
**Status:** Merged  
**Role:** Contributor  
**Contribution Program:** Contributed under SSoC Season 5

## Overview
This contribution improved LocalMind’s build and deployment documentation by adding clear guidance for citation merging in RAG workflows and strengthening regression checks so deployment docs stay accurate and enforceable.

## Problem
The build and deployment documentation did not clearly explain how citation merging works in LocalMind’s RAG pipeline. This made deployment guidance less complete and left room for missing validation steps during pre-deployment review.

## What I Implemented
- Added a dedicated “Citation Merging in RAG Deployment” section to the README.
- Documented the citation merging flow, including `build_sources()`, deduplication of `(source, chunk)` tuples, and preview truncation behavior.
- Created a new documentation guide in `docs/citation-merging.md` covering architecture, schema details, compatibility considerations, and deployment validation steps.
- Updated the deployment checklist to include running `pytest tests/test_citations.py` before deployment.
- Added regression tests to ensure the README and deployment docs remain consistent with the citation merging documentation.

## Technical Details
- **Core focus:** Documentation quality and deployment validation reliability.
- **Files changed:**
  - `README.md`
  - `docs/citation-merging.md`
  - `backend/tests/test_deploy_docs_config.py`
- **Validation:** Regression tests were added around documentation consistency and deployment checklist coverage.
- **Systems affected:** RAG deployment docs, citation handling guidance, and documentation validation flow.

## Impact
This change made LocalMind’s deployment guidance more complete and practical for contributors. It improved clarity around citation merging behavior, reduced documentation drift, and made pre-deployment validation easier to follow.

## Skills Demonstrated
- Documentation engineering
- Technical writing for developer workflows
- Regression testing and validation
- Open-source collaboration and iterative review handling

## Why This Matters
This contribution reflects a strong focus on maintainability and developer experience by improving the clarity and reliability of deployment documentation for the broader project community.