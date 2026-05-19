# PAGE-002 - 手机号登录 Figma Restore Report

## 基本信息

- 源 JSON：`product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-002-手机号登录/figma-nodes.json`
- 目标 Figma：https://www.figma.com/design/ZP4TjUsWJyHncnukJuVfRD/Agent-Test?node-id=215-55&t=A760mTWqS5Qcfqy5-1
- fileKey：`ZP4TjUsWJyHncnukJuVfRD`
- nodeId：`215:55`
- 目标页面：`Case Tracking PRD7`
- 写入模式：`append`，按用户要求不覆盖现有同名画板
- consistencyMode：`profile-locked`
- 渲染模式：`pm08 RenderPlan + editable fallback components`
- 根画板：`PAGE-002 - 手机号登录 - PC Web (2)`
- 根节点 ID：`239:2`
- pageIntent：`auth`
- layoutKey：`客户端-PC-Web`
- pageIntentKey：`auth`
- stateGroupKey：`STATE-001`
- designSpecKey：`antd-admin-design-spec`
- styleFingerprint：`b2968cd4bc2c/cacd70c11191/fc0e36bec1f5`
- semanticValidation：`pass`
- renderSpec：已读取并应用关键组件、文本、约束和 QA 预期
- consistency：已读取并写入 metadata
- layoutBudget：`pass`
- duplicateAudit：`pass`
- slotContracts：`slot-auth-primary-content`
- 执行时间：`2026-05-19`

## 还原结果

- 状态：`Done`
- 创建节点数：45
- 更新节点数：0
- 跳过节点数：6 个非默认状态记录未写入 Figma
- 组件适配数量：7
- 组件降级数量：7，均为可编辑 Frame/Text fallback
- token 命中数量：使用主色、背景、表面、文本、边框、状态色、间距和圆角
- token 缺失数量：0 个阻塞项
- profile 命中数量：3
- profile 缺失数量：0
- fingerprint 匹配：`pass`
- driftCheck：`pass`
- root/shell collision：`pass`
- duplicateAudit：`pass`
- 关键语义区校验：`pass`
- 截图 QA：`pass`

## RenderPlan 校验

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| 使用 figmaDocument.children 作为主来源 | pass | 没有根据页面名称另套模板 |
| RenderPlan 节点总数 | pass | Figma 结果 45 节点 |
| 保留原始节点顺序 | pass | Topbar、Footer、PageBody、AuthPanel 顺序可追踪 |
| 保留 sourceNodeId metadata | pass | 写入 `pm08_md2figma/sourceNodeId` |
| 合并 renderSpec.componentInstances | pass | Tabs、Input、Button、Checkbox、Text fallback 已生成 |
| 合并 renderSpec.textPolicies | pass | 文本节点可编辑，控件保留稳定尺寸 |
| 合并 renderSpec.constraints | pass | 根画板 1440 x 1024，AuthPanel 420 宽 |
| 合并 renderSpec.layoutBudget | pass | 内容未越界 |
| 合并 renderSpec.slotContracts | pass | `slot-auth-primary-content` 命中 |
| 合并 renderSpec.duplicateAudit | pass | 无阻塞重复主操作 |
| 合并 consistency profiles | pass | metadata 写入 layout/intent/state profile fingerprint |
| 应用 locked properties | pass | 画板尺寸、shell、auth 骨架、状态组几何保持稳定 |
| pageIntent 仅用于校验 | pass | 未改写业务节点树 |
| excludedStates 未进入主节点树 | pass | 未生成空、加载、错误态 |

## 一致性 Profile 校验

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| layoutProfile | pass | `product/style-profiles/客户端-PC-Web/layout-profile.json` |
| intentProfile | pass | `product/style-profiles/客户端-PC-Web/intent-auth-profile.json` |
| stateGroupProfile | pass | `product/style-profiles/客户端-PC-Web/state-STATE-001-profile.json` |
| layoutProfileHash | pass | `b2968cd4bc2c` |
| intentProfileHash | pass | `cacd70c11191` |
| stateGroupProfileHash | pass | `fc0e36bec1f5` |
| styleFingerprint | pass | `b2968cd4bc2c/cacd70c11191/fc0e36bec1f5` |
| driftCheck | pass | JSON 中为 pass |
| existing Figma fingerprint | pass | append 模式未覆盖旧画板 |
| metadata 写入 | pass | 根画板包含 layoutKey、pageIntentKey、stateGroupKey、styleFingerprint、writeMode |

## 页面结构

| 区域 | 节点类型 | 还原结果 | 备注 |
| --- | --- | --- | --- |
| `GlobalShell/Topbar` | Frame/Text | pass | 顶栏品牌与动作入口可编辑 |
| `GlobalShell/Footer` | Frame/Text | pass | 底栏链接与版权可编辑 |
| `PageBody/PAGE-002-手机号登录` | Frame | pass | 页面本体未混入登录后侧栏 |
| `Section/AuthPanel` | Frame | pass | 认证卡片独立区域 |
| 登录方式切换 | Frame/Text | pass | 手机号、微信扫码、邮箱密码三段控件 |
| 凭证输入 | Frame/Text | pass | 手机号、短信验证码、获取验证码 |
| 协议与帮助 | Frame/Text/Rectangle | pass | 协议勾选框与辅助链接 |
| 主操作 | Frame/Text | pass | 登录按钮 |

## 非默认状态排除记录

| 状态 | 来源 | 处理方式 |
| --- | --- | --- |
| 空状态 | `excludedStates` | 仅记录，未写入 Figma |
| 加载状态 | `excludedStates` | 仅记录，未写入 Figma |
| 错误状态 | `excludedStates` | 仅记录，未写入 Figma |
| 空状态文案 | `excludedStates` | 仅记录，未写入 Figma |
| 加载文案 | `excludedStates` | 仅记录，未写入 Figma |
| 错误文案 | `excludedStates` | 仅记录，未写入 Figma |

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
| page-intent-patterns.json | 是 | auth 语义校验 |
| layout-profile.json | 是 | `客户端-PC-Web` |
| intent-profile.json | 是 | `auth` |
| state-group-profile.json | 是 | `STATE-001` |

## 组件处理

| 组件 | 节点 | 处理方式 | 备注 |
| --- | --- | --- | --- |
| Text | 平台名称、说明、链接 | fallback | 可编辑 Text |
| Tabs / Segmented Control | 登录方式分段控件 | fallback | 可编辑 Frame/Text |
| Input | 手机号、短信验证码 | fallback | 可编辑输入框结构 |
| Button | 获取验证码、登录 | fallback | 可编辑 Frame/Text |
| Checkbox | 协议勾选框 | fallback | Rectangle + Text |
| Tag | 客户账号访问、服务购买、案件追踪 | fallback | 可编辑状态标签 |

## RenderSpec 命中情况

| 项目 | 命中数量 | 降级数量 | 说明 |
| --- | ---: | ---: | --- |
| componentInstances | 7 | 7 | 全部使用可编辑 fallback |
| textPolicies | 11 | 0 | 文本未明显溢出 |
| constraints | 18 | 0 | 核心尺寸和布局已应用 |
| qaExpectations | 6 | 0 | 关键 auth 区命中 |
| consistencyProfiles | 3 | 0 | layout/intent/state 已记录 |
| lockedProperties | 6 | 0 | profile-locked |

## 两阶段布局 QA

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| 根画板尺寸正确 | pass | 1440 x 1024 |
| 第一阶段尺寸计算完成 | pass | 所有节点创建时有稳定尺寸 |
| 第二阶段 append 后 fill/hug 设置完成 | pass | 使用固定安全尺寸与可编辑 fallback |
| 主要容器高度正常 | pass | PageBody 888，AuthPanel 576 |
| Hug 容器未异常拉伸 | pass | 协议区、辅助链接区高度正常 |
| Root bounds 未越界 | pass | outOfBounds = 0 |
| Shell/Header/Footer 未与内容冲突 | pass | Topbar/Footer 与 body 分区明确 |
| layoutBudget 命中 | pass | 内容在 1024 高根画板内 |
| 压缩/滚动策略执行 | pass | 未触发压缩或滚动 |
| 文本未明显挤压或溢出 | pass | 控件宽度 356，按钮高度 44 |
| 组件最小尺寸与文本策略已应用 | pass | 输入、按钮满足最小点击尺寸 |
| Profile locked 尺寸/间距已应用 | pass | AuthPanel、Topbar、Footer、body 按 profile 稳定尺寸 |
| 主要容器无明显堆叠 | pass | 截图与结构 QA 均通过 |
| 空白位置追加且未覆盖原内容 | pass | 新根画板位于 x=6240，旧 `PAGE-002 - 手机号登录 - PC Web` 保留 |

## 重复语义与动作 QA

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| duplicateAudit 已读取 | pass | 源 JSON 无 blocking issue |
| 重复可见文本 | pass | 同类导航与协议文本属于不同角色 |
| 重复 actionIntent | pass | 主登录动作唯一 |
| 重复 primary action | pass | 仅一个主登录按钮 |
| 允许的角色差异 | pass | 获取验证码为辅助动作 |
| Renderer 是否引入新重复 | pass | 未引入阻塞重复动作 |

## 关键语义区校验

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| pageIntent | pass | `auth` |
| 必备区域命中 | pass | AuthPanel、AuthMethodSwitch、IdentityInput、credential control、agreement、primary submit |
| 源 JSON 是否包含关键区 | pass | renderSpec.qaExpectations 已声明 |
| Figma 结果是否包含关键区 | pass | 结构 QA 全部为 true |
| 是否发生 renderer loss | pass | 未丢失关键区域 |
| 是否使用固定模板替代 | pass | 未出现列表、表格或管理台筛选模板 |

## 截图/结构 QA

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| 截图 URL | pass | `https://www.figma.com/api/mcp/asset/3e8e3e18-9595-44d3-ba97-6e2deae31f5f` |
| 直接子节点 | pass | `GlobalShell/Topbar`、`GlobalShell/Footer`、`PageBody/PAGE-002-手机号登录` |
| 总节点数 | pass | 45 |
| Frame 节点数 | pass | 18 |
| Text 节点数 | pass | 26 |
| 关键文本样本 | pass | 合规服务平台、Case Tracking Portal、手机号登录、请输入手机号、请输入短信验证码、登录 |
| 不应出现的状态页/表格/截图占位 | pass | table=false、emptyState=false、screenshotImage=false |

## 警告

- 截图 URL 为 Figma MCP 临时资源，后续可能过期；结构 QA 和 Figma 节点 ID 已记录。
- 基础组件未导入真实设计系统组件，使用可编辑 fallback 还原。

## 人工检查项

- 对比 `PAGE-002 - 手机号登录 - PC Web` 与新建的 `PAGE-002 - 手机号登录 - PC Web (2)` 的视觉差异。
- 如需真实组件实例，可后续补充 Code Connect 或组件 key 后重跑 pm08。
