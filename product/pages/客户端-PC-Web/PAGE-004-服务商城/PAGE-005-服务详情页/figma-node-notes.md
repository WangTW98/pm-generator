# PAGE-005 服务详情页 Figma Node Notes

## 1.来源

- layout.md：`product/pages/客户端-PC-Web/PAGE-004-服务商城/PAGE-005-服务详情页/layout.md`
- 来源Sitemap：`product/layout/客户端-PC-Web-sitemap.md`
- 设计规范：`design/ant-design-admin/`

## 2.页面意图

- pageIntent：`detail`
- 默认状态：查看服务说明、办理流程、SKU规格和下单信息。
- 非默认状态：仅记录到 excludedStates，不生成 Figma 主节点。

## 3.RenderSpec

- 根画板：1440 x 900。
- 布局策略：profile-locked + two-pass auto layout。

## 4.一致性 Profile

| 项目 | 值 |
|---|---|
| layoutKey | 客户端-PC-Web |
| layoutVariantKey | workspace |
| pageIntentKey | detail |
| stateGroupKey |  |
| layoutProfile | `product/style-profiles/客户端-PC-Web/layout-workspace-profile.json` |
| intentProfile | `product/style-profiles/客户端-PC-Web/intent-detail-profile.json` |
| stateGroupProfile | `` |
| driftCheck | pass |
