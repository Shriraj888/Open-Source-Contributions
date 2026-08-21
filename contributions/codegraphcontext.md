# PR #1629 — Retain Enclosing Context for Calls in Anonymous JS/TS Callbacks (CodeGraphContext)

CodeGraphContext is an open-source project focused on code understanding through graph-based analysis, relationship extraction, and accurate cross-language call resolution.

**Repository:** [CodeGraphContext/CodeGraphContext](https://github.com/CodeGraphContext/CodeGraphContext)  
**Pull Request:** [#1629](https://github.com/CodeGraphContext/CodeGraphContext/pull/1629)  
**Status:** Merged (August 18, 2026)  
**Role:** Contributor (Author)  
**Contribution Program:** Contributed under GSSoC (Girlscript Summer of Code)

## Overview
This contribution fixed a parser bug in CodeGraphContext where function calls made inside anonymous JavaScript and TypeScript callbacks lost their enclosing caller context. The fix preserved the correct `CALLS` relationships in the generated call graph, especially in async and event-driven code patterns.

## Problem
When a function call appeared inside an anonymous callback such as an arrow function, `.then()` callback, `setTimeout`, event listener, or array callback, the TypeScript and JavaScript parsers stopped traversing the AST at the anonymous function node. Because the callback had no identifier name, `_get_parent_context` returned `(None, curr.type, line)`, which caused the call to be attributed to a nameless caller instead of the real enclosing function.

This led to dropped `(Function)-[:CALLS]->(...)` edges and caused `find_callers` to miss valid enclosing functions.

## What I Implemented
- Updated `_get_parent_context` in the JS and TS parsers to continue walking upward until a named enclosing scope is found.
- Preserved caller attribution for nested anonymous callbacks and anonymous IIFEs.
- Added support for resolving names from:
  - variable declarators
  - member assignment expressions like `obj.fn = ...`
  - object property definitions like `{ fn: ... }`
  - class field definitions
- Kept parser behavior consistent across related languages by ensuring they only return a context once a name has been resolved.
- Added regression tests covering nested callbacks, callback-driven calls, and cross-language call resolution.

## Technical Details
- **Core focus:** AST context traversal and accurate call graph attribution.
- **Files involved:**
  - `javascript.py`
  - `typescript.py`
  - `typescriptjsx.py` (inherited fix)
  - `cpp.py`
  - `csharp.py`
  - `go.py`
  - `lua.py`
  - `tests/unit/parsers/test_typescript_parser.py`
  - `tests/unit/parsers/test_javascript_parser.py`
  - `tests/unit/tools/test_cross_lang_call_resolution_patterns.py`
- **Validation performed:**
  - `pytest tests/unit/parsers/test_typescript_parser.py tests/unit/parsers/test_javascript_parser.py tests/unit/tools/test_cross_lang_call_resolution_patterns.py`
- **Systems affected:** JavaScript/TypeScript parser behavior, function attribution logic, and graph edge generation.

## Impact
This fix restored accurate function-to-function call relationships in real-world JavaScript and TypeScript codebases. It prevents valid calls from being silently dropped when they occur inside anonymous callbacks, which makes the generated code graph much more reliable for downstream analysis and developer tooling.

## Skills Demonstrated
- Parser debugging and root-cause analysis
- AST traversal and context resolution
- Call graph correctness and edge attribution
- Regression testing under complex callback patterns
- Cross-language consistency in code analysis
- Open-source collaboration and maintainer review response

## Why This Matters
This bug was subtle but high-impact: the code still parsed successfully, but the graph lost important relationships between functions. Fixing it at the parser layer improves the trustworthiness of CodeGraphContext’s graph output and helps developers reason about real-world code more accurately.

## Contribution Outcome
- Fixed bug reported in issue [#1570](https://github.com/CodeGraphContext/CodeGraphContext/issues/1570)
- Closed the issue via this PR
- Merged into `main` after maintainer validation
