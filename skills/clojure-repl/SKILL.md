---
name: clojure-repl
description: Explore Clojure namespaces, APIs, errors, and live behavior at a REPL. Use for interactive investigation, not as a prerequisite for every Clojure edit.
---

# Clojure REPL Exploration

Use this skill when an interactive probe can reduce uncertainty: tracing a runtime value, understanding an unfamiliar namespace, checking a library API, or isolating an exception.

## Investigation loop

1. Load the smallest relevant namespace, preferably with an alias.
2. Inspect the current behavior with representative inputs, including edge cases only when they could change the result.
3. Use REPL tools to answer the next question, then make the source change when the evidence is sufficient.
4. Reload the changed namespace and verify the behavior or run the affected test.

Typical probes:

```clojure
(require '[clojure.repl :refer [doc source dir pst]])
(require '[my.namespace :as ns] :reload)
(doc ns/useful-function)
(source ns/useful-function)
(ns/useful-function sample-input)
```

For an exception, inspect `*e` and use `(pst)` when the stack trace is needed. Keep exploratory definitions and side effects small; live application state may be shared with the running process.

## Boundaries

- Prefer source reading or a targeted test when no live runtime is needed.
- Do not add dependencies or mutate production-like data merely to explore an API.

Read [references/exploration.md](references/exploration.md) for a compact catalog of `clojure.repl` tools and longer examples.
