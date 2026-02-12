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

### 5. Win/Loss Analysis

Provide overall proposal pipeline statistics:

**Summary:**
- Total proposals submitted: X
- Won: X (Y% win rate)
- Lost: X
- Pending/Submitted: X
- Cancelled: X
- Total estimated value of won proposals: AED X
- Total estimated value of submitted (pending) proposals: AED X
- Average proposal value: AED X

**Won Proposals Detail** (with source references if RFP scope is available):

| # | Proposal No | Project Title | Client | Estimated Value (AED) | Matching RFP Scope | RFP Source |
|---|------------|---------------|--------|----------------------:|--------------------|-----------:|
| 1 | T-002-2026 | ADQ Solar/PV 2.25GW... | Siemens Energy | 0 | Electrical, Substation | 📄 [TOR.pdf, p.12] |

### 6. Project Financial Performance

Analyze active and completed projects:

**By Status:**

| Status | Count | Total Contract Value (AED) | Total Actual Costs (AED) | Avg Profit % |
|--------|------:|---------------------------:|-------------------------:|-------------:|
| In-Progress | X | X | X | X% |
| Completed | X | X | X | X% |
| Blocked | X | X | X | X% |

**Top 10 Projects by Contract Value:**

| # | Project | Client | Contract Value | Actual Costs | Profit % | Status |
|---|---------|--------|---------------:|-------------:|---------:|--------|

**Underperforming Projects** (negative profit or profit < 5%):

| # | Project | Client | Contract Value | Actual Costs | Profit % | Over/Under Hours |
|---|---------|--------|---------------:|-------------:|---------:|-----------------:|

### 7. Profitability Scenario Modeling

Using the RFP staffing and pricing data (if available from `/estimate-proposal`), or the current portfolio average, model these scenarios:

**Scenario A — Current Portfolio Average**
Use the actual average profit % from the projects file as the baseline.

**Scenario B — 10% Target Profit Margin**
```
Required Revenue = Total Estimated Costs / (1 - 0.10)
Profit = Revenue - Costs
```

**Scenario C — 15% Target Profit Margin**
```
Required Revenue = Total Estimated Costs / (1 - 0.15)
Profit = Revenue - Costs
```

**Scenario D — Contract Win Scenarios**
Model outcomes based on different win rates for pending proposals:

| Scenario | Win Rate | Proposals Won | New Revenue (AED) | At 10% Profit | At 15% Profit | At Current Avg Profit |
|----------|---------|:-------------:|------------------:|--------------:|--------------:|---------------------:|
| Conservative | 10% | X | X | X | X | X |
| Moderate | 25% | X | X | X | X | X |
| Optimistic | 40% | X | X | X | X | X |
| Best Case | 60% | X | X | X | X | X |

**Scenario E — Capacity Utilization**
Using the projects data (Contract Hours vs Actual Hours):
- Current utilization rate
- Revenue per hour
- Projected revenue at 80%, 90%, 100% utilization
- Impact on profit at each utilization level

### 8. RFP-Specific Profitability (if estimate available)

If `Proposal_Cost_Estimate_AED.xlsx` exists (from `/estimate-proposal`), create a scenario analysis specific to this RFP:

| Scenario | Bid Amount (AED) | Estimated Costs (AED) | Profit (AED) | Profit % | Notes |
|----------|----------------:|---------------------:|--------------:|---------:|-------|
| At Cost | X | X | 0 | 0% | Break-even point |
| 10% Margin | X | X | X | 10% | Conservative bid |
| 15% Margin | X | X | X | 15% | Standard target |
| 20% Margin | X | X | X | 20% | Premium bid |
| Match Portfolio Avg | X | X | X | X% | Aligned with current performance |
| Competitive (5%) | X | X | X | 5% | Aggressive to win |

### 9. Save Output

Save the full analysis as `Portfolio_Analysis_Report.md` in the user's folder.

**Include source references throughout:**
- Every scope match references the RFP source document (📄 [filename, page])
- Every project/proposal reference includes the source file and row: 📄 [all-projects.xlsx, Row X] or 📄 [proposals.xlsx, Row X]
- Include a Source Document Index at the top

### 10. Present Summary

Present a concise summary to the user highlighting:
- Key scope matches (strong/weak areas)
- Win rate and pipeline health
- Top profitability insights
- Recommended bid strategy based on the scenarios

$ARGUMENTS
