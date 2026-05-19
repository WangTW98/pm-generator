# 微信扫扫登录 Figma Node Notes

## 0.生成状态

| 项目 | 内容 |
|---|---|
| 源页面 | `product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-003-微信扫扫登录/layout.md` |
| 来源Sitemap | `product/layout/客户端-PC-Web-sitemap.md` |
| 使用Layout | 客户端 / PC Web |
| 页面清单ID | PAGE-003 |
| 状态组 | STATE-001 |
| 使用设计规范 | `design/antd-admin-design-spec` |
| 输出JSON | `product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-003-微信扫扫登录/figma-nodes.json` |
| 生成日期 | 2026-05-19 |

## 1.设计规范匹配说明

- 匹配方式：自动匹配。
- 匹配原因：当前仓库只有一个完整有效的 pm-05 设计规范目录 `design/antd-admin-design-spec`，且 JSON 依赖均解析通过。
- 读取文件：`design-spec.md`、`tokens.json`、`figma-tokens.json`、`component-rules.json`、`component-blueprints.json`、`figma-render-rules.json`、`ai-generation-rules.json`、`consistency-rules.json`、`layout-profile-schema.json`、`page-intent-patterns.json`。

## 2.Sitemap Layout合并说明

- 已读取来源Sitemap：是。
- 已读取全局Layout：是。
- 已合并的全局区域：Topbar、Footer、Page Container、Global Actions。
- 页面挂载容器：`PageBody/PAGE-003-微信扫扫登录`，位于 `Frame/Primary/客户端 / PC Web` 内。

## 3.页面意图与语义校验

| 项目 | 内容 |
|---|---|
| 页面意图 | auth |
| 置信度 | high |
| 主要证据 | 页面属于账号访问场景；默认元素包含账号/验证码/密码/二维码、协议确认和主登录按钮 |
| 使用语义模式 | `page-intent-patterns.patterns.auth` |
| 语义校验状态 | pass |
| 冲突与修复 | 将微信扫码登录视为 auth credential slot 的 QR code variant，未生成表格或非默认状态节点。 |

## 4.pm-03默认状态页面元素映射说明

| pm-03区域 | pm-03元素 | pm-03类型 | Figma节点类型 | 使用组件规则 | 说明 |
|---|---|---|---|---|---|
| 品牌说明区 | 平台名称 | 文本/展示 | TEXT | Text | 默认状态元素，映射到可编辑节点 |
| 品牌说明区 | 登录方式分段控件 | 文本/展示 | COMPONENT_INSTANCE | Tabs / Segmented Control | 默认状态元素，映射到可编辑节点 |
| 登录方式切换区 | 账号输入框 | 输入框 | COMPONENT_INSTANCE | Input | 默认状态元素，映射到可编辑节点 |
| 登录方式切换区 | 验证码或密码输入框 | 输入框 | COMPONENT_INSTANCE | Input | 默认状态元素，映射到可编辑节点 |
| 凭证输入区 | 主登录按钮 | 按钮 | COMPONENT_INSTANCE | Button | 默认状态元素，映射到可编辑节点 |
| 凭证输入区 | 协议勾选框 | 文本/展示 | COMPONENT_INSTANCE | Checkbox | 默认状态元素，映射到可编辑节点 |
| 协议与帮助区 | 忘记密码/帮助入口 | 文本/展示 | TEXT | Text | 默认状态元素，映射到可编辑节点 |

## 5.RenderSpec渲染契约

| 项目 | 内容 |
|---|---|
| 使用组件蓝图 | `component-blueprints.json`；基础组件缺少专用蓝图时使用 `component-rules.json` fallback adapter |
| 使用渲染规则 | `figma-render-rules.json` |
| 组件实例绑定 | 7 个组件/集合绑定 |
| 文本溢出策略 | 文本默认换行最多 2 行；表格单元格单行省略；父容器允许按内容增高 |
| 布局约束策略 | Shell 固定，PageBody fill width + hug height，卡片/分组使用 24px padding 与 16px gap |
| 关键区域QA | keyRegions=AuthBrandPanel, AuthMethodSwitch, CredentialSlot, AgreementRow, PrimaryLoginAction；forbiddenRegions=default-visible data table, sidebar as page body content |
| 还原风险 | 源 layout 的元素清单仍使用通用账号输入/验证码命名；已按状态组差异说明记录为扫码登录凭证槽。 |

## 6.一致性 Profile

| 项目 | 内容 |
|---|---|
| layoutKey | 客户端-PC-Web |
| pageIntentKey | auth |
| stateGroupKey | STATE-001 |
| Layout Profile | `product/style-profiles/客户端-PC-Web/layout-profile.json` |
| Intent Profile | `product/style-profiles/客户端-PC-Web/intent-auth-profile.json` |
| State Group Profile | `product/style-profiles/客户端-PC-Web/state-STATE-001-profile.json` |
| Profile状态 | layout=created, intent=reused, state=reused |
| layoutHash | b2968cd4bc2c |
| intentHash | cacd70c11191 |
| stateGroupHash | fc0e36bec1f5 |
| 锁定属性 | canvas；global shell；content padding；responsive fallback；auth skeleton；state group geometry |
| 允许页面差异 | credential slot content and active login method |
| Drift检查 | pass |

## 7.响应式与状态排除处理

### 7.1 响应式策略

| 设备 | 画板宽度 | 布局策略 | 说明 |
|---|---:|---|---|
| Desktop | 1440 | public centered auth card with topbar and footer | 主输出画板 |
| Tablet | 768 | collapse sidebar, keep card/table content stacked with 20px page padding | 折叠导航并保持内容可读 |
| Mobile | 390 | single-column auth card, 16px margin, controls 40px high | 控件高度不低于 40px |

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
