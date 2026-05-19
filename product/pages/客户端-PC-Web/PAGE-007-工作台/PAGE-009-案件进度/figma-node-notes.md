# 案件进度 Figma Node Notes

## 0.生成状态

| 项目 | 内容 |
|---|---|
| 源页面 | `product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-009-案件进度/layout.md` |
| 来源Sitemap | `product/layout/客户端-PC-Web-sitemap.md` |
| 使用Layout | 客户端 / PC Web |
| 页面清单ID | PAGE-009 |
| 状态组 | 无 |
| 使用设计规范 | `design/antd-admin-design-spec` |
| 输出JSON | `product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-009-案件进度/figma-nodes.json` |
| 生成日期 | 2026-05-19 |

## 1.设计规范匹配说明

- 匹配方式：自动匹配。
- 匹配原因：当前仓库只有一个完整有效的 pm-05 设计规范目录 `design/antd-admin-design-spec`，且 JSON 依赖均解析通过。
- 读取文件：`design-spec.md`、`tokens.json`、`figma-tokens.json`、`component-rules.json`、`component-blueprints.json`、`figma-render-rules.json`、`ai-generation-rules.json`、`consistency-rules.json`、`layout-profile-schema.json`、`page-intent-patterns.json`。

## 2.Sitemap Layout合并说明

- 已读取来源Sitemap：是。
- 已读取全局Layout：是。
- 已合并的全局区域：Topbar、Sidebar、Footer、Page Container、Global Actions。
- 页面挂载容器：`PageBody/PAGE-009-案件进度`，位于 `Frame/Primary/客户端 / PC Web` 内。

## 3.页面意图与语义校验

| 项目 | 内容 |
|---|---|
| 页面意图 | list-management |
| 置信度 | high |
| 主要证据 | 页面属于进度追踪场景；默认元素包含筛选、搜索、案件集合和行级详情入口 |
| 使用语义模式 | `page-intent-patterns.patterns.list-management` |
| 语义校验状态 | pass |
| 冲突与修复 | 无 |

## 4.pm-03默认状态页面元素映射说明

| pm-03区域 | pm-03元素 | pm-03类型 | Figma节点类型 | 使用组件规则 | 说明 |
|---|---|---|---|---|---|
| 案件概览指标区 | 进行中案件数 | 文本/展示 | TEXT | Text | 默认状态元素，映射到可编辑节点 |
| 案件概览指标区 | 风险案件数 | 文本/展示 | TEXT | Text | 默认状态元素，映射到可编辑节点 |
| 案件筛选区 | 服务类型筛选 | 选择控件 | COMPONENT_INSTANCE | Select | 默认状态元素，映射到可编辑节点 |
| 案件筛选区 | 案件编号搜索 | 文本/展示 | COMPONENT_INSTANCE | Input | 默认状态元素，映射到可编辑节点 |
| 案件列表区 | 案件卡片/表格 | 列表/表格 | COMPONENT_INSTANCE | Card | 默认状态元素，映射到可编辑节点 |
| 案件列表区 | 进度步骤摘要 | 流程组件 | COMPONENT_INSTANCE | Timeline | 默认状态元素，映射到可编辑节点 |
| 进度提示区 | 查看服务详情按钮 | 按钮 | COMPONENT_INSTANCE | Button | 默认状态元素，映射到可编辑节点 |

## 5.RenderSpec渲染契约

| 项目 | 内容 |
|---|---|
| 使用组件蓝图 | `component-blueprints.json`；基础组件缺少专用蓝图时使用 `component-rules.json` fallback adapter |
| 使用渲染规则 | `figma-render-rules.json` |
| 组件实例绑定 | 8 个组件/集合绑定 |
| 文本溢出策略 | 文本默认换行最多 2 行；表格单元格单行省略；父容器允许按内容增高 |
| 布局约束策略 | Shell 固定，PageBody fill width + hug height，卡片/分组使用 24px padding 与 16px gap |
| 关键区域QA | keyRegions=PageHeader, MetricGrid, FilterBar, DataCollection, ProgressSummary, PrimaryAction；forbiddenRegions=duplicated global shell, blocking duplicate primary action |
| 还原风险 | 无 |

## 6.一致性 Profile

| 项目 | 内容 |
|---|---|
| layoutKey | 客户端-PC-Web |
| pageIntentKey | list-management |
| stateGroupKey | 无 |
| Layout Profile | `product/style-profiles/客户端-PC-Web/layout-profile.json` |
| Intent Profile | `product/style-profiles/客户端-PC-Web/intent-list-management-profile.json` |
| State Group Profile | `无` |
| Profile状态 | layout=created, intent=created, state=not-applicable |
| layoutHash | b2968cd4bc2c |
| intentHash | fb8680bf9b7b |
| stateGroupHash | 无 |
| 锁定属性 | canvas；global shell；content padding；responsive fallback；list-management skeleton；no state group |
| 允许页面差异 | business copy and mock data only |
| Drift检查 | pass |

## 7.响应式与状态排除处理

### 7.1 响应式策略

| 设备 | 画板宽度 | 布局策略 | 说明 |
|---|---:|---|---|
| Desktop | 1440 | logged-in portal shell with sidebar, topbar and content cards | 主输出画板 |
| Tablet | 768 | collapse sidebar, keep card/table content stacked with 20px page padding | 折叠导航并保持内容可读 |
| Mobile | 390 | hide sidebar, convert tables to card list, filters into drawer | 控件高度不低于 40px |

### 7.2 非默认状态排除记录

以下状态仅作为页面行为参考，不生成到 `figma-nodes.json` 的主节点树中。

| 状态ID | 状态名称 | 排除原因 | 后续处理 |
|---|---|---|---|
| EXCLUDED-001 | 空状态 | Non-default state excluded from Figma node generation | 如需状态画板，使用专门状态生成流程 |
| EXCLUDED-002 | 加载状态 | Non-default state excluded from Figma node generation | 如需状态画板，使用专门状态生成流程 |
| EXCLUDED-003 | 错误状态 | Non-default state excluded from Figma node generation | 如需状态画板，使用专门状态生成流程 |
| EXCLUDED-004 | 空状态文案 | Non-default state excluded from Figma node generation | 如需状态画板，使用专门状态生成流程 |
| EXCLUDED-005 | 加载文案 | Non-default state excluded from Figma node generation | 如需状态画板，使用专门状态生成流程 |
| EXCLUDED-006 | 错误文案 | Non-default state excluded from Figma node generation | 如需状态画板，使用专门状态生成流程 |

## 8.待确认与假设

| 类型 | 内容 | 影响 | 后续处理 |
|---|---|---|---|
| 假设 | 使用 `design/antd-admin-design-spec` 作为客户端 PC Web 的设计规范 | 客户端页面呈现 Ant Design 风格后台/门户混合视觉 | 如后续生成客户端专用设计规范，可重新运行 pm-07 批量更新 |
| 假设 | 非默认状态不生成主节点 | 保持 pm-06 默认状态输出纯净 | 状态画板另行生成 |
