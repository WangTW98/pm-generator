# PAGE-006 订单结算 Figma Node Notes

## Source

- Layout source: `layout.md`
- Sitemap source: `product/layout/客户端-PC-Web-sitemap.md`
- Layout key: `客户端 / PC Web`
- Page intent: `checkout-payment`

## Restore Scope

This page is restored as the default checkout state for service purchase payment. The Figma render should include:

- Order confirmation with service SKU summary and enterprise subject selector.
- Buyer information with contact details, coupon entry, and invoice entry.
- WeChat payment method selection.
- Sticky fee summary with payable amount and submit payment action.

## Excluded Non-default States

- 空状态
- 加载状态
- 错误状态

These states are recorded for report completeness only and must not be rendered as visible boards in the default Figma frame.

## Manual QA Notes

- Verify the checkout primary action is placed in the fee summary rather than duplicated across the page.
- Verify WeChat payment is visible as the only default payment method.
- Verify the page uses the client PC Web shell and does not create admin-side table layouts.
