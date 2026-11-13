---
name: clojure-evaluation
description: Evaluate Clojure or ClojureScript expressions through an available nREPL using clj-nrepl-eval. Use to inspect runtime state, prototype behavior, or verify a running application.
---

# Clojure nREPL Evaluation

Use `clj-nrepl-eval` when a live REPL can answer the question more directly than static reading or a test run. It is optional for edits that do not need a running process.

## Connect deliberately

Discover project-local servers first:

```bash
clj-nrepl-eval --discover-ports
```

Choose the server that matches the project and runtime you need. If several plausible servers exist and the choice affects behavior, ask; otherwise use the matching project server. For a shadow-cljs browser session, enter the appropriate build after connecting:

```bash
clj-nrepl-eval -p <PORT> "(shadow/repl :app)"
```

If discovery is unavailable in a sandbox, inspect `.nrepl-port` or `.shadow-cljs/nrepl.port` and try the relevant port. A socket-permission failure can be a sandbox limitation rather than a missing REPL.

## Evaluate and verify

Pass a small expression as a quoted argument:

```bash
clj-nrepl-eval -p <PORT> "(+ 1 2 3)"
```

Reload a changed namespace before checking its behavior:

```bash
clj-nrepl-eval -p <PORT> "(require '[my.namespace :as ns] :reload) (ns/my-function input)"
```

Sessions retain definitions and namespace state. Reset only when stale session state is the problem:

```bash
clj-nrepl-eval -p <PORT> --reset-session
```

Use a targeted timeout for work that is known to take longer; do not treat a REPL evaluation as a substitute for the test suite when the change needs broader coverage.

See [examples.md](examples.md) for multiline expressions, namespace loading, and test examples. Run `clj-nrepl-eval --help` for the complete option list.
