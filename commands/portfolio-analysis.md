---
description: Cross-reference RFP scope against current projects and proposals, analyze win rates, and model profitability scenarios
allowed-tools: Read, Glob, Grep, Bash, Task, TodoWrite, Write
---

Analyze the company's project portfolio and proposal pipeline against a given RFP scope of work, showing business intelligence on win rates, project financials, and profitability scenarios.

## Steps

### 0. Verify Setup Data

Before starting portfolio analysis, check for the required data files. **Ask the user to provide any missing files before proceeding.**

| File | Purpose | Required |
|------|---------|----------|
| Projects export (`.xlsx`) | Active/past projects with financials | **Yes** |
| Proposals export (`.xlsx`) | Submitted proposals with win/loss status | **Yes** |
| `RFP_Analysis_Summary.md` | Scope items for matching against portfolio | Optional (enables scope matching) |

**Check procedure:**
1. Look for a **projects export file** in the user's working folder first: files named like `all-projects*.xlsx`, `projects*.xlsx`, or `project_export*.xlsx`. If not found, tell the user: *"Please provide your projects export file (Excel). See the template at `setup/projects_template.xlsx` for the expected format."*
2. Look for a **proposals export file** in the user's working folder: files named like `proposals*.xlsx`, `proposal_export*.xlsx`, or `bids*.xlsx`. If not found, tell the user: *"Please provide your proposals export file (Excel). See the template at `setup/proposals_template.xlsx` for the expected format."*
3. Optionally check for `RFP_Analysis_Summary.md` in the user's folder (from `/analyze-rfp`). If found, enable scope matching. If not, inform the user: *"No RFP Analysis found — running portfolio analysis without scope matching. Run `/analyze-rfp` first if you want scope-to-portfolio matching."*

**Do not proceed until both the projects and proposals export files are available.**

### 1. Locate and Validate Input Files

Confirm the files found in Step 0:

- **Projects file**: The file has a summary header section followed by a data table. **Important**: The header row may not be at row 0 — scan for the row containing "Project Number" as the first column to find the correct header row.

- **Proposals file**: **Important**: Filter out summary/total rows at the bottom of the file. Only use rows where Status is one of: Submitted, Won, Lost, Cancelled, To Be Submitted, To Be Resubmitted, Resubmitted.

### 2. Parse and Clean Data

Use Python with pandas and openpyxl to read both files:

```python
# Projects: find the header row dynamically
# Look for the row where column 0 == "Project Number"
# Filter out rows with NaN Project Number

# Proposals: filter to actual proposal rows only
# Valid statuses: Submitted, Won, Lost, Cancelled, To Be Submitted, To Be Resubmitted, Resubmitted
```

### 3. Scope Item Identification & Portfolio Matching

This is the core analysis step. Extract **every individual scope item** from the RFP Analysis Summary and match each one against the full project portfolio and proposal pipeline.

#### 3a. Extract Scope Items

From the RFP Analysis Summary, identify each distinct scope item. These are the individual deliverables, disciplines, or work packages the RFP requires. Examples: "132kV Substation Design", "Civil Works & Foundations", "SCADA/Telemetry System", "Construction Supervision", "BIM Modeling", "Environmental Impact Assessment", etc.

List every scope item — do not group or summarize them. Each item should be as specific as the RFP describes it.

#### 3b. Match Against Projects

For each scope item, search the **projects export** for matches. Use keyword matching on the **Project Name** column (e.g., scope item "Substation Design" matches projects containing "substation", "132kV", "33kV", "switchgear", etc.). Also consider the project's sector and discipline context.

For every match, record:
- **Project Number** (e.g., P19860102)
- **Project Name** (full name from the export)
- **Client**
- **Contract Value (AED)**
- **Status** (in-progress, completed, etc.)

#### 3c. Match Against Proposals

For each scope item, search the **proposals export** for matches. Use keyword matching on the **Project Title** column. Also match by sector, discipline, and end user context.

For every match, record:
- **Proposal No** (e.g., T-008-1-2026)
- **Project Title** (full title from the export)
- **Client**
- **Estimated Value (AED)**
- **Status** (Won, Submitted, Lost, etc.)

#### 3d. Scope Coverage Matrix

Present the complete matching results. **Every matching project and proposal must show its number and name.**

| # | RFP Scope Item | RFP Source | Matching Projects (No — Name) | Matching Proposals (No — Name) | Experience Level |
|---|---------------|------------|-------------------------------|-------------------------------|-----------------|
| 1 | 132kV Substation Design | 📄 [TOR.pdf, p.8] | **P19860102** — 400kV ICAD4 Substation (completed); **P19860017** — Al Nabbah 33kV Primary Substation (in-progress) | **T-008-1-2026** — 132kV GIS Substation for TRANSCO (Submitted) | STRONG |
| 2 | Civil Works & Foundations | 📄 [TOR.pdf, p.12] | **P19860045** — Khazna Solar PV4 Substation Civil Works (in-progress); **P19860089** — Pedestrian Facilities ADM (in-progress) | **T-012-2026** — ADDC Substation Civil Package (Submitted) | STRONG |
| 3 | SCADA / Telemetry | 📄 [TOR.pdf, p.15] | None | **T-475-2025** — SCADA Upgrade TRANSCO (Won) | LIMITED |
| 4 | Environmental Impact Assessment | 📄 [TOR.pdf, p.20] | None | None | NONE |

#### 3e. Experience Level Classification

- **STRONG**: 3+ matching projects and/or proposals, with at least 1 completed project
- **MODERATE**: 1-2 matching projects and/or proposals
- **LIMITED**: Only proposals (no completed/active projects), or only 1 match total
- **NONE**: No matching projects or proposals found — this is a gap

#### 3f. Scope Coverage Summary

After the matrix, provide:
- Total scope items identified: X
- STRONG coverage: X items (list them)
- MODERATE coverage: X items (list them)
- LIMITED coverage: X items (list them)
- NONE / Gap: X items (list them) — **flag these as risks**

### 4. Client Relationship Analysis

Cross-reference clients between projects and proposals:

| Client | Active Projects | Completed Projects | Total Project Value (AED) | Proposals Submitted | Proposals Won | Win Rate | Relationship |
|--------|----------------|-------------------|--------------------------|--------------------|--------------|---------:|-------------|
| Siemens Energy | 2 | 0 | 500,000 | 3 | 1 | 33% | REPEAT CLIENT |
| EMC | 5 | 1 | 2,200,000 | 4 | 0 | 0% | ACTIVE — NO WINS |

### 5. Comparable Win/Loss Analysis

**Do NOT analyze the full proposal pipeline.** Filter down to only proposals that are comparable to this RFQ's scope, then analyze that filtered set.

#### 5a. Build the Comparable Set

Using the scope keywords identified in Step 3, filter the proposals export to only those whose **Project Title** matches the same keywords. Also consider:
- Sector similarity (e.g., if the RFQ is for power infrastructure, include power/substation/solar proposals but exclude unrelated sectors like residential villas)
- Service type similarity (e.g., if the RFQ is for design consultancy, include design/engineering proposals but exclude pure supervision or PMC proposals)
- Scale similarity (exclude proposals that are 10x+ larger or smaller unless there's a direct scope match)

List every comparable proposal with its status:

| # | Proposal No | Project Title | Client | Estimated Value (AED) | Status | Scope Match |
|---|------------|---------------|--------|----------------------:|--------|-------------|
| 1 | T-008-1-2026 | 132kV GIS Substation TRANSCO | TRANSCO | 450,000 | Submitted | Substation Design |
| 2 | T-012-2026 | ADDC Civil Package | ADDC | 275,000 | Won | Civil Works |
| 3 | T-019-2025 | MEP Design Al Reem | Developer LLC | 1,600,000 | Lost | Building Services |

#### 5b. Comparable Win/Loss Statistics

From the filtered set only:
- Comparable proposals found: X
- Won: X (win rate: Y%)
- Lost: X
- Pending/Submitted: X
- Average value of won proposals: AED X
- Average value of lost proposals: AED X
- Value range of won proposals: AED X – AED Y

#### 5c. Win/Loss Insight

Analyze what distinguishes won vs. lost proposals in this comparable set:
- Were wins typically smaller or larger in value?
- Were wins concentrated with specific clients or sectors?
- What is the "sweet spot" value range where the company wins?
- Flag if the current RFQ's likely value falls inside or outside the winning range

This is the key bid intelligence — it tells the user where they actually compete successfully.

### 6. Portfolio Baseline (Brief)

Provide a **one-paragraph** summary of the portfolio's financial health — just enough to feed into the cost estimate. Include only:
- Average profit % across active projects (from projects export)
- Median cost per hour (Actual Costs ÷ Actual Hours across projects with valid data)
- Current utilization rate (Actual Hours ÷ Contract Hours)

Do NOT include full project breakdowns, top-10 lists, or underperformer tables. This section exists solely to provide inputs for Section 7.

### 7. Independent Cost Estimate & Bid Recommendation

**This is not about modeling scenarios around an existing bid.** Build an independent bottom-up estimate of what this RFQ should cost, then recommend a bid range.

#### 7a. Method 1 — Rate Card Staffing Model

Using the scope items identified in Step 3, estimate the team composition and duration this RFQ requires, then price it using the company's own rate card.

1. For each scope item, determine which discipline roles are needed and for how many months (use the RFP Analysis Summary if available, or estimate from comparable projects)
2. Apply the company's billing rates from `${CLAUDE_PLUGIN_ROOT}/setup/rate_card.csv`:
   - Hourly rate × 176 hours = monthly rate
   - Monthly rate × months × quantity = line item cost
3. Add third-party/subcontractor costs for any PARTNER/NONE disciplines (from org_capabilities.md) at a 10–15% markup
4. Sum all line items

Present the staffing model:

| # | Role | Discipline | Rate Category | Monthly Rate (AED) | Months | Qty | Line Total (AED) |
|---|------|-----------|--------------|-------------------:|-------:|----:|-----------------:|
| 1 | Project Manager | PM | PM-SE | 132,000 | 12 | 1 | 1,584,000 |
| 2 | Senior Civil Engineer | Civil | ENG-SE | 44,000 | 8 | 1 | 352,000 |
| ... | ... | ... | ... | ... | ... | ... | ... |
| | | | | | | **Subtotal** | **X** |
| | | | | | | **VAT (5%)** | **X** |
| | | | | | | **Total (Method 1)** | **X** |

#### 7b. Method 2 — Internal Cost + Overhead Multiplier

Use the portfolio's actual cost data to estimate from the bottom up:

1. Calculate the **median cost per hour** from the projects export (Actual Costs ÷ Actual Hours, filtering to projects with valid data)
2. Estimate total hours for this RFQ (from the staffing model in 7a, or from comparable project hours)
3. Apply an overhead multiplier range:
   - **2.0x** = cost recovery + modest margin (competitive bid)
   - **2.5x** = standard consultancy markup
   - **3.0x** = premium/complex project markup

| Multiplier | Estimated Hours | Cost/Hr (AED) | Base Cost (AED) | Bid Amount (AED) |
|-----------|----------------:|--------------:|----------------:|-----------------:|
| 2.0x (competitive) | X | X | X | X |
| 2.5x (standard) | X | X | X | X |
| 3.0x (premium) | X | X | X | X |

#### 7c. Method 3 — Portfolio Revenue Benchmarking

Use comparable completed projects to benchmark:

1. From the projects matched in Step 3 (scope-similar projects), calculate:
   - Median contract value
   - Median revenue per hour (Contract Value ÷ Contract Hours)
   - Median billing rate per staff-month
2. Apply the median billing rate to the estimated team composition from 7a

| Metric | Value |
|--------|------:|
| Comparable projects used | X |
| Median contract value (AED) | X |
| Median revenue per hour (AED) | X |
| Estimated hours for this RFQ | X |
| **Benchmark estimate (AED)** | **X** |

#### 7d. Cross-Reference with Comparable Proposals

Compare the three estimates against the comparable won/lost proposal values from Section 5:

| Method | Estimate (AED) | vs. Avg Won Value | vs. Avg Lost Value | Signal |
|--------|---------------:|------------------:|-------------------:|--------|
| Method 1: Rate card staffing | X | +/-X% | +/-X% | Within/above/below winning range |
| Method 2: Internal cost × 2.5x | X | +/-X% | +/-X% | Within/above/below winning range |
| Method 3: Portfolio benchmark | X | +/-X% | +/-X% | Within/above/below winning range |
| Comparable won avg | X | — | — | Reference |
| Comparable lost avg | X | — | — | Reference |

#### 7e. Recommended Bid Range

Based on all three methods and the comparable win/loss data, recommend:

- **Floor** (minimum bid): The lowest of the three estimates, but not below the Method 2 cost-recovery (2.0x) level
- **Target** (recommended bid): The convergence point of the three methods, adjusted toward the "winning range" from comparable proposals
- **Ceiling** (premium bid): The highest justifiable amount — above this, the win probability drops based on comparable lost proposal values

| | Amount (AED) | Basis |
|--|-------------:|-------|
| **Floor** | X | Cost recovery — competitive but thin margin |
| **Target** | X | Best estimate — balances margin and win probability |
| **Ceiling** | X | Premium — justifiable for strong scope match but higher risk of loss |

**Bid recommendation narrative**: Write 2-3 sentences explaining the recommended target amount, why it's positioned where it is relative to the comparable won/lost values, and any key risk factors (e.g., "The target of AED X is positioned 15% above the average comparable win value, reflecting the additional SCADA scope. However, comparable losses averaged AED Y, suggesting bids above AED Z face significantly lower win probability.")

### 8. Save Output

Save the full analysis as `Portfolio_Analysis_Report.md` in the user's folder.

**Include source references throughout:**
- Every scope match references the RFP source document (📄 [filename, page])
- Every project/proposal reference includes the source file and row: 📄 [all-projects.xlsx, Row X] or 📄 [proposals.xlsx, Row X]
- Every comparable proposal references its row in the proposals export
- Include a Source Document Index at the top

### 9. Present Summary

Present a concise summary to the user highlighting:
- **Scope coverage**: How many scope items have STRONG/MODERATE/LIMITED/NONE coverage
- **Comparable win intelligence**: Win rate on similar proposals, winning value range
- **Independent cost estimate**: The three method estimates and where they converge
- **Recommended bid range**: Floor / Target / Ceiling with the reasoning
- **Key risks**: Scope gaps (NONE items), areas where the bid is outside winning range, capacity concerns

$ARGUMENTS
