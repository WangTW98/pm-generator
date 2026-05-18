# PAGE-006 订单结算 Figma Node Notes

## 1.来源

- layout.md：`product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/PAGE-006-订单结算/layout.md`
- 来源Sitemap：`product/layout/客户端-PC-Web-sitemap.md`
- 设计规范：`design/ant-design-admin/`

## 2.页面意图

- pageIntent：`checkout-payment`
- 默认状态：确认服务规格、联系人信息、发票信息和微信支付方式。
- 非默认状态：仅记录到 excludedStates，不生成 Figma 主节点。

## 3.RenderSpec

- 根画板：1440 x 900。
- 布局策略：profile-locked + two-pass auto layout。

## 4.一致性 Profile

| 项目 | 值 |
|---|---|
| layoutKey | 客户端-PC-Web |
| layoutVariantKey | workspace |
| pageIntentKey | checkout-payment |
| stateGroupKey |  |
| layoutProfile | `product/style-profiles/客户端-PC-Web/layout-workspace-profile.json` |
| intentProfile | `product/style-profiles/客户端-PC-Web/intent-checkout-payment-profile.json` |
| stateGroupProfile | `` |
| driftCheck | pass |
