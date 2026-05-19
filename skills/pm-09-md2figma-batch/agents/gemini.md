# pm-09-md2figma-batch for Gemini

Batch process `product/pages/**/figma-nodes.json` files and restore them into the Figma link provided by the user.

Execution requirements:

1. Discover and sort JSON files by designSpecKey, layoutKey, pageIntentKey, stateGroupKey, then page path when `consistency` exists.
2. Exclude task, hidden, temporary, backup, and draft directories/files.
3. Create `product/pages/_md2figma-batch-tasks/<batch-id>-md2figma-batch.md`.
4. Validate each JSON.
5. For each valid item, invoke `pm-08-md2figma` once as a fresh single-page execution for the original JSON tree, including `renderSpec`, consistency profileRefs, styleFingerprint, driftCheck, component blueprints, text policies, layout constraints, and QA expectations.
6. Pass `consistencyMode=profile-locked` to pm08.
7. Record Done, Failed, Skipped, profile/fingerprint/drift, layoutBudget, duplicateAudit, post-render QA, and manual-check results.

Keep the process deterministic and resumable. Do not carry prior page conversational context into the next page. User wording, generated intermediate instructions, performance concerns, or model limitations must not change the loop shape. If a separate pm08 invocation is unavailable, mark the affected task Failed with `pm08 invocation unavailable`. Do not compress all pages into a simplified payload, ignore renderSpec or consistency, inline pm08 rules, or replace pm08 recursive rendering with a shared template.
