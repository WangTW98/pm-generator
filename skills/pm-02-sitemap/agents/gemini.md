# Gemini Adapter Prompt

Use the `pm-02-sitemap` skill to create or update `product/layout/<应用端>-sitemap.md` from `product/Product Overview.md`.

Read `SKILL.md` as the workflow and `assets/sitemap-template.md` as the canonical Markdown template. Preserve the exact section order, headerless status HTML table, layout ID table header, page table columns, ID formats, and user reply placeholders. Write the final document in Chinese.

## Mandatory Blocking Check For Gemini Flash

Before updating an existing sitemap or converting it to Release, run this check first:

1. Extract every layout element from `1.2.区域、分组与元素` mind map as `区域 / 分组 / 元素 / 类型`.
2. Extract every layout table row as `区域 / 分组 / 元素 / 类型` with `LYT-xxx / LYG-xxx / LYE-xxx` IDs.
3. Extract every sitemap item from `2.1.sitemap思维导图`.
4. Extract every row from `2.2.页面清单`.
5. For layout, compare:
   - `区域`
   - `分组`
   - `元素`
   - `类型`
6. For sitemap, compare:
   - name / `页面/模块`
   - parent path / `父级ID` hierarchy
   - type: exactly one of `节点`, `页面`, `状态`
7. The layout mind-map type must come from the explicit element suffix `元素名称（类型）`.
8. The sitemap mind-map type must come from the explicit suffix `（节点）`, `（页面）`, or `（状态）`.
9. If a required suffix is missing, treat the type as `类型未知`.
10. If any name, hierarchy, missing item, extra item, or type differs in either layout or sitemap, stop immediately.

When the layout check fails, do not update the file, do not output a revised sitemap, and do not continue with generation. Reply only with:

```markdown
检测到 layout 思维导图与区域分组元素表不一致，需先确认优先级后才能继续更新。

不一致项：
- <区域> / <分组> / <元素>：思维导图=<类型>，数据列表=<类型>

请选择本次更新以哪一侧为准：
1. 思维导图优先
2. 数据列表优先
3. 逐项合并
```

When the sitemap check fails, do not update the file, do not output a revised sitemap, and do not continue with generation. Reply only with:

```markdown
检测到 sitemap 思维导图与页面清单不一致，需先确认优先级后才能继续更新。

不一致项：
- <页面/模块>：思维导图=<类型>，页面清单=<类型>

请选择本次更新以哪一侧为准：
1. 思维导图优先
2. 数据列表优先
3. 逐项合并
```

If both checks fail, include both inconsistency summaries and ask the user to choose priority separately for layout and sitemap. Do not assume one answer applies to both unless the user explicitly says so.

Do not infer priority automatically. Continue only after the user chooses one of the three options.

After the user chooses a priority source, freeze that source for the current update:

- `思维导图优先` / `按思维导图为主`: do not change the selected mind map's structure, hierarchy, visible text, item order, type suffixes, or placement. Rewrite only the corresponding data table to match it. For layout, preserve/add stable `LYT-xxx`, `LYG-xxx`, and `LYE-xxx` rows. For sitemap, preserve/add stable `PAGE-xxx` rows.
- `数据列表优先` / `以列表为准`: do not change the selected table/list rows, row order, visible text, IDs, parent IDs, layout/page types, state groups, source IDs, or notes. Rewrite only the corresponding mind map to match it.
- `逐项合并`: do not change either side until the user gives item-level decisions.

Never automatically improve, flatten, rename, reclassify, or regroup the chosen priority source. If the chosen source appears to violate a hard rule, such as a layout element missing `（类型）`, invalid/duplicate layout IDs, a container marked as `页面` while it has child `状态`, generic state names, scattered same-state groups, missing type suffixes, missing page-list IDs for mind-map items, or invalid/duplicate page IDs, stop and ask for permission before making the minimal required change.

## Generation Rules

Determine application end/form files from Product Overview section `2.产品设计概览`, especially `2.3.产品端与形态表`.

Use only three `页面类型` values: `节点`, `页面`, and `状态`. Never output `页面类型 = Tab`, `弹窗`, `页面组`, or any other custom type. Tabs are represented as `页面类型 = 状态`. Modal/dialog/drawer items that need separate layout documents are represented as `页面类型 = 页面` with `呈现形式：弹窗/抽屉/对话框` in `备注/关联待确认ID`. Use `STATE-xxx` in `状态组` only for `页面类型 = 状态`, with same-function states and tab views sharing the same state-group ID. If a page is split into state rows, mark the parent/container row as `节点` unless the protected mind map intentionally keeps it as a standalone `页面`. Rows sharing the same `状态组` must have one clear semantic state-group container and should not be scattered under intermediate branch nodes. Give each `状态` row a specific standalone state name, and record `状态组名称：<名称>` in `备注/关联待确认ID` when the state group represents a multi-step flow.

In `2.1.sitemap思维导图`, every item must include an explicit type suffix: `（节点）`, `（页面）`, or `（状态）`, and that suffix must match the corresponding `2.2.页面清单` row's `页面类型`.

In `1.2.区域、分组与元素`, the layout mind map and layout table must fully mirror each other: every mind-map element has exactly one table row, every table row appears exactly once in the mind map, and `区域 / 分组 / 元素 / 类型` reconstruct the same tree. Every layout mind-map element must include `元素名称（类型）`, and the suffix must match the table's `类型`.

`2.2.页面清单` must fully mirror `2.1.sitemap思维导图`: every mind-map item has exactly one `PAGE-xxx` row, every row appears exactly once in the mind map, and `父级ID` / `层级` reconstruct the same tree. When `思维导图优先` is selected, preserve the mind map exactly and rewrite the page list to match it, adding stable `PAGE-xxx` rows for mind-map items that do not yet exist.

If updating an existing sitemap, treat direct user edits in sections `1` and `2` as approved content unless superseded by newer instructions, and preserve stable layout IDs and `PAGE-xxx` IDs unless newer source evidence or user review input supersedes them.

If the user requests a formal/release/正式版 document, update the same canonical file `product/layout/<应用端>-sitemap.md` in place; never create `release/layout/` or any separate release copy. Set `文档类型` to `Release` and remove `## 3.待确认与假设` and `## 4.用户补充说明` completely before returning the document.
