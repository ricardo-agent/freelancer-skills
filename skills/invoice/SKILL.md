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

Always output **both**:

1. A clean **HTML** invoice the user can paste directly into an email (inline
2.    styles only — no external CSS, so it renders in any mail client).
3.2. A matching **Markdown** copy for their records.

  ## Required fields — ask only if missing

  - **From** (freelancer/business name). If unknown, use `[Your name]` as a placeholder.
  - - **Bill to** (client name).
    - - **Line items**: description, quantity/hours, unit rate. Expenses are qty 1.
      - - **Invoice number**: if not given, use `INV-0001`.
        - - **Issue date**: default to today.
          - - **Payment terms / due date**: default Net-15 if not specified.
           
            - Do not invent a rate or a client name. If a rate is genuinely missing and can't
            - be inferred from the user's stated day/hour rate, ask one short question.
           
            - ## Rules
           
            - - Compute line totals (qty × rate), a subtotal, optional tax (only if the user
              -   mentions one), and a clear **Total due**.
              -   - Currency: match what the user used ($ by default). Never mix currencies.
                  - - Keep it professional and plain — no emojis, no marketing language.
                    - - Right-align all money columns. Show the due date explicitly.
                      - - If the user gives a flat project price with no line items, produce a single
                        -   line item for the project.
                       
                        -   ## Output shape (HTML)
                       
                        -   A header with "Invoice #<num>", issued/due dates, a From/Bill-to block, a table
                        -   with Description / Qty / Rate / Amount columns, a bold Total due, and any notes
                        -   or payment details the user provided.
                       
                        -   ## Example
                       
                        -   Input: `12 hrs website fixes at $75/hr for Acme Corp, plus $40 hosting`
                       
                        -   Output: an invoice (HTML + Markdown) with two line items — "Website fixes, 12 ×
                        -   $75 = $900" and "Hosting, 1 × $40 = $40" — subtotal and Total due of **$940.00**,
                        -   issued today, due in 15 days.
                       
                        -   ---

                        This is the free `invoice` skill. The full **Claude Code Freelancer Pack** adds
                        6 more that handle the rest of the freelance admin grind — proposals, meeting
                        notes to action items, contract red-flagging, cold outreach, scope-creep replies,
                        and rate math: https://agentia11.gumroad.com/l/dukuod
