---
name: shop-sales
description: Use when the user wants to make a quotation, estimate, sales order or GST invoice, record a customer payment or advance, handle a sales return, cancel a bill, or print an invoice in their Ryobiz shop.
---

# Shop sales: quotation → order → invoice → payment → print

## Conventions

- Money in tool calls is **integer paise** (₹1,250.50 = 125050). Quantities are
  **thousandths of a selling unit** (2.5 boxes = 2500). Always show the user ₹ and units.
- Payments and advances are bookkeeping records. They do not collect or move money.
- Writes may need in-app approval. If so, ask the user to approve it in Ryobiz, then
  repeat the same call with the same arguments plus `approvalId`.

## Workflow

1. **Workspace.** `list_workspaces`. If there are several, ask which one. Pass
   `workspaceId` to every call.
2. **Customer.** `list_shop_parties` with the name or phone.
   - One match: use it. Several: ask which.
   - None: offer to create it. Show name, phone, GSTIN (if any), billing state, then on
     "yes" call `save_shop_party`.
3. **Items.** `list_shop_products` to resolve each item, rate, unit and GST rate.
   - For tiles or flooring by area, use `calculate_shop_room_quantity` to convert room
     size into whole boxes, and show the result before using it.
4. **Stock check** (before any order or invoice): `list_shop_inventory` for the items.
   Flag any shortfall and ask whether to continue, reduce the quantity, or make a quotation.
5. **Draft summary.** Before creating anything, show:
   customer, each line (item, qty + unit, rate, GST %), taxable value, GST, **total in ₹**,
   and document date. Ask "Create this quotation/order/invoice?"
6. **Create** on "yes":
   - Quotation: `create_shop_quotation`. Order: `create_shop_order`.
     Invoice: `create_shop_invoice` (issuing an invoice posts to the books; confirm first).
   - Moving forward from an existing document: `convert_shop_document` (quotation →
     order, order → invoice) with the new document date. Confirm before converting to an
     invoice.
7. **Payment.**
   - Against an invoice: confirm amount, mode (cash/UPI/bank/cheque), date and reference,
     then `record_shop_payment`.
   - Before an invoice exists: `record_shop_advance`. Later, apply it with
     `allocate_shop_advance` after showing which invoice and how much.
8. **Print / share.**
   - `get_shop_document_print` returns printable HTML. Offer it for saving or printing.
   - `print_shop_document` queues a physical print at an office station. Confirm the
     station and number of copies, and reuse the same `requestId` on retry. "Queued"
     does not prove pages printed.

## Returns and cancellations (always confirm)

- **Goods return:** `return_shop_invoice_goods`. Show the invoice number, items and
  quantities returned, and the ₹ credit, then wait for "yes".
- **Cancel:** `cancel_shop_document`. Show the document number, party, total and any
  linked payments, and warn that it reverses the posting. Wait for "yes".

## After every write

Confirm with the document number and new status, e.g. "Invoice INV-0142 created for
Sharma Traders, ₹1,18,000 incl. GST, status Unpaid." Then offer one next step
(record payment, print, schedule delivery).

## Guardrails

- If a call is denied, say so plainly. Never try another tool to get around it.
- Don't give tax advice. Use the GST rate stored on the product. If it's missing, ask.
- Never paste raw JSON. Use ₹ with Indian grouping (₹1,25,000) and dates as DD MMM YYYY.
