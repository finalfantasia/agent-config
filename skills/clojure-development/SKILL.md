---
name: clojure-development
description: Implement, debug, or refactor Clojure and ClojureScript code. Use for substantive Clojure changes; use the narrower REPL skill when that is the whole task.
---

# Clojure Development

Work from the requested behavior and the relevant code, then choose the cheapest evidence that can validate the change. A REPL is valuable for live state, interop, and behavior that is hard to infer statically; it is not a prerequisite for every small or purely structural edit.

## Working approach

1. Read the files and tests that define the behavior in scope.
2. Make a focused change that follows the local namespace, data, and error-handling conventions.
3. Validate proportionately: use a running REPL for runtime questions, targeted tests for behavior, and lint/build checks when they cover the changed boundary.
4. Re-check the final diff and report what was verified, along with any remaining uncertainty.

## Choose a narrower skill when useful

- Read [clojure-repl](../clojure-repl/SKILL.md) for interactive exploration techniques such as `doc`, `source`, and exception inspection.

Keep functions small and data-oriented where that matches the surrounding code. Avoid silent fallbacks that change observable failure behavior unless the requested behavior calls for one.
