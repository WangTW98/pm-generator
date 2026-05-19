# pm-08-md2figma for Claude

Use this skill to restore one `product/pages/**/figma-nodes.json` file into a user-specified Figma link.

Rules:

- Require a Figma link before writing.
- Restore only the default state page structure.
- Skip all non-default states and record them in `figma-restore-report.md`.
- Preserve editable Figma nodes; do not use a full-page image as the result.
- Build a RenderPlan from `figmaDocument.children` before writing; do not replace the page with a fixed generic template.
- Merge `renderSpec.componentInstances`, `renderSpec.textPolicies`, `renderSpec.constraints`, and `renderSpec.qaExpectations` into the RenderPlan when present.
- Merge `consistency.profileRefs`, `styleFingerprint`, and `driftCheck` into the RenderPlan when present; apply profile-locked layout/pageIntent/stateGroup rules before fallback sizing.
- Resolve pm05 tokens through a TokenResolver instead of scattering hardcoded style values.
- Read pm05 `component-blueprints.json`, `figma-render-rules.json`, `consistency-rules.json`, `layout-profile-schema.json`, and `page-intent-patterns.json` when referenced by the source JSON; use them for editable fallback structure, overflow handling, append behavior, profile locking, drift checks, and QA rules.
- Use component adapters for Button, Input, Select, Tabs, Checkbox, Badge, Steps, Upload, Table, Card, Topbar, Sidebar, and Footer.
- Use two-pass layout: create nodes and local sizes first, then set fill/hug sizing after append.
- Preserve source node hierarchy, names, content, layout, and token references whenever possible.
- Update or create only the matching root frame; never delete unrelated Figma content.
- Use pm05 token and render JSON files when referenced by the source JSON.
- If the user asks for comparison or no overwrite, force append mode and place the new root frame in empty canvas space while still using JSON profile-locked styling.
- After writing, run semantic key-region QA, fingerprint/drift QA, layout-budget/root-bounds/shell-collision QA, duplicate-action QA, and screenshot or structural QA, then record the results in `figma-restore-report.md`.

Before any Figma write action, load the Figma use workflow required by the host Agent environment.
