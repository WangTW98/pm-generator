# Ant Design Admin 设计规范

## 0.文档状态

| 项目 | 内容 |
|---|---|
| 设计规范名称 | Ant Design Admin 设计规范 |
| 用户原始描述 | antd风格后台管理设计规范 |
| 自动推断方向 | Ant Design 5 / Ant Design Pro 风格，面向高密度后台与企业级 PC Web |
| 适用设备 | Mobile App、Mobile Web、Tablet、PC Web、PC Desktop App |
| 输出目录 | `design/ant-design-admin/` |
| HTML预览 | `design/ant-design-admin/index.html` |
| 通用Token | `design/ant-design-admin/tokens.json` |
| Figma Token | `design/ant-design-admin/figma-tokens.json` |
| 组件规则 | `design/ant-design-admin/component-rules.json` |
| 组件蓝图 | `design/ant-design-admin/component-blueprints.json` |
| Figma渲染规则 | `design/ant-design-admin/figma-render-rules.json` |
| AI生成规则 | `design/ant-design-admin/ai-generation-rules.json` |
| 一致性规则 | `design/ant-design-admin/consistency-rules.json` |
| Layout Profile Schema | `design/ant-design-admin/layout-profile-schema.json` |
| 页面意图模式 | `design/ant-design-admin/page-intent-patterns.json` |
| 文档状态 | Development |
| 创建/更新时间 | 2026-05-18 |

## 1.设计风格定位

### 1.1 风格关键词

| 关键词 | 含义 | 页面生成影响 |
|---|---|---|
| Ant Design | 企业级、清晰、规整 | 使用蓝色主色、紧凑表单、明确状态色、标准控件尺寸 |
| 高密度后台 | 信息可扫描 | 减少装饰，优先表格、表单、筛选、状态标签 |
| 稳定一致 | 同 layout 和同状态组保持基线一致 | 使用 layout/pageIntent/stateGroup profile 锁定尺寸与间距 |

### 1.2 设计气质

- 核心气质：理性、专业、清晰、可靠。
- 适合场景：管理后台、客户工作台、登录认证、订单/案件/配置类页面。
- 不适合场景：营销落地页、强插画消费类页面。
- 信息密度：PC Web 中高密度，移动端降密度。
- 交互节奏：操作路径明确，反馈克制，状态可见。

### 1.3 设计原则

| 原则 | 说明 | AI生成页面时的执行规则 |
|---|---|---|
| 结构优先 | 先固定 shell 与区域，再填业务内容 | 不允许为每页重新决定全局容器宽度和 auth card 尺寸 |
| 控件真实 | 控件需可编辑、可复用 | 按 component-blueprints 生成 Button/Input/Tabs/Table 等 |
| 默认状态优先 | 只生成默认可见状态 | 空/加载/错误/弹窗打开态只记录，不进入 Figma 主树 |
| 防溢出 | 文本、表单、表格需有最小宽高和截断规则 | 使用 renderSpec 与 profile locked properties |

## 2.设备形态与响应式策略

### 2.1 适配范围

| 设备形态 | 适配目标 | 布局策略 | 关键限制 |
|---|---|---|---|
| Mobile App | 认证、待办、详情处理 | 单列、底部主操作 | 触控目标不低于 44px |
| Mobile Web | 轻量访问 | 单列、折叠导航 | 禁止横向滚动 |
| Tablet | 审核/表单 | 双栏或主从结构 | 侧栏可折叠 |
| PC Web | 核心后台与门户 | 1440 基准画板，顶栏/侧栏/内容区 | 内容区 padding 24px |
| PC Desktop App | 高密度操作 | 多栏、紧凑表格 | 支持键盘和固定操作区 |

### 2.2 断点规则

| 断点 | 宽度范围 | 布局变化 | 组件变化 |
|---|---|---|---|
| Mobile | 0-599px | 单列堆叠 | 表单全宽，按钮满宽 |
| Tablet | 600-1023px | 双栏/主从 | 表格转列表 |
| Desktop | 1024-1439px | 标准后台布局 | 表格/筛选常规密度 |
| Wide Desktop | 1440px+ | 内容最大宽度受控 | 保持容器宽度，不无限拉伸 |

### 2.3 多端转换规则

- 移动端到平板：保留主任务，次要信息进入折叠区。
- 平板到PC Web：恢复侧栏、表格、批量操作。
- PC Web到PC桌面应用：提高密度，固定工具栏与操作列。
- 导航转换：移动端底部/抽屉，PC 顶栏+侧栏。
- 表格/列表转换：移动端使用卡片列表，PC 使用表格。
- 表单转换：移动端单列，PC 使用 1-2 列表单。

## 3.颜色系统

### 3.1 核心色板

| Token | 色值 | 用途 | 使用规则 |
|---|---|---|---|
| `--color-primary` | #1677FF | 主操作/激活态 | 每屏只突出 1 个主操作 |
| `--color-secondary` | #0958D9 | Hover/强调 | 用于主色深色态 |
| `--color-accent` | #13C2C2 | 辅助强调 | 用于信息提示，不替代主按钮 |
| `--color-background` | #F5F7FA | 页面背景 | 低对比灰底 |
| `--color-surface` | #FFFFFF | 卡片/表单容器 | 默认内容面 |
| `--color-surface-raised` | #FFFFFF | 顶栏/浮层 | 配合阴影或边框 |
| `--color-text-primary` | #1F2329 | 主文本 | 标题与关键字段 |
| `--color-text-secondary` | #646A73 | 次文本 | 说明和辅助信息 |
| `--color-border` | #D9DDE5 | 边框/分割线 | 控件与卡片边界 |
| `--color-success` | #52C41A | 成功 | 标签/反馈 |
| `--color-warning` | #FAAD14 | 警告 | 待处理/风险 |
| `--color-error` | #FF4D4F | 错误 | 校验/危险操作 |
| `--color-info` | #1677FF | 信息 | 通知和链接 |

### 3.2 配色使用规则

- 主色使用：按钮、链接、激活 tab、focus ring。
- 辅助色使用：hover、强调信息。
- 背景与容器：页面灰底，内容白面，边界清晰。
- 文本与边框：主文本 #1F2329，弱文本 #86909C，边框 #D9DDE5。
- 语义色：成功/警告/错误只用于状态，不用于大面积背景。
- 禁止事项：不要使用大面积渐变、营销式色块、过度装饰。

## 4.字体与排版

### 4.1 字体族

| 场景 | 字体 | 说明 |
|---|---|---|
| 中文界面 | Inter, PingFang SC, Microsoft YaHei, sans-serif | Figma 默认用 Inter；中文由系统回退 |
| 英文/数字 | Inter | 表格数字、状态码 |
| 代码/等宽数字 | SFMono-Regular, Menlo, monospace | 编号、验证码 |

### 4.2 字号阶梯

| Token | 桌面端 | 移动端 | 字重 | 行高 | 用途 |
|---|---:|---:|---|---|---|
| `--font-display` | 32 | 26 | 600 | 40 | 认证页主标题 |
| `--font-title` | 24 | 22 | 600 | 32 | 页面标题 |
| `--font-heading` | 18 | 17 | 600 | 26 | 区块标题 |
| `--font-body` | 14 | 14 | 400 | 22 | 正文/表格 |
| `--font-label` | 13 | 13 | 500 | 20 | 表单标签 |
| `--font-caption` | 12 | 12 | 400 | 18 | 辅助说明 |
| `--font-number` | 20 | 18 | 600 | 28 | 数据指标 |

### 4.3 排版规则

- 标题：短句、强层级、避免营销文案。
- 正文：14px/22px，表格单元格允许省略号。
- 辅助信息：12-13px，颜色使用 secondary。
- 数字与金额：使用 tabular 风格或等宽数字。
- 表格文字：默认单行截断，关键列允许两行。
- 移动端排版：减少列数，按钮和输入框全宽。

## 5.布局系统

### 5.1 栅格与宽度

| 场景 | 宽度/列数 | 边距 | 间距 | 说明 |
|---|---|---|---|---|
| Mobile App | 390 / 1列 | 16 | 12 | 单列表单 |
| Mobile Web | 390 / 1列 | 16 | 12 | 浏览器安全区域 |
| Tablet | 834 / 8列 | 24 | 16 | 双栏 |
| PC Web | 1440 / 12列 | 24 | 16/24 | 标准后台 |
| PC Desktop App | 1440 / 多栏 | 20 | 12/16 | 高密度 |

### 5.2 间距系统

| Token | 值 | 用途 |
|---|---:|---|
| `--space-1` | 4 | 小图标间距 |
| `--space-2` | 8 | 控件内部 |
| `--space-3` | 12 | 表单行间距 |
| `--space-4` | 16 | 组件间距 |
| `--space-6` | 24 | 区块间距 |
| `--space-8` | 32 | 卡片内边距 |
| `--space-10` | 40 | 页面组间距 |
| `--space-12` | 48 | 大区块间距 |

### 5.3 页面布局模式

- 移动端单列：表单、按钮、协议文案堆叠。
- 平板双栏：品牌信息与操作面板分栏。
- PC Web内容区：顶栏 64px，内容区 padding 24px，页脚 56px。
- PC桌面应用多栏：侧栏 240px，内容区流式。
- 固定操作区：表单提交区固定在卡片底部或页面底部。

### 5.4 跨页面一致性规则

| 一致性层级 | 锁定内容 | 允许变化 | 下游执行规则 |
|---|---|---|---|
| Design Spec | 色彩、字号、控件规则、阴影、圆角 | 文案、业务字段 | pm06/pm08 必须读取 token 与 component rules |
| 使用Layout | 画板尺寸、顶栏、页脚、内容区、认证左右栏 | 页面主体 slot 内容 | `layout-profile.json` 锁定 |
| Page Intent | auth 骨架、卡片宽度、方法切换、主操作位置 | 认证方式字段 | `intent-auth-profile.json` 锁定 |
| State Group | 登录组共享卡片/切换区/主按钮/辅助链接 | 手机/邮箱/微信差异内容 | `state-STATE-001-profile.json` 锁定 |

### 5.5 Profile 与 Fingerprint

- Layout Profile：`product/style-profiles/<layoutKey>/layout-profile.json`，锁定画板和全局壳。
- Intent Profile：`product/style-profiles/<layoutKey>/intent-auth-profile.json`，锁定 auth 页面骨架。
- State Group Profile：`product/style-profiles/<layoutKey>/state-STATE-001-profile.json`，锁定同登录组面板和 slot。
- Fingerprint字段：`layoutHash`、`intentHash`、`stateGroupHash`。
- Drift处理：locked 字段不一致时 warning 或 failed，不静默覆盖。

## 6.视觉层级

### 6.1 圆角

| Token | 值 | 用途 |
|---|---:|---|
| `--radius-sm` | 4 | 小标签/控件 |
| `--radius-md` | 6 | 输入框/按钮 |
| `--radius-lg` | 8 | 卡片/面板 |
| `--radius-xl` | 12 | 登录卡片 |
| `--radius-pill` | 999 | 徽标/胶囊 |

### 6.2 阴影与边框

| Token | 值 | 用途 |
|---|---|---|
| `--shadow-sm` | 0 1px 2px rgba(0,0,0,.04) | 控件/轻卡片 |
| `--shadow-md` | 0 8px 24px rgba(31,35,41,.08) | 登录卡片 |
| `--shadow-lg` | 0 16px 40px rgba(31,35,41,.12) | 浮层 |
| `--border-subtle` | 1px solid #D9DDE5 | 默认边框 |

### 6.3 层级规则

- 页面背景：#F5F7FA。
- 基础容器：白色，1px 边框，8px 圆角。
- 强调容器：白色 + shadow-md。
- 浮层：shadow-lg，圆角 8px。
- 禁止事项：不使用卡套卡堆叠；页面区块不做营销 hero。

## 7.核心组件规范

| 组件 | 默认样式 | 状态 | 响应式规则 | AI生成约束 |
|---|---|---|---|---|
| Button | 32/40/44 高，6px 圆角 | primary/default/danger/disabled/loading | 移动端可全宽 | 主按钮每面板仅一个 |
| Input | 40px 高，边框 #D9DDE5 | default/focus/error/disabled | 宽度随容器填充 | 必须有 label 或 placeholder |
| Tabs/Segmented | 40px 高，active 蓝色 | active/hover/disabled | 移动端横向滚动 | 同状态组位置固定 |
| Card | 白底，8/12px 圆角 | default/raised | 移动端全宽 | 不嵌套卡片 |
| Table | 48px 表头，56px 行高 | hover/selected/empty | 移动端转卡片 | 列必须显式定义 |
| QRCode | 180x180 默认 | loading/expired | 移动端居中 | 不压缩到小于 160px |

## 8.Figma节点生成规则

- 所有组件生成可编辑节点，不允许整页截图。
- Frame 优先使用 Auto Layout。
- 文本需设置最大宽度、换行或省略。
- 认证卡片宽度固定 420px，内容槽由 state group profile 控制。
- 默认画板 PC Web 为 1440x900 或 1440x1024。

## 9.AI页面生成约束

- 只生成默认状态主界面。
- 页面结构必须来自 layout.md，不能根据页面名套模板。
- pageIntent 为 `auth` 时必须包含品牌区、认证卡片、登录方式切换、身份输入/凭证控件、主按钮、协议/帮助链接。
- 同 `STATE-001` 登录组的卡片尺寸、切换区、主按钮位置必须一致。

## 10.HTML预览说明

HTML 预览展示色板、排版、控件状态和 PC Web 登录样例，作为 pm05 输出的人工检查面板。

## 11.下游使用说明

- pm06 使用本目录 JSON 生成 `figma-nodes.json` 与 `product/style-profiles`。
- pm07 批量时按 `layoutKey/pageIntentKey/stateGroupKey` 复用 profile。
- pm08 导入 Figma 时使用 `profile-locked` 模式。
- pm09 批量导入时按一致性分组排序。

## 12.待确认与假设

- A-001【假设】
  - 内容：虽然本次用户要求是后台 antd 风格，但登录页位于客户端 PC Web；因此规范使用 Ant Design 企业风格并适配客户端认证页。
  - 影响范围：客户端登录页视觉会更接近企业级门户，而非营销官网。
  - 用户回复：
