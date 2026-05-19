# PAGE-009 - 案件进度 Figma Restore Report

## 0.恢复状态

| 项目 | 内容 |
|---|---|
| 源 JSON | `product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-009-案件进度/figma-nodes.json` |
| 目标 Figma | https://www.figma.com/design/ZP4TjUsWJyHncnukJuVfRD/Agent-Test?node-id=215-55&t=A760mTWqS5Qcfqy5-1 |
| fileKey | ZP4TjUsWJyHncnukJuVfRD |
| nodeId | 215:55 |
| 目标页面 | Case Tracking PRD7 |
| 写入模式 | append |
| 最新根画板 | PAGE-009 - 案件进度 - PC Web (2) |
| 最新 Figma 根节点ID | 274:2 |
| 既有对比画板 | PAGE-009 - 案件进度 - PC Web / 235:179；PAGE-009 - 案件进度 - PC Web / backup before pm10 2026-05-19T08-01-27-371Z / 237:179 |
| 渲染模式 | pm08 RenderPlan + editable fallback components |
| consistencyMode | profile-locked |
| 生成日期 | 2026-05-19 |
| 结果 | Done |

## 0.1.追加历史

| 时间 | 根画板 | 根节点 ID | Figma 链接 | QA |
|---|---|---|---|---|
| 2026-05-19 | `PAGE-009 - 案件进度 - PC Web` | `235:179` | https://www.figma.com/design/ZP4TjUsWJyHncnukJuVfRD/Agent-Test?node-id=235-179&t=A760mTWqS5Qcfqy5-1 | pass |
| 2026-05-19 | `PAGE-009 - 案件进度 - PC Web (2)` | `274:2` | https://www.figma.com/design/ZP4TjUsWJyHncnukJuVfRD/Agent-Test?node-id=274-2&t=A760mTWqS5Qcfqy5-1 | pass |

## 1.RenderPlan 与 TokenResolver

- 主渲染来源：`figmaDocument.children` 规范化后的 RenderPlan。
- 读取并保留：`pageIntent`、`semanticValidation`、`renderSpec`、`consistency`、`excludedStates`。
- Token 命中：使用 `design/antd-admin-design-spec/tokens.json` 的主色、背景、表面、文本、边框、状态色、间距和圆角。
- Token 缺失：0 个阻塞项。
- 组件适配：基础组件使用可编辑 fallback；未使用整页截图或不可编辑图片。
- 降级说明：当前设计规范的 `component-blueprints.json` 偏页面/壳级，Button/Input/Select/Card/Navigation 等基础组件按 `component-rules.json` 与 renderSpec sizing 生成可编辑 Frame/Text。

## 2.一致性与 Profile

| 项目 | 内容 |
|---|---|
| layoutKey | 客户端-PC-Web |
| pageIntentKey | list-management |
| stateGroupKey | 无 |
| designSpecKey | antd-admin-design-spec |
| styleFingerprint | b2968cd4bc2c/fb8680bf9b7b/- |
| driftCheck | pass |
| profile-locked | 已应用 |
| metadata | 已写入 Figma shared plugin data：sourceNodeId、layoutKey、pageIntentKey、stateGroupKey、designSpecKey、styleFingerprint、rendererVersion、consistencyMode |

## 3.非默认状态排除

- 已排除非默认状态数量：6。
- 空状态、加载状态、错误状态及其文案只记录为行为参考，未生成独立画板或默认可见节点。

## 4.Post-render QA

| 检查项 | 结果 | 说明 |
|---|---|---|
| Root Bounds | pass | 根画板尺寸 1440 x 1024；Topbar 1440 x 64、PageBody/Sidebar/Footer 使用固定安全尺寸 |
| Shell Collision | pass | 顶栏、侧栏/页体、底栏无非预期重叠 |
| Text Overflow | pass | 文本使用可编辑 Text，控件和卡片使用稳定尺寸 |
| Incoherent Overlap | pass | Auto Layout / 固定分区布局，无结构性堆叠 |
| Duplicate Action | pass | 未引入阻塞级重复主操作 |
| Key Regions | pass | list-management 关键区域已生成 |
| Layout Budget | pass | 来源 renderSpec.layoutBudget 为 pass，导入后未触发失败 |
| Screenshot QA | pass | 最新截图 URL：`https://www.figma.com/api/mcp/asset/954eab45-0456-4694-bbad-5f7a3a97687b` |

## 5.结构统计

| 指标 | 数量 |
|---|---:|
| 总节点 | 104 |
| Text 节点 | 54 |
| Frame 节点 | 25 |
| 根直接子节点 | 3 |

## 6.人工检查项

- 无阻塞项。
- 最新截图 URL 已写入本地报告；本次使用 Figma metadata、结构 QA 与截图作为验收依据。
