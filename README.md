# Proposal Estimator

Analyze client RFP/BOQ packages, assess staffing gaps against company capabilities, and generate complete commercial proposals with pricing estimates for engineering consultancy services.

## What It Does

This plugin automates the end-to-end proposal workflow for engineering consultancy firms bidding on infrastructure projects (water, power, roads, buildings) in the GCC region.

**Every extracted item is traced back to its source** — file name, page number, sheet, and row — so you can verify any finding against the original RFP documents.

**Workflow:**

1. **Analyze RFP** — Parse client documents (PDF, Excel, Word) and extract scope by discipline, staffing requirements, and commercial terms — **with source references for every item**
2. **Gap Analysis** — Compare RFP requirements against your staff list to identify what's available in-house, what needs hiring, and what should be subcontracted — **with RFP source traceability**
3. **Estimate Proposal** — Apply your rate card to calculate pricing with Excel formulas, grouped by consultancy stage — **with a Source Traceability sheet linking costs to RFP requirements**
4. **Generate Proposal** — Create a complete proposal package: Excel pricing workbook, Word report, and PowerPoint presentation — **with source references in documents and speaker notes**

## Commands

| Command | Description |
|---------|-------------|
| `/analyze-rfp` | Reads all client documents and produces a structured RFP Analysis Summary |
| `/gap-analysis` | Compares RFP staffing needs vs. company capabilities |
| `/estimate-proposal` | Calculates proposal pricing using your rate card |
| `/generate-proposal` | Creates the full proposal package (Excel + Word + PowerPoint) |
| `/portfolio-analysis` | Cross-references RFP scope against your project portfolio and proposal pipeline — win rates, financials, and profitability scenarios |

## Setup

**Every command will check for required setup data before starting and will ask you to provide any missing files.** Prepare your data in the `setup/` folder using the provided templates as a guide.

### Required Setup Files

| File to Create | Template Provided | Description |
|---------------|-------------------|-------------|
| `setup/org_capabilities.md` | `org_capabilities_template.md` | Copy the template, fill in your company profile, disciplines, certifications, project references, and differentiators |
| `setup/rate_card.csv` | (includes sample rates) | Replace sample rates with your actual billing rates by staff category (hourly, AED) |
| `setup/staff_list.csv` | (includes sample staff) | Replace sample staff with your actual team — roles, disciplines, experience, availability |

### Data Files (Provide Your Own Exports)

| File to Provide | Template for Format | Description |
|----------------|--------------------| -------------|
| Projects export (`.xlsx`) | `projects_template.xlsx` | Export from your PM system — all current/past projects with contract values, actual costs, profit %, hours, and status |
| Proposals export (`.xlsx`) | `proposals_template.xlsx` | Export from your BD system — all submitted proposals with estimated values, clients, win/loss status, and dates |

Place the projects and proposals export files in your working folder alongside the RFP documents. The plugin will prompt you for them when needed.

### Setup Verification

Each command runs a **Step 0: Verify Setup Data** check before doing any work. If a required file is missing or still contains only sample/template data, the command will stop and tell you exactly what's needed. You won't lose any progress — just provide the file and re-run the command.

### Organization Capabilities Profile

Drives the compliance check, company overview in proposals, and "Why Choose Us" content. Covers company profile, discipline capabilities (CORE / CAPABLE / PARTNER / NONE), sector experience, project references, certifications, software, subcontractor network, capacity, and differentiators.

### Rate Card

Your company billing rates. Example columns:

| Category_Code | Category_Name | Department | Hourly_Rate_AED |
|---------------|---------------|------------|-----------------|
| PM-SE | PM Senior Engineer | Project Management | 750 |
| PM-E | PM Engineer | Project Management | 400 |
| ENG-SE | Senior Engineer | Engineering | 250 |
| ENG-E | Engineer | Engineering | 200 |
| ENG-JE | Junior Engineer | Engineering | 100 |
| ENG-SD | Senior Designer | Engineering | 120 |
| ENG-D | Designer | Engineering | 100 |

### Staff List

Your current team and availability. Example columns:

| Name | Current_Role | Discipline | Experience_Years | Availability |
|------|-------------|------------|-----------------|--------------|
| Ahmed Khan | Senior PM | Project Management | 18 | Available Apr 2026 |
| John Smith | Senior Civil Eng | Civil | 14 | Available |

### Projects Export

An export of all your current and past projects. The file should have a header row containing "Project Number" as the first column (the header row can be preceded by a summary section). Required columns:

| Column | Description |
|--------|-------------|
| Project Number | Unique project identifier |
| Project Name | Full project name |
| Client | Client company name |
| Type | Contract type (LS = Lump Sum, RB = Rate-Based) |
| Contract Value (AED) | Total contract value |
| Actual Costs (AED) | Costs incurred to date |
| Forecast Costs (AED) | Projected total costs |
| Contract Hours | Budgeted hours |
| Actual Hours | Hours worked to date |
| Invoiced Amount (AED) | Amount billed to client |
| Expected Result (AED) | Expected profit/loss |
| Profit % | Profit as percentage of contract value |
| Status | Project status (in-progress, completed, blocked, draft, cancelled) |
| Project Manager | Name of PM |

### Proposals Export

An export of all submitted and tracked proposals. Required columns:

| Column | Description |
|--------|-------------|
| Proposal No | Unique proposal identifier |
| Project Title | Full project/tender title |
| Client | Client company name |
| End User | Ultimate end user (if different from client) |
| Type | Contract type (LS, RB, or RB and LS) |
| Status | Proposal status (Submitted, Won, Lost, Cancelled, To Be Submitted, Resubmitted) |
| Final Estimated Value | Estimated contract value in AED |
| Currency | Currency code (default: AED) |
| Received Date | Date RFP was received |
| Submission Date | Date proposal was submitted |
| Proposal Manager | Name of proposal lead |
| Lead Discipline Engineer | Name of technical lead |

## Typical Usage

1. **Prepare setup data** — Fill in `setup/org_capabilities.md`, `setup/rate_card.csv`, and `setup/staff_list.csv` with your company's actual data (each command will verify these exist before starting)
2. **Place RFP documents** — Put the client's RFP package (PDFs, Excel BOQs, Word files) in your working folder
3. **Place data exports** — Add your projects and proposals Excel exports to the working folder (needed for `/portfolio-analysis`)
4. Run `/analyze-rfp` to extract scope requirements
5. Run `/gap-analysis` to check team coverage, compliance, and subcontractor strategy
6. Run `/estimate-proposal` to calculate pricing
7. Run `/generate-proposal` to create the full proposal package (Excel + Word + PowerPoint)
8. Run `/portfolio-analysis` to see how this RFP aligns with your existing portfolio, check win rates, and model profitability scenarios

## Rate Categories

The plugin maps positions to these standard rate categories:

- **PM-SE**: Project Managers, Design Leads, PMC Directors
- **PM-E**: Planning Engineers, Project Coordinators
- **ENG-SE**: Senior discipline engineers (Civil, Structural, Mechanical, Electrical, I&C)
- **ENG-E**: Resident Engineers, HSE Engineers, QA/QC Engineers, Quantity Surveyors
- **ENG-JE**: Assistant Engineers, HSE Officers, Junior Inspectors
- **ENG-SD**: Lead CAD/BIM Designers
- **ENG-D**: CAD Operators, Draftsmen

## Pricing Method

- Monthly rate = Hourly rate x 176 hours (22 days x 8 hours)
- Rates are all-inclusive billing rates (no additional overhead/profit markup)
- VAT at 5% per UAE Federal Tax Authority
- Third-party positions can include a 10-15% markup

## Source Referencing

All outputs include full traceability back to the original RFP documents. The reference format is:

- **PDFs**: `📄 [filename.pdf, p.12]`
- **Excel**: `📄 [filename.xlsx, Sheet "Staff", Row 15]`
- **Word**: `📄 [filename.docx, Section 3.2, p.8]`

Every analysis output includes a **Source Document Index** at the top, listing all files analyzed. Every table includes a **Source column**. The Excel pricing workbook includes a dedicated **Source Traceability** sheet. Word reports include a **Source Reference Index appendix**. PowerPoint presentations carry source references in **speaker notes**.

## Author

NCO
