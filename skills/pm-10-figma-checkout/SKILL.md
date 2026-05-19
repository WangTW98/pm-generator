---
name: pm-10-figma-checkout
description: "Inspect and repair layout quality issues in a user-specified Figma canvas, frame, component, section, or node. Use when an AI assistant needs to analyze a Figma link and target canvas/frame/component for layout disorder, broken Auto Layout, overused absolute positioning, missing FILL/HUG sizing, hierarchy mistakes, reusable component drift, wrong active states, redundant generated content, or color/style inconsistency. Read product/Product Overview.md and product/layout/*.md first to infer a full reusable element catalog, navigation, page regions, sitemap page/state relationships, state variant clusters, and style tokens. Prioritize Figma remote MCP reads and use_figma."
---

# PM 10 Figma Checkout

## 目的

根据用户提供的 Figma 链接、指定画布、以及可选的指定 frame/component/section/node，对设计稿进行结构体检、问题定位和安全自动修复。

目标画布是必需输入。目标 frame/component/section/node 是可选输入：如果用户没有指定具体修改哪个 frame/component/node，则对该 Figma 画布中的所有符合条件元素一起判断并修正。

本 skill 用于检查并修复：

- 布局错乱：节点重叠、脱离父容器、元素堆叠、绝对定位漂移。
- 挤压/溢出：文本溢出、按钮文字挤压、表格列过窄、卡片内容被裁切。
- 不合理布局：间距不统一、内容区过密/过松、容器尺寸异常、画板留白异常。
- 层级错误：全局 Shell 混入 Page Body、卡片套卡片、弹窗/状态节点错误出现在默认态。
- Auto Layout 错误：应使用 Auto Layout 的分组仍为自由定位、FILL/HUG 失效、父子 sizing 冲突。
- 绝对布局滥用：应使用动态布局的内容组仍依赖固定 `x/y/width/height`，或 Auto Layout 子节点错误使用 absolute positioning。
- 动态尺寸缺失：应盛满父容器的区域未使用 `FILL`，应随内容伸缩的文本、按钮、Tag 未使用 `HUG` 或合理 min/max。
- 设计一致性错误：同 layout/pageIntent/stateGroup 的尺寸、位置、组件风格发生 drift。
- frame 之间的关联关系错误：同一画布中的页面根 frame、状态 frame、流程前后置 frame、同名/同角色复用区域之间结构不一致。
- 复用组件样式错误：导航、Topbar、Sidebar、Footer、Tabs、表格行、卡片等重复区域样式漂移，或激活态与当前页面不匹配。
- 复用组件颜色错误：同一产品文档定义的复用组件在 Figma 中出现 fill、stroke、text fill、active/inactive 色、状态色、圆角、阴影等样式 drift。
- 全量复用元素遗漏：只检查导航而忽略 sitemap 定义的 Topbar、Footer、内容区、筛选区、表单项、按钮、Tag、Card、Table Row、Metric、State Tabs、状态提示、步骤条等通用结构。
- 同 state 页面一致性错误：同一状态组下的不同页面共享区域不一致，或本应作为变体的区域被错误统一/错误拆分。
- 冗余元素：明显为页面生成说明、设计注释、临时介绍文案、非当前页面展示范围的辅助内容，却出现在默认页面画板内。

本 skill 必须先读取产品文档，再判断 Figma 复用结构：

- `product/Product Overview.md`
- `product/layout/*.md`

产品文档是复用结构、页面区域、导航、状态和样式语义的主要依据。Figma 节点名称和结构相似度只能作为补充证据。

默认优先使用 Figma remote MCP：

1. `get_metadata` 获取目标结构。
2. `get_design_context` 获取截图和代码/节点上下文。
3. `get_screenshot` 做视觉复核。
4. `get_variable_defs`、`get_libraries`、`search_design_system` 读取变量与设计系统。
5. 写入前加载 `figma:figma-use`，再用 `use_figma` 执行检查或修复脚本。

不得优先使用截图 OCR 或手工臆测替代 Figma 节点结构。截图只作为视觉 QA 辅助。

## 输入

用户必须提供：

- Figma 链接，通常为 `https://www.figma.com/design/<fileKey>/...?node-id=...`。
- 指定画布名称或画布 node-id。若 Figma URL 的 `node-id` 指向画布/PAGE，可视为已指定画布。

用户可选提供：

- 指定 frame/component/section/node 名称或 node-id。
- 修复模式：`check-only`、`safe-fix`、`aggressive-fix`。
- 允许修复范围：仅当前 frame、当前画布、同名组件实例、同状态组页面。
- 是否允许创建备份副本。

如果用户没有指定 Figma 画布：

1. 先用 `get_metadata` 读取页面/画布列表。
2. 中断当前执行，不得自动选择第一个画布，也不得进入修复。
3. 向用户列出可选画布名称/node-id，并询问“要检查并修复哪个画布？”
4. 用户回复画布后，从该画布继续执行原任务。

如果用户指定了画布，但没有指定 frame/component/section/node：

1. 将检查范围设为 `canvas-all-elements`。
2. 扫描该画布下所有顶层 frame/component/section/group 及其可见子节点。
3. 如果画布中存在多个顶层 frame/component/section，必须进入 `batch-loop` 执行模型，先建立轻量全局索引，再按批次循环检查、修复和 QA。
4. 不得一次性读取、分析或修复整张大画布的完整深层节点树。
5. 对画布内所有符合条件元素按批次做问题判断、优先级排序、备份、修复和 QA，最后汇总成全局结果。
6. 报告中必须明确“未指定具体 frame，已按全画布范围处理”，并列出批处理执行记录。

## 输出

默认生成本地报告：

```text
product/figma-checkout-reports/<YYYYMMDD-HHMMSS>-<figma-file-or-canvas>-checkout.md
```

报告必须记录：

- 输入链接、fileKey、目标 nodeId、目标画布/节点名称。
- 检查模式与修复模式。
- 产品文档读取记录、`ProductLayoutModel` 摘要和未读取原因。
- sitemap 复用元素抽取结果、Figma 复用元素聚类结果、文档到 Figma 的映射置信度。
- 同 state 页面/状态组的共享区域、变体区域和一致性检查结果。
- frame 关联关系、复用组件一致性、导航激活态检查结果。
- 复用组件颜色/样式 drift、token 匹配、已统一样式和未修复原因。
- 绝对布局与动态布局检查结果，包括命中例外的元素。
- 多 frame 画布的批处理执行计划、每批范围、每批 QA 和全局 rollup。
- 检测到的问题列表，按严重程度排序。
- 每个问题的节点 ID、节点名称、问题类型、证据、建议修复。
- 已剔除或移出展示范围的冗余元素及判定依据。
- 已自动修复项、未修复项、需要人工确认项。
- 修复前/后关键结构数据和截图/截图不可用原因。
- 是否创建备份、备份节点 ID。
- QA 结果和残余风险。

如果用户只要求“列出思路方案”，不要写入 Figma；只输出检查与修复方案。

## 修复模式

| 模式 | 行为 |
|---|---|
| `check-only` | 只分析，不修改 Figma。生成问题清单、修复建议和风险。 |
| `safe-fix` | 默认模式。只做可逆、低风险、局部修复；不删除业务节点；不改变文案语义。 |
| `aggressive-fix` | 需要用户明确要求。允许重建局部 Auto Layout、重排同组节点、批量应用 spacing/sizing，但仍不得删除无关内容。 |

安全修复允许：

- 调整 frame 宽高、min/max、padding、gap、alignment。
- 将明显同组的节点包成 Auto Layout frame。
- 修复 `layoutSizingHorizontal/Vertical` 的 `FILL`、`HUG`、`FIXED` 使用。
- 扩宽按钮、输入框、表格列、卡片容器，避免文本挤压。
- 将超出父容器的节点移回安全边界。
- 修正层级：把 page body 内容挂回内容容器，把 shell 内容挂回 shell 容器。
- 隐藏或移出明确标记为非默认状态的状态画板，但必须先备份或记录。
- 对同一画布内同角色复用区域应用一致的尺寸、间距、字体、填充、描边、圆角和层级结构。
- 修正导航/Tab/Sidebar 等复用组件的激活态：当前页面对应项 active，其余项 inactive。
- 根据产品文档和 token 基准统一复用组件颜色样式：fill、stroke、text fill、corner radius、opacity、shadow/effect、active/inactive 色。
- 对 Button、Tag、Card、Input、Select、Table Row、Sidebar Item、Tabs 等文档确认的复用组件应用 `sync-reusable-color` / `sync-active-state-style`。
- 将明显同组、规律排列且低风险的元素改为 Auto Layout，设置 padding/gap/alignment。
- 将应盛满父容器的 PageBody、Section、Card、列表项、表单控件设置为 `FILL` 或等效动态宽高。
- 将应随内容变化的标题、按钮文字、Tag、标签文本设置为 `HUG` 或保留固定宽高但添加最小尺寸/风险记录。
- 将明确不是页面展示内容的生成说明、临时注释、AI 介绍文案、示例占位说明移入 `PM10 Removed / <timestamp>` 隔离分组或隐藏，并在报告中记录。

安全修复禁止：

- 删除无关页面、画板、组件、实例或业务节点。
- 物理删除疑似冗余但无法确认的节点；`safe-fix` 下必须以移出展示范围、隐藏或隔离分组替代删除。
- 改写业务文案以适配空间，除非用户明确允许。
- 将可编辑设计转为截图或不可编辑图片。
- 静默覆盖组件主件或变量定义。
- 将不同页面套用同一固定模板。
- 仅因视觉不同就统一业务含义不同的组件样式；必须先确认它们属于同一角色/同一复用组件族。
- 覆盖有业务语义的颜色差异，例如 warning、success、error、risk、disabled、selected、processing，除非产品文档或 token 明确规定。
- 在产品文档缺失、冲突或无法确认语义时批量统一颜色；必须记录为人工检查项。
- 强行把复杂表格、图表、地图、二维码、图片裁切、Overlay、Modal、Drawer、Toast、Popover 等需要精确定位的节点改成动态布局。
- 在无法判断内容语义和响应行为时批量重排节点。
- 在没有报告的情况下批量修改。

## 工作流程

### 1. 解析目标

1. 从 Figma URL 提取 `fileKey` 和 `nodeId`。
2. 如果 URL 包含 `node-id=215-55`，转换为 `215:55`。
3. 先解析并确认目标画布。画布可来自用户明确给出的画布名称、画布 node-id，或 URL 中指向 `PAGE` 的 node-id。
4. 如果无法确认目标画布，停止执行，读取画布列表并询问用户选择；用户回复后继续执行。
5. 再解析可选的 frame/component/section/node 目标。
6. 如果用户未指定具体 frame/component/section/node，将目标范围设为整张画布，检查并修复该画布中的所有符合条件元素。
7. 如果整张画布内存在多个根 frame/component/section，设置 `executionMode=batch-loop`；若只有一个根目标，设置 `executionMode=single-target`。
8. 如果用户指定了具体节点，确认目标类型：`FRAME`、`COMPONENT`、`COMPONENT_SET`、`SECTION` 或普通 `SceneNode`，并将范围限定在该节点及其子树。

### 2. 产品文档读取与语义建模

在读取或修改 Figma 前，先读取产品文档：

1. 使用 `rg --files product` 查找 `product/Product Overview.md` 和 `product/layout/*.md`。
2. 若文件存在，读取并提取结构化信息；若不存在，报告 `product-doc-missing`，但可继续用 Figma 结构兜底。
3. 若文档互相冲突，例如导航命名、页面归属、状态定义不一致，记录 `layout-doc-conflict`，冲突项不得自动修复。
4. 生成内部 `ProductLayoutModel`，供后续 Figma 检查使用。

`ProductLayoutModel` 至少包含：

```json
{
  "productName": "",
  "navigationItems": [],
  "globalShell": {
    "topbar": {},
    "sidebar": {},
    "footer": {}
  },
  "pageRegions": [],
  "reusableComponents": [
    {
      "name": "",
      "role": "button|tag|card|input|select|tabs|table|nav|shell|state|other",
      "aliases": [],
      "requiredStructure": [],
      "styleTokens": {},
      "stateRules": {}
    }
  ],
  "reusableElementCatalog": [
    {
      "name": "",
      "role": "shell.topbar|shell.sidebar|shell.footer|region|button|tag|card|input|select|tabs|table-row|metric|state-tabs|state-panel|stepper|other",
      "scope": "client-pc-web|admin-pc-web|global",
      "source": {"file": "", "section": ""},
      "aliases": [],
      "expectedInPages": [],
      "requiredStructure": [],
      "styleRules": {},
      "stateRules": {}
    }
  ],
  "stateVariantClusters": [
    {
      "stateGroup": "",
      "parentPage": "",
      "frames": [],
      "sharedRegions": [],
      "variantRegions": [],
      "stateRules": {},
      "source": {"file": "", "section": ""}
    }
  ],
  "stateGroups": [],
  "activeStateRules": [],
  "styleTokens": {}
}
```

文档解析规则：

- 从 `Product Overview.md` 提取产品定位、核心模块、导航项、页面分组、角色和状态定义。
- 从 `product/layout/*.md` 提取全局 Shell、页面区域、分组、元素、复用组件、状态组、颜色语义、布局规则。
- 从 layout 文档的“区域、分组与元素”表格中生成 `reusableElementCatalog`：区域转 `shell/region`，分组转 `region group`，元素转 `reusable element`。
- 从 sitemap 思维导图和页面清单表格中生成页面关系图：父子页面、节点/页面/状态/弹窗、状态组、同父级状态变体。
- 从页面清单的 `状态组` 字段生成 `stateVariantClusters`；同一 `状态组` 内的 frame 必须区分共享区域与变体区域。
- 从同一 sitemap 中重复出现或跨页面共享的元素名称生成候选复用组件，例如 Button、Tab、Select、Input、Card、Table、Metric、State Tabs、步骤条、状态提示。
- 记录每条规则的来源文件和片段标题，报告中必须能追溯。
- 如果文档中出现 token 名称或样式描述，例如 `primary`、`success`、`warning`、`active`、`sidebar active`、`button primary`，写入 `styleTokens`。
- 如果文档没有明确 token，则允许从 `design/antd-admin-design-spec/tokens.json`、`figma-tokens.json` 或 Figma 变量/样式中补充。

样式基准优先级：

1. `product/layout/*.md` 中明确指定的组件/状态样式。
2. `product/Product Overview.md` 中明确指定的语义或品牌样式。
3. `design/antd-admin-design-spec/tokens.json` 或 `figma-tokens.json`。
4. Figma `get_variable_defs`、Figma styles、组件主件/实例属性。
5. 同画布中出现频率最高、无 P0/P1 问题、且符合产品语义的样式。

如果上述来源冲突，按优先级选择，并在报告中记录冲突和选择原因。

### 2.1 sitemap 复用元素抽取

必须先生成 `ReusableElementCatalog`，再进入 Figma 修复。不得只抽取导航。

抽取来源：

1. `layout布局方式`：识别全局 Shell、页面容器、侧栏、顶栏、底栏、全局工具。
2. `区域、分组与元素`：区域是一级复用结构，分组是区域内复用组，元素是可映射到 Figma 节点的复用元素。
3. sitemap 思维导图：识别页面节点、状态节点、页面流程和可复用状态入口。
4. 页面清单表格：识别 `页面类型`、`状态组`、`父级ID`、`核心场景` 和 `来源PEF-ID`。
5. 产品概览：补充交易、案件、消息、风险、支付、退款、资料审核等业务语义。

输出要求：

- 每个复用元素必须有 `name`、`role`、`scope`、`source`、`aliases`。
- 每个状态组必须形成 `stateVariantCluster`，列出共享区域和允许不同的变体区域。
- 文档未显式列出的通用 UI 组件也要从页面/区域语义中推断，例如筛选区中的 Input/Select/Button、列表区中的 Row/Status Tag/Action Button。
- 如果 sitemap 与 Figma 命名不一致，保留 aliases，不得直接判定为缺失。

### 2.2 Figma 复用元素发现与映射

进入 Figma 后必须执行 `FigmaReusableDiscovery`，扫描当前目标范围内所有候选复用元素，而不是只扫描导航。

候选识别方式：

- 名称前缀：`GlobalShell/*`、`Component/*`、`Region/*`、`Section/*`、`Card/*`、`Metric/*`、`FilterBar`、`DataCollection`、`StateGroupTabs`、`TaskCardList`、`CheckoutSteps`、`ProgressSummary`。
- 角色语义：Button、Tag、Input、Select、Tabs、Table Row、Card、Metric、Stepper、Status、Timeline、Filter、Search、ActionBar、OrderSummary。
- 结构相似度：节点类型序列、子节点数量、文本/图标/容器组合、尺寸比例、布局方向、间距模式。
- 跨 frame 重复：同名、同前缀、同位置、同用途或同状态组内重复出现。
- Figma 组件关系：如果存在 component/instance/mainComponent，优先归入同一 cluster。

映射结果必须包含置信度：

| 置信度 | 条件 | 自动修复 |
|---|---|---|
| high | 文档命中 + 名称/别名命中 + 结构相似或组件关系命中 | 可 safe-fix |
| medium | 文档命中 + 结构相似，但命名不一致或状态不明确 | 只修低风险样式/布局，重要差异需人工确认 |
| low | 仅 Figma 聚类相似，无法映射到文档 | 不自动统一，报告人工检查 |

每个 cluster 必须输出：

- `clusterName`、`role`、`sourceDoc`、`confidence`。
- `matchedNodes`：节点 ID、名称、所属根 frame。
- `baselineNode` 或 `baselineToken`。
- `sharedSignature`：结构、布局、样式签名。
- `driftNodes`、`safeFixableNodes`、`manualReviewNodes`。

### 2.3 同 State 页面关系建模

同一 `stateGroup` 下的不同页面必须建立 `StateVariantCluster`，例如支付进行中/成功/失败、登录方式状态、服务详情流程状态。

检查原则：

- 共享区域必须一致：GlobalShell、Topbar、Sidebar、Footer、StateGroupTabs、固定摘要区、公共筛选区、公共操作区。
- 变体区域允许不同：状态主视觉、状态标题、状态说明、结果操作、错误/成功/处理中提示。
- 同一 state group 的 Tabs/Segmented Control 必须只有当前状态 active。
- 不得把变体区域强行统一成同一内容；只能统一通用结构、样式 token、布局规则。
- 如果某个状态页面缺少共享区域，记录 `state-cluster-shared-region-missing`。
- 如果某个状态页面的变体区域被放到共享区域中，记录 `state-cluster-region-boundary-error`。

### 3. 远程 MCP 读取

按需读取，不一次性拉取全文件：

1. `get_metadata(fileKey, nodeId)`：快速确认节点层级、名称、尺寸。
2. `get_design_context(fileKey, nodeId)`：获取截图和结构上下文；用于判断视觉问题。
3. `get_screenshot(fileKey, nodeId)`：对目标节点或关键问题节点截图。
4. `get_variable_defs(fileKey, nodeId)`：读取颜色、间距、字体变量。
5. `get_libraries(fileKey)` / `search_design_system`：当需要确认组件规范或替换组件时使用。
6. 写入或深度检查前，加载 `figma:figma-use`，再调用 `use_figma`。

如果目标是 PAGE/画布，或用户未指定具体 frame/component/node：

- 先用 `get_metadata` 找出候选 frame/component/section，只建立轻量索引，不拉取所有深层子节点。
- 如果候选根节点超过 1 个，必须创建 `BatchExecutionPlan` 并循环执行；不得用一次超大 `use_figma` 脚本处理全部 frame。
- 将候选节点按顶层区域、页面根 frame、流程/状态关系、组件/实例、状态分组拆批。
- 建立 frame 关系图：同一页面的默认态/空态/加载态/错误态、同一功能模块的列表/详情/弹窗、同一导航体系下的多个页面。
- 识别跨 frame 复用区域：`GlobalShell/Topbar`、`GlobalShell/Sidebar`、`GlobalShell/Footer`、导航项、Tabs、表格行、筛选区、Metric Card、按钮、Tag 等。
- 扫描全部 `ReusableElementCatalog` 对应的候选节点，不得只检查导航或 Shell。
- 对同 `stateVariantCluster` 的 frame 分批检查共享区域和变体区域。
- 分批对候选节点调用 `use_figma` 结构检查和安全修复。
- 不要对整个大文件做一次超大脚本。
- 画布级修复只处理当前指定画布内元素，不跨画布修改。

### 3.1 多 frame 画布批处理执行模型

当指定画布包含多个顶层 frame/component/section，或任一批次预估深层节点过多时，必须使用循环批处理：

```json
{
  "canvas": {"nodeId": "", "name": ""},
  "executionMode": "batch-loop",
  "scope": "canvas-all-elements|target-node",
  "globalIndex": {
    "rootFrames": [],
    "reusableClusters": [],
    "stateVariantClusters": [],
    "styleBaselines": []
  },
  "batches": [
    {
      "batchId": "",
      "type": "single-frame|state-cluster|reusable-cluster|qa",
      "frameIds": [],
      "purpose": "",
      "allowedOps": [],
      "status": "pending|done|failed"
    }
  ],
  "rollup": {
    "issues": [],
    "operations": [],
    "qa": {}
  }
}
```

批处理必须按以下阶段执行：

1. **Global Index Batch**：读取产品文档、目标画布 metadata、顶层根节点列表；建立 `frameMap`、`ReusableElementCatalog` 映射候选、`stateVariantClusters`、样式/token 基准。该阶段只保留轻量摘要。
2. **Frame Local Batches**：每批处理 1 到 3 个根 frame，检查局部布局、挤压、溢出、绝对布局、FILL/HUG、冗余内容和低风险修复。
3. **State Cluster Batches**：按 `stateVariantCluster` 处理同状态组 frame，检查共享区域、变体区域、状态语义和 active 状态。
4. **Reusable Cluster Batches**：按复用组件族处理跨 frame 的 Sidebar、Topbar、Tabs、Button、Tag、Card、Filter、Metric、Table Row 等结构和样式一致性。
5. **Final QA Batch**：重新检查全局 P0/P1、跨 frame 关系、复用组件一致性、状态组一致性和残余风险。

批次大小约束：

- 每批最多处理 1 到 3 个页面根 frame。
- 同一 `stateVariantCluster` 一批最多处理 5 个 frame；超过 5 个必须继续拆分。
- 单批深层节点建议不超过 300 到 500 个；超过时按 Shell、PageBody、Overlay、State References 或主要区域拆子批。
- 单个根 frame 过大时，不得强行整体深读，应先读取一级结构，再按区域循环。
- `use_figma` 写入脚本必须只作用于当前批涉及的节点及其明确关联节点。

批处理上下文规则：

- 全局阶段只保存轻量索引、cluster baseline、状态组关系和 token 摘要。
- 每个批次只读取当前批相关节点深层结构，批次结束后将 issue、operation、QA 摘要写入 `rollup`。
- 不把所有 frame 的完整节点树、截图和设计上下文同时放入当前推理上下文。
- 跨 frame 统一必须先引用全局 baseline，再在 `Reusable Cluster Batches` 中逐组应用。
- 如果某一批失败，记录 `batch-failed` 和失败原因；除非 Global Index 失败，否则继续处理后续低风险批次。
- `safe-fix` 下每批修复前只备份该批涉及的根 frame/component/section，不复制整个画布。

### 4. 结构检查

使用 `use_figma` 读取目标节点树并计算以下指标：

| 检查项 | 触发条件 | 严重程度 |
|---|---|---|
| root bounds overflow | 子节点超出根 frame，且父节点未标记 scroll/clip | High |
| sibling overlap | 同层节点 bounding box 交叉面积超过阈值 | High |
| shell collision | Page Body 与 Topbar/Sidebar/Footer 重叠 | High |
| text overflow | 文本宽高不足、按钮/标签文字明显超出容器 | High |
| clipped content | `clipsContent=true` 且子节点被裁切 | High |
| squeezed control | Button/Input/Select 高度小于规范或文字水平空间不足 | Medium/High |
| broken auto layout | 同组节点规律排列但父容器非 Auto Layout | Medium |
| absolute-layout-overuse | 父 frame 为自由布局但子元素呈明显横向/纵向结构，且不命中例外 | Medium/High |
| absolute-child-in-autolayout | Auto Layout 父级中子节点使用 `layoutPositioning=ABSOLUTE` 且非浮层/装饰/角标 | Medium/High |
| fixed-size-content-risk | 内容可变的文本、按钮、标签、表单控件、列表项使用固定尺寸导致扩展风险 | Medium |
| missing-fill-parent | 主内容区、Section、Card、列表项、表单控件应盛满父容器却使用固定宽度 | Medium |
| missing-hug-content | 按钮、Tag、文本标签等应随内容伸缩却固定尺寸且无 min/max 策略 | Medium |
| inconsistent spacing | 同组间距差异过大且无语义原因 | Medium |
| hierarchy mismatch | shell、body、overlay、state 节点归属错误 | Medium/High |
| nested cards | 卡片套卡片且非表单分组/列表项 | Medium |
| duplicate primary action | 同一 Page Body 出现重复主操作意图 | High |
| state pollution | 空/加载/错误/弹窗打开态混入默认画板 | Medium/High |
| frame relation mismatch | 同一画布中前后置/状态/同模块 frame 的页面结构无法对应，或状态 frame 混入默认页面 | Medium/High |
| reusable component drift | 同角色复用区域的尺寸、间距、字体、颜色、边框、子节点结构明显不一致 | Medium/High |
| reusable-element-missing | sitemap 定义的复用元素在应出现的页面/frame 中缺失 | Medium/High |
| reusable-element-extra | Figma 中出现不属于当前页面/状态展示范围的复用元素 | Medium |
| reusable-element-structure-drift | 同一复用元素族的子节点结构、顺序、布局方向不一致 | Medium/High |
| reusable-element-state-drift | 同一复用元素族的 active/selected/disabled/success/warning/error/risk/processing 状态不一致 | Medium/High |
| reusable-style-drift | 产品文档定义的同一复用组件出现 fill、stroke、text fill、corner radius、shadow/effect drift | Medium/High |
| component-family-layout-drift | Button/Tag/Input/Select/Card/Table Row/Metric 等组件族布局模式、padding、gap、sizing 漂移 | Medium |
| component-family-token-drift | 同一组件族使用不同 token 或硬编码色值，且语义状态一致 | Medium |
| semantic-color-mismatch | 组件颜色与产品文档或 token 的语义状态不匹配 | Medium/High |
| active-state-style-drift | active/inactive 状态结构正确，但背景色、文本色、描边或 opacity 与基准不一致 | Medium/High |
| token-mismatch | 节点使用硬编码色值或错误 token，与产品文档/design token 不一致 | Medium |
| reusable-component-unmapped | Figma 中疑似复用组件无法映射到 ProductLayoutModel | Medium |
| sitemap-figma-mapping-missing | 文档中存在规则，但 Figma 没有可置信匹配节点 | Medium |
| sitemap-figma-mapping-low-confidence | 仅能低置信度映射，不允许自动统一 | Medium |
| state-cluster-shared-region-drift | 同状态组页面的共享区域结构或样式不一致 | Medium/High |
| state-cluster-shared-region-missing | 同状态组页面缺少应共享的区域 | Medium/High |
| state-cluster-variant-region-mismatch | 状态变体区域与当前状态语义不匹配 | Medium/High |
| state-cluster-region-boundary-error | 共享区域和变体区域边界错误，导致错误统一或错误分散 | Medium/High |
| same-state-frame-inconsistent | 同 state 或同状态组 frame 的公共结构无法对应 | Medium/High |
| active state mismatch | 导航、Sidebar、Tabs 的 active 项与当前页面名称/页面意图不匹配，或多个 active 同时存在 | High |
| redundant generated content | 页面默认展示范围内出现“页面说明/生成说明/使用说明/设计稿说明/示例文案”等非业务内容 | Medium/High |
| product-doc-missing | 未找到或无法读取产品概览/布局文档 | Medium |
| layout-doc-conflict | 产品概览和 layout 文档之间存在结构、导航、状态或样式冲突 | Medium/High |

布局检测应使用真实 Figma 几何数据：

- `absoluteBoundingBox` 或节点 `x/y/width/height`。
- 父子关系、同层关系、Auto Layout 属性。
- `layoutMode`、`itemSpacing`、padding、alignment、layout sizing。
- `visible`、`opacity`、`clipsContent`。
- TEXT 节点字符数、字体大小、行高、文本框宽度。
- 跨 frame 的同名/同前缀节点签名：节点名称、类型、子节点数量、主要尺寸、视觉 token、active/inactive 状态。
- 页面语义推断：从根 frame 名称、PageBody 名称、页面标题、导航标签、URL/PRD 页码中推断当前页面对应的导航激活项。
- 产品语义模型：用 `ProductLayoutModel` 判断节点是否属于文档定义的复用结构/组件，不得只依赖 Figma 名称猜测。
- 颜色样式签名：fill、stroke、text fill、font size/weight、corner radius、shadow/effect、opacity、active/inactive 状态。

### 4.1 产品文档驱动的复用组件识别

识别复用组件时必须按以下顺序：

1. **文档定义优先**：节点名、页面区域、组件角色命中 `ProductLayoutModel.reusableElementCatalog` 或 `ProductLayoutModel.reusableComponents`。
2. **别名匹配**：匹配文档中的 aliases、中文名、英文名、设计稿命名前缀。
3. **结构相似度匹配**：子节点数量、文本/图标/容器结构、布局方向、尺寸模式相近。
4. **页面语义匹配**：当前页面所属模块、状态组、导航项与文档一致。
5. **Figma 组件关系**：如存在 main component/instance，优先归并到同一 cluster。

每个 cluster 必须输出：

- cluster name / role。
- 来源文档和规则。
- 涉及的 Figma frame/node。
- style baseline 来源。
- 被自动修复、跳过、需要人工确认的节点。

若 Figma 节点疑似复用组件但无法映射到文档，记录 `reusable-component-unmapped`，不得自动统一样式。

必须覆盖的复用族：

- Shell：Topbar、Sidebar、Footer、PageBody、Content Container。
- 导航：Sidebar Item、Topbar Link、Breadcrumb、Tabs、Segmented Control、StateGroupTabs。
- 表单：Input、Select、Checkbox、Radio、Date、Upload、Form Row、Filter Item。
- 操作：Primary Button、Default Button、Danger Button、Link Button、ActionBar。
- 展示：Card、Metric Card、Info Panel、Order Summary、Price Summary、Progress Summary。
- 列表/表格：Table Header、Table Row、List Item、Task Card、Case Row、Pagination。
- 状态：Tag、Status Pill、Risk/Warning/Success/Error Notice、Empty/Loading/Error State。
- 流程：Stepper、Timeline Step、Progress Step、Checkout Steps。
- 业务复用区域：从 sitemap 区域/分组/元素表中抽取的任意重复结构。

### 4.2 frame 关联与复用组件一致性检查

当检查范围为画布级或包含多个 frame 时，必须额外执行关系检查：

1. **建立 frame map**：记录每个根 frame 的页面编号、页面名称、pageIntent、stateGroup、是否默认态。
2. **建立 reusable cluster**：按节点名称前缀和角色聚类，例如 `GlobalShell/Sidebar/*`、`GlobalShell/Topbar`、`Component/Tabs`、`Component/Card/Metric`、`Table/Row`。
3. **建立 state variant cluster**：按 sitemap 的 `状态组` 聚合 frame，标记共享区域和变体区域。
4. **生成结构签名**：对每个 cluster 记录子节点顺序、类型、尺寸、间距、字体大小、填充/描边、圆角、布局模式。
5. **生成状态签名**：记录 default、active、inactive、selected、disabled、success、warning、error、risk、processing 等状态。
6. **找基准样式**：优先使用产品文档和 token；其次使用组件主件/实例；最后才使用同画布高置信、无 P0/P1 问题、出现频率最高的结构。
7. **比较 drift**：同角色节点的结构、尺寸、状态或样式超出容差时，记录对应 drift issue。
8. **检查激活态**：根据当前根 frame 名称、页面标题、stateGroup、状态页面名称推断 active 项；同一个导航组/Tab 组只能有一个 active。
9. **修正策略**：只修复明确同角色且低风险的 drift；无法判断是否业务差异时记录为人工检查项。

导航激活态判定示例：

- 根 frame 名含 `待办事项`，Sidebar 中 `待办事项` 应为 active，`工作台`、`案件进度` 等应为 inactive。
- 根 frame 名含 `案件进度`，Sidebar 中 `案件进度` 应为 active。
- 登录方式页面中 Tabs 应仅激活对应登录方式：`手机号登录`、`微信扫码`、`邮箱密码`。

同状态组检查示例：

- `PAGE-036 / PAGE-037 / PAGE-038` 同属支付状态组，GlobalShell、Sidebar、StateGroupTabs、OrderSummary 应一致；状态主视觉和操作按钮可随进行中/成功/失败不同。
- `PAGE-002 / PAGE-003 / PAGE-034` 同属登录方式状态组，Topbar、Footer、AuthPanel 基础结构和 Segmented Control 应一致；表单/二维码区域可随登录方式不同。
- 服务详情流程状态同属服务详情状态组，Shell、服务摘要、流程导航、资料区容器规则应一致；当前步骤内容可不同。

### 4.3 复用组件颜色样式检查

对每个文档确认的复用 cluster 生成 `StyleSignature`：

```json
{
  "nodeId": "",
  "role": "",
  "state": "default|active|inactive|disabled|success|warning|error|risk|processing|selected",
  "fill": "",
  "stroke": "",
  "textFill": "",
  "fontSize": "",
  "fontWeight": "",
  "cornerRadius": "",
  "opacity": "",
  "effect": ""
}
```

检查规则：

1. 同一 `role + state` 的复用组件必须使用一致的 fill、stroke、text fill、corner radius、opacity 和 effect。
2. active/inactive 状态必须分别比较，不得把 active 样式套给 inactive 节点。
3. warning/success/error/risk/disabled/processing 等语义状态必须保留业务语义；只有文档或 token 明确冲突时才修复。
4. 若组件使用硬编码颜色但文档或 token 有明确值，记录 `token-mismatch`。
5. 若 Figma 中多数样式与文档基准不一致，优先遵循文档和 token，不以多数票覆盖文档。

`safe-fix` 自动统一颜色样式的条件：

- cluster 已被产品文档确认。
- baseline 来自产品文档、design token、Figma 变量/样式或明确组件主件。
- 被修复节点与 baseline 属于同一 `role + state`。
- 操作仅修改样式属性，不改业务文案、不改变节点结构、不删除节点。
- 修复前已备份目标 frame。

不满足上述条件时只记录为人工检查项。

### 4.4 冗余元素识别

冗余元素必须满足“非业务展示内容”证据，不得只因位置异常就剔除。优先识别：

- 名称含 `Intro`、`Description`、`Guide`、`Instruction`、`Prompt`、`生成说明`、`页面说明`、`设计说明`、`注释`、`临时`、`示例说明`。
- 文本内容明显描述页面如何生成、如何使用设计稿、组件说明、AI 生成提示词，而不是用户会在页面中看到的业务内容。
- 节点位于默认页面主体或首屏可见区域，但不属于当前 pageIntent 的标题、表单、列表、表格、导航、状态、操作区域。
- 与当前页面展示范围无关的跨页面说明块、PRD 摘要块、设计验收说明块。

`safe-fix` 下的剔除方式：

1. 先备份受影响根 frame。
2. 创建或复用当前画布中的 `PM10 Removed / <timestamp>` 分组。
3. 将冗余节点移动到隔离分组，或设置 `visible=false`；不得直接删除。
4. 在报告中记录节点 ID、名称、文本摘要、剔除原因和回滚方式。

`aggressive-fix` 下若用户明确允许删除，才可物理删除冗余节点；仍必须在报告中记录删除前信息和备份节点。

### 4.5 动态布局与绝对布局检查

检查 frame 内元素时，必须判断它们是否过度依赖绝对布局，并优先建议动态布局。判断顺序：

1. **识别父级布局**：读取父 frame 的 `layoutMode`、padding、gap、alignment、primary/counter axis sizing。
2. **识别子级定位**：读取子节点 `x/y/width/height`、`layoutPositioning`、`layoutSizingHorizontal`、`layoutSizingVertical`。
3. **判断排列规律**：若同组子节点 x 或 y 有稳定间距、尺寸相近、名称/类型同族，却父级 `layoutMode=NONE`，记录 `absolute-layout-overuse`。
4. **判断内容弹性**：根据文本长度、节点名称、组件角色判断宽高是否应动态变化。
5. **检查父容器填充**：PageBody、Section、Card、列表项、表单控件等需要跟随父容器宽度变化时，应优先 `FILL` 或等效动态宽度。
6. **检查内容包裹**：按钮、Tag、文本标签、导航项、Tab 项等内容驱动节点，应优先 `HUG` 或使用 min width 加水平 padding。
7. **应用例外规则**：命中例外的节点不强制改为动态布局，但报告中应记录“命中例外/无需修复”。

优先使用动态布局的对象：

- 表单区、筛选区、搜索区、按钮组、Tabs、导航列表。
- 卡片组、Metric Card、列表项、表格行的非复杂区域。
- 主内容区 PageBody、Section、Panel、Card 容器。
- 文本长度可变的标题、副标题、按钮、Tag、placeholder、表格单元格。

允许保留绝对布局的例外：

- 根页面 frame，例如 PC Web 画板 `1440x1024`。
- Shell 外层固定区：Topbar、Sidebar、Footer 的根容器位置。
- Overlay、Modal、Drawer、Toast、Popover、Tooltip、Dropdown 等浮层定位。
- 背景、遮罩、分割线、装饰层、图标内部路径、角标、状态点。
- 表格列在没有真实表格组件能力时的固定列定位；仍需检查列宽风险。
- QR、地图、图表、图片裁切、视频、画布预览等固定视觉容器。
- 需要精确叠放的头像组、步骤线、时间轴连接线、进度条轨道。

动态布局建议：

- 主内容容器：优先 `layoutSizingHorizontal = "FILL"`，高度按页面策略 `FILL` 或固定。
- Section/Card/ListItem/FormControl：优先横向 `FILL`，内部用 Auto Layout 管理 padding/gap。
- 文本、按钮、Tag、Tab/Nav item：优先 `HUG`，必要时设置 min width 和 padding。
- 双列/多列筛选区：父级 horizontal Auto Layout，子控件 `FILL` 或固定 min width，避免硬编码 x。
- 纵向列表：父级 vertical Auto Layout，列表项 `FILL`，gap 来自设计 token 或局部基准。

`safe-fix` 自动修复边界：

- 只修复低风险、局部且结构明确的动态布局问题，如按钮/Tag HUG、表单控件 FILL、列表项 FILL、简单纵向/横向 stack。
- 对复杂表格、图表、地图、二维码、浮层、状态机画板，只记录建议，不自动改。
- 转换为 Auto Layout 前必须保存 `before/after`，且确保节点视觉顺序和业务文本不变。
- 如果转换后产生更多 overlap、clip 或内容丢失，必须回滚本次转换或标记失败。

### 5. 问题分级

| 等级 | 定义 | 自动修复策略 |
|---|---|---|
| P0 | 画板无法使用：主内容不可见、大面积重叠、根节点被裁切 | `safe-fix` 可修复明显边界/尺寸；复杂重排需要确认 |
| P1 | 关键流程受损：主按钮、表单、表格、导航、状态被挤压或重叠，或导航激活态错误 | 默认自动修复 |
| P2 | 体验问题：间距不统一、层级弱、局部拥挤、复用组件样式 drift、颜色/token drift、明显冗余说明内容、绝对布局滥用、动态尺寸缺失 | 自动修复低风险项，其余记录 |
| P3 | 视觉 polish：微小对齐、命名、非阻塞变量缺失 | 默认只记录，除非用户要求 polish |

### 6. 生成修复计划

在写入前必须形成 `RepairPlan`：

```json
{
  "target": {"fileKey": "", "nodeId": "", "name": ""},
  "mode": "check-only|safe-fix|aggressive-fix",
  "executionMode": "single-target|batch-loop",
  "batchExecutionPlan": {
    "globalIndexReady": true,
    "currentBatchId": "",
    "batchCount": 0,
    "batchOrder": []
  },
  "backup": {"required": true, "strategy": "duplicate-target-frame"},
  "issues": [],
  "operations": [
    {
      "op": "resize|move|wrap-auto-layout|set-auto-layout|set-padding-gap|set-layout-sizing|set-fill-parent|set-hug-content|convert-stack-to-autolayout|reparent|sync-reusable-style|sync-reusable-color|sync-reusable-stroke|sync-text-color|sync-active-state-style|apply-product-token|normalize-component-style|normalize-reusable-element|sync-component-family-layout|sync-component-family-token|sync-state-shared-region|set-active-state|quarantine-redundant|hide-non-default-state",
      "nodeId": "",
      "reason": "",
      "before": {},
      "after": {},
      "risk": "low|medium|high"
    }
  ],
  "qa": {}
}
```

`RepairPlan` 还必须包含复用元素和状态组维度：

```json
{
  "reusableMappings": [
    {
      "clusterName": "",
      "confidence": "high|medium|low",
      "baseline": "",
      "nodes": []
    }
  ],
  "stateVariantClusters": [
    {
      "stateGroup": "",
      "frames": [],
      "sharedRegions": [],
      "variantRegions": [],
      "operations": []
    }
  ]
}
```

`safe-fix` 只能执行 `risk=low` 或明确可逆的 `risk=medium` 操作。`risk=high` 必须要求用户确认，除非用户要求 `aggressive-fix`。

多 frame 画布下，`RepairPlan` 必须分为全局计划和批次计划：

- 全局计划只描述 frame map、cluster baseline、状态组关系、token baseline 和批次顺序。
- 每个批次生成独立 `issues`、`operations`、`backupNodeIds`、`qa`。
- 批次计划不得引用当前批之外的深层节点对象；跨批节点只能通过全局索引中的 nodeId、clusterName、role、state、baseline 摘要引用。
- 执行完成后将每批结果合并到全局 `rollup`，并在报告中保留批次来源。

### 7. 备份策略

自动修复前：

- 如果目标是单个 frame/component：复制目标节点，命名为 `<原名称> / backup before pm10 <timestamp>`，放到右侧或下方 120px。
- 如果目标是 PAGE/画布或 `canvas-all-elements`：只复制将要修改的顶层根 frame/component/section，命名为 `<原名称> / backup before pm10 <timestamp>`，不要复制整个页面。
- 如果目标是组件主件或 component set：默认先 `check-only`；修复组件主件必须要求用户确认，除非用户明确指定可修改组件库。

不得删除备份，除非用户明确要求。

### 8. 自动修复执行

写入前必须加载 `figma:figma-use`。

修复脚本要求：

- 每次 `use_figma` 最多切换一次 page。
- 多 frame 画布必须按 `BatchExecutionPlan.batches` 循环调用 `use_figma`；每次调用只处理当前批。
- 单次写入不得同时深度遍历整张画布的所有 frame。
- 对每个修改节点记录 `before/after`。
- 返回 `createdNodeIds`、`mutatedNodeIds`、`backupNodeIds`、`repairPlanId`。
- 多批执行时额外返回 `batchId`、`batchStatus`、`batchIssueCount`、`batchQaStatus`。
- 使用 `setSharedPluginData` 写入 `pm10/issueId`、`pm10/fixApplied`、`pm10/checkoutTimestamp`。
- 不使用 `getPluginData` / `setPluginData`。
- 加载字体后再修改 TEXT。

批处理执行顺序：

1. 运行 Global Index Batch，若失败则停止并报告 `global-index-failed`。
2. 循环运行 Frame Local Batches，优先处理 P0/P1 和低风险 P2。
3. 循环运行 State Cluster Batches，只统一共享区域，不覆盖变体业务内容。
4. 循环运行 Reusable Cluster Batches，只对 high confidence 映射执行结构/样式/token 同步。
5. 运行 Final QA Batch，合并所有批次 issue、operation、备份和 QA 结果。

批次失败处理：

- 当前批修复失败但未破坏节点时，标记 `batch-failed` 并继续后续 `check-only` 或低风险批次。
- 当前批修复后 QA 变差时，回滚或保留备份并标记该批 `qa-regressed`。
- 不得因为一个局部 frame 失败而静默跳过全局报告。

常用修复规则：

1. **文本挤压**：扩大文本父容器宽度；按钮至少 `textWidth + 2*padding`；标签至少 `textWidth + 16`。
2. **表格列过窄**：按列标题和样例文本估算最小宽；保留操作列；超宽时设置横向滚动容器或记录失败。
3. **同层重叠**：若节点应纵向排列，父容器设 Auto Layout vertical；若应横向排列，设 horizontal 并补 gap。
4. **Shell 冲突**：Page Body 的 `x/y/width/height` 按 Topbar/Sidebar/Footer 安全区域重算。
5. **裁切**：若父容器非滚动语义，关闭不合理 clipping 或增大父容器；若是滚动容器，记录为可接受。
6. **FILL/HUG 错误**：先 append 到 Auto Layout parent，再设置 layout sizing。
7. **层级错误**：将明显属于 body 的节点 reparent 到 Page Body，将导航/顶栏/底栏 reparent 到 Shell。
8. **非默认状态污染**：以名称/metadata 识别 `empty/loading/error/modal/drawer/toast`，移动到“State References”分组或隐藏，不能删除。
9. **复用组件漂移**：对同角色 cluster 使用基准样式同步尺寸、间距、填充、描边、圆角、字体大小和布局模式；业务文案和业务数据不得被覆盖。
10. **导航激活态错误**：按当前根 frame 名称/page title 设置 active/inactive；active 样式从同一导航组中已有 active 样式或设计系统 token 推断。
11. **复用组件颜色样式漂移**：根据 `ProductLayoutModel`、design tokens 或 Figma 变量统一同 `role + state` 的 fill、stroke、text fill、corner radius、opacity 和 effect。
12. **active 状态样式漂移**：当前 active 节点结构正确但颜色不一致时，执行 `sync-active-state-style`；inactive siblings 也必须同步 inactive 样式。
13. **token mismatch**：文档或 token 明确时执行 `apply-product-token`，优先绑定变量/样式；无法绑定时才使用对应色值，并记录来源。
14. **冗余生成内容**：将明确非业务展示的生成说明类节点移入 `PM10 Removed / <timestamp>` 或隐藏；不得在 `safe-fix` 中直接删除。
15. **绝对布局滥用**：将明显横向/纵向排列的低风险子节点转换为 Auto Layout，并保留视觉顺序、padding、gap。
16. **FILL 父容器**：将应盛满父容器的 Section/Card/ListItem/FormControl 设置为 `FILL`；设置前确认父级是 Auto Layout 或先转换父级。
17. **HUG 内容**：将按钮、Tag、Tab/Nav item、短文本标签设置为 `HUG`；若内容过长，保留 min width 并记录风险。
18. **全量复用元素统一**：对 high confidence 的 `ReusableElementCatalog` 映射执行 `normalize-reusable-element`；同步结构、状态、样式和动态布局，但不覆盖业务文案。
19. **组件族布局统一**：对 Button、Tag、Input、Select、Card、Metric、Table Row 等同 `role + state` 的组件族执行 `sync-component-family-layout`。
20. **组件族 token 统一**：对同 `role + state` 且 token 明确的组件族执行 `sync-component-family-token`；warning/success/error/risk 等语义色不得被普通 primary/default 覆盖。
21. **同 state 共享区域统一**：对同 `stateVariantCluster` 的共享区域执行 `sync-state-shared-region`；变体区域只检查语义和布局边界，不强制统一内容。

### 9. 修复后 QA

修复后必须重新检查：

- Root bounds。
- sibling overlap。
- shell collision。
- text overflow。
- clipped content。
- key region presence。
- duplicate primary action。
- Auto Layout validity。
- absolute layout overuse。
- fill parent validity。
- hug content validity。
- dynamic layout stability。
- frame relation consistency。
- reusable component consistency。
- reusable element catalog coverage。
- sitemap-to-Figma mapping confidence。
- component family layout consistency。
- component family token consistency。
- state variant cluster consistency。
- same-state shared region consistency。
- reusable style consistency。
- semantic color consistency。
- active state style consistency。
- token consistency。
- navigation active state。
- redundant generated content removed from visible page scope。
- 关键节点数量是否未减少。

如果自动修复让问题变多，应回滚本次修复或标记失败并保留备份。

多批 QA 要求：

- 每个 Frame Local Batch 完成后，至少复查该批 root bounds、overlap、text overflow、Auto Layout validity。
- 每个 State Cluster Batch 完成后，复查共享区域一致性和变体区域边界。
- 每个 Reusable Cluster Batch 完成后，复查同 `role + state` 的结构、布局、颜色和 token drift。
- Final QA Batch 必须汇总每批结果，并重新检查是否存在跨批引入的不一致。

### 10. 报告

使用 `assets/figma-checkout-report-template.md` 结构生成报告。

报告必须明确：

- `Done`：所有 P0/P1 问题已修复或不存在。
- `Warning`：仍有 P2/P3 或需要人工确认的问题。
- `Failed`：P0/P1 未修复、修复后 QA 失败、或目标不可写。
- `Batch Summary`：多 frame 画布必须列出批次数、每批涉及 frame、每批状态、失败批次和最终 rollup。

## 与 pm08/pm09 的关系

- `pm08-md2figma`：负责从 `figma-nodes.json` 写入 Figma。
- `pm09-md2figma-batch`：负责批量调用 pm08。
- `pm10-figma-checkout`：负责对已经在 Figma 中的画布/frame/component 做质量检查与修复。

pm10 不重新解释 `figma-nodes.json`，也不重跑 pm08。若发现设计稿问题来自 pm08/pm09 的导入缺陷，应：

1. 在报告中标记为 `renderer-regression`。
2. 优先修复当前 Figma 节点。
3. 建议同步修复 pm08/pm09 生成逻辑，但不要在 pm10 中擅自改写 pm08/pm09。

## 思路方案

### 方案 A：只检查，不修复

适合评审、验收、交付前 QA。

流程：

1. 读取 Figma metadata / design context / screenshot。
2. 用 `use_figma` 计算结构问题。
3. 输出按 P0-P3 排序的问题清单和修复建议。
4. 不写入 Figma。

优点：零风险。缺点：需要人工执行修复。

### 方案 B：安全自动修复

默认推荐。

流程：

1. 创建目标 frame 备份。
2. 自动修复低风险问题：尺寸、间距、Auto Layout、文本挤压、边界溢出。
3. 重跑 QA。
4. 输出修复报告和残余问题。

优点：效率高、风险可控。缺点：复杂重排仍需人工确认。

### 方案 C：强修复/重排

适合草稿质量很差、用户明确允许重排。

流程：

1. 备份目标 frame。
2. 按识别出的 Shell、Page Body、Overlay、State References 重建局部层级。
3. 批量应用 Auto Layout、统一 spacing、重设主要容器尺寸。
4. 对所有关键区域做截图和结构 QA。

优点：可处理严重错乱。缺点：可能改变视觉结构，需要用户授权。

### 方案 D：批量画布体检

适合一个画布中有多张页面稿。

流程：

1. 用户指定画布。
2. 先执行 Global Index Batch，扫描画布中所有顶层 frame/component/section，建立轻量 frame map。
3. 根据 frame 数量、状态组、复用组件族生成 `BatchExecutionPlan`。
4. 循环执行 Frame Local Batches，每个根 frame 或 1 到 3 个同类 frame 生成独立 issue list。
5. 只对 P0/P1 且低风险的问题自动修复；其余进入人工检查项。
6. 汇总所有批次结果，执行 Final QA Batch。

优点：适合批量验收。缺点：必须控制单批数量，避免超大 Figma 脚本。

### 方案 E：跨 frame 关联与复用一致性修复

适合同一画布中有多个页面、多个状态或多套复用 Shell/导航组件。

流程：

1. 建立根 frame 关系图，识别默认态、状态态、同模块页面和流程前后置关系。
2. 聚类复用区域，例如导航、Topbar、Sidebar、Tabs、表格行、Metric Card。
3. 先在 Global Index 中确定基准来源，再按 reusable cluster 拆成多个 Reusable Cluster Batches。
4. 每个 batch 只处理一个组件族或少量强相关组件族，检查样式和结构 drift。
5. 根据当前页面语义修正导航/Tab 激活态。
6. 剔除明显非业务展示内容的生成说明类节点，并将其隔离到 `PM10 Removed / <timestamp>`。

优点：能保证多页面设计的一致性和正确状态。缺点：语义不明确的 drift 必须进入人工检查，不能强行统一。

### 方案 F：产品文档驱动的复用组件与颜色统一

适合 Figma 画布需要与产品信息架构、布局规范和后台管理设计规范保持一致。

流程：

1. 读取 `product/Product Overview.md` 和 `product/layout/*.md`。
2. 建立 `ProductLayoutModel`，提取导航、全局 Shell、页面区域、复用组件、状态组和样式 token。
3. 在 Global Index Batch 中将 Figma 顶层节点映射到文档定义的复用组件 cluster，不读取所有深层节点。
4. 将每个高置信 cluster 拆成 Reusable Cluster Batch，逐批建立 `StyleSignature`，比较 fill、stroke、text fill、corner radius、opacity、effect、active/inactive 状态。
5. 逐批执行 `sync-component-family-token`、`sync-active-state-style`、`normalize-reusable-element`，最后由 Final QA Batch 汇总验证。
5. 按文档和 token 优先级统一同 `role + state` 的颜色样式。
6. 对语义不明确或文档冲突的颜色差异只记录，不自动覆盖。

优点：让修复不只依赖 Figma 节点名称，而是符合产品结构和设计语义。缺点：文档缺失或冲突时需要人工确认。

### 方案 G：全量复用元素与同 State 页面修复

适合用户要求“获取所有可通用/复用的元素”，或同一画布包含多组状态页面。

流程：

1. 从 `product/layout/*.md` 建立 `ReusableElementCatalog`，覆盖 Shell、Region、Form、Action、Display、List/Table、Status、Stepper 等复用族。
2. 从页面清单和 sitemap 建立 `stateVariantClusters`，识别同 state / 同状态组页面。
3. 用 `FigmaReusableDiscovery` 扫描画布所有候选复用元素，生成 cluster、匹配节点、置信度和基准。
4. 对 high confidence cluster 做结构、样式、状态、动态布局检查和 safe-fix。
5. 对 medium confidence cluster 只修复低风险 token/尺寸问题，并把结构差异列为人工检查。
6. 对 low confidence cluster 只报告，不自动统一。
7. 对同 state 页面统一共享区域，保留变体区域差异。

优点：能覆盖 sitemap 和状态组驱动的全量复用元素，而不是只修导航。缺点：需要更严格的映射置信度，避免把业务差异误判为 drift。

## 质量门槛

最终响应前检查：

- 已解析 Figma fileKey 和目标 nodeId/画布名称。
- 已读取 `product/Product Overview.md` 和 `product/layout/*.md`，或已记录缺失/冲突原因。
- 已生成 `ProductLayoutModel`，并用它识别复用结构/组件。
- 已生成 `ReusableElementCatalog`，并覆盖 sitemap 中的区域、分组、元素、状态组和通用 UI 组件族。
- 已执行 `FigmaReusableDiscovery`，报告 Figma 复用元素聚类和文档映射置信度。
- 已建立 `stateVariantClusters`，并区分同 state 页面共享区域与变体区域。
- 已使用 Figma remote MCP 读取目标结构。
- 写入前已加载 `figma:figma-use`。
- `check-only` 不应写入 Figma。
- `safe-fix` 或 `aggressive-fix` 已创建备份或说明无法备份。
- 画布级检查已完成 frame 关联关系、复用组件一致性、导航/Tab 激活态检查。
- 画布级检查已完成全量复用元素覆盖率、组件族布局一致性、组件族 token 一致性、同 state 页面一致性检查。
- 已完成复用组件颜色样式、语义颜色、active state style 和 token 一致性检查。
- 已完成绝对布局和动态布局检查；命中例外的节点已记录或说明无需修复。
- 明确冗余元素已从当前页面展示范围剔除，无法确认的疑似冗余元素已进入人工检查项。
- 报告已生成。
- 修复后 QA 不存在 P0/P1 failed。
- 未删除无关 Figma 内容。
