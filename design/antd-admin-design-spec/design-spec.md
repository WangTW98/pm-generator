# Ant Design 风格后台管理设计规范

## 0.文档状态

| 项目 | 内容 |
|---|---|
| 设计规范名称 | Ant Design 风格后台管理设计规范 |
| 用户原始描述 | 使用pm-05-design-spec skill生成antd风格的完整后台管理设计规范 |
| 自动推断方向 | Ant Design v5 气质的企业级后台管理系统，面向知识产权与合规服务平台，强调高信息密度、清晰操作、流程状态可追踪和权限可控 |
| 适用设备 | PC Web 为主；同时定义 Mobile App、Mobile Web、Tablet、PC Desktop App 的降级/延展规则 |
| 输出目录 | `design/antd-admin-design-spec/` |
| HTML预览 | `design/antd-admin-design-spec/index.html` |
| 通用Token | `design/antd-admin-design-spec/tokens.json` |
| Figma Token | `design/antd-admin-design-spec/figma-tokens.json` |
| 组件规则 | `design/antd-admin-design-spec/component-rules.json` |
| 组件蓝图 | `design/antd-admin-design-spec/component-blueprints.json` |
| Figma渲染规则 | `design/antd-admin-design-spec/figma-render-rules.json` |
| AI生成规则 | `design/antd-admin-design-spec/ai-generation-rules.json` |
| 一致性规则 | `design/antd-admin-design-spec/consistency-rules.json` |
| Layout Profile Schema | `design/antd-admin-design-spec/layout-profile-schema.json` |
| 页面意图模式 | `design/antd-admin-design-spec/page-intent-patterns.json` |
| 文档状态 | Development |
| 创建/更新时间 | 2026-05-19 |

## 1.设计风格定位

### 1.1 风格关键词

| 关键词 | 含义 | 页面生成影响 |
|---|---|---|
| Ant Design | 使用企业级中后台常见的侧栏、顶栏、表格、筛选、表单、抽屉与反馈组件语言 | 默认生成标准 Admin Shell；组件尺寸、圆角、状态接近 Ant Design v5 |
| 高信息密度 | 管理员需要快速处理订单、案件、流程配置和风险项 | 以表格、分组表单、指标卡、状态标签为主，不使用营销式大图 |
| 流程可追踪 | 合规服务有接单、资料审核、协议回填、案件交付等节点 | 详情页必须包含状态摘要、步骤条/时间线、审核面板和日志入口 |
| 权限与风控清晰 | 后台区分超管、管理员、录入专员 | 危险操作、权限入口、风险提报要有显著状态和二次确认 |
| 双品牌运营 | 一套后台管理两个前端域名/品牌 | 客户、订单、商品、消息等列表需要显示平台/品牌标签和筛选项 |

### 1.2 设计气质

- 核心气质：克制、专业、可靠、可扫描，类似成熟 SaaS 中台而非品牌展示页。
- 适合场景：工作台、订单接单、案件办理、客户管理、服务模板配置、消息模板配置、权限管理。
- 不适合场景：强视觉营销页、沉浸式品牌首页、重插画活动页。
- 信息密度：桌面端中高密度；首屏优先显示任务、风险、核心指标和可执行列表。
- 交互节奏：以快速筛选、批量处理、抽屉查看、表格行内操作和二次确认构成。

### 1.3 设计原则

| 原则 | 说明 | AI生成页面时的执行规则 |
|---|---|---|
| Shell 稳定 | 全局侧栏和顶栏是后台识别锚点 | 页面正文不得重复生成 Logo、侧栏菜单、顶栏用户区 |
| 操作唯一 | 每页主任务必须明确 | 同一 Page Body 只能有一个默认可见 primary action intent |
| 数据先行 | 后台页面价值来自数据、状态和工作流 | 首屏优先放指标、待办、风险、表格或当前流程状态 |
| 状态显性 | 订单、案件、消息、风险必须一眼可辨 | 使用 Tag、Alert、Badge、Timeline，不用自由色块 |
| 配置可控 | 表单与模板配置需防错 | 关键配置分段、显示校验、保存前确认影响范围 |

## 2.设备形态与响应式策略

### 2.1 适配范围

| 设备形态 | 适配目标 | 布局策略 | 关键限制 |
|---|---|---|---|
| Mobile App | 处理通知、待办、简单审核 | 底部导航 + 单列卡片列表 + 详情 Sheet | 不承载复杂模板配置和大表格编辑 |
| Mobile Web | 临时查看后台状态 | 顶部简化导航 + 筛选抽屉 + 列表卡片 | 禁止横向溢出；表格必须转卡片 |
| Tablet | 轻量办公和审核 | 左侧窄导航 + 主从双栏 | 抽屉宽度不超过屏宽 70% |
| PC Web | 主要工作形态 | 232px 侧栏 + 56px 顶栏 + 24px 内容 padding | 首屏避免超过两层卡片，表格操作不拥挤 |
| PC Desktop App | 更密集多窗口作业 | 多栏工作台 + 可停靠面板 + 快捷工具栏 | 保持键盘可达和焦点可见 |

### 2.2 断点规则

| 断点 | 宽度范围 | 布局变化 | 组件变化 |
|---|---|---|---|
| Mobile | 0-599px | 单列，隐藏侧栏，筛选进入抽屉 | 表格转卡片；按钮高度 40px |
| Tablet | 600-1023px | 窄侧栏或顶部导航，主体双栏 | 表格只保留关键列；详情可右侧抽屉 |
| Desktop | 1024-1439px | 标准后台 Shell，内容区 24px padding | 表格、表单、抽屉使用默认尺寸 |
| Wide Desktop | 1440px+ | 内容最大宽度可放宽，多列看板 | 指标卡 4-6 列；详情页可三栏 |

### 2.3 多端转换规则

- 移动端到平板：列表卡片升级为关键列表格，详情从全屏页升级为右侧详情面板。
- 平板到PC Web：启用完整侧栏、面包屑、筛选区和分页表格。
- PC Web到PC桌面应用：允许增加可停靠日志面板、批量任务面板和快捷操作栏。
- 导航转换：PC 侧栏为主；移动端使用底部一级导航和页面内二级 Tabs。
- 表格/列表转换：桌面表格至少保留状态、主体对象、负责人、更新时间、操作；移动端转摘要卡片。
- 表单转换：PC 可双列；移动端必须单列；长表单分段并固定底部操作。

## 3.颜色系统

### 3.1 核心色板

| Token | 色值 | 用途 | 使用规则 |
|---|---|---|---|
| `--color-primary` | #1677FF | 主操作、当前菜单、链接 | 每页唯一主操作使用，不用于大面积背景 |
| `--color-secondary` | #13C2C2 | 辅助强调、跨品牌提示 | 少量用于次级图表或信息标签 |
| `--color-accent` | #722ED1 | 高级配置、权限提示 | 只用于权限、模板、自动化等配置型模块 |
| `--color-background` | #F5F7FA | 页面背景 | 所有后台正文背景统一使用 |
| `--color-surface` | #FFFFFF | 卡片、表格、表单容器 | 配合 1px 细边框，不嵌套卡片 |
| `--color-surface-raised` | #FFFFFF | 抽屉、弹窗、下拉 | 配合中等阴影 |
| `--color-text-primary` | #1F2329 | 标题和重要字段 | 不低于 14px |
| `--color-text-secondary` | #4E5969 | 辅助说明、表格次要列 | 避免用于关键数值 |
| `--color-border` | #D9D9D9 | 输入框、按钮、分割线 | 控件边框默认色 |
| `--color-success` | #52C41A | 成功、已完成、审核通过 | 配合浅绿背景标签 |
| `--color-warning` | #FAAD14 | 待处理、临期、需关注 | 不代表失败 |
| `--color-error` | #FF4D4F | 错误、驳回、风险、危险操作 | 危险按钮必须二次确认 |
| `--color-info` | #1677FF | 处理中、普通信息 | 与主色一致 |

### 3.2 配色使用规则

- 主色使用：只用于当前导航、主按钮、链接和选中态，避免全屏蓝色背景。
- 辅助色使用：青色用于跨品牌或系统同步状态，紫色用于权限/模板/自动化能力。
- 背景与容器：页面背景 #F5F7FA，容器白底，表头 #FAFAFA。
- 文本与边框：正文 #1F2329，次级 #4E5969，说明 #86909C，边框 #D9D9D9/#F0F0F0。
- 语义色：成功、警告、错误、信息色固定，不允许根据个人偏好改色。
- 禁止事项：禁止一页内出现多个高饱和大色块；禁止用渐变背景包装后台核心内容。

## 4.字体与排版

### 4.1 字体族

| 场景 | 字体 | 说明 |
|---|---|---|
| 中文界面 | PingFang SC / Microsoft YaHei / system-ui | 保持后台长时间阅读舒适 |
| 英文/数字 | Segoe UI / Arial / system-ui | 与 Ant Design 常见 Web 字体兼容 |
| 代码/等宽数字 | SFMono-Regular / Consolas / Menlo | 用于订单号、案件号、日志和金额对齐 |

### 4.2 字号阶梯

| Token | 桌面端 | 移动端 | 字重 | 行高 | 用途 |
|---|---:|---:|---|---|---|
| `--font-display` | 28px | 24px | 600 | 1.28 | 登录页标题、大型指标组 |
| `--font-title` | 24px | 22px | 600 | 1.32 | 页面级标题或重要详情标题 |
| `--font-heading` | 20px | 18px | 600 | 1.4 | 分区标题、卡片标题 |
| `--font-body` | 14px | 14px | 400 | 1.57 | 表格、正文、表单 |
| `--font-label` | 14px | 14px | 500 | 1.43 | 表单标签、字段名 |
| `--font-caption` | 12px | 12px | 400 | 1.5 | 辅助信息、时间、说明 |
| `--font-number` | 24px | 22px | 600 | 1.2 | 指标数值、金额、统计 |

### 4.3 排版规则

- 标题：页面标题 20px/600；卡片标题 16px/600；标题不使用负字距。
- 正文：默认 14px，行高 22px，表格单元格单行省略。
- 辅助信息：12px，颜色 #86909C，不能承载关键状态。
- 数字与金额：使用等宽数字或 tabular-nums，指标数字 24px/600。
- 表格文字：主体列 14px，辅助列 12-14px；操作列保持动词短语。
- 移动端排版：不使用小于 12px 的文本；按钮文本不换行溢出。

## 5.布局系统

### 5.1 栅格与宽度

| 场景 | 宽度/列数 | 边距 | 间距 | 说明 |
|---|---|---|---|---|
| Mobile App | 1列 | 16px | 12px | 任务和消息卡片单列 |
| Mobile Web | 1列 | 16px | 12px | 筛选抽屉，列表卡片 |
| Tablet | 8列或主从双栏 | 20px | 16px | 可用窄侧栏 |
| PC Web | 12列，侧栏232px | 24px | 16/24px | 标准后台主要形态 |
| PC Desktop App | 12-16列，多面板 | 16-24px | 12/16px | 信息密度更高 |

### 5.2 间距系统

| Token | 值 | 用途 |
|---|---:|---|
| `--space-1` | 4px | 图标与文字间距、紧凑标签 |
| `--space-2` | 8px | 小组件内部间距 |
| `--space-3` | 12px | 表格行内、表单辅助间距 |
| `--space-4` | 16px | 组件组间距、卡片内部小分组 |
| `--space-6` | 24px | 页面 padding、卡片 padding |
| `--space-8` | 32px | 大分区间距 |
| `--space-10` | 40px | 登录页、空状态主间距 |
| `--space-12` | 48px | 大屏看板分区 |

### 5.3 页面布局模式

- 移动端单列：任务、消息、订单摘要以卡片堆叠；复杂操作进入全屏表单或底部 Sheet。
- 平板双栏：左侧列表/筛选，右侧详情预览；不可同时打开多个重型抽屉。
- PC Web内容区：PageHeader + FilterBar/MetricGrid + Table/Form/Detail，内容区 padding 24px。
- PC桌面应用多栏：支持左侧对象列表、中间详情、右侧日志/属性面板。
- 固定操作区：长表单、审核流程和模板配置必须使用底部 sticky footer actions。

### 5.4 跨页面一致性规则

| 一致性层级 | 锁定内容 | 允许变化 | 下游执行规则 |
|---|---|---|---|
| Design Spec | 颜色、字体、间距、圆角、组件状态 | 业务文案、模拟数据 | 不允许局部页面自由改 token |
| 使用Layout | 侧栏、顶栏、页面容器、正文背景 | 页面主体内容 | 同一端形态必须共享 AdminShell |
| Page Intent | 页面骨架、关键区域、组件组合 | 字段、表格列、卡片内容 | 根据 page-intent-patterns.json 锁定 |
| State Group | 流程详情骨架、步骤区、操作区 | active 状态、状态特有内容 | 同状态组只允许状态内容变化 |

### 5.5 Profile 与 Fingerprint

- Layout Profile：`layout-profile-schema.json#/definitions/layoutProfile`，用于锁定后台 Shell、断点、预算。
- Intent Profile：`layout-profile-schema.json#/definitions/intentProfile`，用于锁定列表、详情、表单、流程页骨架。
- State Group Profile：`layout-profile-schema.json#/definitions/stateGroupProfile`，用于服务详情流程等多状态页面。
- Fingerprint字段：layoutKey、pageIntentKey、stateGroupKey、requiredRegions、primaryActionIntent、componentFamilies。
- Drift处理：文案和模拟数据漂移可接受；布局、主操作、必备区域、组件族漂移必须失败或复核。

### 5.6 布局预算与安全边界

| 页面意图 | 适用设备 | 可用内容高度/宽度 | 主要容器上限 | 压缩顺序 | 滚动/失败策略 |
|---|---|---|---|---|---|
| dashboard | PC Web | viewport - 56px header - 48px padding | 指标区 160px，待办表格 420px | 图表说明、次要卡片、辅助列 | 表格滚动；核心指标不可裁切 |
| list-management | PC Web | 内容宽度 >= 960px | 筛选区最多两行，表格填充剩余 | 次要筛选、次要列、批量说明 | 保留主体列与操作列，否则失败 |
| detail | PC Web | 详情主体 >= 720px | 摘要区 160px，日志区可滚动 | 辅助字段、备注、历史记录 | 详情 Tab 内滚动 |
| form-create-edit | PC Web | 表单区 720-1040px | 每段 8-12 个字段 | 帮助文案、非必填字段、示例 | 底部操作固定；必填项不可隐藏 |
| wizard-flow | PC Web | 主区 + 320px 侧栏 | Steps 64px，表单自适应 | 次要说明、侧栏历史 | 当前步骤内容不可折叠 |

规则：

- 必须预留全局 Shell、固定导航、底部操作区、Footer 和页面 padding。
- `hug` 容器应按内容真实高度增长，不能被拉伸为填满剩余区域。
- 内容超出预算时，按设计定义的压缩顺序处理；无法安全容纳时，下游应失败或要求人工复核。
- 同 `layoutKey + pageIntentKey + stateGroupKey` 的页面必须共享预算与 slot 合同，除非明确记录 allowed override。

## 6.视觉层级

### 6.1 圆角

| Token | 值 | 用途 |
|---|---:|---|
| `--radius-sm` | 4px | Tag、Badge、小型控件 |
| `--radius-md` | 6px | Button、Input、Select |
| `--radius-lg` | 8px | Card、Modal、Drawer |
| `--radius-xl` | 12px | 登录页容器、空状态容器 |
| `--radius-pill` | 999px | Badge、圆形头像、胶囊筛选 |

### 6.2 阴影与边框

| Token | 值 | 用途 |
|---|---|---|
| `--shadow-sm` | 0 1px 2px rgba(0, 0, 0, 0.04) | 卡片轻微抬升 |
| `--shadow-md` | 0 6px 16px rgba(0, 0, 0, 0.08) | 下拉、Popover、抽屉 |
| `--shadow-lg` | 0 12px 32px rgba(0, 0, 0, 0.12) | Modal、全局浮层 |
| `--border-subtle` | 1px solid #F0F0F0 | 表格、卡片分割 |

### 6.3 层级规则

- 页面背景：统一 #F5F7FA，不放装饰背景。
- 基础容器：白底 + 1px #F0F0F0 + 8px 圆角。
- 强调容器：浅蓝、浅黄、浅红状态背景，只用于提示和风险。
- 浮层：白底 + 阴影 + 8px 圆角，必须有明确关闭方式。
- 禁止事项：禁止卡片套卡片；禁止使用超过 12px 的后台卡片圆角；禁止泛滥阴影。

## 7.核心组件规范

| 组件 | 默认样式 | 状态 | 响应式规则 | AI生成约束 |
|---|---|---|---|---|
| Button | 高 32px，圆角 6px，主按钮 #1677FF | hover #4096FF，focus 蓝色外环，disabled 灰底 | 移动端高 40px，工具栏按钮仍可 32px | 每页默认可见主按钮最多一个 |
| Input | 高 32px，白底，1px #D9D9D9 | focus 主色边框和浅蓝外环，error 红边框 | 移动端占满一行 | 搜索、筛选、表单字段语义清晰 |
| Select | 高 32px，下拉白底中阴影 | hover/focus 同 Input | 筛选过多进入抽屉 | 不用 Select 表达二元开关 |
| Checkbox / Switch | Switch 用于即时开关，Checkbox 用于多选 | checked 主色，disabled 灰色 | 移动端保持 40px 点击热区 | 危险开关需要确认 |
| Tabs / Segmented Control | Tabs 用于内容分区，Segmented 用于同维度切换 | active 主色，hover 浅灰 | 移动端可横向滚动 | 不把导航菜单伪装为 Tabs |
| Navigation | 深色侧栏，当前项主色背景，顶栏白底 | collapsed 仅图标，hover 深色高亮 | 移动端隐藏侧栏 | Page Body 不重复生成全局导航 |
| Card | 白底、8px、细边框、24px padding | hover 可轻微阴影，仅可点击卡片使用 | 移动端 padding 16px | 不嵌套卡片；指标卡可成组 |
| Table | 表头 #FAFAFA，行高 48px，分割线 #F0F0F0 | hover 行浅灰，selected 浅蓝 | 移动端转卡片，平板保留关键列 | 行操作最多 3 个可见项 |
| List | 单行/双行摘要 + 状态标签 | hover 背景 #FAFAFA | 移动端替代表格 | 列表项必须有主对象和状态 |
| Modal / Drawer / Sheet | Modal 居中，Drawer 右侧，Sheet 移动端底部 | 遮罩可关闭需按业务确认 | 移动端复杂内容用全屏页 | 批量危险操作必须二次确认 |
| Toast / Alert | Alert 占据内容流，Toast 用轻反馈 | 语义色固定 | 移动端不遮挡底部操作 | 风险和错误不能只用 Toast |
| Empty / Loading / Error | 空状态含原因和下一步 | loading 使用骨架屏/Spin | 移动端保持空间稳定 | 不写营销语；必须提供可执行下一步 |

## 8.页面模板规则

### 8.0 通用页面意图与组件模式

| 页面意图 | 适用场景 | 必备区域 | 推荐组件模式 | Slot合同/预算 | 禁止误用 |
|---|---|---|---|---|---|
| auth | 管理员登录 | brand-panel、login-form | Form、Input、Button、Alert | 登录框 360-400px | 不提供公开注册入口 |
| dashboard | 工作台、运营概览 | page-header、metric-grid、todo-table、risk-panel | StatisticCard、Table、Alert | 首屏显示关键任务 | 不使用营销 hero |
| list-management | 客户、订单、商品、消息模板列表 | page-header、filter-bar、data-table、pagination | InlineForm、Table、Tag | 筛选最多两行 | 不重复全局导航 |
| detail | 客户详情、订单详情、案件详情 | summary-header、tabs、description-list、log | Descriptions、Tabs、Timeline | 摘要固定，详情滚动 | 不平铺所有字段 |
| form-create-edit | 服务模板、商品、角色、协议配置 | sectioned-form、sticky-footer | Form、Upload、Select | 720-1040px 表单宽 | 不出现多个等价主按钮 |
| checkout-payment | 后台代创线下订单 | order-summary、customer-select、confirm | Steps、Form、Result | 只用于后台代创单 | 不展示前台营销结算样式 |
| wizard-flow | 流程模板创建、服务配置向导 | steps、active-form、review-sidebar | Steps、Form、Card | 当前步骤优先 | 不跳过校验 |
| profile-settings | 账号、权限、品牌配置 | tabs、sectioned-form、danger-zone | Tabs、Form、Alert | 危险区独立 | 不把危险操作放在普通保存旁 |
| message-center | 消息通知、模板配置 | tabs、list/table、detail-drawer | Badge、Table/List、Drawer | 分类清晰 | 不混合不同渠道 |
| document-viewer | 协议、回执、证书预览 | toolbar、preview-pane、metadata-panel | Preview、Descriptions | 预览区优先 | 不嵌入表格单元格 |
| custom | 特殊业务页 | page-header、primary-content | 按业务选择 | 必须声明 slot 合同 | 不生成自由散乱布局 |

### 8.1 移动端页面

- 首页：显示待办、风险、最近案件和消息摘要，底部导航不超过 5 项。
- 列表页：筛选进入抽屉，列表项包含对象名、状态、时间和主操作。
- 详情页：摘要置顶，内容分 Tabs，关键操作固定底部。
- 表单页：单列、分段、底部固定保存；文件上传使用独立区域。
- 设置页：按账号、安全、通知、品牌权限分组。

### 8.2 PC Web页面

- 仪表盘：顶部指标卡 + 待办表格 + 风险预警 + 最近消息。
- 列表管理页：页标题/主操作、筛选区、表格、分页、批量操作。
- 详情页：摘要头、状态标签、Tabs、时间线、操作日志、右侧抽屉。
- 表单/配置页：分段表单、预览区、影响范围提示、sticky footer。
- 数据分析页：图表只作为数据解释，不能挤占待办和风险处理入口。

### 8.3 PC桌面应用页面

- 多栏工作台：左对象列表、中详情、右日志/属性，保留主操作唯一。
- 主从详情：列表选中项驱动详情，详情不重复完整列表筛选。
- 工具栏：图标按钮带 tooltip，批量操作进入 dropdown。
- 面板与抽屉：抽屉用于查看/处理，Modal 用于确认，长流程用页面。

## 9.交互与状态

| 状态 | 视觉表现 | 交互规则 | 文案规则 |
|---|---|---|---|
| Hover | 行/按钮轻微变色 | 仅桌面端依赖 hover | 不改变文案 |
| Focus | 主色外环 | 键盘可达，焦点顺序符合视觉顺序 | 不隐藏说明 |
| Active | 主色加深或按下态 | 按钮反馈不超过 150ms | 文案保持动作词 |
| Disabled | 灰底灰字 | 禁止点击，必要时 Tooltip 说明原因 | 说明缺少权限或前置条件 |
| Loading | Spin、骨架屏、按钮 loading | 保持布局尺寸不跳动 | 显示“保存中”“加载中” |
| Success | 绿色 Tag/Alert | 完成后更新列表或状态 | 用“已保存”“审核通过” |
| Warning | 黄色 Tag/Alert | 临期、待处理需保留操作入口 | 用“即将超时”“待补充资料” |
| Error | 红色 Tag/Alert/Form error | 错误字段定位，危险操作二次确认 | 说明失败原因和下一步 |
| Empty | 空状态图形简化 + 操作 | 提供创建、导入或清空筛选 | 不写泛泛欢迎语 |

## 10.AI页面生成约束

### 10.1 必须遵守

- 默认使用 AdminShell：深色侧栏、白色顶栏、灰色正文背景、白色业务容器。
- 区分 Global Shell、Page Body、Overlay/State、Mock Data，避免把全局导航、全局品牌、全局Footer或当前页导航入口重复生成到页面正文。
- 每个默认可见交互元素必须具有明确 action intent；同一 page body 不得出现两个相同 intent 的 primary action。
- 页面生成必须输出布局预算、slot合同、重复语义检查和渲染后QA要求，不能只输出视觉 token。
- 后台管理页面必须优先使用表格、筛选、表单、抽屉、状态标签、时间线和操作日志。
- 订单接单、自动退款、风险提报、审核驳回等关键操作必须有状态反馈和确认机制。

### 10.2 禁止事项

- 禁止生成营销落地页式 hero、装饰性大插画、纯渐变背景或过度留白。
- 禁止卡片套卡片；禁止后台卡片圆角超过 8px。
- 禁止表格行操作堆满文字链接；超过 3 个进入更多菜单。
- 禁止自由定义状态颜色；禁止只靠颜色表达风险，必须有文字标签。
- 禁止在正文重复侧栏菜单、Logo、顶栏用户区。

### 10.3 组件选择规则

- 导航：PC 使用 Sider + Header + Breadcrumb；移动端使用底部导航和页面 Tabs。
- 表格与列表：数据管理首选 Table；移动端和轻量通知使用 List/Card。
- 表单：配置类表单分段；长表单使用 sticky footer；动态字段按模板分组。
- 卡片：指标、摘要、分组使用 Card；不要把页面 section 整体做成浮动卡片。
- 弹窗/抽屉/Sheet：确认用 Modal，详情预览和审核处理用 Drawer，移动端用 Sheet/全屏页。
- 状态反馈：行内校验用 Form error，流程风险用 Alert，轻量完成用 Toast。

### 10.4 页面生成检查清单

- [ ] 页面是否使用本规范的颜色 token。
- [ ] 页面是否使用本规范的字体、字号、间距、圆角和阴影。
- [ ] 页面是否适配移动端、平板、PC Web和PC桌面应用。
- [ ] 组件状态是否覆盖默认、悬停、聚焦、禁用、加载和错误。
- [ ] 文案、信息密度和布局是否符合设计风格定位。
- [ ] 是否避免了本规范列出的禁止事项。

### 10.5 Figma还原与渲染约束

- 组件还原：Button/Input/Table/Card/Navigation 必须根据 component-blueprints.json 拆成可编辑 Figma 节点；不要截图化。
- 布局还原：先创建 Shell 与 slot，再挂载内容；所有 fill 子节点必须有父容器尺寸。
- 布局预算：PC Web 可用高度需扣除 56px 顶栏和 48px 上下 padding；表格主体可滚动。
- Slot合同：同 pageIntentKey 共享 requiredRegions、fixedSlots 和 primaryActionIntent。
- 文本溢出：表格单元格单行省略；详情说明最多两行；长内容进入抽屉。
- 重复语义：重复主操作失败；行内重复操作允许；全局导航和正文导航重复失败。
- 语义关键区：列表页必须有筛选、表格、分页；详情页必须有摘要、详情、日志/时间线。
- 对比追加：同名页面在画布右侧追加 120px，不覆盖既有设计。
- QA失败处理：出现重叠、裁切、关键区缺失、主操作重复时失败并要求人工复核。

## 11.HTML预览说明

| 项目 | 内容 |
|---|---|
| 文件路径 | `design/antd-admin-design-spec/index.html` |
| 展示内容 | 配色、字体、间距、圆角、阴影、组件、移动端样张、PC Web样张、PC桌面应用样张 |
| 验收方式 | 直接在浏览器打开，检查视觉风格、token一致性和响应式布局 |

### 11.1 机器可读产物

| 文件 | 用途 | 后续Skill使用方式 |
|---|---|---|
| `tokens.json` | 通用设计token | 页面生成、前端生成、HTML token同步 |
| `figma-tokens.json` | Figma变量、文本样式、效果样式、布局默认值 | 生成可导入Figma的JSON节点文档 |
| `component-rules.json` | 组件尺寸、状态、样式、响应式fallback | 将页面元素映射为Figma节点和组件结构 |
| `component-blueprints.json` | 可编辑Figma组件节点蓝图、插槽、文字溢出与布局安全规则 | 供pm-06声明组件实例，供pm-08按蓝图创建Figma节点 |
| `figma-render-rules.json` | 导入器布局、组件适配器、追加策略、溢出处理、截图/结构QA规则 | 供pm-08执行确定性的Figma还原和质量检查 |
| `ai-generation-rules.json` | 页面生成硬约束与禁用规则 | 结合pm-03页面描述生成稳定页面结构 |
| `consistency-rules.json` | 跨页面profile、fingerprint、布局预算、语义唯一性和drift规则 | 供pm-06/07/08/09做一致性校验和失败传播 |
| `layout-profile-schema.json` | layout/intent/stateGroup profile、layoutBudget、slotContract schema | 供pm-06创建可复用profile |
| `page-intent-patterns.json` | 通用页面意图、必备/禁用区域、slot合同、预算和响应式规则 | 供pm-06识别页面意图和生成renderSpec |

## 12.待确认与假设

| 类型 | 内容 | 对设计的影响 | 后续确认方式 |
|---|---|---|---|
| 假设 | 管理后台以 PC Web 为主要使用形态，移动端仅做待办、通知、轻审核适配 | 规范以侧栏+顶栏+表格为核心 | 如需要完整移动后台，可扩展移动端页面模板 |
| 假设 | 后台采用 Ant Design v5 视觉气质，但不强制依赖 antd 运行时 | 下游 Figma/HTML 可用原生节点复刻 | 前端实现时可映射到 antd token |
| 假设 | 产品为知识产权与合规服务平台，后台含订单、案件、服务模板、消息和权限 | 页面意图模式偏向流程、审核、配置 | 后续页面生成可按 sitemap 继续细化 |

