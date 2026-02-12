---
description: Compare RFP requirements against company staff capabilities
allowed-tools: Read, Glob, Grep, Bash, Task, TodoWrite, Write
---

Compare the staffing requirements from the RFP analysis against the company's current staff capabilities to identify gaps.

## Steps

### 0. Verify Setup Data

Before starting analysis, check the `setup/` folder at `${CLAUDE_PLUGIN_ROOT}/setup/` for the following required files. **Ask the user to provide any missing files before proceeding.**

| File | Purpose | Required |
|------|---------|----------|
| `org_capabilities.md` | Discipline levels, certifications, sector experience, subcontractors, capacity | **Yes** |
| `staff_list.csv` | Current team, disciplines, experience, availability | **Yes** |
| `rate_card.csv` | Rate categories for position classification | Optional (improves category mapping) |

**Check procedure:**
1. Look for `org_capabilities.md` (not the `_template` version) in `setup/`. If only the template exists, tell the user: *"Please fill in your company details using `org_capabilities_template.md` as a guide and save as `org_capabilities.md` in the setup/ folder."*
2. Look for `staff_list.csv` in `setup/`. If it only contains sample data (e.g., "John Smith", "Jane Doe"), tell the user: *"Please replace the sample staff list with your actual team data. See the template format for required columns."*
3. Check the user's working folder for `RFP_Analysis_Summary.md` (from `/analyze-rfp`). If not found, tell the user to run `/analyze-rfp` first.

**Do not proceed until org_capabilities.md, staff_list.csv, and RFP_Analysis_Summary.md are all available.**

### 1. Read RFP Analysis

Read the RFP analysis summary from `RFP_Analysis_Summary.md` in the user's folder.

### 2. Read Organization Capabilities

Read `org_capabilities.md` from `${CLAUDE_PLUGIN_ROOT}/setup/`. Extract:
   - Discipline capability levels (CORE / CAPABLE / PARTNER / NONE)
   - Sector experience (matching the RFP's project sector)
   - Certifications held vs. certifications required by the RFP
   - Current capacity (bench staff, active projects, mobilization lead time)
   - Pre-qualified subcontractors for non-core disciplines
   - Software and tools availability vs. RFP requirements
   - Limitations and constraints that may affect the bid

### 3. Read Staff List

Read `staff_list.csv` from `${CLAUDE_PLUGIN_ROOT}/setup/`. Expected columns (flexible — adapt to whatever format is provided):
   - Name / Employee ID
   - Current Role / Title
   - Discipline (Civil, Mechanical, Electrical, etc.)
   - Experience (years)
   - Qualifications (degree, certifications)
   - Current Assignment / Availability
   - Location (UAE-based or not)

### 4. Classify Positions

For each position required by the RFP, classify as:

   **IN-HOUSE (Available)**: Staff member exists with matching discipline, sufficient experience, required qualifications, and is available or can be freed for this project.

   **IN-HOUSE (Needs Upskilling)**: Staff exists but may lack specific certification, experience level, or regional exposure. Note what gap exists.

   **NEW HIRE**: No matching staff available. Position can reasonably be recruited within the mobilization timeline. Note the hiring lead time needed.

   **THIRD-PARTY / SUBCONTRACT**: Specialized role that is not core to the company, or where hiring is not feasible in time. Note recommended subcontract approach.

   **For each position, carry forward the source reference from the RFP Analysis Summary** so the user can trace every requirement back to the original document.

### 5. Gap Analysis Table

Present a gap analysis table **with a Source column** referencing the original RFP document location:

   | # | Position Required | Qty | Category | Status | Matched Staff / Action | Notes | RFP Source |
   |---|---|---|---|---|---|---|---|
   | 1 | Project Manager | 1 | PM-SE | IN-HOUSE | [Name] | Available from [date] | 📄 [BOQ.xlsx, Sheet "Staff", Row 5] |
   | 2 | Senior Structural Eng. | 1 | ENG-SE | NEW HIRE | Recruit | 4-6 weeks lead time | 📄 [TOR.pdf, p.23] |
   | ... | ... | ... | ... | ... | ... | ... | ... |

### 6. Summary Count

Provide a summary count:
   - Total positions required: X
   - In-house available: X
   - In-house (needs upskilling): X
   - New hire required: X
   - Third-party/subcontract: X

### 7. Compliance Check

**Compliance Check** — Compare organization capabilities against RFP requirements. **Include source references for every RFP requirement**:
   - Required certifications (ISO, ~~classification_authority) vs. held certifications
   - Required sector experience vs. completed projects
   - Required software/tools vs. available licenses
   - Minimum company classification vs. current classification
   - UAE office / local presence requirement vs. actual office locations
   - Insurance and bonding limits vs. contract requirements

   Present as a compliance matrix **with an RFP Source column**:

   | Requirement | RFP Asks | Company Has | Status | RFP Source |
   |-------------|----------|-------------|--------|------------|
   | ISO 9001 | Required | Yes, expires Dec 2026 | COMPLIANT | 📄 [TOR.pdf, p.8] |
   | Water sector experience | 5 projects min | 8 projects completed | COMPLIANT | 📄 [TOR.pdf, p.12] |
   | ~~classification_authority Category A | Required | Category B | GAP — needs upgrade | 📄 [Special_Conditions.docx, Section 2.1] |

### 8. Subcontractor Strategy

**Subcontractor Strategy** — For disciplines marked as PARTNER or NONE in the capabilities profile:
   - Identify which positions can be covered by pre-qualified subcontractors
   - Flag disciplines where no subcontractor relationship exists (risk)
   - Note onboarding timelines for subcontractor mobilization

### 9. Risk Flags

Flag any risks:
   - Positions where the client requires interviews (all staff subject to client approval)
   - Positions requiring specific ~~region experience
   - Mobilization timeline constraints
   - UAE office requirement compliance

### 10. Save Output

Save the gap analysis as `Gap_Analysis_Report.md` in the user's folder. **Carry forward the Source Document Index from the RFP Analysis Summary** and add any additional source files referenced (e.g., staff list, org capabilities).

$ARGUMENTS
