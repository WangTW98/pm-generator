# pm-09-md2figma-batch for Gemini

Batch process `product/pages/**/figma-nodes.json` files and restore them into the Figma link provided by the user.

Execution requirements:

1. Discover and sort JSON files by designSpecKey, layoutKey, pageIntentKey, stateGroupKey, then page path when `consistency` exists.
2. Exclude task, hidden, temporary, backup, and draft directories/files.
3. Create `product/pages/_md2figma-batch-tasks/<batch-id>-md2figma-batch.md`.
4. Validate each JSON.
5. For each valid item, apply pm08 restoration rules to the original JSON tree, including `renderSpec`, consistency profileRefs, styleFingerprint, driftCheck, component blueprints, text policies, layout constraints, and QA expectations.
6. Pass `consistencyMode=profile-locked` to pm08.
7. Record Done, Failed, Skipped, profile/fingerprint/drift, and manual-check results.

Keep the process deterministic and resumable. Do not compress all pages into a simplified payload, ignore renderSpec or consistency, or replace pm08 recursive rendering with a shared template.
