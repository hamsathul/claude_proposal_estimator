# Proposal Estimator

Analyze client RFP/BOQ packages, assess staffing gaps against company capabilities, and generate complete commercial proposals with pricing estimates for engineering consultancy services.

## What It Does

This plugin automates the end-to-end proposal workflow for engineering consultancy firms bidding on infrastructure projects (water, power, roads, buildings) in the GCC region.

**Workflow:**

1. **Analyze RFP** — Parse client documents (PDF, Excel, Word) and extract scope by discipline, staffing requirements, and commercial terms
2. **Gap Analysis** — Compare RFP requirements against your staff list to identify what's available in-house, what needs hiring, and what should be subcontracted
3. **Estimate Proposal** — Apply your rate card to calculate pricing with Excel formulas, grouped by consultancy stage
4. **Generate Proposal** — Create a complete proposal package: Excel pricing workbook, Word report, and PowerPoint presentation

## Commands

| Command | Description |
|---------|-------------|
| `/analyze-rfp` | Reads all client documents and produces a structured RFP Analysis Summary |
| `/gap-analysis` | Compares RFP staffing needs vs. company capabilities |
| `/estimate-proposal` | Calculates proposal pricing using your rate card |
| `/generate-proposal` | Creates the full proposal package (Excel + Word + PowerPoint) |

## Setup

Before using the plugin, prepare three files in your working folder:

### Organization Capabilities Profile (Markdown)

Your company's overall capabilities, experience, and certifications. This file drives the compliance check, company overview in proposals, and "Why Choose Us" content. It covers:

- Company profile (name, offices, headcount, classification)
- Discipline capabilities rated as CORE / CAPABLE / PARTNER / NONE
- Sector experience with completed project counts
- Key project references (5-10 relevant projects for proposals)
- Certifications and accreditations (ISO, ADM, etc.)
- Software and tools inventory
- Pre-qualified subcontractor network
- Current capacity (active projects, bench staff, mobilization timelines)
- Company differentiators and known limitations

A comprehensive template is included at `skills/rfp-analysis/templates/org_capabilities_template.md`. Copy it to your folder as `org_capabilities.md` and fill in your details.

### Rate Card (CSV or Excel)

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

A template is included in the plugin at `skills/rfp-analysis/templates/rate_card_template.csv`.

### Staff List (CSV or Excel)

Your current team and availability. Example columns:

| Name | Current_Role | Discipline | Experience_Years | Availability |
|------|-------------|------------|-----------------|--------------|
| Ahmed Khan | Senior PM | Project Management | 18 | Available Apr 2026 |
| John Smith | Senior Civil Eng | Civil | 14 | Available |

A template is included at `skills/rfp-analysis/templates/staff_list_template.csv`.

## Typical Usage

1. Place the client's RFP documents (PDFs, Excel BOQs, Word files) in your folder
2. Add your `org_capabilities.md`, `rate_card.csv`, and `staff_list.csv` to the same folder
3. Run `/analyze-rfp` to extract scope requirements
4. Run `/gap-analysis` to check team coverage, compliance, and subcontractor strategy
5. Run `/estimate-proposal` to calculate pricing
6. Run `/generate-proposal` to create the full proposal package (with company profile, references, and differentiators pulled from capabilities)

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

## Author

NCO
