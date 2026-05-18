# PAGE-004 服务商城 Figma Node Notes

## 1.来源

- layout.md：`product/pages/客户端-PC-Web/PAGE-004-服务商城/layout.md`
- 来源Sitemap：`product/layout/客户端-PC-Web-sitemap.md`
- 设计规范：`design/ant-design-admin/`

## 2.页面意图

- pageIntent：`commerce-list`
- 默认状态：浏览可购买的知识产权与合规服务，按分类和国家筛选。
- 非默认状态：仅记录到 excludedStates，不生成 Figma 主节点。

## 3.RenderSpec

- 根画板：1440 x 900。
- 布局策略：profile-locked + two-pass auto layout。

## 4.一致性 Profile

| 项目 | 值 |
|---|---|
| layoutKey | 客户端-PC-Web |
| layoutVariantKey | workspace |
| pageIntentKey | commerce-list |
| stateGroupKey |  |
| layoutProfile | `product/style-profiles/客户端-PC-Web/layout-workspace-profile.json` |
| intentProfile | `product/style-profiles/客户端-PC-Web/intent-commerce-list-profile.json` |
| stateGroupProfile | `` |
| driftCheck | pass |
