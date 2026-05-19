# pm-08-md2figma for Gemini

Convert the specified `figma-nodes.json` into editable Figma nodes in the user-provided Figma file.

Execution requirements:

1. Parse the Figma URL and source JSON.
2. Validate that the source represents default-state page structure.
3. Build a RenderPlan from `figmaDocument.children`; merge `renderSpec.componentInstances`, `renderSpec.textPolicies`, `renderSpec.constraints`, and `renderSpec.qaExpectations`; merge `consistency.profileRefs` and apply profile-locked layout/pageIntent/stateGroup rules; use `pageIntent` only for validation, not for rewriting the structure.
4. Resolve design tokens, consistency rules, component blueprints, and Figma render rules from pm05 outputs with a TokenResolver and record token/profile hits/misses.
5. Render through component adapters for common UI controls before falling back to generic editable frames; when available, fallback structure must come from `component-blueprints.json`.
6. Apply two-pass layout: create nodes and local sizes first, then set fill/hug sizing after append.
7. Skip non-default states.
8. Run key semantic-region QA, fingerprint/drift QA, layout-budget/root-bounds/shell-collision QA, duplicate-action QA, and screenshot or structural QA.
9. Write `figma-restore-report.md` beside the source JSON.

Do not infer extra states, extra pages, unrelated design variants, or a replacement fixed page template. Do not ignore `consistency`, profileRefs, styleFingerprint, or driftCheck. If append/no-overwrite is requested, always create a new root frame in empty canvas space while still using JSON profile-locked styling.
