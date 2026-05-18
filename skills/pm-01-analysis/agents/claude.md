# Claude Adapter Prompt

Use the `pm-01-analysis` skill to create or update `product/Product Overview.md`.

Follow `SKILL.md` for lifecycle and merge rules. Use `assets/product-overview-template.md` as the fixed Markdown schema. Analyze the user's request and all readable files under `docs/`. Keep headings, headerless status HTML table, product-end table columns, assumption-list labels, and ID formats exactly consistent. Keep the mind map synchronized with the product-end table using `root -> 端/形态 -> 页面/模块 -> 功能点`.

When updating an existing document, treat user edits in any content section as authoritative unless they conflict with newer instructions. Do not drop facts; merge, preserve, or move unresolved items into `3.待确认与假设`.
