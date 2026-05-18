# PAGE-002 手机号登录 Figma Node Notes

## 1.来源

- layout.md：`product/pages/客户端-PC-Web/PAGE-001-登录注册/PAGE-002-手机号登录/layout.md`
- 来源Sitemap：`product/layout/客户端-PC-Web-sitemap.md`
- 设计规范：`design/ant-design-admin/`

## 2.页面意图

- pageIntent：`auth`
- 默认状态：手机号 + 短信验证码登录/注册。
- 非默认状态：校验错误、验证码发送中，仅记录不生成。

## 3.RenderSpec

- 根画板：1440 x 900。
- 组件适配：Tabs、Input、Button。
- 布局策略：profile-locked + two-pass auto layout。

## 4.一致性 Profile

| 项目 | 值 |
|---|---|
| layoutKey | 客户端-PC-Web |
| pageIntentKey | auth |
| stateGroupKey | STATE-001 |
| layoutProfile | `product/style-profiles/客户端-PC-Web/layout-profile.json` |
| intentProfile | `product/style-profiles/客户端-PC-Web/intent-auth-profile.json` |
| stateGroupProfile | `product/style-profiles/客户端-PC-Web/state-STATE-001-profile.json` |
| driftCheck | pass |

## 5.人工检查项

- 三个登录状态页应保持卡片宽度、切换区位置、主操作位置一致。
