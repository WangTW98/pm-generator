# pm-09-md2figma-batch for Claude

Use this skill to batch restore many `product/pages/**/figma-nodes.json` files into one user-specified Figma file.

Rules:

- Require a Figma link.
- Scan matching JSON files under `product/pages`.
- Sort and group JSON files by designSpecKey, layoutKey, pageIntentKey, stateGroupKey, then page path when `consistency` exists.
- Create a task list under `product/pages/_md2figma-batch-tasks/`.
- For each valid JSON, execute the pm08 single-page restoration rules.
- Do not compress or rewrite the original JSON node tree.
- Preserve and pass through `renderSpec`, `consistency`, profileRefs, styleFingerprint, driftCheck, component blueprint references, text policies, layout constraints, and QA expectations.
- Use `consistencyMode=profile-locked` for pm08 restoration.
- Keep default-state-only behavior.
- Update task status as work proceeds, and do not mark a task Done when pm08 post-render QA reports failed root bounds, shell collision, text overflow, blocking duplicate action, renderer loss, or layoutBudget failure.
- Continue after individual task failures unless the user asked to stop on first failure.

Do not directly invent a different batch conversion flow or a shared rendering template.
