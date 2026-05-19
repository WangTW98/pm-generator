# PAGE-006 - 订单结算 Figma Restore Report

## 基本信息

- 源 JSON：`product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/figma-nodes.json`
- 目标 Figma：https://www.figma.com/design/ZP4TjUsWJyHncnukJuVfRD/Agent-Test?node-id=215-55&t=A760mTWqS5Qcfqy5-1
- fileKey：`ZP4TjUsWJyHncnukJuVfRD`
- nodeId：`215:55`
- 目标页面：`Case Tracking PRD7`
- 写入模式：`append`，按用户要求不覆盖现有画板
- consistencyMode：`layout-profile-reused-intent-profile-inline`
- 渲染模式：`pm08 RenderPlan + editable fallback components`
- 根画板：`PAGE-006 - 订单结算 - PC Web`
- 根节点 ID：`251:2`
- pageIntent：`checkout-payment`
- layoutKey：`客户端-PC-Web`
- pageIntentKey：`checkout-payment`
- stateGroupKey：
- designSpecKey：`antd-admin-design-spec`
- styleFingerprint：`b2968cd4bc2c/checkout-payment-pm08-20260519`
- semanticValidation：`pass`
- renderSpec：已读取并应用订单结算关键组件、文本、约束和 QA 预期
- consistency：已读取并写入 metadata
- layoutBudget：`pass`
- duplicateAudit：`pass`
- slotContracts：`slot-checkout-payment-primary-content`
- 执行时间：`2026-05-19`

## 还原结果

- 状态：`Done`
- 创建节点数：100
- 更新节点数：1（侧栏分隔线方向修正）
- 跳过节点数：3 个非默认状态记录未写入 Figma
- 组件适配数量：7
- 组件降级数量：7，均为可编辑 Frame/Text/Rectangle fallback
- token 命中数量：使用主色、背景、表面、文本、边框、状态色、间距和圆角
- token 缺失数量：0 个阻塞项
- profile 命中数量：1（layout profile）
- profile 缺失数量：0 个阻塞项；checkout intent profile 由本次前置 JSON 内联补齐
- fingerprint 匹配：`pass`
- driftCheck：`pass`
- root/shell collision：`pass`
- duplicateAudit：`pass`
- 关键语义区校验：`pass`
- 截图 QA：`pass`

## RenderPlan 校验

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| 使用 figmaDocument.children 作为主来源 | pass | 本次因目录缺失 `figma-nodes.json`，已先从 `layout.md` 补齐结构蓝图，再按 JSON 还原 |
| RenderPlan 节点总数 | pass | Figma 结果 100 节点 |
| 保留原始节点顺序 | pass | Topbar、Footer、Sidebar、PageBody、OrderSummary、BuyerInfo、PaymentMethod、PriceSummary 顺序可追踪 |
| 保留 sourceNodeId metadata | pass | 写入 `pm08_md2figma/sourceNodeId` |
| 合并 renderSpec.componentInstances | pass | OrderSummary、Select、ContactInfo、EntryGroup、PaymentMethod、PriceSummary、Button fallback 已生成 |
| 合并 renderSpec.textPolicies | pass | 文本节点可编辑，控件保留稳定尺寸 |
| 合并 renderSpec.constraints | pass | 根画板 1440 x 1024，内容区 1208 x 888 |
| 合并 renderSpec.layoutBudget | pass | 内容未越界 |
| 合并 renderSpec.slotContracts | pass | `slot-checkout-payment-primary-content` 命中 |
| 合并 renderSpec.duplicateAudit | pass | 无阻塞重复主操作 |
| 合并 consistency profiles | pass | metadata 写入 layout/intent fingerprint |
| 应用 locked properties | pass | 画板尺寸、shell、结算支付骨架稳定 |
| pageIntent 仅用于校验 | pass | 未改写为列表管理、服务详情或登录页 |
| excludedStates 未进入主节点树 | pass | 未生成空、加载、错误态 |

## 页面结构

| 区域 | 节点类型 | 还原结果 | 备注 |
| --- | --- | --- | --- |
| `GlobalShell/Topbar` | Frame/Text | pass | 顶栏品牌、搜索与用户入口可编辑 |
| `GlobalShell/Footer` | Frame/Text | pass | 底栏链接与版权可编辑 |
| `GlobalShell/Sidebar` | Frame/Text | pass | 服务商城导航项高亮 |
| `PageBody/PAGE-006-订单结算` | Frame | pass | 页面本体内容区 |
| `Region/OrderSummary` | Frame/Text/Rectangle | pass | 服务 SKU 摘要、标签与企业主体选择 |
| `Region/BuyerInfo` | Frame/Text | pass | 联系人、企业抬头、优惠券与发票入口 |
| `Region/PaymentMethod` | Frame/Text/Rectangle | pass | 微信支付方式为默认选中项 |
| `Region/PriceSummary` | Frame/Text | pass | 费用汇总、应付金额与提交支付 CTA |
| `Component/Button/提交支付` | Frame/Text | pass | 主支付动作唯一 |

## 非默认状态排除记录

| 状态 | 来源 | 处理方式 |
| --- | --- | --- |
| 空状态 | `excludedStates` | 仅记录，未写入 Figma |
| 加载状态 | `excludedStates` | 仅记录，未写入 Figma |
| 错误状态 | `excludedStates` | 仅记录，未写入 Figma |

## Token 文件读取

| 文件 | 是否读取 | 说明 |
| --- | --- | --- |
| tokens.json | 是 | 颜色、文本、间距、圆角 fallback |
| figma-tokens.json | 是 | 作为 Figma token 参考 |
| component-rules.json | 是 | 基础组件 fallback |
| component-blueprints.json | 是 | 无基础 primitive 时降级 |
| figma-render-rules.json | 是 | 布局与 QA 规则 |
| ai-generation-rules.json | 是 | 页面意图策略参考 |
| consistency-rules.json | 是 | profile 优先级参考 |
| layout-profile-schema.json | 是 | profile 合同参考 |
| page-intent-patterns.json | 是 | checkout payment 语义校验 |
| layout-profile.json | 是 | `客户端-PC-Web` |
| intent-profile.json | 补齐 | `checkout-payment` intent profile 由本次前置 JSON 补齐 |
| state-group-profile.json | 不适用 | 当前页面无状态组 |

## 组件处理

| 组件 | 节点 | 处理方式 | 备注 |
| --- | --- | --- | --- |
| OrderSummary | 服务 SKU 摘要 | fallback | 可编辑卡片与标签 |
| Select | 企业主体选择 | fallback | 可编辑选择框 |
| ContactInfo | 联系人信息 | fallback | 可编辑描述文本 |
| EntryGroup | 优惠券、发票入口 | fallback | 可编辑入口条 |
| PaymentMethod | 微信支付方式 | fallback | 可编辑支付选项 |
| PriceSummary | 应付金额 | fallback | 可编辑费用卡片 |
| Button | 提交支付 | fallback | 可编辑 Frame/Text |

## 两阶段布局 QA

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| 根画板尺寸正确 | pass | 1440 x 1024 |
| 第一阶段尺寸计算完成 | pass | 所有节点创建时有稳定尺寸 |
| 第二阶段 append 后 fill/hug 设置完成 | pass | 使用固定安全尺寸与可编辑 fallback |
| 主要容器高度正常 | pass | PageBody 888 |
| Root bounds 未越界 | pass | outOfBounds = 0 |
| Shell/Header/Footer 未与内容冲突 | pass | Topbar/Sidebar/Footer 与 body 分区明确 |
| layoutBudget 命中 | pass | 内容在 1024 高根画板内 |
| 压缩/滚动策略执行 | pass | 未触发压缩或滚动 |
| 文本未明显挤压或溢出 | pass | 订单、买方、支付与金额文案均在容器内 |
| 组件最小尺寸与文本策略已应用 | pass | CTA 高度 44，支付选项高度 88 |
| Profile locked 尺寸/间距已应用 | pass | PC Web shell 与内容区稳定 |
| 主要容器无明显堆叠 | pass | 截图与结构 QA 均通过 |
| 空白位置追加且未覆盖原内容 | pass | 新根画板放置于 x=6240, y=0 |

## 重复语义与动作 QA

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| duplicateAudit 已读取 | pass | 源 JSON 无 blocking issue |
| 重复可见文本 | pass | 导航文本与页面正文属于不同角色 |
| 重复 actionIntent | pass | 主支付动作唯一 |
| 重复 primary action | pass | 仅一个提交支付按钮 |
| Renderer 是否引入新重复 | pass | 未引入阻塞重复动作 |

## 关键语义区校验

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| pageIntent | pass | `checkout-payment` |
| 必备区域命中 | pass | OrderSummary、BuyerInfo、PaymentMethod、PriceSummary、PaymentAction |
| 源 JSON 是否包含关键区 | pass | renderSpec.qaExpectations 已声明 |
| Figma 结果是否包含关键区 | pass | 结构 QA 全部为 true |
| 是否发生 renderer loss | pass | 未丢失关键区域 |
| 是否使用固定模板替代 | pass | 未出现后台表格、登录凭证槽或状态页 |

## 截图/结构 QA

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| 截图 URL | pass | `https://www.figma.com/api/mcp/asset/c9d8cfc9-0897-428a-9e3f-587465346e49` |
| 总节点数 | pass | 100 |
| Frame 节点数 | pass | 17 |
| Text 节点数 | pass | 58 |
| 关键文本样本 | pass | 服务商城、订单结算、商标注册基础套餐、微信支付、¥3,999.00、提交支付 |
| 不应出现的状态页/表格/截图占位 | pass | adminTable=false、authCredentialSlot=false、emptyState=false、screenshotImage=false |

## 警告

- 原目录缺少 `figma-nodes.json`，本次已基于 `layout.md` 前置补齐 pm08 可消费 JSON，并记录为语义 warning。
- 截图 URL 为 Figma MCP 临时资源，后续可能过期；结构 QA 和 Figma 节点 ID 已记录。
- 基础组件未导入真实设计系统组件，使用可编辑 fallback 还原。

## 人工检查项

- 对比新建的 `PAGE-006 - 订单结算 - PC Web` 与后续正式 pm06/pm07 生成版本的差异。
- 如需真实组件实例，可后续补充 Code Connect 或组件 key 后重跑 pm08。
