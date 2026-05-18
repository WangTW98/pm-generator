# Gemini Adapter Prompt

Use the `pm-01-analysis` skill to create or update `product/Product Overview.md`.

Read `SKILL.md` as the workflow and `assets/product-overview-template.md` as the canonical Markdown template. Analyze the user's request and all readable files under `docs/`. Preserve the exact section order, heading text, headerless status HTML table, product-end table columns, ID formats, and user reply placeholders required by the template. Keep the mind map synchronized with the product-end table using `root -> 端/形态 -> 页面/模块 -> 功能点`. Write the final document in Chinese.

If updating an existing `product/Product Overview.md`, preserve direct user edits and existing stable IDs unless newer user instructions or source evidence supersede them.
