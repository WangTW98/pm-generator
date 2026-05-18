# PAGE-036 支付状态-进行中 Figma Node Notes

## 1.来源

- layout.md：`product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/PAGE-055-支付状态/PAGE-036-支付状态-进行中/layout.md`
- 来源Sitemap：`product/layout/客户端-PC-Web-sitemap.md`
- 设计规范：`design/ant-design-admin/`

## 2.页面意图

- pageIntent：`payment-status`
- 默认状态：订单正在等待微信支付结果，请勿关闭页面。
- 非默认状态：仅记录到 excludedStates，不生成 Figma 主节点。

## 3.RenderSpec

- 根画板：1440 x 900。
- 布局策略：profile-locked + two-pass auto layout。

## 4.一致性 Profile

| 项目 | 值 |
|---|---|
| layoutKey | 客户端-PC-Web |
| layoutVariantKey | workspace |
| pageIntentKey | payment-status |
| stateGroupKey | STATE-003 |
| layoutProfile | `product/style-profiles/客户端-PC-Web/layout-workspace-profile.json` |
| intentProfile | `product/style-profiles/客户端-PC-Web/intent-payment-status-profile.json` |
| stateGroupProfile | `product/style-profiles/客户端-PC-Web/state-STATE-003-profile.json` |
| driftCheck | pass |
