---
name: pm-04-page-layout-batch
description: "Orchestrate batch page layout generation from one specified product/layout/<应用端>-sitemap.md, or from every product/layout/*-sitemap.md when no sitemap is specified, by repeatedly invoking pm-03-page-layout for eligible sitemap rows. Use when an AI assistant needs to generate page layout markdown files for many sitemap rows, or test the batch by computing canonical paths and creating zero-byte Markdown placeholders without page content, while preserving pm-03-page-layout rules. Always read candidates, metadata, and paths from 2.2.页面清单. When a sitemap indicates 思维导图优先, first verify that the page list fully mirrors 2.1.sitemap思维导图; after validation, compute batch paths from the list parent tree, which must match the mind-map hierarchy exactly. State groups must not create shared output directories."
---

# PM 04 Page Layout Batch

## Goal

Batch-generate page layout documents from one user-specified sitemap, or from all sitemap documents under `product/layout/` when the user does not specify one, by orchestrating repeated single-page calls to `pm-03-page-layout`.

When the user's instruction is a test, preview, dry run, or directory-structure-only request, execute the same sitemap parsing, candidate selection, canonical path computation, and task-list tracking, then create zero-byte Markdown placeholder files. Do not generate page layout content.

This skill does not define or create page layout document content directly. It delegates every generated page document to `pm-03-page-layout`, preserving that skill's path rules, template, Release/Development lifecycle, same-state baseline reuse, and single-PAGE generation behavior.

This skill does create and maintain a batch task list Markdown file so users can track progress and resume interrupted work.

Type semantics for batch selection:

- `节点`: structure-only container. It organizes the sitemap tree and output directories but never becomes a page-generation target.
- `页面`: standalone page. It organizes the sitemap tree and generates one page document for that row.
- `状态`: state-specific page. It organizes the sitemap tree and generates one page document for that row; all rows in the same `状态组` together form one state-document set, and batch generation must rely on `pm-03-page-layout` baseline reuse so shared style/common description stay maximally consistent, changing only the state-specific parts.

## Orchestration-Only Invariant

`pm-04-page-layout-batch` is a pure batch orchestrator. It never changes how `pm-03-page-layout` executes and never implements page layout generation itself.

For every eligible `页面` or `状态` row in normal `Development` / `Release` mode, the batch loop must be:

```text
for each eligible row in task-list order:
  mark the task In Progress
  invoke pm-03-page-layout once as a fresh single-page execution for that row only
  validate the generated page document using pm-04 batch guards
  mark the task Done or Failed
```

Hard requirements:

- Each loop iteration must invoke `pm-03-page-layout` exactly once for one `PAGE-xxx` row, except for the explicit retry allowed by the Generic Template Pollution Guard.
- Each `pm-03-page-layout` invocation must be a fresh single-page execution. Do not carry prior page-generation conversational context into the next page. Shared effects must flow only through filesystem artifacts such as source sitemaps, generated page documents, task lists, and pm-03 baseline files.
- Do not combine multiple PAGE IDs, state groups, directories, or sitemap rows into one downstream invocation.
- Do not invent a new batch generation mechanism because of user wording, generated instructions, model context, performance concerns, or perceived optimization opportunities.
- Do not inline, summarize, reinterpret, or reimplement `pm-03-page-layout` rules inside pm-04. The downstream skill's execution method is unchanged by this batch skill.
- If the runtime cannot invoke `pm-03-page-layout` as a separate skill for a task row, mark that row `Failed` with reason `pm03 invocation unavailable`; do not generate page content directly in pm-04.
- `DryRun` and `DirectoryOnly` are the only exceptions to downstream invocation. In those modes, perform only the documented path, directory, task-list, and zero-byte placeholder behavior.

## Cross-Model Compatibility

Support OpenAI/Codex, Gemini, Claude, and other capable AI assistants through the same orchestration rules.

- Treat this `SKILL.md` as the orchestration definition.
- Treat `pm-03-page-layout/SKILL.md` and its template as the source of truth for each page layout document.
- Keep `agents/openai.yaml` only as OpenAI/Codex UI metadata; it is not the source of truth.
- Use `agents/gemini.md` and `agents/claude.md` as lightweight adapter prompts in Gemini or Claude environments.
- Never create model-specific variants of generated page layout documents.

## Sitemap Source Selection

The user may specify one sitemap file:

```text
product/layout/<应用端>-sitemap.md
```

If the user specifies a sitemap file:

1. Use only that sitemap file as the batch source.
2. If the specified sitemap is missing, stop and ask the user to run `pm-02-sitemap` first.

If the user does not explicitly specify a sitemap file:

1. Scan `product/layout/` for every file matching `*-sitemap.md`.
2. Use all matched sitemap files as batch sources.
3. Preserve deterministic order by sorting matched sitemap paths by filename ascending.
4. If no matched sitemap files exist, stop and ask the user to run `pm-02-sitemap` first.
5. Do not ask the user to choose a sitemap solely because no sitemap was specified.

For each source sitemap, read especially:

- `0.文档状态` for `产品端与形态`.
- `1.layout布局方式` for app shell context.
- `2.2.页面清单` for all candidate page rows.

## Candidate Row Selection

From each source sitemap's `2.2.页面清单`, build the candidate set as follows:

0. If the source sitemap indicates `思维导图优先`, first validate that `2.2.页面清单` fully mirrors `2.1.sitemap思维导图` by item presence, name, type, parent path, and hierarchy. If it does not, stop or mark affected rows as failed and ask the user to run `pm-02-sitemap` with `思维导图优先`. After validation, still build the candidate set from `2.2.页面清单`.
1. Include rows whose `页面类型` is one of:
   - `页面`
   - `状态`
2. Exclude rows whose `页面类型 = 节点`.
3. Preserve the original sitemap table order.
4. If the user specifies a subset, filter by the requested IDs, page names, page types, parent node, or state group.
5. If the user specifies `Release`, `正式版`, `正式文档`, `发布版`, or `release版`, set normal generation mode to `Release` and pass that Release intent to every `pm-03-page-layout` invocation. In `DryRun` or `DirectoryOnly`, record the requested lifecycle intent in the task list but do not invoke `pm-03-page-layout`.

Rows with `页面类型 = 节点` are navigation/grouping containers only. They must not be passed to `pm-03-page-layout` as target pages.

In Mindmap Priority Mode, a page-list row is eligible only when its validated `页面类型` is `页面` or `状态`. If the mind map and table disagree about type or hierarchy, do not choose one silently; ask the user to synchronize with `pm-02-sitemap`.

If a mind-map item has no matching `PAGE-xxx` row, do not invent an ID and do not create a page document for it. Mark affected rows as `Failed` or stop with the reason `思维导图节点缺少页面清单ID，请先使用pm-02按思维导图优先同步页面清单`. Text-only structural path segments are not allowed.

## Test / Directory-Only Mode

Before creating or resuming the task list, inspect the user's current instruction for test intent.

Dry-run triggers include:

- `测试`
- `测试批量生成`
- `dry run`
- `dry-run`
- `预演`
- `试运行`
- `不生成文档`
- `不要生成文档`

Directory-only triggers include:

- `只生成目录结构`
- `仅生成目录结构`
- `只生成目录`
- `仅生成目录`
- `仅创建目录`
- `目录结构测试`

Mode selection:

1. `DirectoryOnly` wins when any directory-only trigger is present, even if a dry-run trigger is also present.
2. Otherwise use `DryRun` when any dry-run trigger is present.
3. Otherwise use normal `Development` or `Release` generation according to the user's lifecycle intent.

Mode behavior:

- `Development` / `Release`: create or update page layout Markdown documents by invoking `pm-03-page-layout` once per eligible row.
- `DryRun`: create or resume the batch task list, compute every eligible row's canonical output path, create the canonical parent directory, and create one empty `.md` placeholder file at the canonical output path when the file does not already exist. Do not invoke `pm-03-page-layout` and do not generate page layout content.
- `DirectoryOnly`: create or resume the batch task list, compute every eligible row's canonical output path, create the canonical parent directory, and create one empty `.md` placeholder file at the canonical output path when the file does not already exist. Do not invoke `pm-03-page-layout` and do not generate page layout content.

In `DryRun` and `DirectoryOnly`, the task list itself is still created or updated under `product/pages/_batch-tasks/` because it is the user's traceable test artifact.

In `DryRun` and `DirectoryOnly`, newly created placeholder `.md` files must be empty zero-byte files (`0 bytes`) with no headings, template, comments, or page-layout content. If the canonical target file already exists, preserve it exactly; do not truncate or overwrite it. Mark eligible rows as `Placeholder Created` when a new empty placeholder file is created, or `Placeholder Exists` when an existing target file is preserved. If directory or placeholder creation fails, mark the row as `Failed`.

## Development Update Handling

In normal `Development` batch generation, `pm-04` does not compare or reconcile page document section `1.4` with `1.5`.

Rules:

- Do not compare existing page layout documents for `1.4` / `1.5` consistency.
- Do not pause a batch because `1.4.默认状态页面结构图` and `1.5.页面元素清单` differ in granularity.
- Do not ask the user to choose a source-of-truth side for page document `1.4` / `1.5` differences.
- Do not pass page-document priority wording into `pm-03-page-layout`.
- Delegate page content generation and update behavior to `pm-03-page-layout`; `pm-03` generates `1.4.默认状态页面结构图` from `1.3.1.默认状态页面结构`.
- This does not change sitemap-level `思维导图优先`: if the source sitemap indicates `思维导图优先`, still validate `2.2.页面清单` against `2.1.sitemap思维导图` before computing eligibility and output paths.

## Generic Template Pollution Guard

In normal `Development` / `Release` generation, `pm-04` must not mark a page `Done` only because `pm-03-page-layout` produced a syntactically complete Markdown file.

After each `pm-03-page-layout` invocation, perform a lightweight semantic scan against the source sitemap row and generated `layout.md`.

Guard inputs:

- Source sitemap row fields: `页面清单ID`, `页面/模块`, `页面类型`, `父级ID`, `状态组`, `用户角色`, `核心场景`, and any available description columns.
- Generated `layout.md` sections: `1.1`, `1.3.1`, `1.4`, `1.5`, and `2.Mock数据`.

Pollution indicators:

- The generated page mechanically contains `页面标题 + 查询筛选区 + 表格 + 分页 + 行操作` while the sitemap row indicates an auth, detail, payment, wizard, document, message, profile, or form-specific default page.
- `1.3.1`, `1.4`, or `1.5` uses generic admin phrases such as `筛选区`, `默认数据表格`, `行操作-查看详情`, `批量操作`, or `分页器` without corresponding evidence in the sitemap row.
- The generated page omits the row's core scenario elements, such as phone/password/verification inputs for auth, order summary/payment action for payment, step content for wizard, or field groups/actions for form pages.
- Multiple unrelated pages receive near-identical default-state structures except for title text.

Handling:

1. If pollution is detected and the page can be retried, rerun that single `pm-03-page-layout` invocation once with an explicit constraint: preserve the sitemap row's page intent and core scenario; do not use a generic filter-table admin template unless the source row clearly describes a list-management page.
2. If the retry still looks polluted, mark the task `Failed`, record `generic template pollution` plus the evidence, and continue unless the user requested fail-fast behavior.
3. Do not repair page content inside pm04. The correction must come from a faithful single-page pm03 run.
4. This guard is product-agnostic. It checks consistency between the sitemap row and the generated page document rather than hard-coding current project page names.

## Batch Task List

Before processing any batch row, create or resume a batch task list Markdown file. In normal `Development` / `Release` mode, this must happen before any `pm-03-page-layout` invocation.

Task list path:

```text
product/pages/_batch-tasks/<BATCH-ID>-page-layout-batch.md
```

`<BATCH-ID>` format:

```text
YYYYMMDD-HHmmss-<scope-slug>
```

Rules:

1. Use `assets/page-layout-batch-task-template.md` as the fixed task list template.
2. Create `product/pages/_batch-tasks/` if it does not exist.
3. If the user asks to resume and provides a task list path, use that file.
4. If the user asks to resume but does not provide a task list path, find the latest task list whose `执行状态` is `Running`, `Paused`, or `Failed`, or whose task table contains `Pending`, `In Progress`, `Failed`, `Planned`, `Directory Created`, `Placeholder Created`, or `Placeholder Exists` rows.
5. If no resumable task list exists, create a new task list.
6. Do not create generated page content or empty placeholder files until the task list has been written.

Task list initialization:

- Fill `0.任务状态` with task ID, current dates, generation mode, source mode, and initial `执行状态 = Running`.
- Set `生成模式` to exactly one of `Development`, `Release`, `DryRun`, or `DirectoryOnly`.
- In `1.源Sitemap清单`, add one row for each source sitemap.
- In `4.执行日志` or `失败原因/恢复说明`, record `结构校验 = MindmapSynced` when Mindmap Priority Mode is active and the page list has been validated against the mind map.
- In `2.页面生成任务清单`, add one row for every sitemap table row:
  - `页面类型 = 节点` -> `状态 = Skipped`.
  - Eligible non-node rows -> `状态 = Pending`.
  - Unsupported legacy rows such as `页面类型 = Tab` -> `状态 = Skipped` with the reason `Tab已废弃，请在pm-02中改为状态`.
  - Rows excluded by a user subset filter -> `状态 = Skipped` with the reason in `失败原因/恢复说明`.
- Compute and record each eligible row's expected output path using `pm-03-page-layout` path rules.

In Mindmap Priority Mode, include every page-list row in the task list as usual, but do not override table eligibility from the mind map. The table must already mirror the mind map; if it does not, stop or mark affected rows `Failed` and ask for `pm-02-sitemap` synchronization.

Task status values:

- `Pending`
- `In Progress`
- `Done`
- `Planned`
- `Directory Created`
- `Placeholder Created`
- `Placeholder Exists`
- `Skipped`
- `Failed`

Update the task list during execution:

1. In normal `Development` / `Release` mode, before invoking `pm-03-page-layout` for a row, set that task row to `In Progress`, set `开始时间`, update `0.任务状态/更新时间`, and set `当前恢复位置` to the task ID.
2. In normal `Development` / `Release` mode, after `pm-03-page-layout` reports success, run the Generic Template Pollution Guard. Only set the task row to `Done` after the guard passes; otherwise retry once or mark `Failed`.
3. In `DryRun`, do not invoke `pm-03-page-layout`; create the parent directory and an empty `.md` placeholder at the computed output path when absent, set the row to `Placeholder Created` or `Placeholder Exists`, set `完成时间`, and record the computed output path.
4. In `DirectoryOnly`, do not invoke `pm-03-page-layout`; create the parent directory and an empty `.md` placeholder at the computed output path when absent, set the row to `Placeholder Created` or `Placeholder Exists`, set `完成时间`, and record the computed output path.
5. After failure, set the task row to `Failed`, set `完成时间`, and write the failure reason.
6. After each task, update source sitemap counts and append one row to `4.执行日志`.
7. When all eligible rows are `Done`, `Planned`, `Directory Created`, `Placeholder Created`, `Placeholder Exists`, `Skipped`, or intentionally excluded, set `执行状态 = Completed`.
8. If the run stops early by user request or interruption risk, set `执行状态 = Paused` when possible.

Resume behavior:

- Do not re-run `Done`, `Planned`, `Directory Created`, `Placeholder Created`, or `Placeholder Exists` rows unless the user explicitly asks to rerun them.
- Before resuming or trusting any existing task row, re-read the source sitemap and recompute the row's canonical output path using the current `pm-03-page-layout` path rules.
- If a task row's stored `输出路径` differs from the recomputed canonical output path, update the task list to the canonical path before checking completion or invoking `pm-03-page-layout`.
- For `In Progress` rows, first check whether the expected output path exists and appears complete enough to be a valid page layout document. If valid, mark `Done`; otherwise rerun that single PAGE ID.
- For `Done` rows whose stored output path differs from the recomputed canonical path, check the canonical path first. If the canonical file exists and is valid, keep `Done` and update the path. Otherwise mark the row `Pending` and rerun that single PAGE ID when the user asks to resume unfinished work.
- For `Planned`, `Directory Created`, `Placeholder Created`, and `Placeholder Exists` rows whose stored output path differs from the recomputed canonical path, update the output path and rerun that test-mode task when the current run is `DryRun` or `DirectoryOnly`.
- For `Failed` rows, retry only if the user asks to retry failures or asks to resume all unfinished work.
- Continue `Pending` rows in task-list order.

## Orchestration Procedure

After the task list has been created or resumed, process rows from `2.页面生成任务清单`.

For normal `Development` / `Release` generation, each eligible task row with `状态 = Pending` or a resumable `In Progress` must invoke `pm-03-page-layout` once with:

- That task row's sitemap file path.
- The single row's `页面ID`.
- The user's Release/Development intent.
- Any user-provided constraints relevant to layout generation.

The invocation must be a fresh single-page execution for that row. User instructions and intermediate model-generated instructions may refine the arguments passed to `pm-03-page-layout`, but must not change the loop shape, merge rows, or replace the downstream skill invocation with pm-04-local generation.

For `DryRun` and `DirectoryOnly`, do not invoke `pm-03-page-layout`. Instead:

1. Recompute the canonical output path using the same path algorithm defined in `pm-03-page-layout`.
2. Record the computed output path in the task row.
3. Create the computed output path's parent directory.
4. If the computed output path does not exist, create an empty `.md` placeholder file at that path and mark the row `Placeholder Created`.
5. If the computed output path already exists, preserve it unchanged and mark the row `Placeholder Exists`.
6. Append an execution log entry explaining that only an empty placeholder was created or an existing file was preserved, and no page layout content was generated.

Important:

- In normal `Development` / `Release` mode, call `pm-03-page-layout` separately for each PAGE ID.
- In normal `Development` / `Release` mode, do not combine multiple PAGE IDs in one `pm-03-page-layout` invocation.
- In normal `Development` / `Release` mode, do not ask `pm-03-page-layout` to generate a whole state group at once.
- In normal `Development` / `Release` mode, for state rows, rely on `pm-03-page-layout` to scan same-state baseline documents and reuse Release/Development baselines according to its own rules.
- In normal `Development` / `Release` mode, do not mark a generated page `Done` until the generated document has passed the Generic Template Pollution Guard.
- Continue through task rows in task-list order unless the user explicitly asks to stop on first failure.

## Output Paths

Do not invent output paths in this skill. The output path for each generated page document or test placeholder must use the same canonical path algorithm defined by `pm-03-page-layout`, currently:

```text
product/pages/<来源Sitemap Layout>/<完整父子目录链>/layout.md
```

If the source sitemap indicates `思维导图优先`, the batch path calculation must use the current `pm-03-page-layout` Mindmap Priority Mode algorithm:

1. Parse `2.1.sitemap思维导图`.
2. Validate that every mind-map item has exactly one matching page-list row and every page-list row appears in the mind map.
3. Validate that page-list `父级ID`, `层级`, `页面/模块`, and `页面类型` reconstruct the same tree as the mind map.
4. Build each eligible item's root-to-item chain from the page list after validation.
5. Format every path segment as `<PAGE-ID>-<页面/模块>`.
6. Include the selected item itself as the final directory segment and store `layout.md` inside it.
7. Do not insert `<STATE-ID>-<状态组名称>` directories.
8. Do not let `状态组` collapse or override the validated mind-map/list tree.
9. If any mind-map item lacks a matching `PAGE-xxx`, do not create output files for affected descendants; record the missing ID and ask the user to sync the page list with `pm-02-sitemap`.

For every eligible row, including rows whose `页面类型 = 状态` and whose `状态组` is non-empty, the batch path calculation must use the current `pm-03-page-layout` list-tree algorithm:

1. Build the row's root-to-selected chain from `2.2.页面清单` using `父级ID`.
2. Include the selected row itself as the final directory segment.
3. Format every segment as `<PAGE-ID>-<页面/模块>`.
4. Store the placeholder or generated document as `layout.md` inside that final directory.
5. Never create a raw `STATE-xxx` directory or `<STATE-xxx>-<状态组名称>` directory from `状态组`.

Use `状态组` only for baseline reuse, state consistency, and task metadata; it must not alter output paths.

This skill may summarize generated paths or placeholder paths after the batch is complete, but it must not alter the path rules.

## Failure Handling

If one page fails:

- Record the failed `PAGE-xxx` ID and reason.
- Continue with the remaining rows unless the user explicitly requested stop-on-error behavior.
- At the end, report generated, skipped, and failed rows.

If `pm-03-page-layout` refuses a row because it violates its single-page rules, record that as a failed row and continue.

## Progress Summary

At the end of a batch run, provide a concise summary with:

- Batch task list path.
- Generation mode: `Development`, `Release`, `DryRun`, or `DirectoryOnly`.
- Source sitemap mode: specified single sitemap, or all `product/layout/*-sitemap.md`.
- Source sitemap paths.
- Number of eligible rows.
- Number of skipped `节点` rows.
- Generated or updated file paths.
- Placeholder file paths created or preserved in `DryRun` or `DirectoryOnly`.
- Failed rows and reasons, if any.

Do not create a separate batch report file unless the user explicitly asks for one.

## Quality Checklist

Before finishing:

- If the user specified a sitemap, ensure only that sitemap file was used as the batch source.
- If the user did not specify a sitemap, ensure every `product/layout/*-sitemap.md` file was used as a batch source.
- Ensure `product/pages/_batch-tasks/<BATCH-ID>-page-layout-batch.md` exists before row processing begins.
- Ensure the task list contains one row for every source sitemap page-list row, including skipped `节点` rows.
- Ensure every task row's `输出路径` has been recomputed from the current `pm-03-page-layout` path rules before resume or execution.
- In normal `Development` mode, ensure the batch did not compare or pause on page document `1.4` / `1.5` differences; those sections are not synchronized by pm-04.
- If a source sitemap indicates `思维导图优先`, ensure `2.2.页面清单` was validated against `2.1.sitemap思维导图` before task eligibility or output paths were computed.
- Ensure each task status is updated after the corresponding invocation or test-mode path/directory operation.
- Ensure no `页面类型 = 节点` row was passed to `pm-03-page-layout`.
- In normal `Development` / `Release` mode, ensure every non-node target was handled through a separate `pm-03-page-layout` invocation.
- In `DryRun` and `DirectoryOnly`, ensure no `pm-03-page-layout` invocation occurred.
- In `DryRun` and `DirectoryOnly`, ensure empty placeholder `.md` files were created only when canonical target files did not already exist.
- In `DryRun` and `DirectoryOnly`, ensure existing canonical target files were never truncated, overwritten, moved, or otherwise modified.
- In `DryRun` and `DirectoryOnly`, ensure no generated placeholder contains any content; newly created placeholders must be zero-byte `.md` files.
- Ensure generated or placeholder target paths follow `pm-03-page-layout` rules.
- Ensure generated or placeholder target paths preserve the full sitemap parent chain and do not collapse to the nearest `节点`.
- Ensure no new placeholder or generated path inserts a state-group directory; the page-list parent tree is always the path source.
- In Mindmap Priority Mode, ensure the page-list parent tree mirrors the mind-map hierarchy exactly before generating paths.
- Ensure no new placeholder or generated path uses any `STATE-xxx` directory, with or without a semantic state-group name.
- Ensure no generated Markdown page layout file or placeholder `.md` file is a direct child of `product/pages/<来源Sitemap Layout>/`.
- Ensure no generated path contains a `ROOT-` fallback folder; path fallback behavior belongs to `pm-03-page-layout`.
- In normal `Development` / `Release` mode, ensure Release intent, when requested, was passed to every page generation.
- Ensure the final response lists skipped node IDs, generated/planned/directory-created page IDs, and failed page IDs.
