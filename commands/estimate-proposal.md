---
description: Calculate proposal pricing based on rate card and RFP requirements
allowed-tools: Read, Glob, Grep, Bash, Task, TodoWrite, Write
---

Calculate the commercial proposal amount based on the company's rate card and the staffing requirements from the RFP analysis.

## Steps

1. Read the `proposal-pricing` skill from `${CLAUDE_PLUGIN_ROOT}/skills/proposal-pricing/SKILL.md` for the calculation methodology.

2. Locate and read the company's rate card file. Look for files named like `rate_card.*`, `rates.*`, `billing_rates.*` in the user's folder (Excel or CSV). If not found, ask the user.

   Expected rate card structure:
   | Category | Hourly Rate (AED) |
   |----------|-------------------|
   | PM S. Engr | 750 |
   | PM Engr | 400 |
   | Eng S. Engr | 250 |
   | Eng Engr | 200 |
   | Eng J. Engr | 100 |
   | Eng S. Desg | 120 |
   | Eng Desg | 100 |

3. Read the RFP Analysis Summary (`RFP_Analysis_Summary.md`) and Gap Analysis (`Gap_Analysis_Report.md`) from the user's folder.

4. Map each required position to the appropriate rate category using the discipline mapping in `${CLAUDE_PLUGIN_ROOT}/skills/rfp-analysis/references/discipline-mapping.md`.

5. Calculate costs:
   - Convert hourly rates to monthly: Rate x 176 hours
   - For each position: Monthly Rate x Months x Quantity
   - Group by stage (Tender Prep, Design Review, Site Supervision, Warranty)
   - Apply third-party markup (10-15%) for subcontracted positions from gap analysis
   - Calculate subtotal, VAT (5%), and grand total

6. Use the xlsx skill to create a professional Excel workbook with:
   - **Sheet 1: Rate Card** — Source rates with hourly-to-monthly conversion formulas
   - **Sheet 2: Pricing Summary** — Line-by-line breakdown with rate references, months, quantities, and formula-calculated totals
   - **Sheet 3: Client Submission** — Clean BOQ summary matching the client's required format

   Use Excel FORMULAS (not hardcoded values). Link rates from the Rate Card sheet. All yellow-highlighted cells should be editable inputs.

7. Save as `Proposal_Cost_Estimate_AED.xlsx` in the user's folder.

8. Present a summary to the user showing stage subtotals and grand total.

$ARGUMENTS
