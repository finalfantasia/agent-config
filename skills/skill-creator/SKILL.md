---
name: skill-creator
description: Create or improve an agent skill. Use when a user asks to draft, refactor, validate, or improve the triggering description of a SKILL.md.
---

# Skill Creator

Design skills to provide the right guidance at the right time. A skill should make a specialized workflow easier without turning ordinary work into a rigid ceremony.

## Improve or create a skill

1. Start from the user's actual task, existing skill, and any repeated failure or decision it needs to address. Ask only for details that change the skill's purpose, inputs, safety boundary, or expected result.
2. Write a short description that states the capability and the specific situation where it applies. Include a boundary when a nearby task should use a different skill. Do not use exhaustive trigger examples, broad keyword lists, or pressure to invoke the skill unnecessarily.
3. Keep `SKILL.md` as the routing and common-decision layer. Put alternate workflows, long examples, API details, and troubleshooting into clearly named reference files; put deterministic repeated work into scripts.
4. Prefer guidance that explains the outcome and tradeoff over recipes that prescribe every action. Keep real safety or side-effect boundaries explicit, but let the agent choose ordinary exploration and validation steps based on the task.
5. Remove stale tool names, duplicated rules, obsolete model-specific advice, and instructions that force irrelevant reading, testing, or approval pauses.
6. Validate the frontmatter, local links, and at least one representative use case. Use a comparative evaluation when the user needs evidence that a behaviorally meaningful skill change is better.

## Structure and maintenance

Keep the root document concise enough to load comfortably on every invocation. A useful layout is:

```text
skill-name/
├── SKILL.md              # trigger description, routing, shared boundaries
├── references/           # detail read only for the chosen path
└── scripts/              # deterministic or repetitive operations
```

Preserve an existing skill's name and supported behavior unless the user wants to change them. When changing an installed or shared skill, work in the appropriate writable source rather than silently patching a generated dependency.

## Evaluation options

For a quick change, use representative prompts and inspect whether the skill selects the right workflow without unnecessary steps. For a rigorous comparison, read [references/evaluation.md](references/evaluation.md); its supporting grader and analyzer guides are in [agents/](agents/).

Use [references/schemas.md](references/schemas.md) only when producing artifacts for the existing benchmark viewer.
