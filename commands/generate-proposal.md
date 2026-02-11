---
description: Generate complete proposal package (Excel + Word + PowerPoint)
allowed-tools: Read, Glob, Grep, Bash, Task, TodoWrite, Write, Edit
---

Generate a complete commercial proposal package including Excel pricing, Word report, and PowerPoint presentation.

## Prerequisites

This command builds on previous outputs. Check that these exist in the user's folder:
- `RFP_Analysis_Summary.md` (from /analyze-rfp)
- `Gap_Analysis_Report.md` (from /gap-analysis)
- `Proposal_Cost_Estimate_AED.xlsx` (from /estimate-proposal)
- Organization capabilities file (`org_capabilities.*` or `company_profile.*`) — for company overview, project references, certifications, and differentiators

If analysis files are missing, inform the user which commands to run first. If the capabilities file is missing, ask the user or use the template at `${CLAUDE_PLUGIN_ROOT}/setup/org_capabilities_template.md`.

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
- **Understanding of Scope**: Summarize the project sections and key requirements
- **Scope of Work by Discipline**: Detailed breakdown from the RFP analysis, organized by department with bullet points
- **Project Team & Organization**: Staffing plan showing each position, qualifications, and whether in-house or to be recruited
- **Gap Analysis Summary**: Available vs. new hire vs. third-party (table format)
- **Compliance Matrix**: Certification and requirement compliance check (from gap analysis)
- **Methodology**: High-level approach for each consultancy stage
- **Software & Tools**: Key software available for project delivery (from org capabilities Section 6)
- **Schedule**: Key milestones and durations
- **Commercial Summary**: Stage-by-stage pricing summary table (from the Excel)
- **Compliance Confirmation**: Statement of compliance with key tender requirements

Save as `Commercial_Proposal_Report.docx` in the user's folder.

### 3. PowerPoint Presentation
Use the pptx skill to create a proposal presentation. Read the pptx SKILL.md first.

Create 10-15 slides covering:
- Title slide (project name, company name, date)
- Company overview — years established, offices, headcount, certifications (from org capabilities Sections 1, 5)
- Sector experience — relevant completed projects with value and scope (from org capabilities Section 4)
- Project understanding (scope summary from RFP analysis)
- Our approach (methodology per stage)
- Team structure (org chart or staffing table)
- Staffing capability (in-house vs. hire vs. subcontract from gap analysis)
- Discipline coverage — capability levels per discipline (from org capabilities Section 2)
- Software & tools — key platforms for delivery (from org capabilities Section 6)
- Key deliverables and schedule
- Commercial summary (pricing by stage)
- Why choose us — company differentiators (from org capabilities Section 9)
- Contact information

Save as `Proposal_Presentation.pptx` in the user's folder.

### 4. Summary
List all generated files with links and briefly describe each deliverable.

$ARGUMENTS
