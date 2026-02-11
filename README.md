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

Before using the plugin, prepare three files in the `setup/` folder:

| File | Description |
|------|-------------|
| `setup/org_capabilities.md` | Company profile, discipline capabilities, certifications, project references, and differentiators |
| `setup/rate_card.csv` | Billing rates by staff category (hourly rates in AED) |
| `setup/staff_list.csv` | Current team members, disciplines, experience, and availability |

Each file includes a ready-to-fill template. Open them in the `setup/` folder and replace the sample data with your company's details.

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

## Typical Usage

1. Place the client's RFP documents (PDFs, Excel BOQs, Word files) in your folder
2. Fill in your company data in the `setup/` folder (`org_capabilities.md`, `rate_card.csv`, `staff_list.csv`)
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
