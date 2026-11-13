---
name: clojure-delimiter-repair
description: Repair unbalanced or mismatched delimiters in Clojure-family source files with clj-paren-repair. Use after a delimiter diagnostic or when asked to repair a .clj, .cljs, .cljc, or .edn file.
---

# Clojure Delimiter Repair

Use this skill for structural delimiter errors, not as a general formatter or compiler-error fixer. `clj-paren-repair` edits files in place and formats files it changes, so keep the target set narrow and inspect the resulting diff.

## Repair workflow

1. Start with the file named by the editor, compiler, or test diagnostic.
2. Run the repair tool on that file:

   ```bash
   clj-paren-repair path/to/file.clj
   ```

3. Review the repair and rerun the affected load, test, or lint check. If the result changes code beyond the intended structural repair, resolve it as a normal code change rather than blindly accepting it.

For a related set of known-broken files, pass each path explicitly:

```bash
clj-paren-repair src/app/core.clj test/app/core_test.clj
```

## Boundaries

- Do not run the tool across a repository merely to check syntax; it can create unrelated formatting diffs.
- A successful delimiter repair does not resolve unresolved symbols, invalid data, or other semantic errors.
- If the tool cannot produce a valid structure, use the diagnostic and surrounding code to repair it manually, then verify the affected namespace.

Run `clj-paren-repair --help` only when an option or behavior is unclear.
