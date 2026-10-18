# GST notes for explaining Ryobiz figures

This is background for explaining what the register and reconciliation tools return.
It is not tax advice. Point the user to their accountant or CA for eligibility or filing
decisions.

## CGST + SGST vs IGST

| Supply | Tax charged | Split |
| --- | --- | --- |
| Intra-state (seller and place of supply in the same state) | CGST + SGST (UTGST in union territories) | Rate split equally, e.g. 18% = 9% CGST + 9% SGST |
| Inter-state (different states), and imports or SEZ supplies | IGST | Full rate as IGST, e.g. 18% IGST |

- The **place of supply** decides the split, not the buyer's billing address alone. If a
  party's state looks wrong, the split will look wrong too. Flag it and don't change it.
- The total tax is the same either way. Only the split differs.
- A common data error: an invoice to an out-of-state GSTIN charged CGST+SGST, or a local
  party charged IGST. Report it as a classification mismatch.
- The first two digits of a GSTIN are the state code (e.g. 19 = West Bengal,
  27 = Maharashtra, 29 = Karnataka, 07 = Delhi).

## Register terms

- **Outward supplies:** sales. Tax collected (output tax).
- **Inward supplies:** purchases. Tax paid that may be claimable (input tax credit, ITC).
- **Taxable value:** amount before GST. **Tax amount:** CGST + SGST + IGST (+ cess).
- **B2B vs B2C:** with or without a buyer GSTIN.

## Reconciliation mismatch types

When the books are compared against a portal statement (such as GSTR-2B rows supplied by
the user), results usually fall into these groups. Use the labels the tool returns. This
table explains what each one usually means.

| Type | Meaning | Typical next step for the user |
| --- | --- | --- |
| Matched | Same supplier GSTIN, invoice number, date and amounts within tolerance | None |
| Missing in books | On the portal statement but not recorded in Ryobiz | Check whether the bill was received and record the purchase |
| Missing in statement | Recorded in Ryobiz but the supplier hasn't reported it | Follow up with the supplier to file or correct |
| Amount mismatch | Taxable value or tax differs | Compare the bill copy, then correct the entry or ask the supplier |
| Tax-head mismatch | CGST/SGST vs IGST split differs | Check the place of supply on both sides |
| Invoice number / date mismatch | Probable match with a typo or format difference (e.g. "INV/12" vs "INV-012") | Confirm it's the same bill and correct the reference |
| GSTIN mismatch | Same invoice under a different or invalid GSTIN | Verify the supplier's GSTIN |
| Duplicate | The same invoice appears more than once on one side | Remove or void the duplicate |

When reporting, give the count and ₹ difference for each type, then list the largest
items first. Never say whether ITC can be claimed. That is a call for the user's CA.
