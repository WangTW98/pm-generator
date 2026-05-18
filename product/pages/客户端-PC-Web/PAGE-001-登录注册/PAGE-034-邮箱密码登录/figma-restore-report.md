# Figma 还原报告

## 基本信息

- 源 JSON：`product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-034-邮箱密码登录/figma-nodes.json`
- 目标 Figma：Agent Test / Case Tracking PRD6
- fileKey：`ZP4TjUsWJyHncnukJuVfRD`
- nodeId：`208:57`
- 写入模式：append
- consistencyMode：profile-locked
- 渲染模式：recursive / component-adapter / append
- 根画板：PM08 ProfileLocked/PAGE-034 - 邮箱密码登录 - 客户端-PC-Web
- 根节点 ID：208:57
- pageIntent：auth
- layoutKey：客户端-PC-Web
- pageIntentKey：auth
- stateGroupKey：STATE-001
- designSpecKey：ant-design-admin
- styleFingerprint：layout-client-pc-web-v1 / intent-auth-ant-design-admin-v1 / state-login-register-v1
- semanticValidation：pass
- renderSpec：pass
- consistency：pass
- 执行时间：2026-05-18

## 还原结果

- 状态：Done
- 创建根画板：1
- 根画板尺寸：1440 x 900
- 直接子节点：Shell/Topbar、Section/AuthShell、Shell/Footer
- 组件适配：Tabs、Input、Button
- profile 命中：layoutProfile、intentProfile、stateGroupProfile
- fingerprint 匹配：pass
- driftCheck：pass
- 截图 QA：pass
- 截图 URL：`https://www.figma.com/api/mcp/asset/c696aacd-04d8-438b-8f76-2a07c6809e5d`

## 一致性 Profile 校验

| 项目 | 结果 | 说明 |
| --- | --- | --- |
| layoutProfile | pass | `product/style-profiles/客户端-PC-Web/layout-profile.json` |
| intentProfile | pass | `product/style-profiles/客户端-PC-Web/intent-auth-profile.json` |
| stateGroupProfile | pass | `product/style-profiles/客户端-PC-Web/state-STATE-001-profile.json` |
| metadata 写入 | pass | 根画板写入 layoutKey/pageIntentKey/stateGroupKey/consistencyMode/styleFingerprint |

## 人工检查项

- 与手机号、微信页对比确认卡片宽度、切换区、主操作位置一致。
