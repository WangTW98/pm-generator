# MD2Figma 批量任务

## 批次信息

- 批次 ID：20260519-000000-md2figma-client-auth-workbench
- 创建时间：2026-05-19 00:00:00
- 目标 Figma：https://www.figma.com/design/ZP4TjUsWJyHncnukJuVfRD/Agent-Test?node-id=215-55&t=A760mTWqS5Qcfqy5-1
- fileKey：ZP4TjUsWJyHncnukJuVfRD
- nodeId：215:55
- 扫描范围：product/pages/客户端-PC-Web/PAGE-001-登录注册；product/pages/客户端-PC-Web/PAGE-007-工作台
- 写入模式：append
- consistencyMode：profile-locked
- Dry Run：No

## 汇总

- 总任务数：5
- Pending：0
- In Progress：0
- Done：5
- Skipped：0
- Failed：0
- Consistency Missing：0
- Drift Warning：0
- Drift Failed：0
- Layout Budget Failed：0
- Duplicate Audit Failed：0
- Post-render QA Failed：0

## 一致性分组

| Group ID | designSpecKey | layoutKey | pageIntentKey | stateGroupKey | 任务数 | styleFingerprint | Drift |
| --- | --- | --- | --- | --- | ---: | --- | --- |
| GROUP-001 | antd-admin-design-spec | 客户端-PC-Web | auth | STATE-001 | 3 | b2968cd4bc2c/cacd70c11191/fc0e36bec1f5 | pass |
| GROUP-002 | antd-admin-design-spec | 客户端-PC-Web | dashboard |  | 1 | b2968cd4bc2c/b63cfa7144ff/- | pass |
| GROUP-003 | antd-admin-design-spec | 客户端-PC-Web | list-management |  | 1 | b2968cd4bc2c/fb8680bf9b7b/- | pass |

## 任务列表

| 序号 | 状态 | 页面编号 | 页面名称 | 设备形态 | layoutKey | pageIntentKey | stateGroupKey | pageIntent | semanticValidation | layoutBudget | duplicateAudit | renderSpec | consistency | styleFingerprint | driftCheck | postRenderQA | JSON 路径 | 报告路径 | 备注 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Done | PAGE-002 | 手机号登录 | PC Web | 客户端-PC-Web | auth | STATE-001 | auth | pass | pass | pass | pass | pass | b2968cd4bc2c/cacd70c11191/fc0e36bec1f5 | pass | pass | product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-002-手机号登录/figma-nodes.json | product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-002-手机号登录/figma-restore-report.md |  |
| 2 | Done | PAGE-003 | 微信扫扫登录 | PC Web | 客户端-PC-Web | auth | STATE-001 | auth | pass | pass | pass | pass | pass | b2968cd4bc2c/cacd70c11191/fc0e36bec1f5 | pass | pass | product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-003-微信扫扫登录/figma-nodes.json | product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-003-微信扫扫登录/figma-restore-report.md | semanticValidation warning: QR credential slot fallback |
| 3 | Done | PAGE-034 | 邮箱密码登录 | PC Web | 客户端-PC-Web | auth | STATE-001 | auth | pass | pass | pass | pass | pass | b2968cd4bc2c/cacd70c11191/fc0e36bec1f5 | pass | pass | product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-034-邮箱密码登录/figma-nodes.json | product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-034-邮箱密码登录/figma-restore-report.md |  |
| 4 | Done | PAGE-008 | 待办事项 | PC Web | 客户端-PC-Web | dashboard |  | dashboard | pass | pass | pass | pass | pass | b2968cd4bc2c/b63cfa7144ff/- | pass | pass | product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-008-待办事项/figma-nodes.json | product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-008-待办事项/figma-restore-report.md |  |
| 5 | Done | PAGE-009 | 案件进度 | PC Web | 客户端-PC-Web | list-management |  | list-management | pass | pass | pass | pass | pass | b2968cd4bc2c/fb8680bf9b7b/- | pass | pass | product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-009-案件进度/figma-nodes.json | product/pages/客户端-PC-Web/PAGE-007-工作台/PAGE-009-案件进度/figma-restore-report.md |  |

## 失败记录

| JSON 路径 | 失败阶段 | 原因 | 建议处理 |
| --- | --- | --- | --- |

## 跳过记录

| JSON 路径 | 跳过原因 |
| --- | --- |

## 人工检查项

- 待 Figma 写入完成后补充截图/结构 QA 结果。

## Post-render QA 汇总

| 页面编号 | Root Bounds | Shell Collision | Text Overflow | Incoherent Overlap | Duplicate Action | Key Regions | 结果 |
| --- | --- | --- | --- | --- | --- | --- | --- |

## Figma 写入结果

| 页面编号 | 页面名称 | Figma 根节点ID | Post-render QA |
|---|---|---|---|
| PAGE-002 | 手机号登录 | 235:2 | pass |
| PAGE-003 | 微信扫扫登录 | 235:38 | pass |
| PAGE-034 | 邮箱密码登录 | 235:74 | pass |
| PAGE-008 | 待办事项 | 235:110 | pass |
| PAGE-009 | 案件进度 | 235:179 | pass |
