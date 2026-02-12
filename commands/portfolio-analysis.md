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

### 3. Scope Matching (with Source References)

If an RFP Analysis Summary is available, extract the scope disciplines and match against existing projects and proposals:

For each **discipline/scope area** in the RFP:
- Find **projects** where the Project Name or Description contains matching keywords (e.g., "substation", "pipeline", "civil", "MEP", "HVAC")
- Find **proposals** where the Project Title contains matching keywords
- **Include the source reference** from the RFP Analysis Summary for each scope item (📄 [filename, page])

Present as a **Scope Coverage Matrix**:

| # | RFP Scope Area | RFP Source | Matching Projects | Matching Proposals | Experience Level |
|---|---------------|------------|-------------------|--------------------|-----------------|
| 1 | 132kV Substation Design | 📄 [TOR.pdf, p.8] | P19860102 (completed), P19860017 (in-progress) | T-008-1-2026 (Submitted) | STRONG — 3+ references |
| 2 | SCADA/Telemetry | 📄 [TOR.pdf, p.15] | None | T-475-2025 (Won) | LIMITED — 1 reference |

**Experience Level** classification:
- **STRONG**: 3+ matching projects/proposals
- **MODERATE**: 1-2 matching projects/proposals
- **LIMITED**: Only proposals, no completed projects
- **NONE**: No matching projects or proposals found

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
