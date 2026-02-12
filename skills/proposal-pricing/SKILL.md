---
name: proposal-pricing
description: >
  This skill should be used when the user asks to "estimate proposal cost",
  "calculate pricing", "build a cost estimate", "price this proposal",
  "what should we quote", "generate pricing", "calculate man-month rates",
  or needs to convert hourly billing rates into monthly/total project costs
  for engineering consultancy proposals.
version: 0.1.0
---

# Proposal Pricing & Cost Estimation

Calculate commercial proposal amounts for engineering consultancy services based on the company's rate card, staffing requirements extracted from client RFPs, and gap analysis results.

## Input Requirements

Before pricing, ensure the following are available:

1. **Rate Card**: Read from the company's rate card file (Excel or CSV in the user's folder). Expected columns: Category, Hourly Rate (AED), Description.
2. **Staff Requirements**: Output from the RFP analysis (positions, man-months, rate categories). **Each position must carry its source reference** (file name + page/sheet/row from the original RFP documents).
3. **Gap Analysis**: Which positions are in-house, new hire, or third-party. **Source references must be preserved** from the RFP Analysis Summary.

## Rate Card Structure

The standard rate card has two sections:

### Engineering Rates
| Code | Category | Description |
|------|----------|-------------|
| ENG-SE | Senior Engineer | Senior discipline engineers with 10+ years |
| ENG-E | Engineer | Mid-level engineers, resident engineers, QS, HSE |
| ENG-JE | Junior Engineer | Assistant engineers, officers, inspectors |
| ENG-SD | Senior Designer | Lead CAD/BIM designers |
| ENG-D | Designer | CAD operators, draftsmen |

### Project Management Rates
| Code | Category | Description |
|------|----------|-------------|
| PM-SE | PM Senior Engineer | Project managers, design leads |
| PM-E | PM Engineer | Planning engineers, project coordinators |

## Calculation Method

### Step 1: Monthly Rate Conversion
```
Monthly Rate = Hourly Rate x Standard Hours per Month
Standard Hours per Month = 176 (22 working days x 8 hours)
```

### Step 2: Position Cost Calculation
```
Position Total = Monthly Rate x Number of Months x Quantity
```

### Step 3: Stage Totals
Group positions by consultancy stage:
- **Stage A**: Tender preparation (design team effort during tender phase)
- **Stage B**: Contract administration & design review (design team during EPC)
- **Stage C**: Site supervision (site team, man-month basis)
- **Stage D**: Warranty services (part-time support during defects period)

### Step 4: Third-Party Markup
For positions identified as third-party/subcontract in gap analysis:
```
Third-Party Cost = Base Rate x Third-Party Markup Factor (typically 1.10-1.15)
```

### Step 5: Grand Total
```
Subtotal = Stage A + Stage B + Stage C + Stage D
VAT = Subtotal x 5%
Grand Total = Subtotal + VAT
```

## Important Notes

- Hourly rates are **billing rates** (all-inclusive of salary, benefits, overhead, and profit)
- Do NOT add additional overhead or profit markup on top of billing rates
- VAT at 5% is applied per UAE Federal Tax Authority requirements
- All amounts in AED (UAE Dirhams)
- Rates are firm and fixed — no escalation clauses
- For competitive bids, consider if any rates can be slightly reduced

## Output Format

Generate an Excel workbook with these sheets:
1. **Rate Card** — Company rates (linked from source file)
2. **Pricing Summary** — Detailed line-by-line with formulas. **Include an "RFP Source" column** for each line item showing the original document reference (file name, page/sheet/row) where that position requirement was found. Carry these references from the RFP Analysis Summary.
3. **Client Submission** — Clean summary matching client's BOQ format
4. **Source Traceability** — A dedicated reference sheet listing every pricing line item with its source document, page/sheet, and the specific requirement text. This allows the user to verify every cost against the original RFP.

Use Excel formulas (not hardcoded values) so the client can adjust inputs.

## Reference Material

For output formatting and Excel creation best practices, use the xlsx skill.
For Word report formatting, use the docx skill.
For PowerPoint presentations, use the pptx skill.
