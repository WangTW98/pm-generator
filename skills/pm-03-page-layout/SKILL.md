---
name: pm-03-page-layout
description: "Generate or update one Chinese page layout specification from a specified application sitemap file and page-list ID. Use when an AI assistant needs to read product/layout/<应用端>-sitemap.md, select exactly one PAGE ID, and create product/pages/<来源Sitemap Layout>/<完整父子目录链>/layout.md with reference-only sitemap/layout metadata, default-state-first visible page content, a default-state page structure diagram, detailed atomic page UI element tables, and mock data that covers controls, table columns, row actions, options, and page states. Do not inline global layout/sitemap shell elements; downstream skills should merge them later by reading 来源Sitemap and 使用Layout. Also supports single-page dry-run or directory-only checks that compute the canonical path and create zero-byte Markdown placeholders without page content. Always read page metadata and path IDs from 2.2.页面清单 and generate paths from the page-list parent tree. pm-02-sitemap must keep that list tree synchronized with 2.1.sitemap思维导图. State groups are used for baseline reuse and consistency only; they must not collapse, replace, or override the list/mind-map directory tree."
---

# PM 03 Page Layout

## Goal

Create or update exactly one page layout specification document from a user-specified sitemap file and page-list ID.

Source:

```text
product/layout/<应用端>-sitemap.md
```

Output:

```text
product/pages/<来源Sitemap Layout>/<完整父子目录链>/layout.md
```

Examples:

- `PAGE-028 客户详情`, parent chain `PAGE-041 用户中心 > PAGE-003 客户管理 > PAGE-004 客户列表`, from `管理后台 / PC Web` -> `product/pages/管理后台-PC-Web/PAGE-041-用户中心/PAGE-003-客户管理/PAGE-004-客户列表/PAGE-028-客户详情/layout.md`
- `PAGE-039 用户新增与编辑`, parent chain `PAGE-041 用户中心 > PAGE-042 内部账号管理 > PAGE-038 用户管理`, from `管理后台 / PC Web` -> `product/pages/管理后台-PC-Web/PAGE-041-用户中心/PAGE-042-内部账号管理/PAGE-038-用户管理/PAGE-039-用户新增与编辑/layout.md`
- `PAGE-031 案件办理详情-待客户资料上传与表单填写`, state group `STATE-004`, parent chain `PAGE-015 案件管理 > PAGE-016 案件列表 > PAGE-045 案件办理详情 > PAGE-031 案件办理详情-待客户资料上传与表单填写`, from `管理后台 / PC Web` -> `product/pages/管理后台-PC-Web/PAGE-015-案件管理/PAGE-016-案件列表/PAGE-045-案件办理详情/PAGE-031-案件办理详情-待客户资料上传与表单填写/layout.md`
- `PAGE-036 支付状态-进行中`, when the sitemap records `思维导图优先` and the page list mirrors the mind map path `服务商城 > 服务详情页 > 订单结算 > 支付状态 > 支付状态-进行中`, from `客户端 / PC Web` -> `product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/PAGE-0xx-支付状态/PAGE-036-支付状态-进行中/layout.md`
- `PAGE-001 登录页`, no parent, from `管理后台 / PC Web` -> `product/pages/管理后台-PC-Web/PAGE-001-登录页/layout.md`

Write the document in Chinese. Make it stable enough for downstream wireframe, design, and frontend-generation skills to parse.

When the user's instruction is a test, preview, dry run, or directory-structure-only request, compute the same canonical target path and create an empty `.md` placeholder at that path when absent, but do not generate page layout content.

## Cross-Model Compatibility

Support OpenAI/Codex, Gemini, Claude, and other capable AI assistants through the same workflow and output schema.

- Treat `SKILL.md` as the canonical process definition.
- Treat `assets/page-layout-template.md` as the canonical generation template.
- Keep `agents/openai.yaml` only as OpenAI/Codex UI metadata; it is not the source of truth.
- Use `agents/gemini.md` and `agents/claude.md` as lightweight adapter prompts in Gemini or Claude environments.
- If an environment does not support skill metadata, paste the relevant adapter prompt plus this `SKILL.md` into that assistant and require the same output path and template.
- Never create model-specific variants of page layout documents.

## Canonical Template

Before generating or updating any page layout document, open `assets/page-layout-template.md` and use it as the fixed output skeleton.

Rules:

- Preserve every heading, heading level, table column, list label, ID format, and placeholder label from the template.
- Fill placeholders with content synthesized from the specified sitemap, `product/Product Overview.md`, `docs/`, and the user's current request.
- For Release output, start from the same template and remove only sections `3.待确认与假设` and `4.用户补充说明`.
- If this file and the template appear to conflict during generation, use the template for exact Markdown structure and this file for lifecycle/merge logic.

## Source Inputs

Use the current workspace as the project root unless the user specifies another root.

Required:

- User-specified `product/layout/<应用端>-sitemap.md`.
- User-specified page-list `ID` from that sitemap's `2.2.页面清单`.

Read and synthesize:

- The specified sitemap's `0.文档状态`, `1.layout布局方式`, and `2.sitemap站点/APP地图`.
- The specified sitemap's `1.layout布局方式` and `1.2.区域、分组与元素` only as reference metadata for `来源Sitemap` / `使用Layout`; do not copy those global layout elements into page content or element tables.
- The row matching the specified `PAGE-xxx` ID in `2.2.页面清单`.
- The visible `2.1.sitemap思维导图`, including hierarchy, item text, item type suffixes, and item order.
- Other rows in the same `状态组`, when the specified row has `页面类型 = 状态` and a non-empty `状态组`; use them only as comparison context, not as additional outputs.
- Parent `节点`/`页面` rows referenced by `父级ID`.
- Child `状态` rows or modal/dialog/drawer `页面` rows that belong to the selected page group.
- `product/Product Overview.md` for product-level context and `PEF-xxx` source details.
- Files under `docs/` only as supporting context when sitemap details are ambiguous.
- Existing target layout document, if present.

If the sitemap file is missing, stop and ask the user to run `pm-02-sitemap` first. Do not generate page layout documents from `Product Overview.md` alone.

If the selected sitemap row has `页面类型 = 节点`, stop without creating or updating a page layout document and ask the user to select a child row whose `页面类型` is `页面` or `状态`.

Type semantics for this skill:

- `节点`: structure-only container. It organizes the parent-child directory tree but does not generate a page layout document.
- `页面`: standalone page. It participates in the directory tree and generates exactly one page layout document for that row.
- `状态`: state-specific page. It participates in the directory tree and generates exactly one page layout document for that row; rows in the same `状态组` together form one state-document set and should preserve shared style/common description as much as possible, changing only the state-specific portions.

## Single-Invocation Scope Rule

Each invocation may generate or update only one requested `PAGE-xxx` document.

- Always generate only the selected page row as one Markdown file.
- In Mindmap Priority Mode, eligibility still comes from the validated page-list `页面类型`, because `pm-02-sitemap` must synchronize the list so it mirrors the mind map exactly.
- If the selected row has `页面类型 = 状态` and non-empty `状态组`, use other rows in the same `状态组` only as consistency/reference context.
- Do not generate multiple unrelated pages in one invocation.
- Do not generate other pages from the same `STATE-xxx` group in the same invocation.
- Do not generate multiple different `STATE-xxx` groups in one invocation.
- If the user requests multiple page IDs, one full state group, multiple state groups, batch output such as `全部生成`, `批量生成`, `生成所有页面`, or every page in a sitemap, refuse that scope and ask for one PAGE ID for this run.
- A later orchestration skill may batch-call this skill; this skill itself must remain limited to one PAGE ID per invocation.

When generating a page that belongs to a state group:

- Keep the basic layout, element IDs, visual structure, and shared content consistent with other pages in the group.
- Mark which elements change by state in the element table's `状态/数据分类` and `状态差异说明` columns.
- Generate only the current selected PAGE document.
- Do not generate or update sibling same-state-group page documents.

## Test / Directory-Only Mode

Before entering document lifecycle logic, inspect the user's current instruction for test intent.

Dry-run triggers include:

- `测试`
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

1. `DirectoryOnly` wins when any directory-only trigger is present.
2. Otherwise use `DryRun` when any dry-run trigger is present.
3. Otherwise use normal `Development` or `Release` generation.

For `DryRun`:

- Read the specified sitemap and selected `PAGE-xxx` row.
- Validate that the row is eligible.
- Compute the canonical output path using the Target Path Rules.
- Scan for same-state-group baseline documents only to report what normal generation would reuse.
- Create the canonical parent directory.
- If the canonical target file does not exist, create an empty zero-byte `.md` placeholder file (`0 bytes`) with no headings, template, comments, or page-layout content.
- If the canonical target file already exists, preserve it exactly; do not truncate, overwrite, or update it.
- Return/report the selected row, canonical output path, canonical parent directory, placeholder status, state group, and baseline candidate if any.

For `DirectoryOnly`:

- Perform the same validation and canonical path computation as `DryRun`.
- Create the canonical parent directory for the selected row.
- If the canonical target file does not exist, create an empty zero-byte `.md` placeholder file (`0 bytes`) with no headings, template, comments, or page-layout content.
- If the canonical target file already exists, preserve it exactly; do not truncate, overwrite, or update it.
- Return/report the selected row, canonical output path, created directory path, placeholder status, state group, and baseline candidate if any.

If the selected sitemap row has `页面类型 = 节点`, still stop without creating a page layout document. In `DryRun` or `DirectoryOnly`, report that the row is a navigation/grouping node and no output directory or document should be generated for that row.

`DryRun` and `DirectoryOnly` are primarily for orchestration tests from `pm-04-page-layout-batch`; they must not consume tokens generating section `1` or `2` page content. The placeholder file must be zero-byte and must not use `assets/page-layout-template.md`.

## Mindmap Priority Mode

Use Mindmap Priority Mode when any of the following is true:

- The current user instruction says `思维导图优先`, `按思维导图为主`, `以思维导图为准`, or equivalent wording.
- The source sitemap's `3.待确认与假设` or `4.用户补充说明` records that the latest synchronization used `思维导图优先`.
- The source sitemap contains a user note stating that the current `2.1.sitemap思维导图` is the correct structure.

In Mindmap Priority Mode, the visible `2.1.sitemap思维导图` is the canonical structure, but `pm-03` must still read machine data from `2.2.页面清单`. Therefore, before computing any path, verify that the page list fully mirrors the mind map. Once verified, use the page-list `PAGE-xxx`, `父级ID`, `层级`, `页面/模块`, and `页面类型` for path computation because those fields must reconstruct the exact mind-map tree.

Mind-map parsing rules:

1. Parse only the `2.1.sitemap思维导图` Mermaid `mindmap` block for the source sitemap.
2. Preserve each item exactly as visible, including Chinese text, English casing, step labels, order, and type suffix.
3. Extract each item's visible name and type suffix. Accepted type suffixes are `（节点）`, `（页面）`, and `（状态）`.
4. Match a mind-map item to a page-list row by explicit leading `PAGE-xxx` when present. Otherwise match by exact visible name after removing the type suffix.
5. If duplicate visible names exist in the mind map, disambiguate by parent path plus visible name. If that still cannot identify exactly one page-list row, stop and ask the user to add a unique `PAGE-xxx` prefix to the ambiguous mind-map item or synchronize the page list with `pm-02-sitemap`.
6. If the selected `PAGE-xxx` row cannot be found in the mind map, stop and ask the user to synchronize the page list from the mind map with `pm-02-sitemap`.
7. If the mind map contains any structural item that has no matching page-list row, stop and ask the user to run `pm-02-sitemap` with `思维导图优先` so the list gains a stable `PAGE-xxx` row. Do not create text-only path segments.
8. If the page list contains any row that is absent from the mind map, stop and ask the user to run `pm-02-sitemap` so the two representations are synchronized.

Mindmap-priority output path rules:

1. Validate that the page list mirrors the mind map exactly by name, type, parent path, row presence, and order where possible.
2. Build the root-to-selected-item chain from the page-list `父级ID` after validation.
3. Format every folder segment as `<PAGE-ID>-<页面/模块>`.
4. Include the selected item itself as the final directory segment.
5. Store the Markdown file inside that final directory as `layout.md`.
6. Do not insert `<STATE-ID>-<状态组名称>` directories in Mindmap Priority Mode.
7. Do not flatten, regroup, rename, reclassify, or move mind-map items to satisfy page-list or state-group rules.
8. Do not create `ROOT-` fallback directories.

Mindmap-priority examples:

- Mind map path `服务商城（页面） > 服务详情页（页面） > 订单结算（页面） > 支付状态（页面） > 支付状态-进行中（状态）`, after page-list synchronization gives `支付状态` its own `PAGE-xxx`, must generate:
  `product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/PAGE-0xx-支付状态/PAGE-036-支付状态-进行中/layout.md`
- Mind map path `个人中心（节点） > 我的服务（页面） > 服务详情（页面） > 服务详情-Step02-资料审核（状态） > 服务详情-Step02-资料审核-通过（状态）`, after page-list synchronization gives `服务详情` its own `PAGE-xxx`, must generate:
  `product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-0xx-服务详情/PAGE-042-服务详情-Step02-资料审核/PAGE-043-服务详情-Step02-资料审核-通过/layout.md`

## State Group Scope and Name Resolution

For any selected row whose `页面类型 = 状态` and `状态组` is non-empty, use `状态组` only for baseline reuse, shared state consistency, and document metadata. Do not use `状态组` to compute, collapse, replace, or override output paths.

State-group path rule:

- Always compute the output path from the page-list parent chain.
- Always include the selected row's own `<PAGE-ID>-<页面/模块>` as the final directory segment.
- Never insert a `<STATE-ID>-<状态组名称>` directory into the output path.
- Never use longest common parent-chain prefix to move same-state rows into a shared directory.
- `状态组名称` can still be recorded and used in the document body and baseline notes, but not in folder names.

State-group rows:

1. Read all rows in the same sitemap whose `页面类型 = 状态` and whose `状态组` equals the selected row's `状态组`.
2. These rows form one state-group family even if some rows currently have different intermediate parent nodes.
3. Do not move rows to a different directory solely because they share a `状态组`.

State-group directory segment:

1. Do not create a state-group directory segment.
2. Resolve `<状态组名称>` for baseline notes and document metadata in this order:
   - First, parse `备注/关联待确认ID` on any same-group row for `状态组名称：<名称>` or `状态组名称=<名称>`.
   - Otherwise, infer a shared semantic prefix from same-group `页面/模块` names, such as `支付状态` from `支付状态-进行中/成功/失败`, or `服务详情` from `服务详情-step01/...`.
   - Otherwise, use the nearest common parent node/page name from the page-list parent chain.
   - If no reliable name can be found, use `<状态组>-未命名状态组` and add a `C-xxx【待确认】` item asking the user to name the state group.

State page names:

- Each state row should have a specific standalone state name. If a row name is generic, such as only `通过`, `驳回`, `成功`, or `失败`, preserve the file name from the sitemap but add a `C-xxx【待确认】` item asking the user to rename the state row with page/process context.

## Same-State Baseline Reuse Rule

Before generating a page whose sitemap row has a non-empty `状态组`, scan the target grouping scope for existing Markdown files that:

- Include the same `状态组` value in `0.文档状态`.
- Are not the current target PAGE file, unless updating the current file itself.

The target grouping scope is:

- The selected row's nearest meaningful page-list ancestor directory, plus descendant Markdown files below it.
- Prefer existing files with exact same `状态组` metadata.
- Do not require all documents with the same `状态组` to live in one directory.
- Do not create or search a mandatory `<状态组>-<状态组名称>` directory.

Choose the baseline in this order:

1. Highest-version same-state-group document with `文档类型 = Release`.
2. If no Release baseline exists, highest-version/latest same-state-group document with `文档类型 = Development`.
3. If neither exists, generate from the sitemap and template normally.

If a same-state-group baseline exists:

- Reuse its layout shell, shared layout elements, shared page content structure, element IDs, group IDs, mock data categories, and visual/content conventions wherever applicable.
- Modify only the portions that must differ for the current page state, such as state labels, empty/error/success content, enabled/disabled actions, warnings, table row states, and state-specific mock data.
- Preserve shared `PLE/PGR/PEL` IDs where the element represents the same conceptual element.
- Create new IDs only for elements unique to the current state.
- Record the reused baseline file path and baseline document type (`Release` or `Development`) in section `1.1.页面目标与范围`.

If no same-state-group baseline document exists, generate from the sitemap and template normally, and add an `A-xxx【假设】` item noting that no same-state baseline existed for this state group.

## Target Path Rules

Create `product/pages/<来源Sitemap Layout>/<完整父子目录链>/` during normal document generation, `DryRun`, or `DirectoryOnly` mode. In test modes, create only the directory and empty placeholder file, not page content.

Derive `<来源Sitemap Layout>` from the specified sitemap:

1. Prefer the sitemap status table row `产品端与形态`, such as `客户端 / PC Web`.
2. If missing, use the sitemap filename without `-sitemap.md`, such as `客户端-PC-Web`.
3. Sanitize it using the same filename rules below.

Derive `<完整父子目录链>` from the sitemap `2.2.页面清单`:

0. If Mindmap Priority Mode is active, first validate that the page list fully mirrors the mind map; then use the page-list parent chain because it must reconstruct the exact mind-map hierarchy.
1. Build an ID-indexed map from every row in `2.2.页面清单`.
2. Starting from the selected `PAGE-xxx` row, follow `父级ID` upward until the root, then reverse the result to get the root-to-current chain.
3. Preserve every valid ancestor row in the chain, not only rows whose `页面类型 = 节点`.
4. Format every directory segment as `<ID>-<页面/模块>`, for example `PAGE-041-用户中心`.
5. If the selected row has no `父级ID` and has no `状态组`, the directory chain is the selected row itself: `<PAGE-ID>-<页面/模块>`.
6. If the selected row has an empty `状态组`, the directory chain includes the selected row itself. The md file is stored inside that directory as `layout.md`.
7. If the selected row has a non-empty `状态组`, keep using the same full parent chain as any other row. `状态组` must not alter the directory path.
8. If the selected row has `页面类型 = 节点`, do not generate a page layout document; report that the row is a navigation/grouping node and should be skipped.
9. Sanitize every directory segment using the same filename rules below.
10. Never add a `ROOT-` prefix to generated page paths.

Mindmap Priority Mode overrides:

- Validate that the page-list parent IDs, row names, and types match the mind-map chain exactly; if not, stop and ask the user to run `pm-02-sitemap` synchronization.
- Use page-list IDs and names in folder and file names after validation.
- If the mind-map item type differs from the page-list `页面类型`, stop and ask for `pm-02-sitemap` synchronization; do not choose one silently.
- `状态组` must not create, remove, or move directories.
- Missing intermediate mind-map nodes are not allowed in path generation; every mind-map item must have a page-list row and `PAGE-xxx` ID.

Canonical examples:

- Non-state page with full parents:
  `product/pages/管理后台-PC-Web/PAGE-041-用户中心/PAGE-003-客户管理/PAGE-004-客户列表/PAGE-028-客户详情/layout.md`
- Popup under a parent page:
  `product/pages/管理后台-PC-Web/PAGE-011-订单管理/PAGE-012-订单列表/PAGE-013-订单详情/layout.md`
- State rows follow the page-list tree:
  `product/pages/管理后台-PC-Web/PAGE-015-案件管理/PAGE-016-案件列表/PAGE-045-案件办理详情/PAGE-031-案件办理详情-待客户资料上传与表单填写/layout.md`
  `product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-0xx-服务详情/PAGE-042-服务详情-Step02-资料审核/PAGE-043-服务详情-Step02-资料审核-通过/layout.md`

File name:

- Always use `product/pages/<来源Sitemap Layout>/<完整父子目录链>/layout.md`.
- The page identity belongs to the final directory segment `<PAGE-ID>-<页面/模块>`. In Mindmap Priority Mode, this table value must match the mind-map visible name before generation proceeds.
- Do not include `状态组ID` in the file name. The state group remains recorded in the status table.
- Generate exactly one target file per invocation.
- Do not rename the file per page; the fixed file name is always `layout.md`.
- Do not create combined `STATE-xxx` files.
- Do not create raw `STATE-xxx` directories or `<STATE-xxx>-<状态组名称>` directories for new paths.
- Do not create `ROOT-<来源Sitemap Layout>` or any other `ROOT-` fallback folder.
- Do not flatten the chain to only the nearest node ancestor.
- Same-state-group documents may live in separate page-name directories when the page-list/mind-map tree places them there. Preserve the tree.

Sanitize file and folder names:

- Replace `/`, `\`, `:`, `*`, `?`, `"`, `<`, `>`, `|` with `-`.
- Replace whitespace runs with `-`.
- Keep Chinese characters.

## Canonical Path

Before reading, updating, or writing a page layout document, compute the canonical target path from the rules above.

Canonical target path:

```text
product/pages/<来源Sitemap Layout>/<完整父子目录链>/layout.md
```

Path handling rules:

1. Always use the canonical target path for all new writes and updates.
2. Never create or update a page layout Markdown file directly under `product/pages/<来源Sitemap Layout>/`.
3. The final directory segment identifies the page; the file inside it is always `layout.md`.
4. In `DryRun` and `DirectoryOnly`, only create a new empty placeholder `.md` at the canonical target path when that target file does not already exist.

## Document Lifecycle

Before writing, check whether the canonical target page layout document already exists at the canonical path above.

### New Document

If the target layout document does not exist:

- Generate a Development document by default.
- In `0.文档状态`, write the fixed headerless two-column HTML status table with:
  - `文档类型` = `Development`
  - `文档版本` = `V1`
  - `生成日期` = current local date in `YYYY-MM-DD` format
  - `来源Sitemap` = sitemap file path
  - `使用Layout` = source sitemap `产品端与形态` value or inferred layout name
  - `页面清单ID` = the single `PAGE-xxx` ID for this document
  - `状态组` = `STATE-xxx` or blank
- Include sections `3.待确认与假设` and `4.用户补充说明`.

### Existing Development Document

If the target layout document exists and the user did not explicitly request Release/formal output:

- Parse the full existing document, including status table, page layout section, mock data section, section `3.待确认与假设`, and section `4.用户补充说明`.
- Treat non-empty content in `3.待确认与假设` and `4.用户补充说明` as the user's structured review input for this run.
- Apply confirmed decisions, corrections, and supplementary requirements into sections `1` and `2`.
- Re-read the source sitemap and the user's current prompt, then update the whole layout for consistency.
- Increment the version one step by parsing the numeric suffix: `V1` to `V2`, `V2` to `V3`, `V3` to `V4`, and so on without an upper limit. If the existing version is malformed or missing, set the next version to `V2` and add an `R-xxx【风险/资料缺口】` item noting the repaired version source.
- Keep `文档类型` as `Development`.
- Refresh `生成日期`.
- Rewrite section `3` with only remaining or newly discovered open questions and assumptions.
- Clear incorporated notes from section `4` and leave the standard placeholder.

### Release Document

If the user explicitly requests formal, release, production-ready, 正式版, 正式文档, 发布版, or release版 output, this Release path overrides Development updates.

- Set status-table `文档类型` to `Release`.
- If creating from scratch, set version to `V1`; if updating an existing document, increment the `Vn` numeric suffix by one.
- Refresh `生成日期`.
- Incorporate confirmed decisions from existing sections `3` and `4` wherever possible.
- Remove sections `3.待确认与假设` and `4.用户补充说明` completely, including headings and all content.
- Do not leave unresolved-question language in the Release document. If critical uncertainty remains, make a conservative explicit decision in the relevant section.

Mandatory Release post-processing:

- If `## 3.待确认与假设` exists, delete it and everything after it through the end of section `4` if present.
- If `## 4.用户补充说明` exists, delete it and everything under it.
- Verify the status table contains `<tr><td>文档类型</td><td>Release</td></tr>`.
- Do not output placeholder confirmation items such as `C-000` in Release documents.

## Default-State Structure Diagram Rule

Section `1.4.默认状态页面结构图` is a visual structure diagram for the current page's default state. It is not a synchronized mirror of `1.5.页面元素清单`, and changes to `1.4` must not trigger a blocking question or rewrite of `1.5`.

Generation rules:

- Generate `1.4` from `1.3.1.默认状态页面结构`.
- Use Mermaid `flowchart TB` unless impossible.
- Show the default-state page body structure: page, page areas, content groups, and the main visible default-state elements inside each group.
- Include only current-page body content. Do not include global topbar, sidebar, footer, logo, app-level navigation, global user menu, global notification controls, or upstream layout/sitemap shell nodes.
- Do not use `1.4` as the source of truth for `1.5`. `1.5` remains the detailed atomic element table and may contain finer-grained element rows than the structure diagram.
- Do not force every `1.5` row into `1.4`. The diagram should clarify the default screen structure, not exhaustively duplicate the element table.
- Do not describe non-default states as complete alternate layouts in `1.4`. Non-default states belong in `1.3.3.状态清单`, `1.3.4.状态差异说明`, `1.5.页面元素清单`, and `2.2.Mock数据表` as deltas from the default state.
- If a non-default state changes a default-state container in a way that is important for layout generation, show at most a short note node attached to the affected default-state group, such as `状态差异见 1.3.4`, not a full sibling page structure.

## User Editing Rule

Users may modify any page layout content, including sections `1`, `2`, `3`, and `4`.

The only protected part is the document structure:

- Do not change required heading names, numbering, or nesting.
- Do not change required table column names.
- Do not remove required sections in Development documents.
- Do not change ID formats.

When updating an existing Development document, treat direct edits in sections `1` and `2` as already-approved page layout content unless they conflict with newer user instructions or source documents. Continue to use sections `3` and `4` as optional structured review channels, not as the only valid editing locations.

## Output Structure

Use exactly these top-level sections for Development documents:

```markdown
# <页面/模块> Page Layout

## 0.文档状态

<table>
  <tr><td>文档类型</td><td>Development</td></tr>
  <tr><td>文档版本</td><td>V1</td></tr>
  <tr><td>生成日期</td><td>YYYY-MM-DD</td></tr>
  <tr><td>来源Sitemap</td><td>product/layout/<应用端>-sitemap.md</td></tr>
  <tr><td>使用Layout</td><td><产品端与形态 / layout名称></td></tr>
  <tr><td>页面清单ID</td><td>PAGE-001</td></tr>
  <tr><td>状态组</td><td></td></tr>
</table>

## 1.页面布局说明
### 1.1.页面目标与范围
### 1.2.使用的layout与状态
### 1.3.完整页面内容
### 1.4.默认状态页面结构图
### 1.5.页面元素清单

## 2.Mock数据
### 2.1.数据分类说明
### 2.2.Mock数据表

## 3.待确认与假设

## 4.用户补充说明
```

For Release documents, use the same structure but omit sections `3` and `4`.

## Section Guidance

### 0.文档状态

Use a headerless two-column HTML table, never subheadings, bullet lists, or a Markdown pipe table.

Required rows:

- `文档类型`
- `文档版本`
- `生成日期`
- `来源Sitemap`
- `使用Layout`
- `页面清单ID`
- `状态组`

`使用Layout` records the source sitemap status value `产品端与形态` when available, such as `客户端 / PC Web` or `管理后台 / PC Web`; otherwise use the sanitized sitemap filename. This is a reference for downstream layout-composition skills. It must not cause this page document to inline global layout elements.

### 1.1.页面目标与范围

Summarize:

- Selected sitemap row IDs.
- Page/module name.
- Page type: `页面` or `状态`.
- User role and core scenario.
- Parent node/page and child states/modals.
- Source `PEF-xxx` IDs.

`页面类型 = 节点` rows are traversal/entry containers only and must not produce page layout Markdown. If a structure item also needs its own page document, `pm-02-sitemap` should model that item as `页面`; if it needs multiple state-specific documents, `pm-02-sitemap` should add child `状态` rows with a shared `状态组`.

### 1.2.使用的layout与状态

This section is a reference-only layout declaration. It tells downstream skills which sitemap/layout to merge with this page later, but it must not describe or inline the global app shell.

Explicitly state:

- Which source sitemap provides the layout.
- Which `产品端与形态` / layout is used.
- Which page container, navigation context, or parent page chain this page belongs to.
- Which upstream sitemap section downstream skills should read for global app-shell composition, normally `1.layout布局方式` and `1.2.区域、分组与元素`.
- Whether the current page itself is a full page, modal/dialog/drawer-like page, or a state page.
- For state groups, list the state group and sibling state names as reference metadata only.

Use a Markdown table with exactly these columns:

| 引用项 | 值 | 说明 |
|---|---|---|

Rules:

- Do not list global topbar, sidebar, footer, logo, user menu, notification, app navigation, breadcrumb, or other upstream layout elements as page content.
- Do not copy the source sitemap's `1.2.区域、分组与元素` rows into this document.
- Do not create `Sitemap Layout` rows in `1.5.页面元素清单`.
- If a downstream skill needs the global shell, it must read the referenced `来源Sitemap` and `使用Layout` and merge the app shell outside this page document.
- Only mention active navigation or parent chain as reference context, not as visible element rows.
- If the source sitemap lacks enough layout reference metadata, add a section `3` assumption or risk item, but still do not inline global layout elements.

### 1.3.完整页面内容

Describe the complete visible page content in structured prose or bullets. This section is the primary human-readable screen description; a product manager, designer, frontend engineer, or AI agent must be able to understand what the current page itself looks like without opening any other file.

- Header/title area.
- Primary content blocks.
- Forms, tables, cards, timelines, lists, filters, actions.
- Empty/loading/error/success states when relevant.
- Modal or drawer content when a `页面` row has `呈现形式：弹窗/抽屉/对话框` in `备注/关联待确认ID`.
- State-specific differences for state groups.

Do not describe global layout/sitemap shell content here. Exclude topbar, sidebar, footer, logo, app-level navigation, global user menu, and global notification controls unless one of those controls is part of the current page body itself.

Required subsection structure:

#### 1.3.1.默认状态页面结构

Describe the default, initial, or most common state of the current page. This is the only full-page baseline description. All regions, groups, element order, table columns, form fields, buttons, modal triggers, and repeated item structures should be described here.

#### 1.3.2.默认状态元素细节

List every default-state visible element at the required granularity: buttons, inputs, dropdowns, radio/checkbox options, tabs, table columns, row actions, cards, timeline nodes, dialogs, empty placeholders embedded in default containers, and validation hints.

#### 1.3.3.状态清单

Provide a state list table with exactly these columns:

| 状态ID | 状态名称 | 状态类型 | 触发条件 | 影响区域/元素 | 是否默认状态 | 布局处理方式 |
|---|---|---|---|---|---|---|

The state list must include default state plus all current-page UI states and business states that can appear in this document, such as `默认列表态`, `空状态`, `加载状态`, `错误状态`, `成功状态`, `待审核`, `已通过`, `已驳回`, `支付成功`, or `支付失败`.

#### 1.3.4.状态差异说明

Describe only how each non-default state differs from the default state. Do not restate the entire page layout for every state.

Mandatory detail level:

- Do not stop at a component inventory; describe the actual page content a user sees.
- For every visible content area, state its title, layout position, visible fields, default values or placeholders, helper text, validation hints, empty-state text, and primary/secondary actions.
- For every button or action, state the visible label, enabled/disabled/loading state, trigger condition, and resulting state change or destination.
- For every text input, textarea, password input, number input, date picker, upload control, switch, checkbox, radio group, segmented control, tab, dropdown, cascader, or search field, state the label, placeholder/default value, available options when applicable, validation rule, and disabled/required state.
- For every table, state the table name, toolbar actions, filters, all visible column headers in order, important cell content types, row-level actions, pagination/sorting/selection behavior, empty state, and every row status type that can appear on the current page.
- For every card/list/timeline/stepper, state the repeated item fields, badges/status chips, item actions, ordering rule, and empty/loading/error variants.
- For every modal, drawer, popover, toast, confirmation dialog, or inline alert that can appear from this page, describe its trigger, title, body fields, actions, and success/failure feedback.
- For `页面类型 = 状态`, clearly separate shared page content from current-state-only content. Name the current state and list all visible differences from sibling states in the same `状态组`.
- If source materials do not explicitly define a visible element that is required for the workflow, make a conservative assumption, include the element, and record the assumption in section `3`.
- The default state is the layout baseline. Later states reuse default-state regions, groups, and IDs unless the state list explicitly says a region is replaced.
- Non-default states must be expressed as deltas: replaced text, changed data, disabled/enabled actions, added alert, shown modal/drawer, replaced table rows, or substituted empty/loading/error content.
- Do not generate multiple full-page descriptions for sibling states in the same page document.

### 1.4.默认状态页面结构图

Provide a Mermaid `flowchart TB` unless impossible. This diagram must be generated from `1.3.1.默认状态页面结构`.

Hierarchy:

`页面` -> `页面区域` -> `内容分组` -> `默认状态可见元素`

Rules:

- The structure diagram must show the default-state visual body structure of the current page.
- The structure diagram must not be treated as a synchronized copy of `1.5.页面元素清单`.
- Do not require every `1.5` element row to appear in the diagram.
- Do not rewrite `1.5` from the diagram, and do not rewrite the diagram from `1.5`; regenerate the diagram from `1.3.1.默认状态页面结构`.
- Every node label should be human-readable and can include IDs where helpful, such as `查询筛选区 - PGR-001`, but IDs are not mandatory for every leaf node.
- Do not include global layout/sitemap shell nodes.
- Do not draw full non-default state structures. Put state handling in `1.3.3`, `1.3.4`, `1.5`, and `2.2`.
- Do not duplicate the full page structure once per state.
- Do not include sitemap/layout shell nodes such as topbar, sidebar, footer, logo, app navigation, global user menu, or notification controls.
- For state groups, shared elements should appear once; changed elements should include state/category children that name the state delta.

### 1.5.页面元素清单

Provide a stable Markdown table with exactly these columns:

| ID | 元素来源 | 区域 | Group ID | 分组 | Element ID | 元素 | 类型 | 状态/数据分类 | 是否状态差异元素 | 状态差异说明 | 数据来源 | 交互/校验规则 | 备注/关联待确认ID |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|

Rules:

- Use `PLE-001`, `PLE-002`, `PLE-003` for page layout element row IDs.
- `元素来源` must be one of: `Page Content`, `Derived State`, `Mock Data`.
- Do not use `元素来源 = Sitemap Layout`.
- Do not include global layout/sitemap shell rows from the source sitemap.
- If an element is page-specific and no sitemap layout ID exists, create page-local IDs:
  - `PGR-xxx` for page content groups.
  - `PEL-xxx` for page elements.
- Each table row represents one element in one group in one area.
- Do not combine multiple elements in one row.
- `状态/数据分类` identifies dropdown values, filter categories, table row categories, status chips, tab values, modal states, or state-group values.
- Default-state elements must appear first and use `状态/数据分类 = 默认状态` or a more specific default data category such as `默认状态/表格行数据`.
- For state groups, mark elements that differ between states with `是否状态差异元素 = 是`; otherwise use `否`.
- Preserve unchanged `PLE/PGR/PEL` IDs during updates.

Element granularity rules:

- Every visible control must have its own row. This includes buttons, icon buttons, text inputs, textareas, password inputs, verification-code inputs, number inputs, date/time pickers, file uploads, dropdowns, cascaders, radio options, checkbox options, switches, segmented controls, tabs, search fields, links, status chips, alerts, toasts, and inline validation messages.
- Every complex container must have a container row plus rows for critical child elements. For example, a table must have a `表格` container row, one row for each visible column header, one row for each row-level action, and rows for status chips or special cells that affect behavior.
- Table column rows should use `类型 = 表格列` or a more specific type such as `表格列/状态标签`, and `状态/数据分类` should name the table data category and, when relevant, the row status category.
- Form field rows should use specific control types such as `输入框`, `密码框`, `验证码输入框`, `下拉框`, `单选Radio`, `复选框`, `日期选择器`, `文件上传`, `开关`, or `文本域`; do not use broad labels like `表单项` when the control type is known or inferable.
- Selection controls must expose their options. For dropdowns, tabs, segmented controls, radio groups, checkboxes, and status filters, create either one row per option or one parent control row whose `状态/数据分类` lists every option explicitly. Prefer one row per option when options affect content or validation.
- Button rows must include the actual visible label, not only `主操作按钮` or `操作入口`. If the exact label is not in source materials, infer a concrete label from the workflow and mark it as an assumption.
- For empty/loading/error/success states, create `Derived State` rows for the visible state container, message text, retry/return action, and any state-specific illustration or alert if present.
- For modal/drawer/dialog content, use a distinct `PGR-xxx` group and include trigger action, title, body fields, footer buttons, and close/cancel behavior as separate rows.
- Avoid vague element names such as `动态内容`, `详情信息`, `操作按钮`, `列表`, `表单`, or `状态提示` for page-specific content. Use concrete names such as `订单编号列`, `退款申请按钮`, `审核结果Radio-通过`, or `资料附件上传框`.
- The element table must be detailed enough that the Mermaid mind map can visually reconstruct the page's main structure and that `2.2.Mock数据表` can provide data for every data-bearing element.
- State-difference rows must describe only the changed element, not duplicate the full default structure.
- `状态差异说明` must be phrased as a delta from default state, such as `默认表格替换为空状态文案`, `提交按钮由启用变为禁用`, or `显示驳回原因文本域`.

### 2.Mock数据

Mock data must be created from the data categories in `1.5.页面元素清单`. The mock section is not a loose example list; it is the data backing the visible page described in section `1`.

Default-state mock data must be complete enough to render the default page. Non-default state mock data must only describe the changed data, messages, enum/status values, or replacement content relative to the default state.

#### 2.1.数据分类说明

List data categories that need mock data, such as:

- 默认状态数据集
- 状态差异数据集
- 下拉选项
- Tab选项
- 状态枚举
- 表格行数据
- 卡片数据
- 时间线节点
- 消息/通知
- 文件/附件
- 用户/角色

#### 2.2.Mock数据表

Provide a stable Markdown table with exactly these columns:

| Mock ID | 关联元素ID | 数据分类 | 字段 | 示例值 | 数据类型 | 适用状态组/页面类型 | 备注 |
|---|---|---|---|---|---|---|---|

Rules:

- Use sequential IDs starting at `MOCK-001`; create as many rows as needed to cover all required examples.
- Each mock row maps to one element or one data category.
- Every `关联元素ID` must match either an `ID` or an `Element ID` from `1.5.页面元素清单`.
- Default-state mock rows must appear first.
- Include enough rows to demonstrate realistic list/table/card/dropdown content.
- Cover every visible option, enum, state chip, tab value, radio value, checkbox value, dropdown option, table row status, empty/loading/error/success state, and current state-group value that can appear on the current page.
- For every table, include mock data for all visible column headers. Use one mock row per field per representative sample row, and identify the sample row and status in `备注`, such as `示例行1/待审核`, `示例行2/已通过`, or `空状态`.
- For every list/card/timeline/stepper, include mock data for every visible repeated-item field and at least one example for each status type shown on the current page.
- For every form control, include placeholder/default value, valid sample value, and invalid/empty-state example when validation affects the UI.
- For state-group pages, include the current page state plus all visible state categories referenced by the current page's controls, timeline, status chips, or table rows. Do not generate sibling page documents, but the current document's mock data must show the state examples needed to understand this state screen.
- Empty/loading/error/success mock rows should reference the affected `Derived State` elements and should not repeat all default-state table/card/form data.
- Mock rows must not reference sitemap/layout shell elements.
- Do not include sensitive real personal data.

## Assumptions and Confirmation IDs

In Development documents, section `3.待确认与假设` must be a Markdown list with numbered IDs and a user reply position for each item.

Use this format:

```markdown
- A-001【假设】
  - 内容：...
  - 影响范围：...
  - 用户回复：
- C-001【待确认】
  - 内容：...
  - 影响范围：...
  - 用户回复：
- R-001【风险/资料缺口】
  - 内容：...
  - 影响范围：...
  - 用户回复：
```

Use:

- `A-xxx` for reasonable assumptions made because sources are incomplete.
- `C-xxx` for decisions the user should confirm or correct.
- `R-xxx` for source limitations, conflicts, or risks that affect page layout definition.

If no open items remain in a Development document, still keep section `3` and write exactly:

```markdown
- C-000【待确认】
  - 内容：暂无待确认项。
  - 影响范围：无。
  - 用户回复：
```

## User Supplement Section

In Development documents, section `4.用户补充说明` is the user's scratch area for the next review cycle. After incorporating prior notes, leave:

```markdown
用户可在此补充新的页面布局想法、确认项修改或元素范围调整：

```

## Quality Checklist

Before finishing:

- Ensure the invocation scope is exactly one PAGE ID.
- Ensure the selected sitemap row is not `页面类型 = 节点`.
- In normal `Development` / `Release` mode, ensure the output file is directly under `product/pages/<来源Sitemap Layout>/<完整父子目录链>/`.
- In `DryRun` and `DirectoryOnly`, ensure the canonical parent directory and empty placeholder `.md` file were created only when the canonical target file did not already exist.
- In `DryRun` and `DirectoryOnly`, ensure any existing canonical target file was preserved exactly and was not truncated, overwritten, or updated.
- In `DryRun` and `DirectoryOnly`, ensure any newly created placeholder file is zero-byte and does not use the page layout template.
- Ensure `<完整父子目录链>` is derived from the complete sitemap parent chain, not only the nearest `节点`; in Mindmap Priority Mode, ensure the sitemap parent chain was first validated against the complete mind-map chain.
- Ensure `<完整父子目录链>` always includes the selected row's own `<PAGE-ID>-<页面/模块>` segment, regardless of whether `状态组` is blank or non-empty.
- Ensure no path inserts a raw `STATE-xxx` folder or a `<STATE-xxx>-<状态组名称>` folder.
- Ensure the current `PAGE-ID` is not written as a direct child file under `product/pages/<来源Sitemap Layout>/`.
- In normal `Development` / `Release` mode, ensure the generated file represents exactly one sitemap row.
- Ensure state-group sibling pages are not generated or updated in the same invocation, while the current state's document still reuses shared baseline structure/style so the whole state-document set remains as consistent as possible.
- If the current row has `状态组`, ensure baseline candidates were scanned under the nearest meaningful page-list ancestor scope and exact same `状态组` metadata was preferred. Do not scan or create a mandatory state-group directory.
- Ensure each document is in Chinese.
- Ensure Development documents include sections `3` and `4`; Release documents do not.
- For Release documents, fail and revise if `## 3.待确认与假设`, `## 4.用户补充说明`, `C-000`, `A-001`, or `用户回复：` appears anywhere.
- Ensure `0.文档状态` is the required headerless two-column HTML table.
- Ensure state-group pages keep shared element style/content consistent.
- Ensure `0.文档状态` records both `来源Sitemap` and `使用Layout`.
- Ensure `1.2` is reference-only: it declares the source sitemap/layout and merge context but does not inline global app-shell elements.
- Ensure `1.3` describes the concrete visible screen, including actual labels, controls, fields, table headers, row actions, repeated item fields, dialogs, and visible state variants; fail and revise if it only contains broad module names.
- Ensure `1.3` follows default-state-first structure: `1.3.1.默认状态页面结构`, `1.3.2.默认状态元素细节`, `1.3.3.状态清单`, and `1.3.4.状态差异说明`.
- Ensure state differences are deltas from the default state and do not duplicate full page structure per state.
- Ensure `1.4` is a Mermaid `flowchart TB` default-state page structure diagram generated from `1.3.1.默认状态页面结构`.
- Ensure `1.4` contains only current page body structure and does not include global layout/sitemap shell nodes.
- Ensure `1.4` is not treated as a synchronized mirror of `1.5`; do not block updates or ask priority questions because the diagram and element table differ in granularity.
- Ensure `1.5` uses the exact element table columns and stable `PLE/PGR/PEL` IDs.
- Ensure `1.5` does not include `元素来源 = Sitemap Layout` and does not include global layout/sitemap shell rows.
- Ensure `1.5` decomposes every visible control and complex container to the required granularity: buttons, inputs, selects, radio/checkbox options, tabs, table container, table columns, row actions, status chips, modal fields, empty/loading/error/success states.
- Ensure `2.2` mock data is derived from `状态/数据分类` values in `1.5`.
- Ensure `2.2` mock data covers every data-bearing default-state element, every table column, every option/enum/status value, and every current-page state example; every `关联元素ID` must exist in `1.5`.
- Ensure default-state mock data appears first and non-default state mock data only records deltas.
- Ensure no mock row references global layout/sitemap shell elements.
- Ensure no existing element, mock item, state, layout decision, or ID was dropped without incorporating it or preserving it as an open item.
