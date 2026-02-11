---
description: Compare RFP requirements against company staff capabilities
allowed-tools: Read, Glob, Grep, Bash, Task, TodoWrite, Write
---

Compare the staffing requirements from the RFP analysis against the company's current staff capabilities to identify gaps.

## Steps

1. Read the RFP analysis summary. Look for `RFP_Analysis_Summary.md` in the user's folder, or ask the user to point to it.

2. Read the company's **organization capabilities profile**. Look for files named like `org_capabilities.*`, `company_profile.*`, or `capabilities.*` in the user's folder (Markdown, Excel, CSV, or any format). If not found, ask the user. The template is at `${CLAUDE_PLUGIN_ROOT}/skills/rfp-analysis/templates/org_capabilities_template.md`.

   From the capabilities file, extract:
   - Discipline capability levels (CORE / CAPABLE / PARTNER / NONE)
   - Sector experience (matching the RFP's project sector)
   - Certifications held vs. certifications required by the RFP
   - Current capacity (bench staff, active projects, mobilization lead time)
   - Pre-qualified subcontractors for non-core disciplines
   - Software and tools availability vs. RFP requirements
   - Limitations and constraints that may affect the bid

3. Read the company's **staff list**. Look for files named like `staff_list.*`, `team.*`, or `resources.*` in the user's folder (Excel, CSV, or any format). If not found, ask the user where their staff list is.

   Expected staff file columns (flexible — adapt to whatever format is provided):
   - Name / Employee ID
   - Current Role / Title
   - Discipline (Civil, Mechanical, Electrical, etc.)
   - Experience (years)
   - Qualifications (degree, certifications)
   - Current Assignment / Availability
   - Location (UAE-based or not)

4. For each position required by the RFP, classify as:

   **IN-HOUSE (Available)**: Staff member exists with matching discipline, sufficient experience, required qualifications, and is available or can be freed for this project.

   **IN-HOUSE (Needs Upskilling)**: Staff exists but may lack specific certification, experience level, or regional exposure. Note what gap exists.

   **NEW HIRE**: No matching staff available. Position can reasonably be recruited within the mobilization timeline. Note the hiring lead time needed.

   **THIRD-PARTY / SUBCONTRACT**: Specialized role that is not core to the company, or where hiring is not feasible in time. Note recommended subcontract approach.

5. Present a gap analysis table:

   | # | Position Required | Qty | Category | Status | Matched Staff / Action | Notes |
   |---|---|---|---|---|---|---|
   | 1 | Project Manager | 1 | PM-SE | IN-HOUSE | [Name] | Available from [date] |
   | 2 | Senior Structural Eng. | 1 | ENG-SE | NEW HIRE | Recruit | 4-6 weeks lead time |
   | ... | ... | ... | ... | ... | ... | ... |

6. Provide a summary count:
   - Total positions required: X
   - In-house available: X
   - In-house (needs upskilling): X
   - New hire required: X
   - Third-party/subcontract: X

7. **Compliance Check** — Compare organization capabilities against RFP requirements:
   - Required certifications (ISO, ADM) vs. held certifications
   - Required sector experience vs. completed projects
   - Required software/tools vs. available licenses
   - Minimum company classification vs. current classification
   - UAE office / local presence requirement vs. actual office locations
   - Insurance and bonding limits vs. contract requirements

   Present as a compliance matrix:

   | Requirement | RFP Asks | Company Has | Status |
   |-------------|----------|-------------|--------|
   | ISO 9001 | Required | Yes, expires Dec 2026 | COMPLIANT |
   | Water sector experience | 5 projects min | 8 projects completed | COMPLIANT |
   | ADM Category A | Required | Category B | GAP — needs upgrade |

8. **Subcontractor Strategy** — For disciplines marked as PARTNER or NONE in the capabilities profile:
   - Identify which positions can be covered by pre-qualified subcontractors
   - Flag disciplines where no subcontractor relationship exists (risk)
   - Note onboarding timelines for subcontractor mobilization

9. Flag any risks:
   - Positions where the client requires interviews (all staff subject to client approval)
   - Positions requiring specific GCC experience
   - Mobilization timeline constraints
   - UAE office requirement compliance

10. Save the gap analysis as `Gap_Analysis_Report.md` in the user's folder.

$ARGUMENTS
