---
description: Calculate proposal pricing based on rate card and RFP requirements
allowed-tools: Read, Glob, Grep, Bash, Task, TodoWrite, Write
---

Calculate the commercial proposal amount based on the company's rate card and the staffing requirements from the RFP analysis.

## Steps

### 0. Verify Setup Data

Before starting pricing, check the `setup/` folder at `${CLAUDE_PLUGIN_ROOT}/setup/` for the following required files. **Ask the user to provide any missing files before proceeding.**

| File | Purpose | Required |
|------|---------|----------|
| `rate_card.csv` | Billing rates by staff category | **Yes** |
| `org_capabilities.md` | Company profile for proposal context | Optional |

**Check procedure:**
1. Look for `rate_card.csv` in `setup/`. If it only contains sample rates, tell the user: *"Please update the rate card with your actual billing rates. The current file appears to contain sample data."*
2. Check the user's working folder for `RFP_Analysis_Summary.md` (from `/analyze-rfp`) and `Gap_Analysis_Report.md` (from `/gap-analysis`). If either is missing, tell the user which commands to run first.

**Do not proceed until rate_card.csv and RFP_Analysis_Summary.md are available.**

### 1. Read Skill

Read the `proposal-pricing` skill from `${CLAUDE_PLUGIN_ROOT}/skills/proposal-pricing/SKILL.md` for the calculation methodology.

### 2. Read Rate Card

Read `rate_card.csv` from `${CLAUDE_PLUGIN_ROOT}/setup/`. Expected structure:

   | Category | Hourly Rate (AED) |
   |----------|-------------------|
   | PM S. Engr | 750 |
   | PM Engr | 400 |
   | Eng S. Engr | 250 |
   | Eng Engr | 200 |
   | Eng J. Engr | 100 |
   | Eng S. Desg | 120 |
   | Eng Desg | 100 |

### 3. Read Analysis Outputs

Read the RFP Analysis Summary (`RFP_Analysis_Summary.md`) and Gap Analysis (`Gap_Analysis_Report.md`) from the user's folder.

### 4. Map Positions

Map each required position to the appropriate rate category using the discipline mapping in `${CLAUDE_PLUGIN_ROOT}/skills/rfp-analysis/references/discipline-mapping.md`.

### 5. Calculate Costs

Calculate costs:
   - Convert hourly rates to monthly: Rate x ~~working_hours_per_month hours
   - For each position: Monthly Rate x Months x Quantity
   - Group by stage (Tender Prep, Design Review, Site Supervision, Warranty)
   - Apply third-party markup (~~third_party_markup) for subcontracted positions from gap analysis
   - Calculate subtotal, ~~tax_name (~~tax_rate), and grand total

### 6. Create Excel Workbook

Use the xlsx skill to create a professional Excel workbook with:
   - **Sheet 1: Rate Card** — Source rates with hourly-to-monthly conversion formulas
   - **Sheet 2: Pricing Summary** — Line-by-line breakdown with rate references, months, quantities, and formula-calculated totals. **Include an "RFP Source" column** for each line item showing the original document reference (file name, page/sheet/row) where that position requirement was found. Carry these references from the RFP Analysis Summary.
   - **Sheet 3: Client Submission** — Clean BOQ summary matching the client's required format
   - **Sheet 4: Source Traceability** — A dedicated reference sheet listing every pricing line item with its source document, page/sheet, and the specific requirement text. This allows the user to verify every cost against the original RFP.

   Use Excel FORMULAS (not hardcoded values). Link rates from the Rate Card sheet. All yellow-highlighted cells should be editable inputs.

### 7. Save Output

Save as `Proposal_Cost_Estimate_AED.xlsx` in the user's folder.

### 8. Present Summary

Present a summary to the user showing stage subtotals and grand total. **Include a note that source references are available in the "RFP Source" column on the Pricing Summary sheet and the Source Traceability sheet.**

$ARGUMENTS
