# Claude Adapter Prompt

Use the `pm-07-page-style-batch` skill to batch-generate render-ready Figma JSON node documents for `product/pages/**/layout.md`.

Follow `SKILL.md` as the canonical orchestration workflow and `assets/page-style-batch-task-template.md` as the batch task list schema. Create or resume a task list under `product/pages/_style-batch-tasks/`. Group eligible pages by designSpecKey, layoutKey, pageIntentKey, and stateGroupKey, create or reuse `product/style-profiles/<layoutKey>/` profiles, and record profile/fingerprint/drift status. For each eligible `layout.md`, invoke or perform `pm-06-page-style` once and write `figma-nodes.json` plus `figma-node-notes.md` beside the source file. Preserve pm-06's default-state-only, `pageIntent`, `semanticValidation`, `renderSpec`, and `consistency` rules and do not generate non-default states as Figma nodes. Confirm each output includes component adapters, text policies, constraints, layout safety, QA expectations, profileRefs, styleFingerprint, lockedPropertiesApplied, and driftCheck.

Do not synthesize generic JSON in pm07 itself. If pm06 cannot be faithfully run page-by-page, or if semantic ownership, duplicate-action, layout-budget, or slot-contract checks fail, mark the task Failed.

If the user specifies a `design/<name>` directory, pass it to every pm-06 run. Otherwise let pm-06 auto-match valid design specs under `design/`.
