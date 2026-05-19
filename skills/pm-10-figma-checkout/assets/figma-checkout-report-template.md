# {{目标名称}} Figma Checkout Report

## 0.检查状态

| 项目 | 内容 |
|---|---|
| 目标 Figma | {{figma-url}} |
| fileKey | {{fileKey}} |
| nodeId | {{nodeId}} |
| 目标名称 | {{targetName}} |
| 目标类型 | {{targetType}} |
| 检查模式 | {{check-only / safe-fix / aggressive-fix}} |
| 执行模式 | {{single-target / batch-loop}} |
| 修复状态 | {{Done / Warning / Failed}} |
| 备份节点 | {{backupNodeIds}} |
| 创建时间 | {{YYYY-MM-DD HH:mm:ss}} |

## 1.输入与范围

- 指定画布：{{canvasName}}
- 指定 frame/component/node：{{targetNode}}
- 本次检查范围：{{scope}}
- 未指定具体 frame 时的处理：{{canvas-all-elements / not-applicable}}
- 排除范围：{{excludedScope}}
- 画布内顶层 frame/component/section 数：{{rootFrameCount}}
- 是否启用批处理循环：{{yes/no + reason}}

## 2.Figma 读取记录

| MCP / 工具 | 是否使用 | 结果 |
|---|---|---|
| get_metadata | {{是/否}} | {{结果}} |
| get_design_context | {{是/否}} | {{结果}} |
| get_screenshot | {{是/否}} | {{结果}} |
| get_variable_defs | {{是/否}} | {{结果}} |
| get_libraries / search_design_system | {{是/否}} | {{结果}} |
| use_figma | {{是/否}} | {{结果}} |

## 2.0 批处理执行计划

| 项目 | 内容 |
|---|---|
| 执行模式 | {{single-target / batch-loop}} |
| Global Index 状态 | {{ready/failed/not-needed}} |
| 批次数 | {{batchCount}} |
| 拆批原因 | {{multiple-root-frames / oversized-frame / state-clusters / reusable-clusters / not-needed}} |
| 单批限制 | {{frameLimit/nodeLimit}} |
| 失败批次 | {{failedBatchIdsOrNone}} |
| Rollup 状态 | {{ready/warning/failed}} |

### Batch 列表

| Batch ID | 类型 | 涉及 Frame | 目的 | 允许操作 | 状态 |
|---|---|---|---|---|---|
| {{batchId}} | {{single-frame/state-cluster/reusable-cluster/qa}} | {{frameIds}} | {{purpose}} | {{allowedOps}} | {{pending/done/failed}} |

## 2.1 产品文档读取与语义模型

| 文档 | 是否读取 | 提取内容 | 冲突/缺失 |
|---|---|---|---|
| product/Product Overview.md | {{是/否}} | {{modules/navigation/state/style summary}} | {{conflictsOrMissing}} |
| product/layout/*.md | {{是/否}} | {{layout/reusable components/style tokens}} | {{conflictsOrMissing}} |

### ProductLayoutModel 摘要

- 产品/模块：{{productNameAndModules}}
- 导航项：{{navigationItems}}
- 全局 Shell：{{globalShellRules}}
- 页面区域：{{pageRegions}}
- 复用组件：{{reusableComponents}}
- 复用元素目录：{{reusableElementCatalogSummary}}
- 状态组：{{stateGroups}}
- 同 State 页面关系：{{stateVariantClusters}}
- 样式 token：{{styleTokens}}

## 2.2 Sitemap 复用元素抽取

| 复用元素 | 角色 | 适用端/范围 | 来源文档 | aliases | 应出现页面/区域 | 规则 |
|---|---|---|---|---|---|---|
| {{elementName}} | {{role}} | {{scope}} | {{sourceDocAndSection}} | {{aliases}} | {{expectedInPagesOrRegions}} | {{structureStyleStateRules}} |

## 2.3 Figma 复用元素聚类与映射

| Cluster | 角色 | 文档来源 | 匹配节点 | 基准 | 置信度 | 处理策略 |
|---|---|---|---|---|---|---|
| {{clusterName}} | {{role}} | {{sourceDoc}} | {{matchedNodes}} | {{baselineNodeOrToken}} | {{high/medium/low}} | {{safeFix/manualReview/skip}} |

## 2.4 同 State 页面关系

| State Group | 父页面 | 涉及 Frame | 共享区域 | 变体区域 | Active/状态规则 | 检查结果 |
|---|---|---|---|---|---|---|
| {{stateGroup}} | {{parentPage}} | {{frames}} | {{sharedRegions}} | {{variantRegions}} | {{stateRules}} | {{pass/warning/failed}} |

## 3.问题清单

| Issue ID | 等级 | 类型 | 节点ID | 节点名称 | 证据 | 建议 |
|---|---|---|---|---|---|---|
| PM10-001 | {{P0/P1/P2/P3}} | {{issueType}} | {{nodeId}} | {{nodeName}} | {{evidence}} | {{recommendation}} |

## 3.0 批处理结果汇总

| Batch ID | 类型 | 涉及 Frame | 发现问题 | 执行修复 | 备份节点 | QA 结果 | 备注 |
|---|---|---|---|---|---|---|---|
| {{batchId}} | {{batchType}} | {{frameIds}} | {{issueCount}} | {{operationCount}} | {{backupNodeIds}} | {{pass/warning/failed}} | {{notes}} |

## 3.1 frame 关联与复用组件检查

| Cluster / 关系 | 涉及 frame | 基准节点 | 差异节点 | 检查结果 | 处理 |
|---|---|---|---|---|---|
| {{clusterName}} | {{frameIds}} | {{baselineNode}} | {{driftNodes}} | {{pass/warning/failed}} | {{fixOrReason}} |

## 3.1.1 产品文档映射的复用组件

| 组件/结构 | 来源文档 | Figma 匹配节点 | 样式基准 | 映射结果 | 处理 |
|---|---|---|---|---|---|
| {{componentName}} | {{sourceDoc}} | {{figmaNodes}} | {{styleBaseline}} | {{mapped/unmapped/conflict}} | {{fixOrReason}} |

## 3.1.2 全量复用元素覆盖检查

| 复用元素 | 应出现位置 | 实际匹配 | 缺失/额外 | 置信度 | 处理 |
|---|---|---|---|---|---|
| {{elementName}} | {{expectedScope}} | {{actualNodes}} | {{missingOrExtra}} | {{confidence}} | {{fixOrReason}} |

## 3.1.3 组件族一致性检查

| 组件族 | 状态 | 节点 | 布局签名 | 样式/token 签名 | Drift | 处理 |
|---|---|---|---|---|---|---|
| {{componentFamily}} | {{state}} | {{nodes}} | {{layoutSignature}} | {{styleTokenSignature}} | {{driftSummary}} | {{fixOrReason}} |

## 3.2 导航与激活态检查

| frame | 当前页面语义 | 导航组 | 应激活项 | 实际激活项 | 处理 |
|---|---|---|---|---|---|
| {{frameName}} | {{pageIntent}} | {{navGroup}} | {{expectedActive}} | {{actualActive}} | {{fixOrReason}} |

## 3.3 冗余元素剔除记录

| 节点ID | 节点名称 | 文本摘要/证据 | 原位置 | 处理方式 | 回滚方式 |
|---|---|---|---|---|---|
| {{nodeId}} | {{nodeName}} | {{evidence}} | {{originalParent}} | {{quarantine/hidden/deleted}} | {{rollback}} |

## 3.4 动态布局与绝对布局检查

| 节点ID | 节点名称 | 当前布局 | 内容判断 | 推荐布局 | 是否例外 | 处理 |
|---|---|---|---|---|---|---|
| {{nodeId}} | {{nodeName}} | {{layoutMode / positioning / sizing}} | {{contentRole}} | {{auto-layout / fill / hug / keep-fixed}} | {{yes/no + reason}} | {{fixOrReason}} |

## 3.5 复用组件颜色与样式检查

| Cluster | 状态 | 样式属性 | 基准来源 | 基准值 | 差异节点 | 处理 |
|---|---|---|---|---|---|---|
| {{clusterName}} | {{state}} | {{fill/stroke/textFill/radius/effect}} | {{product-doc/token/figma-variable/component/majority}} | {{baseline}} | {{driftNodes}} | {{fixOrReason}} |

## 3.5.1 同 State 共享区域一致性检查

| State Group | 共享区域 | 基准 Frame/节点 | 差异 Frame/节点 | 差异类型 | 处理 |
|---|---|---|---|---|---|
| {{stateGroup}} | {{sharedRegion}} | {{baselineFrameOrNode}} | {{driftFramesOrNodes}} | {{structure/style/state/layout}} | {{fixOrReason}} |

## 3.5.2 State 变体区域检查

| State Group | 变体区域 | 当前状态 | 期望语义 | 实际内容/样式 | 结果 | 处理 |
|---|---|---|---|---|---|---|
| {{stateGroup}} | {{variantRegion}} | {{state}} | {{expectedSemantic}} | {{actual}} | {{pass/warning/failed}} | {{fixOrReason}} |

## 3.6 Token 与语义颜色检查

| 节点ID | 节点名称 | 语义 | 当前样式 | 期望 token/样式 | 结果 | 处理 |
|---|---|---|---|---|---|---|
| {{nodeId}} | {{nodeName}} | {{semanticRole/state}} | {{actualStyle}} | {{expectedToken}} | {{pass/warning/failed}} | {{fixOrReason}} |

## 4.修复计划

| Operation ID | 操作 | 节点ID | 修复前 | 修复后 | 风险 | 是否执行 |
|---|---|---|---|---|---|---|
| OP-001 | {{op}} | {{nodeId}} | {{before}} | {{after}} | {{low/medium/high}} | {{是/否}} |

## 5.修复结果

| 节点ID | 节点名称 | 修复内容 | 结果 |
|---|---|---|---|
| {{nodeId}} | {{nodeName}} | {{fix}} | {{pass/warning/failed}} |

## 6.修复后 QA

| 检查项 | 结果 | 说明 |
|---|---|---|
| Root Bounds | {{pass/warning/failed}} | {{说明}} |
| Sibling Overlap | {{pass/warning/failed}} | {{说明}} |
| Shell Collision | {{pass/warning/failed}} | {{说明}} |
| Text Overflow | {{pass/warning/failed}} | {{说明}} |
| Clipped Content | {{pass/warning/failed}} | {{说明}} |
| Auto Layout Validity | {{pass/warning/failed}} | {{说明}} |
| Absolute Layout Overuse | {{pass/warning/failed}} | {{说明}} |
| Fill Parent Validity | {{pass/warning/failed}} | {{说明}} |
| Hug Content Validity | {{pass/warning/failed}} | {{说明}} |
| Dynamic Layout Stability | {{pass/warning/failed}} | {{说明}} |
| Frame Relation Consistency | {{pass/warning/failed}} | {{说明}} |
| Reusable Element Catalog Coverage | {{pass/warning/failed}} | {{说明}} |
| Sitemap-to-Figma Mapping Confidence | {{pass/warning/failed}} | {{说明}} |
| Reusable Component Consistency | {{pass/warning/failed}} | {{说明}} |
| Component Family Layout Consistency | {{pass/warning/failed}} | {{说明}} |
| Component Family Token Consistency | {{pass/warning/failed}} | {{说明}} |
| State Variant Cluster Consistency | {{pass/warning/failed}} | {{说明}} |
| Same-State Shared Region Consistency | {{pass/warning/failed}} | {{说明}} |
| Reusable Style Consistency | {{pass/warning/failed}} | {{说明}} |
| Semantic Color Consistency | {{pass/warning/failed}} | {{说明}} |
| Active State Style Consistency | {{pass/warning/failed}} | {{说明}} |
| Token Consistency | {{pass/warning/failed}} | {{说明}} |
| Navigation Active State | {{pass/warning/failed}} | {{说明}} |
| Redundant Generated Content | {{pass/warning/failed}} | {{说明}} |
| Duplicate Primary Action | {{pass/warning/failed}} | {{说明}} |
| State Pollution | {{pass/warning/failed}} | {{说明}} |
| Key Regions | {{pass/warning/failed}} | {{说明}} |

## 7.截图与结构证据

- 修复前截图：{{beforeScreenshot}}
- 修复后截图：{{afterScreenshot}}
- 截图不可用原因：{{screenshotUnavailableReason}}
- 结构节点统计：{{nodeStats}}

## 8.人工检查项

- {{manualReviewItem}}

## 9.回滚与后续

- 备份节点：{{backupNodeIds}}
- 回滚方式：将备份节点恢复为主版本，或将修复节点移到旁侧保留对比。
- 后续建议：{{nextActions}}
