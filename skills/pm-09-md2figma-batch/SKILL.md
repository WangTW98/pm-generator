# pm-09-md2figma-batch

## 目的

批量调用 `pm-08-md2figma`，将 `product/pages/**/figma-nodes.json` 还原到用户指定的 Figma 链接中。

此 skill 用于把 pm06 生成的多页面 Figma JSON 节点文档集中写入 Figma。它只负责编排、筛选、任务追踪和失败续跑；单个页面的 Figma 还原规则必须由 `pm-08-md2figma` 执行，包括 RenderPlan、TokenResolver、组件蓝图、renderSpec、布局预算、slot 合同、重复动作检查、两阶段布局和 post-render QA。

pm09 不得压缩、改写或重新解释 `figma-nodes.json` 的业务结构。批量阶段只能把每个原始 JSON 逐个交给 pm08；不得把 JSON 转成简化 payload 后用统一脚本画页面。批量导入必须保留 pm06/pm07 写入的 `consistency`、profile refs、styleFingerprint 和 driftCheck。

## 输入

用户必须提供：

- 一个目标 Figma 链接。

用户可选提供：

- 指定扫描目录，例如 `product/pages/管理后台-PC-Web`。
- 指定页面编号，例如 `PAGE-027`。
- 指定单批数量。
- 指定 dry run，只生成任务清单不写入 Figma。
- 指定写入模式：`update`、`append`、`replace-root`。

若用户未提供 Figma 链接，必须先要求用户补充链接。不要创建新的 Figma 文件。

## 输出

必须生成批量任务清单：

`product/pages/_md2figma-batch-tasks/<YYYYMMDD-HHMMSS>-md2figma-batch.md`

每个被执行的 `figma-nodes.json` 同目录应由 pm08 生成：

`figma-restore-report.md`

## 扫描规则

默认扫描：

`product/pages/**/figma-nodes.json`

必须排除：

- `product/pages/_style-batch-tasks/**`
- `product/pages/_md2figma-batch-tasks/**`
- 任意隐藏目录。
- 任意临时、备份、草稿 JSON。

排序规则：

1. 按 `consistency.designSpecKey` 排序。
2. 按 `consistency.layoutKey` 排序。
3. 按 `consistency.pageIntentKey` 排序。
4. 按 `consistency.stateGroupKey` 排序，`null` 排在具名 state group 之前。
5. 按产品端目录和页面编号排序。
6. 父页面排在子页面之前。

旧版 JSON 若缺少 `consistency`，按产品端目录和页面编号排序，但必须在任务清单中标记 `consistency missing`。

## 批量工作流程

### 1. 发现任务

1. 扫描符合条件的 `figma-nodes.json`。
2. 对每个 JSON 读取页面编号、页面名称、设备形态、来源 sitemap。
3. 读取 `consistency.layoutKey`、`pageIntentKey`、`stateGroupKey`、`designSpecKey`、profile refs、styleFingerprint 和 driftCheck。
4. 按一致性排序规则生成批量任务清单。
5. 初始状态全部标记为 `Pending`。

### 2. 预检查

对每个任务执行：

- JSON 可解析。
- 存在可还原节点。
- 优先存在 `figmaDocument.children`；若没有，必须存在 pm08 可接受的 `layoutShell.nodes` + `pageContent.defaultStateNodes`。
- 若存在 `pageIntent.primary`，记录到任务清单和报告。
- 若存在 `semanticValidation.status = failed`，该任务标记 `Failed`，不得写入 Figma。
- 若存在 `renderSpec`，必须记录并交给 pm08 使用；若缺失，标记为 `warning` 或 `Failed`，具体取决于用户是否要求严格还原。
- 若 `renderSpec.layoutBudget.status = failed`，标记为 `Failed`，不得写入 Figma。
- 若 `renderSpec.duplicateAudit` 存在 blocking duplicate primary action，标记为 `Failed`，不得写入 Figma。
- 若 `renderSpec.slotContracts` 缺失且页面属于同 `layoutKey + pageIntentKey + stateGroupKey` 分组，标记为 `warning` 或 `Failed`，具体取决于用户是否要求批量一致性。
- 若存在 `consistency`，必须记录并交给 pm08 使用。
- 若缺少 `consistency`，标记为 `warning`；若用户要求严格一致性或批量对比一致性，标记为 `Failed` 并要求先用新版 pm06/pm07 重生成。
- 若 `consistency.profileRefs` 指向的 profile 文件缺失，标记为 `warning` 或 `Failed`，具体取决于 `driftCheck.status` 和用户严格度。
- 若 `consistency.driftCheck.status = failed`，标记为 `Failed`，不得写入 Figma。
- `stateGenerationPolicy` 若存在，应为 `default-state-only`。
- 记录 `excludedStates`，但不写入 Figma。
- 检查同目录是否存在 `figma-node-notes.md`。

预检查失败时：

- 将任务标记为 `Failed`。
- 写明失败原因。
- 不阻断其他任务。

### 3. 调用 pm08

对每个通过预检查的任务，调用 `pm-08-md2figma`。

传入：

- 当前 `figma-nodes.json` 路径。
- 用户指定 Figma 链接。
- 批量写入模式。
- 当前任务编号。
- `consistencyMode=profile-locked`。
- 当前任务的 `layoutKey`、`pageIntentKey`、`stateGroupKey`、`designSpecKey`、profile refs 和 styleFingerprint。

pm09 不得绕过 pm08 直接自由写入 Figma。若因 Agent 环境限制无法真正“调用 skill”，必须逐项严格套用 pm08 的 `SKILL.md` 规则执行。

即使在批量性能优化场景，也不得：

- 合并多个页面 JSON 后丢弃原始 `figmaDocument` 层级。
- 将所有页面转成同一组 `sections/items`。
- 用固定模板替代 pm08 的递归 JSON 渲染。
- 只因节点写入成功就忽略 `pageIntent` 或 `semanticValidation`。
- 忽略 `renderSpec`、组件蓝图、Figma渲染规则、文本策略、布局约束或截图/结构 QA。
- 忽略 `renderSpec.layoutBudget`、`renderSpec.slotContracts`、`renderSpec.duplicateAudit` 或 pm08 的 post-render QA 失败结果。
- 忽略 `consistency`、profile refs、styleFingerprint、driftCheck 或 pm08 的 profile-locked 渲染模式。

### 4. 任务状态更新

每个任务必须维护状态：

- `Pending`：已发现，未开始。
- `In Progress`：正在还原。
- `Done`：已还原、生成报告，且 pm08 post-render QA 未发现 failed 项。
- `Skipped`：按用户范围或规则跳过。
- `Failed`：预检查或还原失败。

任务不得在以下情况下标记为 `Done`：

- pm08 报告 root bounds、shell collision、text overflow、incoherent overlap、blocking duplicate action、missing key region、renderer loss、fingerprint mismatch failed、layoutBudget failed 中任意 failed。
- `figma-restore-report.md` 缺失或没有记录 RenderPlan、layoutBudget、duplicateAudit、semantic key-region QA、profile/fingerprint QA。
- 页面只写入了部分节点但缺少 JSON 中的主要默认状态区域。

长批量任务应边执行边更新清单，不要只在全部结束后一次性写入状态。

### 5. Figma 写入策略

所有页面写入同一个用户指定 Figma 文件。

若 Figma 链接包含 nodeId：

- 将所有根画板写入该目标节点所在页面或容器。

若 Figma 链接不包含 nodeId：

- 在 Figma 文件中按页面生成或更新对应根画板。

根画板命名由 pm08 决定，建议：

`<页面编号> - <页面名称> - <设备形态>`

批量执行不得删除无关 Figma 内容。

## Dry Run

若用户指定 dry run：

- 只扫描、校验、生成任务清单。
- 不加载 Figma 写入工具。
- 不写入 Figma。
- 不生成 `figma-restore-report.md`，除非用户明确要求生成 dry-run 报告。

## 失败续跑

若用户要求续跑：

1. 找到最近或用户指定的 `_md2figma-batch-tasks/*.md`。
2. 只处理 `Pending`、`Failed`、`Skipped` 中用户要求重试的任务。
3. 已 `Done` 的任务默认不重复执行，除非用户指定 `force`。

## 批量任务清单格式

使用 `assets/md2figma-batch-task-template.md`。

任务清单必须包含：

- 批次信息。
- Figma 链接。
- 扫描范围。
- 写入模式。
- dry run 状态。
- 总数、成功数、失败数、跳过数。
- 每个 `figma-nodes.json` 的状态、页面名、路径、报告路径、layoutKey、pageIntentKey、stateGroupKey、styleFingerprint、driftCheck、错误原因。

## 质量检查

批量结束后必须汇总：

- 总任务数。
- 成功数量。
- 失败数量。
- 跳过数量。
- 失败路径列表。
- 需要人工检查的页面列表。
- 每个成功任务是否由 pm08 递归渲染原始 JSON 节点树。
- 每个成功任务是否命中 `renderSpec`，并完成组件适配、文本策略、布局约束和 QA 检查。
- 每个成功任务是否按 `consistencyMode=profile-locked` 还原。
- 每个成功任务是否读取 profile refs、写入 metadata、完成 fingerprint 匹配或明确记录无法匹配原因。
- 每个成功任务是否通过 pm08 的 layoutBudget、root/shell collision、duplicateAudit、text overflow、bounds、semantic key-region post-render QA。
- 同 `layoutKey`、同 `pageIntentKey`、同 `stateGroupKey` 的任务是否存在 drift warning 或 drift failed。
- 是否存在 `semanticValidation.status = warning` 的页面。
- 是否存在 `renderSpec.layoutBudget.status = warning`、duplicate warning、slot contract warning 或 pm08 post-render warning 的页面。

如果所有任务成功，明确说明完成。

## 禁止事项

- 不得在没有 Figma 链接时开始写入。
- 不得绕过 pm08 的单页还原规则。
- 不得压缩或改写原始 `figma-nodes.json` 节点树。
- 不得用统一 Figma 绘制模板替代 pm08 递归渲染。
- 不得忽略 `renderSpec` 或 pm08 的组件蓝图/渲染规则合同。
- 不得忽略 `consistency` 或 pm08 的 profile-locked 一致性合同。
- 不得生成非默认状态页面。
- 不得删除无关 Figma 页面、画板或组件。
- 不得因单个页面失败终止整个批次，除非用户明确要求遇错停止。
- 不得把任务清单写到 `design/` 目录。

## Agent 适配说明

- GPT/Codex：按本文件编排，并在每个真实写入任务中使用 pm08 规则。
- Claude：按 `agents/claude.md` 执行，重点保持任务清单和失败续跑。
- Gemini：按 `agents/gemini.md` 执行，重点保持任务发现、排序和状态更新的确定性。
