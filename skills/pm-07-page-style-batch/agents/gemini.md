# Gemini Adapter Prompt

Use the `pm-07-page-style-batch` skill to generate render-ready page style outputs for every eligible `product/pages/**/layout.md`.

Read `SKILL.md` as the workflow and `assets/page-style-batch-task-template.md` as the task list template. Maintain a resumable batch task list under `product/pages/_style-batch-tasks/`. Group tasks by designSpecKey, layoutKey, pageIntentKey, and stateGroupKey, create or reuse `product/style-profiles/<layoutKey>/` profiles, and record profile/fingerprint/drift status. For each page, invoke the single-page `pm-06-page-style` skill once as a fresh execution to create `figma-nodes.json` and `figma-node-notes.md` next to `layout.md`. Do not carry prior page conversational context into the next page. Keep all outputs default-state-only and preserve `pageIntent`, `semanticValidation`, `renderSpec`, and `consistency`: non-default states such as empty, loading, error, success, modal-open, and drawer-open must be recorded as excluded states rather than generated as Figma nodes. Confirm each output includes component adapters, text policies, constraints, layout safety, QA expectations, profileRefs, styleFingerprint, lockedPropertiesApplied, and driftCheck.

Do not generate all pages with one shared template. User wording, generated intermediate instructions, performance concerns, or model limitations must not change the loop shape. If a separate pm06 invocation is unavailable, or if semantic ownership, duplicate-action, layout-budget, or slot-contract checks fail, fail the affected task.

Use a user-specified `design/<name>` directory for all pages when provided; otherwise use pm-06 design matching for each page.
