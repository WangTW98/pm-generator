# Page Layout Batch Task List

## 0.任务状态

<table>
  <tr><td>任务ID</td><td>20260519-000000-all-page-layout-batch</td></tr>
  <tr><td>任务类型</td><td>pm-04-page-layout-batch</td></tr>
  <tr><td>创建日期</td><td>2026-05-19</td></tr>
  <tr><td>更新时间</td><td>2026-05-19</td></tr>
  <tr><td>执行状态</td><td>Completed</td></tr>
  <tr><td>生成模式</td><td>Development</td></tr>
  <tr><td>Sitemap来源模式</td><td>all product/layout/*-sitemap.md</td></tr>
  <tr><td>当前恢复位置</td><td>60/60 eligible done; 8 nodes skipped</td></tr>
</table>

## 1.源Sitemap清单

| Sitemap ID | Sitemap路径 | 产品端与形态 | 处理状态 | Eligible数量 | Skipped节点数量 | Done数量 | Failed数量 | 备注 |
|---|---|---|---|---:|---:|---:|---:|---|
| SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | 客户端 / PC Web | Completed | 30 | 4 | 30 | 0 | 结构校验 = PageListTreeUsed |
| SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | 管理后台 / PC Web | Completed | 30 | 4 | 30 | 0 | 结构校验 = PageListTreeUsed |

## 2.页面生成任务清单

| Task ID | Sitemap ID | Sitemap路径 | 页面ID | 父级ID | 层级 | 页面/模块 | 页面类型 | 状态组 | 输出路径 | 通用模板污染检查 | 状态 | 开始时间 | 完成时间 | 失败原因/恢复说明 |
|---|---|---|---|---|---:|---|---|---|---|---|---|---|---|---|
| TASK-001 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-001 |  | 1 | 登录注册 | 节点 |  |  | N/A | Skipped |  | 2026-05-19 | 页面类型为节点，仅作为目录结构，不生成 layout.md |
| TASK-002 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-002 | PAGE-001 | 2 | 手机号登录 | 状态 | STATE-001 | product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-002-手机号登录/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-003 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-034 | PAGE-001 | 2 | 邮箱密码登录 | 状态 | STATE-001 | product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-034-邮箱密码登录/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-004 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-003 | PAGE-001 | 2 | 微信扫扫登录 | 状态 | STATE-001 | product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-003-微信扫扫登录/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-005 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-007 |  | 1 | 工作台 | 节点 |  |  | N/A | Skipped |  | 2026-05-19 | 页面类型为节点，仅作为目录结构，不生成 layout.md |
| TASK-006 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-008 | PAGE-007 | 2 | 待办事项 | 页面 |  | product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-008-待办事项/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-007 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-009 | PAGE-007 | 2 | 案件进度 | 页面 |  | product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-009-案件进度/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-008 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-004 |  | 1 | 服务商城 | 页面 |  | product/pages/客户端-PC-Web/PAGE-004-服务商城/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-009 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-005 | PAGE-004 | 2 | 服务详情页 | 页面 |  | product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-010 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-006 | PAGE-005 | 3 | 订单结算 | 页面 |  | product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-011 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-055 | PAGE-006 | 4 | 支付状态 | 节点 |  |  | N/A | Skipped |  | 2026-05-19 | 页面类型为节点，仅作为目录结构，不生成 layout.md |
| TASK-012 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-036 | PAGE-055 | 5 | 支付状态-进行中 | 状态 | STATE-003 | product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/PAGE-055-支付状态/PAGE-036-支付状态-进行中/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-013 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-037 | PAGE-055 | 5 | 支付状态-成功 | 状态 | STATE-003 | product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/PAGE-055-支付状态/PAGE-037-支付状态-成功/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-014 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-038 | PAGE-055 | 5 | 支付状态-失败 | 状态 | STATE-003 | product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/PAGE-055-支付状态/PAGE-038-支付状态-失败/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-015 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-014 |  | 1 | 个人中心 | 节点 |  |  | N/A | Skipped |  | 2026-05-19 | 页面类型为节点，仅作为目录结构，不生成 layout.md |
| TASK-016 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-015 | PAGE-014 | 2 | 消息通知 | 页面 |  | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-015-消息通知/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-017 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-039 | PAGE-014 | 2 | 我的服务 | 页面 |  | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-018 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-040 | PAGE-039 | 3 | 服务材料查看 | 页面 |  | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-040-服务材料查看/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-019 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-056 | PAGE-039 | 3 | 服务详情 | 页面 |  | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-020 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-041 | PAGE-056 | 4 | 服务详情-Step01-客户资料上传与表单填写 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-041-服务详情-Step01-客户资料上传与表单填写/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-021 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-042 | PAGE-056 | 4 | 服务详情-Step02-资料审核 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-042-服务详情-Step02-资料审核/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-022 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-043 | PAGE-042 | 5 | 服务详情-Step02-资料审核-通过 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-042-服务详情-Step02-资料审核/PAGE-043-服务详情-Step02-资料审核-通过/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-023 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-044 | PAGE-042 | 5 | 服务详情-Step02-资料审核-驳回 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-042-服务详情-Step02-资料审核/PAGE-044-服务详情-Step02-资料审核-驳回/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-024 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-045 | PAGE-056 | 4 | 服务详情-Step03-协议模版下载 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-045-服务详情-Step03-协议模版下载/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-025 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-046 | PAGE-056 | 4 | 服务详情-Step04-协议模版填写上传与表单填写 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-046-服务详情-Step04-协议模版填写上传与表单填写/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-026 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-047 | PAGE-056 | 4 | 服务详情-Step05-资料审核 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-047-服务详情-Step05-资料审核/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-027 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-048 | PAGE-047 | 5 | 服务详情-Step05-资料审核-通过 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-047-服务详情-Step05-资料审核/PAGE-048-服务详情-Step05-资料审核-通过/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-028 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-049 | PAGE-047 | 5 | 服务详情-Step05-资料审核-驳回 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-047-服务详情-Step05-资料审核/PAGE-049-服务详情-Step05-资料审核-驳回/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-029 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-050 | PAGE-056 | 4 | 服务详情-Step06-案件交付 | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-050-服务详情-Step06-案件交付/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-030 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-054 | PAGE-056 | 4 | 服务详情-step0-idle | 状态 | STATE-004 | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-054-服务详情-step0-idle/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-031 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-051 | PAGE-014 | 2 | 我的订单 | 页面 |  | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-051-我的订单/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-032 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-052 | PAGE-051 | 3 | 订单详情 | 页面 |  | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-051-我的订单/PAGE-052-订单详情/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-033 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-012 | PAGE-051 | 3 | 退款申请 | 页面 |  | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-051-我的订单/PAGE-012-退款申请/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-034 | SITEMAP-001 | product/layout/客户端-PC-Web-sitemap.md | PAGE-053 | PAGE-014 | 2 | 账户设置 | 页面 |  | product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-053-账户设置/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-035 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-016 |  | 1 | 登录页 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-016-登录页/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-036 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-017 |  | 1 | 工作台 | 节点 |  |  | N/A | Skipped |  | 2026-05-19 | 页面类型为节点，仅作为目录结构，不生成 layout.md |
| TASK-037 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-018 | PAGE-017 | 2 | 待办事项 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-017-工作台/PAGE-018-待办事项/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-038 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-019 | PAGE-017 | 2 | 消息通知 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-017-工作台/PAGE-019-消息通知/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-039 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-020 |  | 1 | 客户管理 | 节点 |  |  | N/A | Skipped |  | 2026-05-19 | 页面类型为节点，仅作为目录结构，不生成 layout.md |
| TASK-040 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-021 | PAGE-020 | 2 | 客户列表 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-020-客户管理/PAGE-021-客户列表/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-041 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-022 | PAGE-020 | 2 | 客户详情 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-020-客户管理/PAGE-022-客户详情/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-042 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-023 |  | 1 | 服务发布管理 | 节点 |  |  | N/A | Skipped |  | 2026-05-19 | 页面类型为节点，仅作为目录结构，不生成 layout.md |
| TASK-043 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-024 | PAGE-023 | 2 | 服务分类管理 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-023-服务发布管理/PAGE-024-服务分类管理/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-044 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-025 | PAGE-023 | 2 | 服务模版管理 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-023-服务发布管理/PAGE-025-服务模版管理/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-045 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-026 | PAGE-023 | 2 | 商品管理 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-023-服务发布管理/PAGE-026-商品管理/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-046 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-027 |  | 1 | 订单管理 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-027-订单管理/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-047 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-029 | PAGE-027 | 2 | 订单详情 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-027-订单管理/PAGE-029-订单详情/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-048 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-030 | PAGE-027 | 2 | 线下订单转入 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-027-订单管理/PAGE-030-线下订单转入/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-049 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-052 |  | 1 | 服务管理 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-052-服务管理/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-050 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-053 | PAGE-052 | 2 | 服务详情 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-051 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-044 | PAGE-053 | 3 | 服务详情-step0-idle | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-044-服务详情-step0-idle/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-052 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-034 | PAGE-053 | 3 | 服务详情-step01-待客户资料上传与表单填写 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-034-服务详情-step01-待客户资料上传与表单填写/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-053 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-035 | PAGE-053 | 3 | 服务详情-step02-客户资料审核 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-035-服务详情-step02-客户资料审核/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-054 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-036 | PAGE-035 | 4 | 服务详情-step02-客户资料审核-通过 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-035-服务详情-step02-客户资料审核/PAGE-036-服务详情-step02-客户资料审核-通过/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-055 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-037 | PAGE-035 | 4 | 服务详情-step02-客户资料审核-驳回 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-035-服务详情-step02-客户资料审核/PAGE-037-服务详情-step02-客户资料审核-驳回/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-056 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-038 | PAGE-053 | 3 | 服务详情-step03-上传需客户回填的协议模版 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-038-服务详情-step03-上传需客户回填的协议模版/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-057 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-039 | PAGE-053 | 3 | 服务详情-step04-审核客户回填后的协议 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-039-服务详情-step04-审核客户回填后的协议/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-058 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-040 | PAGE-039 | 4 | 服务详情-step04-审核客户回填后的协议-通过 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-039-服务详情-step04-审核客户回填后的协议/PAGE-040-服务详情-step04-审核客户回填后的协议-通过/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-059 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-041 | PAGE-039 | 4 | 服务详情-step04-审核客户回填后的协议-驳回 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-039-服务详情-step04-审核客户回填后的协议/PAGE-041-服务详情-step04-审核客户回填后的协议-驳回/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-060 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-042 | PAGE-053 | 3 | 服务详情-step05-案件交付 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-042-服务详情-step05-案件交付/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-061 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-043 | PAGE-053 | 3 | 服务详情-step06-案件交付完成 | 状态 | STATE-010 | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-043-服务详情-step06-案件交付完成/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-062 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-045 | PAGE-052 | 2 | 风险提报 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-045-风险提报/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-063 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-046 |  | 1 | 系统管理 | 节点 |  |  | N/A | Skipped |  | 2026-05-19 | 页面类型为节点，仅作为目录结构，不生成 layout.md |
| TASK-064 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-047 | PAGE-046 | 2 | 通知模版配置 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-047-通知模版配置/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-065 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-048 | PAGE-046 | 2 | 风险提报模版配置 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-048-风险提报模版配置/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-066 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-049 | PAGE-046 | 2 | 角色权限管理 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-049-角色权限管理/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-067 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-050 | PAGE-046 | 2 | 用户账号管理 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-050-用户账号管理/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |
| TASK-068 | SITEMAP-002 | product/layout/管理后台-PC-Web-sitemap.md | PAGE-051 | PAGE-046 | 2 | 合规协议管理 | 页面 |  | product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-051-合规协议管理/layout.md | Passed | Done | 2026-05-19 | 2026-05-19 | pm-03-page-layout generated; generic template pollution guard passed by intent-specific groups/elements. |

## 3.中断恢复说明

- `Pending`：尚未执行，可继续执行。
- `In Progress`：上次中断时可能正在执行，恢复时应重新检查输出路径；若输出文件完整则改为 `Done`，否则重新执行。
- `Done`：已完成，不重复执行，除非用户明确要求重跑。
- `Skipped`：页面类型为 `节点` 或不在用户筛选范围内，不传给 `pm-03-page-layout`。
- `Failed`：执行失败；恢复时可优先重试，失败原因保留在表格最后一列。

## 4.执行日志

| 时间 | Task ID | 操作 | 结果 | 说明 |
|---|---|---|---|---|
| 2026-05-19 | TASK-001 | Skip Node | Skipped | PAGE-001 登录注册 为节点。 |
| 2026-05-19 | TASK-002 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-002-手机号登录/layout.md。 |
| 2026-05-19 | TASK-003 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-034-邮箱密码登录/layout.md。 |
| 2026-05-19 | TASK-004 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-003-微信扫扫登录/layout.md。 |
| 2026-05-19 | TASK-005 | Skip Node | Skipped | PAGE-007 工作台 为节点。 |
| 2026-05-19 | TASK-006 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-008-待办事项/layout.md。 |
| 2026-05-19 | TASK-007 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-009-案件进度/layout.md。 |
| 2026-05-19 | TASK-008 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-004-服务商城/layout.md。 |
| 2026-05-19 | TASK-009 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/layout.md。 |
| 2026-05-19 | TASK-010 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/layout.md。 |
| 2026-05-19 | TASK-011 | Skip Node | Skipped | PAGE-055 支付状态 为节点。 |
| 2026-05-19 | TASK-012 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/PAGE-055-支付状态/PAGE-036-支付状态-进行中/layout.md。 |
| 2026-05-19 | TASK-013 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/PAGE-055-支付状态/PAGE-037-支付状态-成功/layout.md。 |
| 2026-05-19 | TASK-014 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/PAGE-055-支付状态/PAGE-038-支付状态-失败/layout.md。 |
| 2026-05-19 | TASK-015 | Skip Node | Skipped | PAGE-014 个人中心 为节点。 |
| 2026-05-19 | TASK-016 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-015-消息通知/layout.md。 |
| 2026-05-19 | TASK-017 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/layout.md。 |
| 2026-05-19 | TASK-018 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-040-服务材料查看/layout.md。 |
| 2026-05-19 | TASK-019 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/layout.md。 |
| 2026-05-19 | TASK-020 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-041-服务详情-Step01-客户资料上传与表单填写/layout.md。 |
| 2026-05-19 | TASK-021 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-042-服务详情-Step02-资料审核/layout.md。 |
| 2026-05-19 | TASK-022 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-042-服务详情-Step02-资料审核/PAGE-043-服务详情-Step02-资料审核-通过/layout.md。 |
| 2026-05-19 | TASK-023 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-042-服务详情-Step02-资料审核/PAGE-044-服务详情-Step02-资料审核-驳回/layout.md。 |
| 2026-05-19 | TASK-024 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-045-服务详情-Step03-协议模版下载/layout.md。 |
| 2026-05-19 | TASK-025 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-046-服务详情-Step04-协议模版填写上传与表单填写/layout.md。 |
| 2026-05-19 | TASK-026 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-047-服务详情-Step05-资料审核/layout.md。 |
| 2026-05-19 | TASK-027 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-047-服务详情-Step05-资料审核/PAGE-048-服务详情-Step05-资料审核-通过/layout.md。 |
| 2026-05-19 | TASK-028 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-047-服务详情-Step05-资料审核/PAGE-049-服务详情-Step05-资料审核-驳回/layout.md。 |
| 2026-05-19 | TASK-029 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-050-服务详情-Step06-案件交付/layout.md。 |
| 2026-05-19 | TASK-030 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-039-我的服务/PAGE-056-服务详情/PAGE-054-服务详情-step0-idle/layout.md。 |
| 2026-05-19 | TASK-031 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-051-我的订单/layout.md。 |
| 2026-05-19 | TASK-032 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-051-我的订单/PAGE-052-订单详情/layout.md。 |
| 2026-05-19 | TASK-033 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-051-我的订单/PAGE-012-退款申请/layout.md。 |
| 2026-05-19 | TASK-034 | Generate Layout | Done | 已生成 product/pages/客户端-PC-Web/PAGE-014-个人中心/PAGE-053-账户设置/layout.md。 |
| 2026-05-19 | TASK-035 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-016-登录页/layout.md。 |
| 2026-05-19 | TASK-036 | Skip Node | Skipped | PAGE-017 工作台 为节点。 |
| 2026-05-19 | TASK-037 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-017-工作台/PAGE-018-待办事项/layout.md。 |
| 2026-05-19 | TASK-038 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-017-工作台/PAGE-019-消息通知/layout.md。 |
| 2026-05-19 | TASK-039 | Skip Node | Skipped | PAGE-020 客户管理 为节点。 |
| 2026-05-19 | TASK-040 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-020-客户管理/PAGE-021-客户列表/layout.md。 |
| 2026-05-19 | TASK-041 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-020-客户管理/PAGE-022-客户详情/layout.md。 |
| 2026-05-19 | TASK-042 | Skip Node | Skipped | PAGE-023 服务发布管理 为节点。 |
| 2026-05-19 | TASK-043 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-023-服务发布管理/PAGE-024-服务分类管理/layout.md。 |
| 2026-05-19 | TASK-044 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-023-服务发布管理/PAGE-025-服务模版管理/layout.md。 |
| 2026-05-19 | TASK-045 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-023-服务发布管理/PAGE-026-商品管理/layout.md。 |
| 2026-05-19 | TASK-046 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-027-订单管理/layout.md。 |
| 2026-05-19 | TASK-047 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-027-订单管理/PAGE-029-订单详情/layout.md。 |
| 2026-05-19 | TASK-048 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-027-订单管理/PAGE-030-线下订单转入/layout.md。 |
| 2026-05-19 | TASK-049 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/layout.md。 |
| 2026-05-19 | TASK-050 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/layout.md。 |
| 2026-05-19 | TASK-051 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-044-服务详情-step0-idle/layout.md。 |
| 2026-05-19 | TASK-052 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-034-服务详情-step01-待客户资料上传与表单填写/layout.md。 |
| 2026-05-19 | TASK-053 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-035-服务详情-step02-客户资料审核/layout.md。 |
| 2026-05-19 | TASK-054 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-035-服务详情-step02-客户资料审核/PAGE-036-服务详情-step02-客户资料审核-通过/layout.md。 |
| 2026-05-19 | TASK-055 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-035-服务详情-step02-客户资料审核/PAGE-037-服务详情-step02-客户资料审核-驳回/layout.md。 |
| 2026-05-19 | TASK-056 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-038-服务详情-step03-上传需客户回填的协议模版/layout.md。 |
| 2026-05-19 | TASK-057 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-039-服务详情-step04-审核客户回填后的协议/layout.md。 |
| 2026-05-19 | TASK-058 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-039-服务详情-step04-审核客户回填后的协议/PAGE-040-服务详情-step04-审核客户回填后的协议-通过/layout.md。 |
| 2026-05-19 | TASK-059 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-039-服务详情-step04-审核客户回填后的协议/PAGE-041-服务详情-step04-审核客户回填后的协议-驳回/layout.md。 |
| 2026-05-19 | TASK-060 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-042-服务详情-step05-案件交付/layout.md。 |
| 2026-05-19 | TASK-061 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-053-服务详情/PAGE-043-服务详情-step06-案件交付完成/layout.md。 |
| 2026-05-19 | TASK-062 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-052-服务管理/PAGE-045-风险提报/layout.md。 |
| 2026-05-19 | TASK-063 | Skip Node | Skipped | PAGE-046 系统管理 为节点。 |
| 2026-05-19 | TASK-064 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-047-通知模版配置/layout.md。 |
| 2026-05-19 | TASK-065 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-048-风险提报模版配置/layout.md。 |
| 2026-05-19 | TASK-066 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-049-角色权限管理/layout.md。 |
| 2026-05-19 | TASK-067 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-050-用户账号管理/layout.md。 |
| 2026-05-19 | TASK-068 | Generate Layout | Done | 已生成 product/pages/管理后台-PC-Web/PAGE-046-系统管理/PAGE-051-合规协议管理/layout.md。 |
