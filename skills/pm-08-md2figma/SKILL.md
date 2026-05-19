# pm-08-md2figma

## 目的

将用户指定的 `product/pages/**/figma-nodes.json` 还原到用户指定的 Figma 文件或节点中，生成可继续编辑的页面设计稿。

此 skill 面向 GPT/Codex、Gemini、Claude 等 AI Agent。Agent 必须把 `figma-nodes.json` 当作结构化设计蓝图，而不是普通 Markdown 摘要；还原目标是 Figma 中的默认状态页面，不生成空状态、加载态、错误态、成功态、弹窗打开态、抽屉打开态等非默认状态。

pm08 是通用 JSON renderer。业务结构必须来自 `figma-nodes.json`，尤其是 `figmaDocument`、`layoutShell`、`pageContent`、`pageIntent`、`semanticValidation`、`renderSpec` 和 `consistency`。pm08 必须执行布局预算、slot 合同、重复动作和渲染后 QA；不得把不同页面重新套入固定模板，也不得根据页面名称二次推断业务结构。

## 输入

用户至少需要提供：

- 一个 `product/pages/**/figma-nodes.json` 路径。
- 一个 Figma 链接，格式通常为 `https://www.figma.com/design/<fileKey>/...`，可带 `node-id`。

可选输入：

- 指定写入模式：`create`、`update`、`replace-root`、`append`。
- 指定目标 Figma 页面名或容器节点。
- 指定是否只做 dry run。

若用户未提供 Figma 链接，必须先要求用户补充链接。不要猜测或创建新的 Figma 文件。

## 输出

必须完成：

- 将 JSON 中的默认状态页面结构写入指定 Figma 文件。
- 在源 JSON 同目录生成 `figma-restore-report.md`。

`figma-restore-report.md` 记录：

- 源 `figma-nodes.json` 路径。
- 目标 Figma 链接、fileKey、nodeId。
- 写入模式。
- 根画板名称。
- 渲染模式、节点数量、组件适配/降级数量、token 解析情况。
- consistency profile 命中情况、fingerprint 匹配情况、drift 检查结果。
- 被跳过的非默认状态。
- 关键语义区校验结果。
- 截图或结构 QA 结果。
- 警告、失败项、人工检查项。

## 必须遵守的 Figma 工具规则

在任何 Figma 写入动作前，必须先加载 `figma:figma-use` skill，然后再调用 Figma 的 `use_figma` 工具。

不得直接在未加载 `figma:figma-use` 的情况下调用 Figma 写入工具。

## 工作流程

### 1. 解析输入

1. 解析用户指定的 `figma-nodes.json` 路径。
2. 解析用户指定的 Figma URL：
   - 提取 `fileKey`。
   - 若 URL 包含 `node-id=1-2`，转为 Figma nodeId `1:2`。
3. 判断写入模式：
   - 默认使用 `update`。
   - 若目标 Figma 中不存在同名根画板，则创建。
   - 若用户明确要求追加，使用 `append`。
   - 若用户明确要求重建当前根画板，使用 `replace-root`，但不得删除无关画板或无关页面。

### 2. 读取并校验 JSON

读取 `figma-nodes.json` 后必须校验：

- JSON 可解析。
- 存在 `meta` 或同等页面元信息。
- 存在可还原的节点树，优先使用 `figmaDocument.children`。若缺失，可使用 `layoutShell.nodes` + `pageContent.defaultStateNodes` 组合还原；不得使用自创压缩结构替代。
- 若存在 `pageIntent`，必须保留为元数据；不得用 pm08 重新分类页面。
- 若存在 `semanticValidation.status = failed`，不得写入 Figma，必须报告失败。
- 若存在 `renderSpec`，必须把它作为组件适配、布局约束、文本溢出和 QA 的约束合同；不得忽略后自行猜测。
- 若存在 `consistency`，必须把它作为跨页面一致性的约束合同；不得忽略 profile refs、styleFingerprint 或 driftCheck。
- 若存在 `stateGenerationPolicy`，其值应为 `default-state-only`。
- 若存在 `excludedStates`，这些内容只记录到报告，不写入 Figma。

同时读取同目录 `figma-node-notes.md`（若存在），用于补充页面来源、布局意图和人工检查项。

### 2.1 一致性契约校验

若 `figma-nodes.json.consistency` 存在，pm08 必须读取并校验：

- `layoutKey`
- `pageIntentKey`
- `stateGroupKey`，可为 `null`
- `designSpecKey`
- `profileRefs.layoutProfile`
- `profileRefs.intentProfile`
- `profileRefs.stateGroupProfile`，可为空
- `styleFingerprint`
- `lockedPropertiesApplied`
- `driftCheck.status`

根据 `profileRefs` 加载 profile 文件。相对路径从当前工作区根目录解析；绝对路径按原样读取。若 profile 文件不存在：

- 当 `driftCheck.status = pass` 且 profile 缺失时，标记为 `Failed`，因为 JSON 声称可复用但基线不存在。
- 当 JSON 明确标记 profile 是本次首次创建但文件尚未落盘时，可降级为 `warning`，但报告必须记录缺失 profile。

profile 校验规则：

1. `layout-profile.json` 控制同 layout 的 shell、画板尺寸、主要区域宽高、全局导航、内容区 padding、主要 gap。
2. `intent-<pageIntentKey>-profile.json` 控制同 layout + 同页面意图的页面骨架密度、表单/表格/卡片/向导等结构规格。
3. `state-<stateGroupKey>-profile.json` 控制同一状态组的卡片尺寸、主操作位置、标题/切换区位置、面板宽高、局部导航和内容 slot。
4. profile 中标记为 locked 的属性优先级高于导入器 fallback 默认值。
5. profile 不得重写业务节点树；它只能约束尺寸、间距、布局模式、组件规格和视觉 token。

如果 `consistency` 缺失：

- 可以继续还原旧版 JSON，但必须在报告中标记 `consistency missing`。
- 不得为了弥补缺失而从 Figma 旧画板反推 profile，除非用户明确要求“使用 Figma 现有画板作为基线”。
- 对批量导入场景，应提示先使用新版 pm06/pm07 重生成 JSON。

### 3. 默认状态过滤

生成 Figma 前必须过滤非默认状态：

- 不生成 `empty`、`loading`、`error`、`success`、`disabled-only`、`modal-open`、`drawer-open`、`popover-open`、`tooltip-open`、`toast-visible` 等状态专用节点。
- 不生成 pages/layout.md 中的“状态说明”“异常态”“空态”“加载态”“校验错误态”等描述性内容。
- 对被排除状态，仅写入 `figma-restore-report.md` 的“非默认状态排除记录”。

### 4. Token 与样式映射

优先使用 `figma-nodes.json` 中的 token 引用。

若 JSON 引用了 pm05 输出：

- `designRefs.tokens`
- `designRefs.figmaTokens`
- `designRefs.componentRules`
- `designRefs.componentBlueprints`
- `designRefs.figmaRenderRules`
- `designRefs.aiGenerationRules`
- `designRefs.consistencyRules`
- `designRefs.layoutProfileSchema`
- `designRefs.pageIntentPatterns`

则读取这些文件，建立 token 解析表。

映射规则：

- 颜色 token 映射到 Figma fills、strokes、effects。
- 字体 token 映射到 font family、font size、line height、font weight。
- spacing token 映射到 padding、gap、position、尺寸。
- radius token 映射到 cornerRadius。
- shadow token 映射到 Figma effects。

若 token 缺失：

- 使用 JSON 节点中已展开的值。
- 仍缺失时使用保守默认值。
- 在报告中记录缺失 token，不中断整体生成。

Token 解析必须形成一个明确的 `TokenResolver`，不要在渲染器中散落硬编码样式。

`TokenResolver` 必须至少支持：

- `color/*`、`color.xxx` -> Figma solid fill/stroke 0-1 RGB。
- `typography/*`、`typography.scale.*` -> font family、size、line height、weight。
- `spacing/*`、`spacing.x` -> padding、gap、坐标、尺寸。
- `radius/*`、`radius.x` -> cornerRadius。
- `shadow/*`、`shadow.x` -> effects。
- component token references such as `component-rules.components.Button` -> component fallback sizing and state rules。
- component blueprint references such as `component-blueprints.componentBlueprints.Button` -> editable node tree, slots, min/max sizing, and text overflow rules。
- render rule references such as `figma-render-rules.layoutEngine` and `figma-render-rules.qaRules` -> two-pass layout, append behavior, overflow handling, and QA checks。
- consistency rule references such as `consistency-rules.profileRules` -> profile precedence, locked property handling, and drift checks。

`figma-restore-report.md` 必须记录：

- token 命中数量。
- token 缺失数量。
- 缺失 token 名称和 fallback 值。
- 是否读取了 `tokens.json`、`figma-tokens.json`、`component-rules.json`、`component-blueprints.json`、`figma-render-rules.json`、`ai-generation-rules.json`。
- 是否读取了 `consistency-rules.json`、`layout-profile-schema.json`、`page-intent-patterns.json` 和 `product/style-profiles/<layoutKey>/` profile 文件。

### 5. RenderPlan 生成

写入 Figma 前，必须先把 `figmaDocument.children` 规范化为 `RenderPlan`。

RenderPlan 是渲染器的唯一输入，不得直接在 Figma 写入代码里边读 JSON 边自由发挥。

RenderPlan 节点字段：

```json
{
  "sourceNodeId": "PAGE-002-auth-panel",
  "name": "Section/AuthPanel",
  "semanticType": "FRAME",
  "component": null,
  "content": {},
  "tokens": {},
  "layout": {},
  "children": []
}
```

RenderPlan 必须合并 `renderSpec`：

- `renderSpec.componentInstances` 绑定到对应 `COMPONENT_INSTANCE` 的 adapter、props、slots、variant 和 sizing。
- `renderSpec.textPolicies` 绑定到对应 `TEXT`、表格单元格、按钮文字、输入框占位和卡片摘要。
- `renderSpec.constraints` 绑定到对应 Frame/Table/Card/Form/Shell 容器。
- `renderSpec.qaExpectations` 进入最终结构 QA，不得只写入报告。
- 当节点和 `renderSpec` 冲突时，保留 `figmaDocument.children` 的业务层级，以 `renderSpec` 修正尺寸、布局、溢出和组件适配，不得反向重写业务结构。

RenderPlan 必须合并 `consistency` profiles：

- `layoutProfile` 锁定同 layout 的 shell、画板尺寸、导航尺寸、内容区域 padding/gap 和主要容器宽高。
- `intentProfile` 锁定同 layout + 同意图的区域顺序、密度、组件最小尺寸、表格/表单/卡片基础规格。
- `stateGroupProfile` 锁定同状态组页面的面板位置、主操作位置、切换区、表单宽度、二维码/验证码/输入区 slot 尺寸。
- profile locked 值优先于 pm08 默认 fallback、组件适配器 fallback 和临时尺寸估算。
- profile 与 `renderSpec` 冲突时，业务层级仍以 `figmaDocument.children` 为准；尺寸、间距、布局模式等 locked property 以 profile 为准，并在报告中记录冲突。
- profile 不得新增业务节点，也不得删除 JSON 中的业务节点。

RenderPlan 规则：

1. 主来源只能是 `figmaDocument.children`。
2. `layoutShell.nodes` 和 `pageContent.defaultStateNodes` 只用于校验和补充缺失报告，不得覆盖主来源。
3. `pageIntent` 只用于语义校验，不得重写节点树。
4. `excludedStates` 只进入报告，不进入 RenderPlan。
5. 保留原始节点顺序、名称、content、tokens、layout、source。
6. 所有输出到 Figma 的节点必须写入 `sourceNodeId` metadata。
7. 若存在 `renderSpec`，必须在 RenderPlan 中标记每个主要节点的适配器、约束和文本策略命中情况。
8. 若存在 `consistency`，必须在 RenderPlan 中标记每个主要节点命中的 layout/intent/state profile 规则和 locked 属性。

若缺少可渲染节点树，停止并报告失败，不得临时根据页面名称生成页面。

### 6. 组件适配器 Registry

pm08 必须使用组件适配器 registry 渲染常见 `COMPONENT_INSTANCE`。组件适配器只解释单个组件节点，不得改写页面结构。

适配器最低覆盖：

| component | 可编辑 fallback 要求 |
|---|---|
| Button | Frame + Text，支持 primary/secondary/ghost/danger/disabled/loading，使用 Button component rules 的高度、padding、radius、fill、text。 |
| Input | Label/placeholder/helper 可编辑，输入框高度、描边、focus/error/disabled 规则来自 Input component rules。 |
| Select | 类 Input 结构，保留下拉指示符或文本占位。 |
| Tabs | Tab item 列表、active item、indicator 或 active fill；不得只画成一行文本。 |
| Checkbox / Radio / Switch | 控件形状 + 文本 label，命中状态样式。 |
| Badge / Tag | 小型可编辑状态标签，文本和状态色分离。 |
| Steps | 多步骤条，包含序号/状态/步骤文本；当前步骤应可辨认。 |
| Upload | 可编辑上传区域，保留上传提示、按钮、文件/材料语义。 |
| Table | Header row、sample row、cell text、status column、operation column、pagination when present。 |
| Card / List | Frame/card item，保留标题、摘要、字段行、操作区。 |
| Topbar / Sidebar / Footer | 作为 shell 区域渲染，保留导航/品牌/全局动作结构。 |

适配器规则：

- 优先导入真实 Figma 组件，仅当 JSON 提供可用 component key / mapping 且导入成功。
- 没有真实组件时，使用 `component-blueprints.json` 中的可编辑 fallback；若蓝图缺失，再使用 component rules 创建保守 fallback，并在报告中标记降级。
- fallback 必须使用 TokenResolver、consistency profiles、component blueprints、renderSpec props/slots/sizing 和 component rules，不得只用一套临时硬编码样式。
- 当 profile 已锁定组件高度、宽度、内边距、gap、字体层级或状态组 slot，组件适配器必须应用这些值；不得因为文本长度、导入顺序或 append 位置改变组件尺寸。
- 每次 fallback 都要在报告中记录组件名、节点名、原因。
- 不支持的 component 使用 `GenericComponentFallback`，但报告中必须标记为人工检查项。

### 7. 节点类型还原

Agent 应递归遍历 `figmaDocument.children`，将语义节点转为 Figma Plugin API 节点。节点顺序、层级、命名、token 引用、content 字段、source 字段都应尽量保留。

渲染源优先级：

1. `figmaDocument.children` 是主渲染源。
2. `layoutShell.nodes` 和 `pageContent.defaultStateNodes` 可用于补充或校验。
3. `pageIntent` 只用于校验和报告，不用于重写页面结构。
4. `excludedStates` 只记录到报告，不生成节点。

支持的语义节点类型：

- `PAGE`：Figma page 或同名顶层分组语义。
- `FRAME`：Figma Frame，优先使用 Auto Layout。
- `SECTION`：Frame，作为页面区域。
- `TEXT`：Text。
- `RECTANGLE`：Rectangle 或 Frame 背景。
- `IMAGE_PLACEHOLDER`：带占位填充的 Frame。
- `ICON`：文本图标、矢量占位或设计系统图标占位。
- `BUTTON`：Frame + Text/Icon。
- `INPUT`、`SELECT`、`CHECKBOX`、`RADIO`、`SWITCH`：Frame 组合，不依赖真实组件时要保持可编辑。
- `TABLE`：Frame 表格结构。
- `TABLE_ROW`：横向 Auto Layout Frame。
- `TABLE_CELL`：Frame/Text。
- `CARD`：Frame。
- `COMPONENT_INSTANCE`：若 JSON 提供可用 component key 或 mapping，优先导入组件；否则降级为等效可编辑 Frame。

降级生成时必须保持：

- 层级清晰。
- 命名可读。
- 默认状态完整。
- 文本可编辑。
- 视觉上符合设计规范。

禁止的还原方式：

- 不得把 `auth`、`detail`、`form`、`dashboard` 等页面统一画成筛选区和表格。
- 不得把 `pageContent.defaultStateNodes` 压缩成 `sections/items` 后丢失层级。
- 不得忽略 `figmaDocument` 中已存在的 shell、panel、form、table、timeline、wizard、auth panel 等结构。
- 不得用截图、单一大图或不可编辑图层替代节点。

## Recursive Rendering Contract

每个节点应按以下通用映射处理：

```text
DOCUMENT -> no canvas node; iterate children
PAGE -> target Figma page or logical root group
FRAME / SECTION -> Auto Layout frame when children are structural
TEXT -> Text node; load font before writing
RECTANGLE -> Rectangle or Frame background
COMPONENT_INSTANCE -> import mapped component when available; otherwise editable fallback frame
TABLE -> Frame containing header and row frames
TABLE_ROW -> horizontal Auto Layout frame
TABLE_CELL -> text/frame cell
VARIANT_GROUP -> Frame group or component set placeholder
```

Renderer must preserve:

- `node.id` as shared plugin data or stable mapping metadata.
- `node.name` as Figma node name.
- `node.content.label/text/value/placeholder` as visible text.
- `node.tokens` via Figma variables when available, or TokenResolver raw styles when variables are unavailable.
- `node.layout` via the two-pass layout algorithm below.
- `node.children` recursively.

Every rendered root frame and major semantic node must store shared metadata where the Figma runtime supports it:

- `sourceNodeId`
- `layoutKey`
- `pageIntentKey`
- `stateGroupKey`
- `designSpecKey`
- `layoutProfileHash`
- `intentProfileHash`
- `stateGroupProfileHash`
- `styleFingerprint`
- `rendererVersion`
- `consistencyMode`

Use shared plugin data when available. If the runtime cannot write metadata, record the limitation in `figma-restore-report.md` and keep node names stable.

### 8. 两阶段布局还原

优先使用 JSON 中的布局信息：

- `layoutMode`
- `direction`
- `padding`
- `gap`
- `width`
- `height`
- `constraints`
- `renderSpec.constraints`
- `renderSpec.layoutSafety`
- `renderSpec.layoutBudget`
- `renderSpec.slotContracts`
- `renderSpec.duplicateAudit`
- `consistency.profileRefs`
- `consistency.lockedPropertiesApplied`
- `responsive`
- `breakpoints`

布局必须分两阶段执行。

#### 8.1 第一阶段：节点创建与本地尺寸

1. 创建节点并设置名称、type、source metadata。
2. 根据 JSON `layout.width`、`layout.height`、`layout.minHeight`、`layoutMode`、`renderSpec.constraints`、consistency profiles、component blueprints、component rules 计算尺寸。
3. 设置 fills、strokes、cornerRadius、effects、text style。
4. 对 `width: "fill"`、`height: "fill"` 暂存为 layout intent，不要提前设置 `layoutSizingHorizontal/Vertical`。
5. 对 `height: "hug"` 的容器必须使用内容测量后的真实高度，不得把它拉伸到父容器剩余高度。
6. 对缺失高度的容器使用内容估算高度，禁止出现明显异常值，例如标题区、协议区、链接区、辅助操作区高度接近整页高度。
7. 根据 `renderSpec.layoutBudget` 或 pm05 `measurementRules` 计算可用内容区、主要面板最大高度、压缩顺序、滚动候选容器和失败阈值。若 JSON 未提供预算，也必须从 canvas、shell、padding 和主要容器估算一个保守预算并在报告中标记 fallback。

#### 8.2 第二阶段：append 后 Auto Layout sizing

1. append child 到 parent 后再设置 `layoutSizingHorizontal` / `layoutSizingVertical`。
2. `FRAME`/`SECTION` 有子节点时使用 Auto Layout，除非 JSON 明确要求绝对布局。
3. 设置 parent 的 padding、gap、alignment。
4. 对 shell 区域使用 profile 或设计规范中的稳定尺寸：
   - PC Topbar 常用 56-72 高。
   - PC Sidebar 使用 sitemap/design rules 宽度。
   - Footer 常用 72-120 高。
   - PageContent 占满剩余宽度或按 JSON minHeight。
5. 对组件使用 component adapter 和 profile locked sizing 计算尺寸，避免只靠文本 Hug 导致控件过窄。
6. 写入后读取根画板结构，检查主要容器高度、宽度和子节点数量。

#### 8.3 布局预算与压缩策略

当内容超过安全区域或主要容器预算时，按以下顺序处理，除非 `renderSpec.layoutBudget.compressionOrder` 或 pm05 recipe 指定了更严格顺序：

1. 压缩非关键区域 gap，但不得低于设计 token 的最小安全间距。
2. 压缩次要容器 padding，但不得压缩表单控件、点击目标或文本行高。
3. 缩小允许缩放的媒体、二维码、插图、预览图或统计图尺寸。
4. 把明确允许滚动的容器设为内部滚动/裁切容器，并保留可见高度。
5. 将非关键辅助链接或说明区折行，但不得覆盖主操作。
6. 如果仍然超过 root 或与 shell/footer/header 冲突，标记 `QA failed`，不要声称 Done。

禁止通过以下方式“解决”超高内容：

- 把根画板或主内容裁切后仍标记成功。
- 让主要面板越过 topbar、sidebar、footer 或 root 边界。
- 把 hug 容器拉伸为大块空白区域。
- 删除 JSON 中的业务节点，除非节点是非默认状态或明确被 `renderSpec` 标记为可折叠。

#### 8.4 重复语义与动作检查

写入前后都必须检查 `renderSpec.duplicateAudit` 和实际 Figma 文本/动作：

- 同一 page body 中不得存在两个默认可见 primary action 指向同一 `actionIntent`。
- 重复文本只有在不同区域角色明确时才允许，例如 global shell brand 与 page hero brand；否则记录 warning 或 failed。
- 如果 JSON 已标记 blocking duplicate，pm08 不得写入 Figma。
- 如果 Figma 结果新增了重复动作或重复区域，报告 `renderer duplicate introduced` 并标记 failed。

若两阶段布局后发现明显异常，必须先尝试局部修正；无法修正时在报告中标记 `QA failed`，该页不得标记 `Done`。

若页面含多设备形态：

- 同一 JSON 中明确包含 Desktop、Tablet、Mobile 时，按设备分别生成根画板。
- 若 JSON 只描述一个设备，按该设备生成。
- 不要凭空创建未在 JSON 中表达的额外设备稿，除非用户明确要求。

默认尺寸建议：

- PC Web：`1440 x 1024`
- PC 应用：`1440 x 900`
- Tablet：`834 x 1194`
- Mobile App：`390 x 844`
- Mobile Web：`390 x 844`

### 9. 写入 Figma

写入策略：

1. 在目标 Figma 文件中查找同名根画板。
2. 若存在且模式为 `update`，先比较同名根画板的 metadata 与当前 JSON 的 `consistency`。
   - metadata 的 `layoutKey`、`pageIntentKey`、`stateGroupKey`、`styleFingerprint` 匹配时，可以更新该根画板内容。
   - metadata 缺失或 fingerprint 不匹配时，不得静默继承旧画板的样式基线；报告 `existing frame fingerprint mismatch`，并按用户写入模式决定更新、新建或失败。
3. 若不存在，创建新根画板。
4. 若模式为 `append`，创建带时间或序号后缀的新根画板。
5. 若模式为 `replace-root`，只替换匹配的根画板内部内容，不删除无关内容。

默认 `consistencyMode` 为 `profile-locked`：

- append/no-overwrite 模式仍必须使用 JSON 的 profile 和 fingerprint，不得从已有 Figma 画板继承样式差异。
- 只有用户明确要求“使用 Figma 现有画板作为基线”时，才允许从目标 Figma 中读取同 profile 画板作为辅助参考。
- 即使使用 Figma 现有画板作为辅助参考，也必须先确认 `styleFingerprint` 或 profile hash 匹配；不匹配时仅记录差异，不自动套用。

当用户明确要求“不要覆盖”“用于对比”“同名则新建”时：

- 强制使用 `append`。
- 不匹配更新任何已有同名根画板。
- 通过扫描 `targetPage.children` 找到空白位置，通常放在所有现有画板下方或右侧，并保持至少 120px 间距。
- 根画板名称使用 `<基础名称> (2)`、`(3)` 等后缀或用户指定的对比命名。
- 新建根画板仍写入 `consistencyMode=profile-locked` 与 profile metadata，以便后续同 layout 对比和更新。

根画板命名建议：

`<页面编号> - <页面名称> - <设备形态>`

例如：

`PAGE-027 - 订单管理 - PC Web`

### 10. 语义关键区校验

写入前后都必须根据 `pageIntent.primary` 和 `renderSpec.qaExpectations` 做关键区校验。此校验不得重写结构，只能 pass、warning 或 failed。

| pageIntent | 必须存在的关键区域或组件 |
|---|---|
| auth | `Section/AuthPanel`、`AuthMethodSwitch`、`IdentityInput`、credential/verification control、agreement/policy link、primary submit action |
| list-management | Page header、filter/search、toolbar/batch actions、table/list/card collection、pagination when applicable |
| form-create-edit | Grouped form sections、input/select/upload controls、submit/cancel action bar |
| detail | Summary/status area、description fields、related records or timeline、action group |
| wizard-flow | Steps/progress、current step body、step status/content、previous/next/submit action |
| checkout-payment | Order summary、price breakdown、payment method、payment action |
| message-center | Message filter/tabs、message list、read/unread state、preview/detail |
| document-viewer | Toolbar、preview panel、metadata/actions panel |
| profile-settings | Settings navigation、profile/security/permission section、save actions |

若关键区缺失：

- 如果源 JSON 已缺失，报告 `source semantic gap`。
- 如果源 JSON 有但 Figma 结果缺失，报告 `renderer loss`，必须尝试修复。
- 修复失败时将该页标记为 failed 或 warning，不得静默通过。

### 11. 质量检查

写入完成后必须检查：

- Figma 中存在目标根画板。
- 根画板尺寸正确。
- 关键区域数量与 JSON 结构一致。
- 关键文本已写入。
- 表格、表单、导航、操作区未明显丢失。
- 未生成非默认状态页面。
- Figma 根画板的主要区域与 `figmaDocument` 或 `pageContent.defaultStateNodes` 的主要区域数量一致。
- 对于 `pageIntent.primary` 指定的页面类型，关键语义区域未被固定模板替换。
- `renderSpec.componentInstances`、`textPolicies`、`constraints` 中的重要条目已命中或被明确记录为降级。
- `consistency.profileRefs` 已读取或明确记录缺失。
- profile locked 属性已应用到同 layout、同 pageIntent、同 stateGroup 的关键尺寸、间距、密度和组件规格。
- Figma 根画板 metadata 中的 `layoutKey`、`pageIntentKey`、`stateGroupKey`、`styleFingerprint` 与 JSON 一致，或报告中明确记录无法写入 metadata。
- 现有同 profile 画板如被用作更新目标，fingerprint 必须匹配或明确报告不匹配。
- 没有明显文本溢出、元素堆叠、表格单元格互相覆盖、按钮文字挤压、表单控件高度异常。
- 根画板所有默认可见节点都在 root bounds 内，除非其父容器被明确标记为可滚动/可裁切。
- 主要内容容器不与 topbar、sidebar、footer、bottom nav、fixed action bar 等 shell 区域发生 incoherent overlap。
- `height: "hug"` 的容器没有被异常拉伸，协议区、辅助链接区、操作区等低内容区域高度与内容量相符。
- `renderSpec.layoutBudget.status`、实际测量结果、压缩/滚动处理结果已记录。
- `renderSpec.duplicateAudit` 与 Figma 实际重复文本/动作检查已记录；阻塞级重复动作不得通过。

可用时，必须生成目标根画板截图进行人工检查；若截图工具不可用，必须至少执行结构 QA。

QA 必须记录：

- 根画板尺寸。
- 直接子节点名称和数量。
- 总节点数、文本节点数、FRAME 节点数。
- Auto Layout frame 数量。
- 关键文本样本。
- 关键语义区命中情况。
- renderSpec 命中率、组件适配命中率、文本策略命中率、约束命中率。
- consistency profile 命中率、fingerprint 匹配结果、driftCheck 结果。
- layoutBudget 命中率、主要容器预计高度与实际高度、压缩/滚动处理结果、root/shell collision 结果。
- duplicateAudit 结果，包括重复文本、重复 actionIntent、重复 primary action。
- 是否存在不应出现的表格/状态页/截图占位。
- 截图 URL 或截图不可用原因。

### 12. 报告

使用 `assets/restore-report-template.md` 的结构，在源 JSON 同目录生成：

`figma-restore-report.md`

报告必须使用中文。

## 禁止事项

- 不得在缺少 Figma 链接时自行创建 Figma 文件。
- 不得删除 Figma 文件中的无关页面、无关画板、无关组件。
- 不得把非默认状态还原成独立页面或独立画板。
- 不得把 `figma-nodes.json` 当成纯文本描述重新自由发挥。
- 不得忽略 pm05 token 文件。
- 不得忽略 `renderSpec` 中的组件适配、文本策略、布局约束、QA期望。
- 不得忽略 `consistency`、profile refs、styleFingerprint 或 driftCheck。
- 不得在 append 模式中为了对齐旧画板而覆盖 JSON/profile 的锁定规格。
- 不得跳过 RenderPlan、TokenResolver、组件适配器和 QA 直接写入简化节点。
- 不得把所有内容拍平成图片。
- 不得生成不可编辑的整页截图替代节点结构。
- 不得在报告中声称已写入 Figma，但实际未执行 Figma 写入。

## Agent 适配说明

- GPT/Codex：按本文件执行；Figma 写入前必须加载 `figma:figma-use`。
- Claude：按 `agents/claude.md` 执行，重点遵守默认状态过滤和非破坏性更新。
- Gemini：按 `agents/gemini.md` 执行，重点保持 JSON 结构到 Figma 节点的确定性映射。
