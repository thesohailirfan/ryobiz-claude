---
name: shop-purchases-inventory
description: Use when the user mentions purchase orders, buying from a supplier, receiving goods (GRN), stock levels, low stock, stock adjustment, damaged or missing goods, stock movement history, or adding/editing products in their Ryobiz shop.
---

# Shop purchases and inventory

## Conventions

- Money in tool calls is **integer paise** (₹1,250.50 = 125050). Quantities are
  **thousandths of a selling unit** (2.5 boxes = 2500). Always show ₹ and units.
- Writes may need in-app approval. If so, ask the user to approve it in Ryobiz, then
  repeat the same call with the same arguments plus `approvalId`.

## Workflow: start every time

`list_workspaces`. If there are several, ask which one. Pass `workspaceId` to every call.

## Checking stock (read-only)

- `list_shop_inventory`: current stock by product (filter by name/code where possible).
  Lead with what's low or out of stock.
- `list_shop_movements`: history for a product: purchases, sales, returns, adjustments.
  Use it to answer "why is stock X?" questions.
- `list_shop_purchases`: purchase orders and bills, with status and balances.

## Purchase order → receive

1. Resolve the supplier (look up the party; if it's new, confirm details first) and the
   products, rates and quantities.
2. Show the PO summary: supplier, lines, total in ₹, expected date. On "yes" call
   `create_shop_purchase_order`.
3. When goods arrive, show the PO's pending quantities, confirm what was actually
   received (partial is fine), then `receive_shop_purchase_order`.
4. If the remainder will never come, show the outstanding lines and ₹ value and, on
   "yes", call `cancel_shop_purchase_remainder`.

## Direct purchase bill

For goods bought without a PO, show the supplier, bill number and date, lines, GST and
total, then on "yes" call `create_shop_purchase`. It posts to the books and stock.

## Stock adjustment (always needs a reason and a "yes")

1. Get the product, the current quantity (`list_shop_inventory`), the new or corrected
   quantity, and a **reason** (damage, breakage, count correction, sample, theft…).
   Don't proceed without a reason.
2. Show: product, current → new quantity, difference, and reason. Wait for "yes".
3. Call `adjust_shop_stock`. Confirm the new quantity.

## Products

To add or edit a product, collect name, unit, selling rate, purchase rate, GST rate, HSN
(if known), and box coverage for area-sold items. Show it back, then call
`save_shop_product` on "yes". Don't guess the GST rate or HSN. Ask.

## After every write

Confirm the record (PO number, bill number, product name) and its new status or quantity,
then offer one next step (e.g. "Receive the rest when it arrives?").

## Guardrails

- If a call is denied, say so plainly. Never try another tool to get around it.
- Never paste raw JSON. Use ₹ with Indian grouping (₹1,25,000) and dates as DD MMM YYYY.
