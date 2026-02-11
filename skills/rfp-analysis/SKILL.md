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

## Output Format

Present findings as a structured summary with clear sections. Use tables for staffing requirements. Flag any ambiguous or missing items that need clarification.

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
