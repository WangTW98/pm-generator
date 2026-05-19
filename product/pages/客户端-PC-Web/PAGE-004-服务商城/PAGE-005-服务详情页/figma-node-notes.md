# 服务详情页 Figma Node Notes

## 0.生成状态

| 项目 | 内容 |
|---|---|
| 源页面 | `product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/layout.md` |
| 来源Sitemap | `product/layout/客户端-PC-Web-sitemap.md` |
| 使用Layout | 客户端 / PC Web |
| 页面清单ID | PAGE-005 |
| 状态组 |  |
| 使用设计规范 | `design/antd-admin-design-spec` |
| 输出JSON | `product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/figma-nodes.json` |
| 生成日期 | 2026-05-19 |

## 1.设计规范匹配说明

- 匹配方式：继承当前项目唯一可用设计规范 `design/antd-admin-design-spec`。
- 读取对象：`tokens.json`、`figma-tokens.json`、`component-rules.json`、`component-blueprints.json`、`figma-render-rules.json`、`ai-generation-rules.json`、`consistency-rules.json`、`layout-profile-schema.json`、`page-intent-patterns.json`。

## 2.Sitemap Layout合并说明

- 已读取来源Sitemap：是。
- 已读取全局Layout：是。
- 已合并的全局区域：Topbar、Sidebar、Footer、Page Container。
- 页面挂载容器：`PageBody/PAGE-005-服务详情页`。

## 3.页面意图与语义校验

| 项目 | 内容 |
|---|---|
| 页面意图 | service-detail |
| 置信度 | high |
| 主要证据 | 页面属于服务采购场景；默认元素包含服务标题、服务卖点、SKU 选择、办理周期、价格合计和立即购买按钮 |
| 语义校验状态 | pass |
| 冲突与修复 | 无 |

## 4.pm-03默认状态页面元素映射说明

| pm-03区域 | pm-03元素 | pm-03类型 | Figma节点类型 | 使用组件规则 | 说明 |
|---|---|---|---|---|---|
| 服务头图区 | 服务标题 | 文本/展示 | TEXT | Text | 默认状态元素，映射到可编辑节点 |
| 服务头图区 | 服务卖点 | 文本/展示 | TEXT/CARD | Text / Tag | 默认状态元素，映射到服务卖点标签 |
| SKU选择区 | 国家地区选择 | 选择控件 | COMPONENT_INSTANCE | Select / Radio group fallback | 默认状态元素，映射到可编辑选项组 |
| SKU选择区 | 类别数量选择 | 选择控件 | COMPONENT_INSTANCE | Stepper / Select fallback | 默认状态元素，映射到可编辑选项组 |
| 服务流程说明区 | 办理周期说明 | 文本/展示 | STEPS / CARD | Steps fallback | 默认状态元素，映射到流程说明 |
| 服务流程说明区 | 价格合计 | 文本/展示 | TEXT/CARD | Text | 默认状态元素，映射到价格摘要 |
| 购买操作区 | 立即购买按钮 | 按钮 | COMPONENT_INSTANCE | Button | 默认状态元素，映射到可编辑按钮 |

## 5.非默认状态排除处理

- 空状态、加载状态、错误状态及其文案仅作为行为参考，不进入主节点树。

## 6.待确认与假设

| 类型 | 内容 | 影响 | 后续处理 |
|---|---|---|---|
| 假设 | 服务详情页使用服务采购详情骨架，而不是列表管理骨架 | 影响画板信息密度与组件结构 | 如后续 pm06 重生成 JSON，可用新版 JSON 重新执行 pm08 |
