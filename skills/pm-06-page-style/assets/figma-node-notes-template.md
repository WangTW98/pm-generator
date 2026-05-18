# {{页面名称}} Figma Node Notes

## 0.生成状态

| 项目 | 内容 |
|---|---|
| 源页面 | `{{layout.md路径}}` |
| 来源Sitemap | `{{来源Sitemap}}` |
| 使用Layout | {{使用Layout}} |
| 页面清单ID | {{PAGE-ID}} |
| 状态组 | {{状态组}} |
| 使用设计规范 | `{{design目录}}` |
| 输出JSON | `{{figma-nodes.json路径}}` |
| 生成日期 | {{YYYY-MM-DD}} |

## 1.设计规范匹配说明

- 匹配方式：{{用户指定 / 自动匹配}}
- 匹配原因：{{说明}}
- 读取文件：
  - `design-spec.md`
  - `tokens.json`
  - `figma-tokens.json`
  - `component-rules.json`
  - `component-blueprints.json`
  - `figma-render-rules.json`
  - `ai-generation-rules.json`

## 2.Sitemap Layout合并说明

- 已读取来源Sitemap：{{是/否}}
- 已读取全局Layout：{{是/否}}
- 已合并的全局区域：{{Topbar / Sidebar / BottomNav / Breadcrumb / Page Container / Global Actions}}
- 页面挂载容器：{{说明}}

## 3.页面意图与语义校验

| 项目 | 内容 |
|---|---|
| 页面意图 | {{auth / dashboard / list-management / detail / form-create-edit / checkout-payment / wizard-flow / profile-settings / message-center / document-viewer / empty-or-state-page / custom}} |
| 置信度 | {{low / medium / high}} |
| 主要证据 | {{页面名称、核心场景、父级链、1.3/1.4/1.5证据}} |
| 使用语义模式 | `component-rules.semanticPatterns.{{intent}}` |
| 语义校验状态 | {{pass / warning / failed}} |
| 冲突与修复 | {{无 / 冲突与修复说明}} |

## 4.pm-03默认状态页面元素映射说明

| pm-03区域 | pm-03元素 | pm-03类型 | Figma节点类型 | 使用组件规则 | 说明 |
|---|---|---|---|---|---|
| {{区域}} | {{元素}} | {{类型}} | {{节点类型}} | {{组件规则}} | {{说明}} |

## 5.RenderSpec渲染契约

| 项目 | 内容 |
|---|---|
| 使用组件蓝图 | `component-blueprints.json` |
| 使用渲染规则 | `figma-render-rules.json` |
| 组件实例绑定 | {{已绑定数量 / 说明}} |
| 文本溢出策略 | {{换行、最大行数、父容器增高、表格单元格处理}} |
| 布局约束策略 | {{宽高模式、最小尺寸、padding/gap、对齐方式}} |
| 关键区域QA | {{keyRegions / forbiddenRegions}} |
| 还原风险 | {{无 / 需要pm-08人工复核的风险}} |

## 6.一致性 Profile

| 项目 | 内容 |
|---|---|
| layoutKey | {{normalized-使用Layout}} |
| pageIntentKey | {{intent}} |
| stateGroupKey | {{状态组-or-empty}} |
| Layout Profile | `{{layout-profile-path}}` |
| Intent Profile | `{{intent-profile-path}}` |
| State Group Profile | `{{state-profile-path-or-empty}}` |
| Profile状态 | {{created / reused / updated}} |
| layoutHash | {{hash}} |
| intentHash | {{hash}} |
| stateGroupHash | {{hash-or-empty}} |
| 锁定属性 | {{画板/Shell/卡片/表单/表格/Tab等}} |
| 允许页面差异 | {{当前页面仅改变业务内容/字段/active状态等}} |
| Drift检查 | {{pass / warning / failed}} |

## 7.响应式与状态排除处理

### 7.1 响应式策略

| 设备 | 画板宽度 | 布局策略 | 说明 |
|---|---:|---|---|
| Desktop | 1440 | {{策略}} | {{说明}} |
| Tablet | 768 | {{策略}} | {{说明}} |
| Mobile | 390 | {{策略}} | {{说明}} |

### 7.2 非默认状态排除记录

以下状态仅作为页面行为参考，不生成到 `figma-nodes.json` 的主节点树中。

| 状态ID | 状态名称 | 排除原因 | 后续处理 |
|---|---|---|
| STATE-VIEW-002 | 空状态 | 非默认状态，避免污染默认页面结构 | 后续如需状态画板，使用专门状态生成流程 |
| STATE-VIEW-003 | 加载状态 | 非默认状态，避免污染默认页面结构 | 后续如需状态画板，使用专门状态生成流程 |
| STATE-VIEW-004 | 错误状态 | 非默认状态，避免污染默认页面结构 | 后续如需状态画板，使用专门状态生成流程 |

## 8.待确认与假设

| 类型 | 内容 | 影响 | 后续处理 |
|---|---|---|---|
| 假设 | {{假设}} | {{影响}} | {{处理}} |
