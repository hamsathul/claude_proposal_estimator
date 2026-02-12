---
name: rfp-analysis
description: >
  This skill should be used when the user asks to "analyze an RFP", "review a BOQ",
  "extract scope of work", "what does this tender require", "break down this RFP",
  "parse this proposal request", or needs to understand the staffing, disciplines,
  and deliverables required by a client tender document for engineering consultancy
  projects (water, power, infrastructure, MEP, structural, electrical).
version: 0.1.0
---

# RFP / BOQ Analysis for Engineering Consultancy

Analyze client Request for Proposal (RFP) and Bill of Quantities (BOQ) packages to extract structured scope-of-work data organized by engineering discipline.

## Input Processing

Accept client documents in any combination of formats:

- **PDF files**: Scope of Work, Terms of Reference, General/Special Conditions
- **Excel files (.xlsx)**: BOQs, price schedules, staffing tables, clarifications (PBC documents)
- **Word files (.docx)**: BOQ narratives, scope descriptions, compliance statements

### Reading Strategy

1. List all files in the user's selected folder or uploaded files
2. Identify file types and read in this priority order:
   - PDF SOW/TOR documents first (they contain the master scope)
   - Excel BOQ/pricing files second (they contain quantities and staffing)
   - Word documents third (supplementary scope details)
   - Excel PBC/clarification files last (amendments to the above)
3. For large PDFs (60+ pages), read in 20-page batches

## Extraction Framework

For each document, extract and organize into these categories:

### 1. Project Overview
- Client name and entity
- Project title and reference number
- Location and site details
- Project sections/lots (pipelines, substations, tanks, buildings, etc.)
- Contract type (EPC, design-build, PMC, etc.)
- Overall duration and key milestones
- Submission deadline

### 2. Consultancy Stages
Identify the service stages, typically:
- Conceptual/Preliminary Design
- Detailed Design / Tender Document Preparation
- Tendering and Bid Evaluation
- Contract Administration and Design Review
- Site Supervision and Testing/Commissioning
- Warranty/Defects Liability Period

### 3. Scope by Discipline
For each engineering discipline found, extract:
- Design phase responsibilities
- Site supervision responsibilities
- Specific deliverables and submittals
- Standards and codes referenced
- Testing and commissioning requirements

Map to standard discipline categories:
- **Project Management** (PM, coordination, scheduling, reporting)
- **Civil Engineering** (earthworks, roads, drainage, external works)
- **Structural Engineering** (foundations, RCC structures, steel)
- **Mechanical Engineering** (piping, HVAC, fire protection, equipment)
- **Hydraulic Engineering** (flow analysis, pump sizing, surge analysis)
- **Electrical Engineering** (power distribution, earthing, lighting)
- **Instrumentation & Control / SCADA** (I&C, telemetry, fiber optic)
- **Architectural** (building design, finishes, landscaping)
- **MEP** (combined mechanical/electrical/plumbing for buildings)
- **HSE** (health, safety, environment monitoring)
- **QA/QC** (quality management, inspection, testing)
- **Commercial / Quantity Surveying** (BOQ, payments, variations, claims)
- **Planning & Scheduling** (master schedule, progress tracking)

### 4. Staffing Requirements
Extract from BOQ and annexures:
- Position titles and quantities
- Duration in man-months for each position
- Minimum qualifications (degree, certifications)
- Minimum experience (years total, years in similar)
- Whether design/home-office or site-based

### 5. Commercial Terms
- Payment structure and milestones
- Pricing basis (lump sum vs. man-month vs. hourly)
- Penalty/LD clauses and rates
- Liability caps
- Insurance requirements
- Bank guarantee requirements
- Warranty period

### 6. Compliance Requirements
- Required certifications (ISO, ADM classification, etc.)
- NOC and approval responsibilities
- Mandatory local presence (UAE office, staff based in-country)
- Past project reference requirements

## Source Referencing (MANDATORY)

Every extracted item MUST include a source reference linking it back to the original document. This ensures full traceability from analysis through to the final proposal.

### Reference Format

Use this inline format for every extracted data point:

```
📄 [filename.pdf, p.12]
```

For Excel files, reference the sheet name and row:
```
📄 [BOQ_Schedule.xlsx, Sheet "Staffing", Row 15]
```

For Word documents, reference the section or page:
```
📄 [Scope_of_Work.docx, Section 3.2, p.8]
```

### Tracking Rules

1. **During extraction**: As you read each file, record the source file name and page number (for PDFs), sheet name and row (for Excel), or section/page (for Word) for every piece of information extracted.
2. **In tables**: Add a **Source** column to all tables (staffing, compliance, commercial terms, etc.) containing the file reference.
3. **In narrative sections**: Append the reference inline after the relevant statement, e.g.:
   > The project duration is 36 months 📄 [TOR_Document.pdf, p.5]
4. **Multiple sources**: When an item is confirmed across multiple documents, list all references:
   > 📄 [TOR_Document.pdf, p.5] [BOQ_Summary.xlsx, Sheet "Overview", Row 3]
5. **Ambiguous items**: When information is unclear or contradictory between sources, flag both references so the user can verify.

### Reference in Staffing Tables

All staffing requirement tables must include a Source column:

| # | Position | Qty | Man-Months | Qualifications | Source |
|---|----------|-----|------------|----------------|--------|
| 1 | Project Manager | 1 | 36 | BSc + 15yr exp | 📄 [BOQ.xlsx, Sheet "Staff", Row 5] |
| 2 | Senior Civil Eng | 2 | 24 | BSc + 10yr exp | 📄 [TOR.pdf, p.23] |

### Reference in Commercial Terms

| Term | Details | Source |
|------|---------|--------|
| Payment | Monthly on timesheet | 📄 [Conditions.pdf, p.14] |
| LD Rate | 0.5% per week, max 10% | 📄 [Conditions.pdf, p.18] |

### Saving the Source Index

When saving the `RFP_Analysis_Summary.md`, include a **Source Document Index** at the top listing all documents analyzed:

```markdown
## Source Document Index
| # | File Name | Type | Pages/Sheets | Key Content |
|---|-----------|------|--------------|-------------|
| 1 | TOR_Document.pdf | PDF | 45 pages | Scope of Work, Terms of Reference |
| 2 | BOQ_Schedule.xlsx | Excel | 3 sheets | Staffing table, BOQ pricing |
| 3 | Special_Conditions.docx | Word | 12 pages | Commercial terms, compliance |
```

This index allows the user to quickly locate the original source documents for verification.

## Output Format

Present findings as a structured summary with clear sections. Use tables for staffing requirements (including Source column). Flag any ambiguous or missing items that need clarification, always referencing the source document and location.

## Organization Capabilities

When performing compliance checks or gap analysis, read the company's organization capabilities profile. Look for `org_capabilities.*` or `company_profile.*` in the user's folder or the `setup/` folder. If not found, provide the template from `${CLAUDE_PLUGIN_ROOT}/setup/org_capabilities_template.md` for the user to fill in.

The capabilities profile provides:
- **Discipline coverage** — which disciplines the company delivers as core, capable, via partners, or not at all
- **Sector experience** — completed projects by sector for reference matching
- **Certifications** — ISO, ADM, and other accreditations for compliance verification
- **Subcontractor network** — pre-qualified partners for non-core disciplines
- **Current capacity** — bench staff, active projects, mobilization timelines
- **Company differentiators** — strengths for proposal content

## Reference Material

For detailed rate mapping and staff categorization patterns, read:
- `references/discipline-mapping.md` — standard discipline-to-role mapping
- `references/qualification-matrix.md` — typical qualification requirements by role level
- `${CLAUDE_PLUGIN_ROOT}/setup/org_capabilities_template.md` — organization capabilities profile template
