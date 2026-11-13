---
name: clojure-test-audit
description: Use when adding, changing, reviewing, or auditing Clojure/ClojureScript tests, especially to assess test value, duplication, implementation coupling, or test-only production seams.
---

# Clojure/ClojureScript Test Audit

Use the authoring gate for every new or changed test and the audit workflow when examining existing coverage. Optimize for confidence at the right behavioral boundary, not test count. Follow the project's test conventions and runner; do not impose a framework or add test infrastructure without a concrete need.

## Authoring gate

Before adding or changing a test, be able to answer:

1. What observable behavior, invariant, or independent contract does it protect?
2. What plausible regression makes it fail?
3. Why does existing coverage not already catch that failure? Give each contract one primary owner at its strongest useful boundary. Another layer needs a distinct risk it can reach, such as a serialization, transport, or lifecycle failure.
4. Does the test require a production-only-for-tests var, wrapper, flag, or injection point? If so, prefer testing through a real production boundary unless that seam itself is part of the intended API.

Check the test against the junk patterns below. A match needs a specific independent contract from the retention bar to justify keeping or adding it. A regression test should fail on the unfixed behavior for the intended reason and pass after the repair; when this cannot be demonstrated, say so rather than treating the test as proof.

## Junk patterns

These are review signals, not automatic deletion rules. Ask what the test actually proves and whether that proof is independently valuable.

- Assertion-free tests, tautologies, self-comparisons, or expected values computed by the function under test.
- Tests of copied inventories, fixture contents, namespace lists, or manifests instead of the behavior those artifacts are meant to enable.
- Exact source/string checks, private-var or call-shape assertions, and tests coupled to internal structure that would break under a behavior-preserving refactor.
- Multiple tests or layers replaying the same scenario without protecting a distinct boundary.
- `with-redefs`, mocks, stubs, or fixtures that implement the behavior being asserted, or supply ordering/state that the production owner must produce.
- Production vars, wrappers, flags, or dead code retained only to make tests reach internals.
- Mocks or fixtures that hide the real contract—for example, a persistence assertion against an in-memory store the exercised path never writes.
- Property tests whose generator never reaches the claimed cases, or whose property merely restates the generator's construction.
- Brittle snapshots or source inspections that pin incidental representation rather than a stable external contract.
- CLJ and CLJS suites duplicating shared `.cljc` behavior without a target-specific difference or independent risk.

Clojure test tools are not inherently bad: `with-redefs`, protocols, `reify`, multimethods, fixtures, and property-based testing can all be appropriate. Keep them when they isolate a real external effect, exercise a meaningful failure boundary, or cover a contract that cannot be reached more directly. For asynchronous CLJS tests, verify that completion/failure is actually reported to the test runner and that callbacks are not the only thing making the assertion appear to pass.

## Retention bar

Keep tests that independently enforce meaningful behavior or a contract: public API, configuration, persistence, migrations, security, protocol or dependency integration, platform-specific behavior, CLJ/CLJS differences, serialization, lifecycle/order, generated output, or a credible regression. Static/source-level checks can be valuable when they are the cheapest independent guard of a stable contract and survive incidental identifier refactors. A passing or failing test that exposes a possible product defect is evidence to investigate at the owning boundary, not a reason to delete it as cleanup.

## Audit workflow

1. Read applicable `AGENTS.md` files and the project's test/build instructions. Find how tests are selected in the runner and CI before judging coverage.
2. Read the complete candidate test and its production owner, entry points, callers/callees, sibling implementations, relevant fixtures, and overlapping tests. If the claim depends on a library or generated behavior, inspect its actual API/source where useful. Check history when it explains a seam or regression.
3. Keep discovery read-only. For each proposed removal or rewrite, record:
   - exact test name and location;
   - the failure it can actually detect;
   - non-test callers of any production/support seam involved;
   - stronger remaining proof, or why none is needed;
   - relevant history and why the test/seam exists;
   - production or test-support cleanup unlocked;
   - risk and the focused validation to run.
4. Choose a coherent owner-boundary batch. Consolidate repeated cases into table-driven tests or shared fixtures when that improves clarity; move retained regressions to the boundary that owns them. Do not turn uncertain candidates into deletions to increase a cleanup count.

For a broad sweep, bound it to a subsystem or test owner at a time. Prefer a small number of high-confidence candidates over a repository-wide speculative inventory.

## Clojure and ClojureScript validation

- Inspect the repository's actual runner and aliases (`deps.edn`, `project.clj`, `shadow-cljs.edn`, package scripts, CI, or project docs). Do not assume a universal test command. Run the smallest relevant namespace/test selection, then sibling or broader suites when the change could affect them.
- Validate the relevant runtime(s). For shared `.cljc` code, run both CLJ and CLJS when reader conditionals, interop, async behavior, or platform-specific paths are involved; avoid duplicating identical coverage without such a risk.
- For compile-time, macro, browser, or integration contracts, use the project's real build/test path rather than substituting a source grep. Keep external services and data safe: use the project's test fixtures and configured test environment, not production data or consequential live operations.
- Run focused formatting/lint/build checks where the project requires them, then `git diff --check` and inspect the final diff. Report commands actually run and any target or environment you could not validate.

## Handoff

Summarize the behavior or contracts examined, low-value patterns removed or retained (and why), production/test-support simplifications, focused and broader validation actually run, and unresolved candidates or follow-ups.

Conceptually adapted from [OpenClaw's Test Audit skill](https://github.com/openclaw/openclaw/blob/main/.agents/skills/test-audit/SKILL.md), with project-specific Clojure/ClojureScript guidance in place of repository-specific tooling.
