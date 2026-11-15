# agent-config

This repository is a small skill bundle for LLM coding agents, centered on the contents of [`skills/`](skills/). It includes reusable Clojure/ClojureScript development and test guidance, code review, and skill-authoring instructions.

The current skills are:

- `clojure-development`: general Clojure and ClojureScript development
- `clojure-repl`: REPL exploration and debugging patterns
- `clojure-test-audit`: author and audit Clojure/ClojureScript tests
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
cp -R skills/pr-review "$SKILLS_DIR"/
cp -R skills/skill-creator "$SKILLS_DIR"/
```

If you only want the Clojure workflow, the practical minimum is:

```bash
mkdir -p "$SKILLS_DIR"
cp -R skills/clojure-development "$SKILLS_DIR"/
cp -R skills/clojure-repl "$SKILLS_DIR"/
cp -R skills/clojure-test-audit "$SKILLS_DIR"/
```

Once installed, the intended workflow is:

1. Use `clojure-development` for general Clojure tasks and `clojure-test-audit` when adding or evaluating tests.
2. Start or connect to an nREPL server.
3. Use `clojure-repl` for `doc`, `source`, edge cases, and debugging.

## Tool Installation

### Install `fd`

The Pi coding agent requires [`fd`](https://github.com/sharkdp/fd) to be available on your `PATH`.

macOS via Homebrew:

```bash
brew install fd
```

Verify:

```bash
fd --version
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
