---
name: neorepl
description: Work with a running Clojure, ClojureScript or babashka program through its nREPL server, with the clojure -M:neorepl command line client. Use it to evaluate code, load edited files, run tests, look up docs and source, read stack traces, trace and profile function calls and repair unbalanced parens.
---

# neorepl

`clojure -M:neorepl` uses the global alias in `~/.clojure/deps.edn` and talks
to the nREPL server of the project you're in. It finds it through the
`.nrepl-port` file in the current directory or a parent, so run it from
inside the project. Every command takes `--help`, and `--format json`
when you'd rather parse the output than read it.

## Getting a server

`clojure -M:neorepl ports` lists the servers it can see. The commands use the first
one that answers; when there are several, a shadow-cljs server next to a
Clojure one, say, `-p PORT` picks another. When there's none, ask the
user before starting one, since they may want their editor's. If they say
go ahead, start one in the background. It keeps running until it's stopped:

```shell
clojure -M:neorepl server
```

It prints its `nrepl://` URL once it's up, and writes the port file the
other commands find.

## Sessions

Evaluations keep their state, `*ns*`, `*1` and `*e`, between calls in a
named session. Use one of your own, `--session agent` (`-s agent`), on the
commands that evaluate, so you don't trample the user's: `eval`, `load`,
`test`, `retest`, `pst` and `interrupt`. The MCP tools use that one
too. `clojure -M:neorepl sessions close agent` starts it over.

A server that restarts loses its sessions, with whatever you defined and
loaded, and one started again with `clojure -M:neorepl server` is usually on another
port. `eval` and `load` with `-s agent` start the session over, saying so
on stderr when the port's the same, while `pst`, `interrupt` and the
lookups say there's no such session until one of them has. Load your files
again before going on.

## After editing a file

Check the delimiters balance, then load the file, then run its tests:

```shell
clojure -M:neorepl check src/my/app.clj
clojure -M:neorepl load -s agent src/my/app.clj
clojure -M:neorepl test -s agent my.app-test
```

When `check` reports a problem, `clojure -M:neorepl check --fix src/my/app.clj` repairs
it from the indentation, the way parinfer would, and writes the file back.
Read the result: it trusts the indentation, so a line indented wrong ends up
in the wrong form. Fix it by hand when `--fix` says it can't.

Loading matters: `clojure -M:neorepl test` loads namespaces that aren't loaded yet, but
not your changes to ones that are. `clojure -M:neorepl reload` reloads every namespace
that changed since the last reload, when the server has cider-nrepl, and
`clojure -M:neorepl eval --reload` does that before evaluating, keeping what `defonce`
defined. When the reload fails, the code doesn't run.

## Evaluating

```shell
clojure -M:neorepl eval -s agent '(my.app/parse-date "2026-10-17")'
clojure -M:neorepl eval -s agent '(range 5)'
clojure -M:neorepl eval -s agent '(map inc *1)'
```

What the code prints shows up as it runs, and each value on a line of its
own. `--ns my.app` evaluates in that namespace, loading it first unless it's
there, without moving the session out of its own. Pass code with single
quotes in it on standard input, instead of fighting the shell:

```shell
clojure -M:neorepl eval -s agent --ns my.app - <<'EOF'
(keys (ns-publics 'my.app))
EOF
```

To evaluate one definition from a file you've edited, without loading the
rest, give its line: `clojure -M:neorepl eval -s agent -f src/my/app.clj --line 42`
evaluates the top-level form there, in the file's namespace.

`--timeout 30` interrupts an evaluation that runs longer than 30 seconds and
`--max-output 2000` cuts long values and output, both worth passing when
you're not sure what the code does. `clojure -M:neorepl interrupt -s agent` stops an
evaluation left running by a call that got killed.

## When something fails

`eval` and `load` print the error and exit 1. `clojure -M:neorepl pst -s agent` prints
the stack trace of the session's last error. `clojure -M:neorepl test` prints each
failure with what it expected, what it got and where, and
`clojure -M:neorepl retest -s agent` reruns only the tests that didn't pass.

To see how a function gets called, and what it returns, trace it, run the
code that calls it, and stop tracing it after. It takes cider-nrepl on the
server:

```shell
clojure -M:neorepl trace my.app/parse-date
clojure -M:neorepl eval -s agent '(my.app/import "data.csv")'
clojure -M:neorepl untrace my.app/parse-date
```

To find out where the time goes, profile the functions, or a whole
namespace, run the code, and read what was measured: each function's calls
and how long they took. `--clear` forgets it, for the next try:

```shell
clojure -M:neorepl profile my.app.db
clojure -M:neorepl eval -s agent '(my.app/import "data.csv")'
clojure -M:neorepl profile
clojure -M:neorepl unprofile my.app.db
```

## Looking things up

```shell
clojure -M:neorepl doc clojure.core/update-in
clojure -M:neorepl source --ns my.app start
clojure -M:neorepl where my.app/start
clojure -M:neorepl refs my.app/start
clojure -M:neorepl apropos update
clojure -M:neorepl macroexpand --all '(-> x (f) (g))'
```

`refs` lists the functions that use a function, with where each is, from the
project's code that's loaded: look before changing what it takes or returns.
`--deps` lists the ones it uses instead. It takes cider-nrepl.

They resolve names in `user`, or the namespace `--ns` names, apropos aside.
With `-s agent` they look as your session sees things, in its namespace, and
in ClojureScript when it's in ClojureScript, where you name the namespace
with `--ns`.

## Exit status

0 is success, 1 means the code, test or check failed, a lookup found
nothing or a server didn't start, 2 is a mistake in the command line, 3
means there's no server to talk to and 124 a timeout.

## What not to do

Don't run `clojure -M:neorepl` without a command: that's the interactive REPL, which
needs a person at a terminal. Don't edit a file to see what some code does
when `eval` can tell you.
