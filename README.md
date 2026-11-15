# agent-config

This repository is a small skill bundle for LLM coding agents, centered on the contents of [`skills/`](skills/). It includes reusable Clojure/ClojureScript development and test guidance, code review, and skill-authoring instructions.

The current skills are:

- `clojure-development`: general Clojure and ClojureScript development
- `clojure-repl`: REPL exploration and debugging patterns
- `clojure-test-audit`: author and audit Clojure/ClojureScript tests
- `neorepl`: evaluate code, load files, run tests, inspect runtime behavior, and repair delimiters through the neorepl CLI
- `pr-review`: review changesets
- `skill-creator`: author, benchmark, and iterate on skills

## Usage

The main thing to use from this repository is the `skills/` directory.

If your agent supports local skills, copy the skill directories you want into its skills folder. For example, with a destination directory stored in `$SKILLS_DIR`:

```bash
mkdir -p "$SKILLS_DIR"
cp -R skills/clojure-development "$SKILLS_DIR"/
cp -R skills/clojure-repl "$SKILLS_DIR"/
cp -R skills/clojure-test-audit "$SKILLS_DIR"/
cp -R skills/neorepl "$SKILLS_DIR"/
cp -R skills/pr-review "$SKILLS_DIR"/
cp -R skills/skill-creator "$SKILLS_DIR"/
```

If you only want the Clojure workflow, the practical minimum is:

```bash
mkdir -p "$SKILLS_DIR"
cp -R skills/clojure-development "$SKILLS_DIR"/
cp -R skills/clojure-repl "$SKILLS_DIR"/
cp -R skills/clojure-test-audit "$SKILLS_DIR"/
cp -R skills/neorepl "$SKILLS_DIR"/
```

## Tool installation

### Configure a global `neorepl` alias

Use the Clojure CLI with a global alias. Add `:neorepl` to the `:aliases` map in `~/.clojure/deps.edn`:

```clojure
{:aliases
 {:neorepl
  {:replace-deps {io.github.nrepl/neorepl {:git/tag "v0.5.1"
                                           :git/sha "155a066"}}
   :replace-paths []
   :main-opts ["--main" "neorepl.main"]
   ;; https://jline.org/docs/troubleshooting#jdk-24-restricted-method-warning
   :jvm-opts ["--enable-native-access" "ALL-UNNAMED"]}}}
```

Merge this alias into your existing configuration rather than replacing the file. The pinned Git SHA makes the client version reproducible.

Verify:

```bash
clojure -M:neorepl --help
clojure -M:neorepl ports
clojure -M:neorepl eval -s agent '(+ 1 2)'
```

### Start an nREPL server in your project

The REPL-driven development skills assume you have a running nREPL server for the target project.

For a `deps.edn` project, a common pattern is an alias like:

```clojure
{:aliases
 {:nrepl
  {:extra-deps {nrepl/nrepl {:mvn/version "1.6.0"}}
   :main-opts ["--main" "nrepl.cmdline"]}}}
```

Then start it with:

```bash
clojure -M:nrepl
```

## Sources

- Local skill docs in [`skills/`](skills/)
- [Upstream neorepl skill](https://github.com/nrepl/neorepl/tree/main/skills/neorepl), vendored into [`skills/neorepl/`](skills/neorepl/) with commands adapted to use the global Clojure CLI alias. Copyright © 2026 Bozhidar Batsov and contributors; distributed under the [Eclipse Public License 1.0](skills/neorepl/LICENSE).
- [neorepl installation and usage](https://github.com/nrepl/neorepl#usage)

