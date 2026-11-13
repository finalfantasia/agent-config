# Skill Evaluation

Use this guide when the user asks for evidence that a new or changed skill improves behavior, or when the change has enough impact to justify a comparison. A small wording cleanup usually needs only a representative smoke check.

## Build a useful task set

Choose a handful of realistic requests that exercise the skill's actual decision boundary:

- cases that should load the skill for distinct reasons;
- close near-misses that should not load it;
- at least one task that needs a detailed reference or bundled script, when the skill has one.

State the observable outcome for each task. Avoid toy prompts that the base agent can complete without the specialized workflow.

## Compare fairly

When comparing versions, hold the task, environment, model, and available tools constant. Compare the candidate against the prior skill or no skill as appropriate. Inspect both the final result and the path taken:

- Did the skill trigger only where useful?
- Did it choose the relevant reference instead of loading irrelevant detail?
- Did it complete the requested work accurately and safely?
- Did it add unnecessary tool calls, context, user gates, or latency?

Use programmatic checks where an outcome is objective. For subjective output, collect a human review or use a blinded comparison rather than inventing brittle assertions.

## Record enough evidence

Keep prompts, outputs, version identity, commands/checks, and notable failures together. Use the existing `agents/grader.md`, `agents/comparator.md`, and `agents/analyzer.md` only when their more structured benchmark workflow is warranted. Consult `references/schemas.md` before writing artifacts for that viewer.

End the evaluation when it answers the user's decision; do not run a large benchmark merely because a skill changed.
