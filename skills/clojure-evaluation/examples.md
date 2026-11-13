# `clj-nrepl-eval` Examples

Read these examples only when the basic command in `SKILL.md` is not enough for the current evaluation.

## Discover or reconnect

```bash
clj-nrepl-eval --discover-ports
clj-nrepl-eval --connected-ports
clj-nrepl-eval -p <PORT> --reset-session
```

## Evaluate multiline code

Use a quoted one-line expression when practical. A single-quoted heredoc avoids shell escaping when the expression genuinely needs several lines:

```bash
clj-nrepl-eval -p <PORT> <<'EOF'
(defn greet [name]
  (str "Hello, " name "!"))

(greet "Ada")
EOF
```

## Load and inspect a project namespace

```bash
clj-nrepl-eval -p <PORT> <<'EOF'
(require '[my.namespace :as ns] :reload)
(ns/my-function sample-input)
EOF
```

A successful `require` usually establishes that the namespace loaded. Check the behavior that matters rather than treating a load alone as full verification.

## Run a focused test namespace

```bash
clj-nrepl-eval -p <PORT> <<'EOF'
(require '[clojure.test :as test])
(require 'my.project.core-test :reload)
(test/run-tests 'my.project.core-test)
EOF
```

Use the project's normal test command instead when it supplies fixtures, build setup, or a more representative environment.
