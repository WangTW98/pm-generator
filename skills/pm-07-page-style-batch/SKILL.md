---
name: pm-07-page-style-batch
description: "Batch-generate render-ready Figma JSON node documents for every product/pages/**/layout.md by orchestrating repeated pm-06-page-style runs. Use when an AI assistant needs to scan product/pages, find page layout Markdown files, resolve or auto-match pm-05 design specs, create or update figma-nodes.json and figma-node-notes.md next to each layout.md, and preserve pm-06's default-state-only, renderSpec, layout profile, page intent profile, state group profile, semantic ownership, duplicate-action, layout-budget, slot-contract, and fingerprint consistency rules. Maintains a resumable batch task list under product/pages/_style-batch-tasks/. Supports GPT/Codex, Gemini, Claude, and other AI agents through the same canonical workflow."
---

# PM 07 Page Style Batch

## Goal

Batch-generate Figma JSON node documents for page layout files under:

```text
product/pages/**/layout.md
```

This skill orchestrates repeated single-page `pm-06-page-style` runs. It does not define Figma node generation rules itself. Every generated page must preserve `pm-06` behavior:

- Generate only the default-state page structure.
- Generate `pageIntent` and `semanticValidation` for every page.
- Generate `renderSpec` with component adapters, layout constraints, text policies, layout safety, and QA expectations.
- Generate and reuse `consistency` metadata, including layout/page-intent/state-group profile refs and style fingerprints.
- Merge the source sitemap global layout.
- Use the selected pm-05 design spec JSON files.
- Exclude empty/loading/error/success/modal-open/drawer-open and other non-default state variants from generated Figma nodes.
- Record excluded states in `figma-nodes.json` and `figma-node-notes.md`.

Important: pm07 is only an orchestrator. It must not generate `figma-nodes.json` by applying one shared generic page template across all pages. If the runtime cannot invoke or faithfully perform pm06 page-by-page, mark affected tasks as `Failed` with reason `pm06 invocation unavailable` instead of creating schema-valid but semantically generic JSON.

For each source `layout.md`, output beside it:

```text
figma-nodes.json
figma-node-notes.md
```

This skill creates and maintains a batch task list so long runs can be resumed.

## Cross-Model Compatibility

Support GPT/Codex, Gemini, Claude, and other capable AI assistants through the same orchestration rules.

- Treat this `SKILL.md` as the batch orchestration definition.
- Treat `pm-06-page-style/SKILL.md` and its templates as the source of truth for each generated page style output.
- Treat `assets/page-style-batch-task-template.md` as the batch task list schema.
- Keep `agents/openai.yaml` only as GPT/Codex UI metadata; it is not the source of truth.
- Use `agents/gemini.md` and `agents/claude.md` as lightweight adapter prompts in Gemini or Claude environments.
- Never create model-specific variants of generated `figma-nodes.json`.

## Source Selection

Default behavior:

1. Recursively scan `product/pages/`.
2. Select every file exactly named `layout.md`.
3. Exclude files under task or archive directories:
   - `product/pages/_batch-tasks/`
   - `product/pages/_style-batch-tasks/`
   - any path segment beginning with `_`
4. Sort paths ascending for deterministic execution.

The user may narrow scope by specifying:

- One `product/pages/**/layout.md`.
- One directory under `product/pages/`.
- One or more `PAGE-xxx` IDs.
- One source sitemap name such as `客户端-PC-Web` or `管理后台-PC-Web`.
- A parent path such as `product/pages/管理后台-PC-Web/PAGE-027-订单管理`.

If the narrowed scope finds no `layout.md`, stop and ask the user to run `pm-03-page-layout` or check the path.

## Design Spec Selection

The user may specify one design directory, for example:

```text
design/ant-design-admin
```

If specified, pass that design directory to every `pm-06-page-style` invocation.

If not specified, each page should let `pm-06-page-style` resolve the design spec using its own selection rules.

Batch-level optimization:

1. Scan `design/*/` for valid pm-05 design spec directories.
2. A valid directory must contain:
   - `design-spec.md`
   - `tokens.json`
   - `figma-tokens.json`
   - `component-rules.json`
   - `component-blueprints.json`
   - `figma-render-rules.json`
   - `ai-generation-rules.json`
   - `consistency-rules.json`
   - `layout-profile-schema.json`
   - `page-intent-patterns.json`
3. Parse all JSON files before starting normal generation.
4. If there is exactly one valid design directory, record it as the batch default.
5. If there are multiple, do not choose globally unless the user specified one; allow each `pm-06` page run to match locally, or stop if a page-level match is ambiguous.

If no valid design spec exists, stop and ask the user to run `pm-05-design-spec` first.

## Generation Modes

Inspect the user's current instruction before creating or resuming the task list.

Dry-run triggers:

- `测试`
- `dry run`
- `dry-run`
- `预演`
- `试运行`
- `不生成文档`
- `不要生成文档`

Directory-only triggers:

- `只生成目录结构`
- `仅生成目录结构`
- `只生成目录`
- `仅生成目录`
- `仅创建目录`
- `目录结构测试`

Mode selection:

1. `DirectoryOnly` wins when any directory-only trigger is present.
2. Otherwise use `DryRun` when any dry-run trigger is present.
3. Otherwise use normal `Development` generation.

Mode behavior:

- `Development`: invoke `pm-06-page-style` once for each eligible `layout.md`.
- `DryRun`: do not invoke `pm-06`; compute and record expected outputs only.
- `DirectoryOnly`: create parent directories only; because outputs live beside existing `layout.md`, this usually records that directories already exist. Do not create JSON or notes placeholders unless the user explicitly requests placeholders.

Development mode is successful only when each page output is produced by pm06's full workflow: source extraction, page-intent recognition, semantic validation, default-state filtering, sitemap layout merge, design-token mapping, renderSpec generation, and final quality gate.

## Consistency Profile Orchestration

pm07 must treat consistency as a batch-level contract, not as an incidental per-page detail. It still delegates page JSON generation to pm06, but it is responsible for stable ordering, profile reuse, and drift detection across the batch.

Before processing normal generation tasks:

1. Read every candidate `layout.md` and extract `来源Sitemap`, `使用Layout`, `页面清单ID`, page name, and device shape.
2. Resolve the design spec that each page will use. If design resolution is ambiguous, mark only that page `Failed`.
3. Derive or request pm06 to derive these keys deterministically:
   - `layoutKey`: from source sitemap, device shape, global layout shell, and `使用Layout`.
   - `pageIntentKey`: from pm06 `pageIntent.primary` and stable page business intent.
   - `stateGroupKey`: from same-layout alternate state pages, such as phone login, email login, and QR login under the same login group. Use `null` when no state group exists.
   - `designSpecKey`: from the selected pm05 design directory and version.
4. Group tasks in this order: `designSpecKey` -> `layoutKey` -> `pageIntentKey` -> `stateGroupKey` -> page id.
5. Ensure profile directories can be created or reused under:

```text
product/style-profiles/<layoutKey>/
```

Expected profile files:

```text
layout-profile.json
intent-<pageIntentKey>-profile.json
state-<stateGroupKey>-profile.json
```

6. Let the first successfully generated page in a group create missing profiles through pm06. Every following page in the same group must reuse those profiles.
7. If a later page proposes a different locked value for an existing profile, mark it `Failed` or `warning` according to pm06 drift rules; do not silently accept split fingerprints.
8. If a profile changes during a batch because the user explicitly asked to regenerate the design baseline, rerun or mark pending every page that already used the old profile in the affected group.

The task list must record profile refs, profile status, and fingerprint/drift result for every page. A task may not be marked `Done` unless its `figma-nodes.json.consistency` is present and consistent with the resolved profile group.

## Batch Task List

Before processing any page, create or resume a batch task list.

Task list directory:

```text
product/pages/_style-batch-tasks/
```

Task list path:

```text
product/pages/_style-batch-tasks/<BATCH-ID>-page-style-batch.md
```

`<BATCH-ID>` format:

```text
YYYYMMDD-HHmmss-<scope-slug>
```

Rules:

1. Use `assets/page-style-batch-task-template.md` as the fixed task list template.
2. Create `product/pages/_style-batch-tasks/` if it does not exist.
3. If the user asks to resume and provides a task list path, use that file.
4. If the user asks to resume but does not provide a task list path, find the latest task list whose `执行状态` is `Running`, `Paused`, or `Failed`, or whose task table contains `Pending`, `In Progress`, or `Failed`.
5. If no resumable task list exists, create a new one.
6. Do not invoke `pm-06-page-style` until the task list has been written.

Task status values:

- `Pending`
- `In Progress`
- `Done`
- `Planned`
- `Directory Checked`
- `Skipped`
- `Failed`

Task list initialization:

- Fill `0.任务状态`.
- Set `生成模式` to `Development`, `DryRun`, or `DirectoryOnly`.
- Record selected scope and design spec mode.
- Add one row in `1.设计规范清单` for every valid design spec directory.
- Add one row in `1.1 一致性Profile清单` for every resolved layout/profile group.
- Add one row in `2.页面样式生成任务清单` for every candidate `layout.md`, including `layoutKey`, `pageIntentKey`, `stateGroupKey`, profile status, and fingerprint status.
- Expected outputs:
  - `<layout.md directory>/figma-nodes.json`
  - `<layout.md directory>/figma-node-notes.md`

## Orchestration Procedure

For each eligible task row:

1. Mark task `In Progress`.
2. Read source `layout.md`.
3. Confirm it contains `来源Sitemap`, `使用Layout`, and `页面清单ID`.
4. Confirm the referenced `来源Sitemap` exists.
5. Resolve the page's profile group and load any existing `product/style-profiles/<layoutKey>/` files.
6. In `Development`, invoke or perform `pm-06-page-style` for that single `layout.md`.
   - Pass existing profile refs when they exist.
   - Instruct pm06 to create missing profiles only when this page is the first successful page in its group.
   - Instruct pm06 to keep `figma-nodes.json.consistency` and profile fingerprints stable.
   - Instruct pm06 to fail or warn on locked-property drift instead of generating a visually divergent page.
7. In `DryRun`, do not generate files; mark `Planned` and record expected outputs and expected profile refs.
8. In `DirectoryOnly`, confirm the source directory exists; mark `Directory Checked`.
9. After success, parse `figma-nodes.json` as JSON.
10. Confirm `figma-node-notes.md` exists.
11. Confirm `pageIntent.primary` exists.
12. Confirm `semanticValidation.status` is `pass` or `warning`.
13. Confirm `semanticValidation.semanticOwnershipAudit`, `semanticValidation.duplicateAudit`, and `semanticValidation.layoutBudgetAudit` exist or are explicitly marked not applicable.
14. Confirm `renderSpec` exists and includes `componentInstances`, `textPolicies`, `constraints`, `layoutBudget`, `slotContracts`, `duplicateAudit`, and `qaExpectations`.
15. Confirm top-level `consistency` exists and includes `layoutKey`, `pageIntentKey`, `designSpecKey`, `profileRefs`, `styleFingerprint`, and `driftCheck`.
16. Confirm referenced profile files exist unless `profileStatus` explicitly says the page created them during this run.
17. Confirm output is not mechanically generic by checking pm06 intent-specific required regions from the selected design spec and `renderSpec.qaExpectations`.
18. Confirm same-profile pages share locked profile values, compatible fingerprints, compatible slot contracts, and compatible layout budgets.
19. If `renderSpec.layoutBudget.status = failed`, `semanticValidation.status = failed`, or `renderSpec.duplicateAudit` contains a blocking duplicate primary action, mark the task `Failed` and do not count it as `Done`.
20. Mark task `Done` with completion time, output paths, profile refs, fingerprint status, semantic audit status, layout budget status, and duplicate audit status.
21. On failure, mark task `Failed` and record the failure reason.
22. Append an execution log row.

When invoking `pm-06-page-style`, pass:

- The single `layout.md` path.
- The user-specified design directory, if any.
- The instruction that output must remain default-state-only.
- The instruction that output must include a complete `renderSpec` and must fail rather than emit visually unsafe or generic JSON.
- The instruction that output must include `consistency` metadata and must reuse existing layout/page-intent/state-group profiles when present.
- The instruction that output must include semantic ownership, duplicate-action, layout-budget, and slot-contract audits, and must fail blocking duplicate primary actions or unsafe layout budgets.

Do not alter `layout.md` files.

Do not synthesize page structures in pm07 itself. Forbidden pm07 behavior:

- Creating every page as `Page Header + Filter Bar + Table`.
- Inferring elements directly from path names without running pm06.
- Compressing page data into a reduced intermediate format and writing that as `figma-nodes.json`.
- Marking a page `Done` only because the JSON parses.
- Marking a page `Done` when pm06 reports failed semantic ownership, blocking duplicate actions, failed layout budget, missing slot contracts, or render-blocking QA issues.
- Regenerating an existing profile differently for a later page without recording drift and rerunning affected pages.

## Resume Rules

When resuming:

- Do not rerun `Done`, `Planned`, or `Directory Checked` rows unless the user explicitly asks to rerun.
- For `In Progress` rows, inspect the expected outputs:
  - If `figma-nodes.json` parses, `figma-node-notes.md` exists, `figma-nodes.json.consistency.driftCheck.status` is `pass` or `warning`, `renderSpec.layoutBudget.status` is not `failed`, `renderSpec.duplicateAudit` has no blocking duplicate primary action, and required audit fields exist, mark `Done`.
  - Otherwise rerun the single page.
- For `Failed` rows, retry only if the user asks to retry failures or resume unfinished work.
- For `Pending` rows, continue in task-list order.

If a source `layout.md` no longer exists, mark the row `Failed` with reason `layout.md missing`.

If a generated output exists before the batch starts:

- In `Development`, update it through `pm-06-page-style`.
- In `DryRun`, mark `Planned` and record that existing output would be updated.
- In `DirectoryOnly`, leave it untouched.

## Dependency Rules

This skill depends on:

- `pm-06-page-style` for every single-page generation.
- `pm-05-design-spec` outputs under `design/*/`.
- `pm-03-page-layout` outputs under `product/pages/**/layout.md`.
- Source sitemap files referenced by each `layout.md`.
- Consistency profiles under `product/style-profiles/<layoutKey>/`, created or reused by pm06.

If any page lacks its source sitemap, mark only that page `Failed` and continue unless the user requests fail-fast behavior.

If design spec resolution fails globally because there are no valid design directories, stop before processing any task.

If design spec resolution fails for one page because multiple designs match ambiguously, mark that page `Failed` and continue unless the user requests fail-fast behavior.

## Quality Gate

Before final response, verify:

- A batch task list exists under `product/pages/_style-batch-tasks/`.
- Every candidate `layout.md` is represented in the task list.
- Every `Done` row has both:
  - `figma-nodes.json`
  - `figma-node-notes.md`
- Every `Done` row's `figma-nodes.json` parses as JSON.
- Every `Done` row has `pageIntent.primary`.
- Every `Done` row has `semanticValidation.status` equal to `pass` or `warning`.
- Every `Done` row has `semanticValidation.semanticOwnershipAudit`, `semanticValidation.duplicateAudit`, and `semanticValidation.layoutBudgetAudit`, or explicit not-applicable markers.
- Every `Done` row has `renderSpec.renderer`, `renderSpec.canvas`, `renderSpec.layoutSafety`, `renderSpec.layoutBudget`, `renderSpec.componentInstances`, `renderSpec.textPolicies`, `renderSpec.constraints`, `renderSpec.slotContracts`, `renderSpec.duplicateAudit`, and `renderSpec.qaExpectations`.
- Every `Done` row has `consistency.layoutKey`, `consistency.pageIntentKey`, `consistency.designSpecKey`, `consistency.profileRefs`, `consistency.styleFingerprint`, and `consistency.driftCheck`.
- Every same-layout page uses the same `layout-profile.json` locked values.
- Every same-intent page under a layout uses the same `intent-<pageIntentKey>-profile.json` locked values.
- Every same-state-group page uses the same `state-<stateGroupKey>-profile.json` locked values when a state group exists.
- Every same intent/state group has compatible `renderSpec.slotContracts` and compatible layout budget strategy; allowed page-specific slot differences are recorded as overrides.
- Every `Done` row satisfies pm06's intent-specific semantic quality gate; schema-only validity is insufficient.
- Every `Done` row is render-ready: major components have adapters or explicit fallbacks, and text/container constraints prevent obvious overflow or stacking during pm08 restoration.
- Every `Done` row has no blocking duplicate primary action and no failed `layoutBudget`.
- Every generated `figma-nodes.json` preserves pm-06's `default-state-only` behavior.
- Failures are recorded with actionable reasons.
- The final task list status is `Completed`, `Paused`, or `Failed`.

Report a concise summary:

- Total candidates.
- Done count.
- Failed count.
- Skipped count.
- Consistency profile groups and drift warnings.
- Task list path.
- Any next action needed.
