---
name: invoice
description: Generate a clean, client-ready invoice from a plain-English description of the work. Use when the user says "make an invoice", "bill this client", "write up an invoice for...", or describes hours, rate, or expenses that should become an invoice. Produces polished HTML (paste straight into an email) plus a matching Markdown copy for records. No API keys, no external calls.
license: Free to use and adapt for your own client work.
---

# invoice

Turn a plain-English description of work into a professional invoice. Free skill
from the Claude Code Freelancer Pack.

## When to use

The user describes work they did for a client and wants it invoiced — e.g.
"12 hrs of website fixes at $75/hr for Acme Corp, plus $40 hosting", or "bill
Nova Studio for the 3-day brand sprint at my day rate of $600".

## What to produce

Always produce **both**, and **save them as files** so the user has a tangible
deliverable:

1. **`invoice-<number>.html`** — a polished invoice using the reference template
   below (inline styles only, so it renders identically in any mail client).
   Tell the user: open it in a browser to print or save as PDF, or paste the
   body into an email.
2. **`invoice-<number>.md`** — a matching Markdown copy for their records.

If the environment can't write files, output both inline instead.

## Required fields — ask only if missing

- **From** (freelancer/business name). If unknown, use `[Your name]` as a placeholder.
- **Bill to** (client name).
- **Line items**: description, quantity/hours, unit rate. Expenses are qty 1.
- **Invoice number**: if not given, use `INV-0001`.
- **Issue date**: default to today. **Due date**: compute and show the real date
  (issue date + terms), not just "Net-15".
- **Payment terms**: default Net-15 if not specified.

Do not invent a rate, a client name, or payment details. If a rate is genuinely
missing and can't be inferred from the user's stated day/hour rate, ask one
short question. If no payment details were given, include a
`[Add payment details — bank / PayPal / link]` placeholder so the user never
sends an invoice that can't be paid.

## Rules

- Compute line totals (qty × rate), a subtotal, optional discount and tax (only
  if the user mentions them), and a clear **Total due**. Round money to 2 decimals.
- Currency: match what the user used ($ by default). Never mix currencies.
- Keep it professional and plain — no emojis, no marketing language.
- Right-align all money columns. Show the due date explicitly as a date.
- If the user gives a flat project price with no line items, produce a single
  line item for the project.

## Reference HTML template

Use this structure and styling (adapt content, keep the look). Inline styles and
table layout only — this is deliberate, for email-client compatibility:

```html
<div style="max-width:640px;margin:0 auto;font-family:-apple-system,Segoe UI,Roboto,Helvetica,Arial,sans-serif;color:#1a1f1c;line-height:1.5">
  <table width="100%" cellpadding="0" cellspacing="0" style="margin-bottom:28px"><tr>
    <td style="font-size:26px;font-weight:700;letter-spacing:-0.02em">INVOICE</td>
    <td align="right" style="font-size:14px;color:#5f6a62">
      <strong style="color:#1a1f1c">INV-0001</strong><br>
      Issued: Jul 18, 2026<br>
      <strong style="color:#1a1f1c">Due: Aug 2, 2026</strong> (Net-15)
    </td>
  </tr></table>
  <table width="100%" cellpadding="0" cellspacing="0" style="margin-bottom:28px;font-size:14px"><tr>
    <td valign="top"><span style="color:#5f6a62;font-size:12px;text-transform:uppercase;letter-spacing:0.05em">From</span><br><strong>[Your name]</strong><br>[email / address]</td>
    <td valign="top" align="right"><span style="color:#5f6a62;font-size:12px;text-transform:uppercase;letter-spacing:0.05em">Bill to</span><br><strong>Acme Corp</strong><br>[contact]</td>
  </tr></table>
  <table width="100%" cellpadding="0" cellspacing="0" style="font-size:14px;border-collapse:collapse">
    <tr style="color:#5f6a62;font-size:12px;text-transform:uppercase;letter-spacing:0.05em">
      <td style="padding:8px 0;border-bottom:1px solid #d8ded9">Description</td>
      <td align="right" style="padding:8px 0;border-bottom:1px solid #d8ded9">Qty</td>
      <td align="right" style="padding:8px 0;border-bottom:1px solid #d8ded9">Rate</td>
      <td align="right" style="padding:8px 0;border-bottom:1px solid #d8ded9">Amount</td>
    </tr>
    <tr>
      <td style="padding:10px 0;border-bottom:1px solid #eef1ee">Website fixes</td>
      <td align="right" style="padding:10px 0;border-bottom:1px solid #eef1ee">12</td>
      <td align="right" style="padding:10px 0;border-bottom:1px solid #eef1ee">$75.00</td>
      <td align="right" style="padding:10px 0;border-bottom:1px solid #eef1ee">$900.00</td>
    </tr>
    <tr>
      <td style="padding:10px 0;border-bottom:1px solid #eef1ee">Hosting</td>
      <td align="right" style="padding:10px 0;border-bottom:1px solid #eef1ee">1</td>
      <td align="right" style="padding:10px 0;border-bottom:1px solid #eef1ee">$40.00</td>
      <td align="right" style="padding:10px 0;border-bottom:1px solid #eef1ee">$40.00</td>
    </tr>
    <tr><td colspan="3" align="right" style="padding:12px 0 4px;color:#5f6a62">Subtotal</td><td align="right" style="padding:12px 0 4px">$940.00</td></tr>
    <tr><td colspan="3" align="right" style="padding:10px 0;font-weight:700;font-size:16px;border-top:2px solid #1a1f1c">Total due</td><td align="right" style="padding:10px 0;font-weight:700;font-size:16px;border-top:2px solid #1a1f1c">$940.00</td></tr>
  </table>
  <p style="margin-top:26px;font-size:13px;color:#5f6a62"><strong style="color:#1a1f1c">Payment:</strong> [Add payment details — bank / PayPal / link]<br>Payment due by <strong>Aug 2, 2026</strong>. Thank you!</p>
</div>
```

Add a Discount row (negative amount) and/or Tax row between Subtotal and Total
due only when the user mentions them.

## Example

Input: `12 hrs website fixes at $75/hr for Acme Corp, plus $40 hosting`

Output: `invoice-INV-0001.html` + `invoice-INV-0001.md` with two line items —
"Website fixes, 12 × $75 = $900" and "Hosting, 1 × $40 = $40" — subtotal and
Total due of **$940.00**, issued today, due date shown as a real date 15 days out.

---

This is the free `invoice` skill from **FreelanceKit**. If it saved you time, the
full **Claude Code Freelancer Pack** adds 6 more skills that handle the rest of the
freelance admin grind — proposals, meeting notes to action items, contract
red-flagging, cold outreach, scope-creep replies, and rate math.

- **Get the pack** — early-adopter price $15 (list $27), 30-day money-back guarantee: https://agentia11.gumroad.com/l/dukuod
- **Free browser tools** (invoices, rates, contracts, proposals): https://freelancekit.tools
