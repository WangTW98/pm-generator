# Page Style Batch Task

## 0.任务状态

| 项目 | 内容 |
|---|---|
| 任务ID | {{BATCH-ID}} |
| 生成模式 | {{Development / DryRun / DirectoryOnly}} |
| 执行状态 | Running |
| 扫描范围 | {{scope}} |
| 设计规范模式 | {{user-specified / auto-match}} |
| 指定设计规范 | {{design-dir-or-empty}} |
| RenderSpec要求 | Required |
| Consistency Profile要求 | Required |
| 创建时间 | {{YYYY-MM-DD HH:mm:ss}} |
| 更新时间 | {{YYYY-MM-DD HH:mm:ss}} |
| 当前恢复位置 |  |

## 1.设计规范清单

| 设计规范目录 | 是否有效 | 说明 |
|---|---|---|
| {{design-dir}} | {{Yes/No}} | {{说明}} |

## 1.1 一致性Profile清单

| Profile ID | 设计规范 | layoutKey | pageIntentKey | stateGroupKey | Profile路径 | 状态 | Fingerprint | Drift |
|---|---|---|---|---|---|---|---|---|
| PROFILE-001 | {{design-dir}} | {{layoutKey}} | {{pageIntentKey}} | {{stateGroupKey-or-null}} | `product/style-profiles/{{layoutKey}}/` | pending / created / reused | {{hash}} | pass / warning / failed |

## 2.页面样式生成任务清单

| Task ID | 状态 | layout.md | 页面清单ID | 使用Layout | 来源Sitemap | 设计规范 | layoutKey | pageIntentKey | stateGroupKey | Profile状态 | Fingerprint | semanticValidation | renderSpec | consistency | figma-nodes.json | figma-node-notes.md | 开始时间 | 完成时间 | 失败原因/恢复说明 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| PSTYLE-001 | Pending | `{{layout.md}}` | {{PAGE-ID}} | {{使用Layout}} | `{{来源Sitemap}}` | {{design-dir}} | {{layoutKey}} | {{pageIntentKey}} | {{stateGroupKey-or-null}} | pending / created / reused | {{hash}} | {{pass/warning/failed}} | {{pass/warning/failed}} | {{pass/warning/failed}} | `{{figma-nodes.json}}` | `{{figma-node-notes.md}}` |  |  |  |

## 3.统计

| 指标 | 数量 |
|---|---:|
| Total | 0 |
| Pending | 0 |
| In Progress | 0 |
| Done | 0 |
| Planned | 0 |
| Directory Checked | 0 |
| Skipped | 0 |
| Failed | 0 |
| Consistency Drift Warning | 0 |
| Consistency Drift Failed | 0 |

## 4.执行日志

| 时间 | Task ID | 动作 | 结果 | 说明 |
|---|---|---|---|---|
| {{YYYY-MM-DD HH:mm:ss}} |  | Batch Created | Running |  |
