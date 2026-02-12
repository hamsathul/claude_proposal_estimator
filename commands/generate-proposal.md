---
description: Generate complete proposal package (Excel + Word + PowerPoint)
allowed-tools: Read, Glob, Grep, Bash, Task, TodoWrite, Write, Edit
---

Generate a complete commercial proposal package including Excel pricing, Word report, and PowerPoint presentation.

## Step 0: Verify Setup Data

Before generating the proposal package, check that ALL required setup and analysis files are available. **Ask the user to provide any missing files before proceeding.**

### Setup Files (in `${CLAUDE_PLUGIN_ROOT}/setup/`)

| File | Purpose | Required |
|------|---------|----------|
| `org_capabilities.md` | Company profile, project references, certifications, differentiators | **Yes** |
| `rate_card.csv` | Rate categories for pricing context | **Yes** |
| `staff_list.csv` | Team data for staffing sections | **Yes** |

**Check procedure for setup files:**
1. Look for `org_capabilities.md` (not the `_template` version) in `setup/`. If only the template exists, tell the user: *"Please fill in your company details using `org_capabilities_template.md` as a guide and save as `org_capabilities.md` in the setup/ folder."*
2. Look for `staff_list.csv` in `setup/`. If it only contains sample data, tell the user to replace it with their actual team data.
3. Look for `rate_card.csv` in `setup/`. If it only contains sample rates, tell the user to update with actual billing rates.

### Analysis Files (in user's working folder)

| File | Source Command | Required |
|------|---------------|----------|
| `RFP_Analysis_Summary.md` | `/analyze-rfp` | **Yes** |
| `Gap_Analysis_Report.md` | `/gap-analysis` | **Yes** |
| `Proposal_Cost_Estimate_AED.xlsx` | `/estimate-proposal` | **Yes** |

**Check procedure for analysis files:**
If any analysis file is missing, inform the user which command to run first (e.g., *"Please run `/analyze-rfp` first to generate the RFP Analysis Summary."*).

**Do not proceed until all 6 files above are available.**

## Steps

### 1. Excel Pricing (if not already created)
If `Proposal_Cost_Estimate_AED.xlsx` does not exist, run the estimate-proposal workflow first.

### 2. Word Report
Use the docx skill to create a professional proposal document. Read the docx SKILL.md first.

Include these sections:
- **Cover Page**: Company name, project title, client name, RFP reference, date (from org capabilities profile)
- **Executive Summary**: 1-page overview of the proposal approach
- **Company Profile**: Company background, years in business, office locations, key certifications (from org capabilities Sections 1, 5)
- **Relevant Experience**: 3-5 most relevant project references matching the RFP's sector and scope (from org capabilities Section 4)
- **Understanding of Scope**: Summarize the project sections and key requirements. **Include source references as footnotes** linking each scope item to the original RFP document and page.
- **Scope of Work by Discipline**: Detailed breakdown from the RFP analysis, organized by department with bullet points. **Each discipline section should note the source document and page** where the scope was defined.
- **Project Team & Organization**: Staffing plan showing each position, qualifications, and whether in-house or to be recruited. **Include a "Ref" column** in the staffing table linking each position to the RFP requirement source.
- **Gap Analysis Summary**: Available vs. new hire vs. third-party (table format) **with RFP Source column**
- **Compliance Matrix**: Certification and requirement compliance check (from gap analysis) **with RFP Source column**
- **Methodology**: High-level approach for each consultancy stage
- **Software & Tools**: Key software available for project delivery (from org capabilities Section 6)
- **Schedule**: Key milestones and durations
- **Commercial Summary**: Stage-by-stage pricing summary table (from the Excel)
- **Compliance Confirmation**: Statement of compliance with key tender requirements
- **Appendix: Source Reference Index** — A table listing every RFP document analyzed, with file name, type, page/sheet count, and key content areas. This allows the client or internal reviewers to trace any proposal item back to the original RFP material.

Save as `Commercial_Proposal_Report.docx` in the user's folder.

### 3. PowerPoint Presentation
Use the pptx skill to create a proposal presentation. Read the pptx SKILL.md first.

Create 10-15 slides covering:
- Title slide (project name, company name, date)
- Company overview — years established, offices, headcount, certifications (from org capabilities Sections 1, 5)
- Sector experience — relevant completed projects with value and scope (from org capabilities Section 4)
- Project understanding (scope summary from RFP analysis) — **include small-text source references** (e.g., "Ref: TOR.pdf, p.5") in speaker notes or as subtle footnotes on the slide
- Our approach (methodology per stage)
- Team structure (org chart or staffing table) — **note source references in speaker notes** for each position's RFP requirement
- Staffing capability (in-house vs. hire vs. subcontract from gap analysis)
- Discipline coverage — capability levels per discipline (from org capabilities Section 2)
- Software & tools — key platforms for delivery (from org capabilities Section 6)
- Key deliverables and schedule
- Commercial summary (pricing by stage)
- Why choose us — company differentiators (from org capabilities Section 9)
- Contact information

**Source referencing in presentations**: Since slides should stay clean, put detailed source references in the **speaker notes** for each slide. In the speaker notes, list all RFP source documents and pages that informed the slide content. This lets the presenter trace claims back to the RFP during Q&A.

Save as `Proposal_Presentation.pptx` in the user's folder.

### 4. Summary
List all generated files with links and briefly describe each deliverable.

$ARGUMENTS
