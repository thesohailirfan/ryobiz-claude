---
name: shop-finance-gst
description: Use when the user asks about profit and loss, cash and bank balance, receivables, payables, who owes money, ageing, expenses, journal entries, trial balance, GST register, GSTR-2B/ITC reconciliation, or bank reconciliation in their Ryobiz shop.
---

# Shop finance and GST

Present figures and flag mismatches. **Do not give tax or legal advice.** For a judgement
call (ITC eligibility, filing position), say it needs their accountant or CA.

## Conventions

- All amounts from the tools are **integer paise**. Convert to ₹ with Indian grouping
  (₹1,25,000.50) and never show paise.
- Every report states its period or "as of" date. Always say which.
- Ledger entries are bookkeeping records. They don't move money.
- Writes may need in-app approval. If so, ask the user to approve it in Ryobiz, then
  repeat the same call with the same arguments plus `approvalId`.

## Start every time

`list_workspaces`. If there are several, ask which one. Pass `workspaceId` to every call.

## Reading finance (read-only)

- **Quick overview:** `get_shop_finance_overview`. Income, profit, GST, cash and bank,
  receivables and payables, inventory value, ageing, balance-sheet summary.
- **Statements:** `get_shop_reports`. P&L, balance sheet, trial balance, registers.
  For the trial balance, state whether debits equal credits.
- **Detail rows:** `list_shop_finance_report_rows`. Paginated, and totals cover all
  matching rows. Page with `nextOffset` while `hasMore` is true. Use it for "who owes
  me", ageing buckets and cash flow.
- **Accounts:** `list_shop_accounts`. Chart of accounts, used to pick accounts for entries.

## GST

- **Register:** `get_shop_gst_register` for the summary, `list_shop_gst_register_rows`
  for paginated rows. Split outward vs inward, and CGST + SGST vs IGST.
- **Reconcile against a portal statement** (e.g. GSTR-2B rows the user supplies):
  - `reconcile_shop_gst_statement` for a small statement.
  - `reconcile_shop_gst_statement_page` for large ones (up to 10,000 rows). Reuse the
    same input to page through results.
  - Both are read-only. They don't import data, change ITC eligibility or file returns.
- Report matched count, then each mismatch type with count and ₹ difference, then the
  top rows to fix. Read `references/gst-notes.md` for CGST/SGST vs IGST rules and the
  meaning of each mismatch type before explaining results.

## Writing entries (always confirm)

Show exactly what will post, then wait for "yes":

- **Expense:** `record_shop_expense`. Date, category or account, ₹ amount, GST (if
  any), paid from (cash/bank), and a note.
- **Journal:** `record_shop_journal`. List every debit and credit line with account and
  ₹. Debits must equal credits. Refuse to submit an unbalanced entry.
- **Opening capital:** `record_shop_opening_capital`. Amount, date, account.
- **Bank reconciliation:** `reconcile_shop_bank_entry`. Show the bank line (date,
  amount, narration) and the book entry it will match.

After posting, confirm the entry reference and its effect ("Expense of ₹4,500 for
electricity recorded on 05 Oct 2026, paid from HDFC Current"), then offer one next step.

## Guardrails

- If a call is denied, say so plainly. Never try another tool to get around it.
- Never invent figures, and never round away differences in a reconciliation.
- Never paste raw JSON. Tables are fine for comparing several rows (ageing, mismatches).
