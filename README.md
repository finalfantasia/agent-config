# agent-config

This repository is a small skill bundle for LLM coding agents, centered on the contents of [`skills/`](skills/). It includes reusable Clojure/ClojureScript development and test guidance, code review, and skill-authoring instructions.

The current skills are:

- `clojure-development`: general Clojure and ClojureScript development
- `clojure-evaluation`: use `clj-nrepl-eval` to evaluate code through a running nREPL server
- `clojure-repl`: REPL exploration and debugging patterns
- `clojure-delimiter-repair`: use `clj-paren-repair` to repair delimiter errors
- `clojure-test-audit`: author and audit Clojure/ClojureScript tests
- `pr-review`: review changesets
- `skill-creator`: author, benchmark, and iterate on skills

## Usage

The main thing to use from this repository is the `skills/` directory.

If your agent supports local skills, copy the skill directories you want into its skills folder. For example, with a destination directory stored in `$SKILLS_DIR`:

```bash
mkdir -p "$SKILLS_DIR"
cp -R skills/clojure-development "$SKILLS_DIR"/
cp -R skills/clojure-evaluation "$SKILLS_DIR"/
cp -R skills/clojure-repl "$SKILLS_DIR"/
cp -R skills/clojure-delimiter-repair "$SKILLS_DIR"/
cp -R skills/clojure-test-audit "$SKILLS_DIR"/
cp -R skills/pr-review "$SKILLS_DIR"/
cp -R skills/skill-creator "$SKILLS_DIR"/
```

If you only want the Clojure workflow, the practical minimum is:

```bash
mkdir -p "$SKILLS_DIR"
cp -R skills/clojure-development "$SKILLS_DIR"/
cp -R skills/clojure-evaluation "$SKILLS_DIR"/
cp -R skills/clojure-repl "$SKILLS_DIR"/
cp -R skills/clojure-delimiter-repair "$SKILLS_DIR"/
cp -R skills/clojure-test-audit "$SKILLS_DIR"/
```

Once installed, the intended workflow is:

1. Use `clojure-development` for general Clojure tasks and `clojure-test-audit` when adding or evaluating tests.
2. Start or discover an nREPL server.
3. Use `clojure-evaluation` to verify assumptions in a live REPL.
4. Use `clojure-repl` for `doc`, `source`, edge cases, and debugging.
5. If an edit introduces bad delimiters, run `clj-paren-repair` immediately.

## Tool Installation

These skills assume a few external tools are available on your `PATH`.

### 1. Install Babashka

`clojure-mcp-light` requires Babashka, and its README notes that `1.12.212+` is required when working with Codex and similar sandboxed tools.

macOS via Homebrew:

```sh
brew install borkdude/brew/babashka
```

Verify:

```bash
bb --version
```

### 2. Install bbin

`bbin` is the Babashka package manager used to install the CLI tools referenced by these skills.

A common Homebrew install on macOS is:

**1. Install via `brew`:**
```bash
brew install babashka/brew/bbin
```

**2. Ensure bbin's install directory is on your `PATH`.**

After installation, verify:

```bash
bbin --version
```

### 3. Install `fd`

The Pi coding agent requires [`fd`](https://github.com/sharkdp/fd) to be available on your `PATH`.

macOS via Homebrew:

```bash
brew install fd
```

Verify:

```bash
fd --version
```

### 4. Install `clj-nrepl-eval`

This is the command used by [`skills/clojure-eval/SKILL.md`](skills/clojure-eval/SKILL.md).

```bash
bbin install https://github.com/bhauman/clojure-mcp-light.git --tag v0.2.2 --as clj-nrepl-eval --main-opts '["--main" "clojure-mcp-light.nrepl-eval"]'
```

Verify:

```bash
clj-nrepl-eval --help
```

### 5. Install `clj-paren-repair`

This is the command used by [`skills/clojure-delimiter-repair/SKILL.md`](skills/clojure-delimiter-repair/SKILL.md).

```bash
bbin install https://github.com/bhauman/clojure-mcp-light.git --tag v0.2.2 --as clj-paren-repair --main-opts '["--main" "clojure-mcp-light.paren-repair"]'
```

Verify:

```bash
clj-paren-repair --help
```

### 6. Optional: install `parinfer-rust`

`clj-paren-repair` can use `parinfer-rust` as its preferred repair backend when available.

```bash
cargo install --git https://github.com/eraserhd/parinfer-rust
```

If you skip it, `clj-paren-repair` can still fall back to its pure Clojure path.

### 7. Start an nREPL server in your project

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

Once it is running, you can discover it with:

```bash
clj-nrepl-eval --discover-ports
```

## Sources

- Local skill docs in [`skills/`](skills/)
- clojure-mcp-light README: https://github.com/bhauman/clojure-mcp-light/blob/main/README.md
- Babashka README: https://github.com/babashka/babashka/blob/master/README.md
- bbin README: https://github.com/babashka/bbin/blob/main/README.md

