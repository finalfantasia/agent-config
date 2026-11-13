# REPL Exploration Reference

Read this when a short interactive probe is not enough and you need a particular `clojure.repl` tool.

## Inspect namespaces and definitions

```clojure
(require '[clojure.repl :refer [all-ns apropos dir doc find-doc source]])

(map ns-name (all-ns))
(dir clojure.string)
(doc map)
(doc clojure.string/upper-case)
(source some?)
(apropos "sequence")
(find-doc "indexed")
```

Require the namespace before asking for its documentation or source. Use a qualified symbol when two namespaces could supply the same name.

## Investigate failures

```clojure
(require '[clojure.repl :refer [pst root-cause]])

;; after an exception
(pst)
(root-cause *e)
```

`*e` holds the most recent exception; `*1`, `*2`, and `*3` hold recent results. Prefer a small reproduction over inspecting a large live object or replaying a destructive operation.

## Reload source after edits

```clojure
(require '[my.namespace :as ns] :reload)
(ns/my-function sample-input)
```

A reload refreshes the named namespace but does not necessarily reset application state. If definitions or state are unexpectedly stale, decide whether resetting the evaluation session is safe before doing so.

## Dynamic dependencies

Clojure 1.12 can add dependencies at a REPL with `clojure.repl.deps`, but use that only in a disposable development session and only when changing the project dependency file is not appropriate:

```clojure
(require '[clojure.repl.deps :refer [add-lib]])
(add-lib 'org.clojure/data.json)
```

Avoid adding dependencies merely to inspect a project. It changes the runtime classpath and can make a session harder to reproduce.
