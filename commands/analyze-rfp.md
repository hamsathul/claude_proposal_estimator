---
description: Analyze client RFP/BOQ files and extract scope by discipline
allowed-tools: Read, Glob, Grep, Bash, Task, TodoWrite
---

Analyze the client RFP/BOQ package to extract a structured scope of work breakdown.

## Steps

### 0. Verify Setup Data

Before starting analysis, check the `setup/` folder at `${CLAUDE_PLUGIN_ROOT}/setup/` for the following required files. **Ask the user to provide any missing files before proceeding.**

| File | Purpose | Required For This Command |
|------|---------|--------------------------|
| `org_capabilities.md` | Company profile, disciplines, sector experience, certifications | Compliance context during extraction |

**Check procedure:**
1. Look for `org_capabilities.md` (not the `_template` version) in `setup/`. If only the template exists, tell the user: *"Please fill in your company details using `org_capabilities_template.md` as a guide and save as `org_capabilities.md` in the setup/ folder."*
2. If the file exists but still contains `<!-- FILL IN -->` placeholders in critical sections (Sections 1-2), warn the user that compliance checking will be limited.
3. List all RFP/BOQ files found in the user's working folder. If none are found, ask the user to place their RFP documents in the folder.

**Do not proceed until at least the RFP documents are present.**

### 1. Read Skill

First, read the `rfp-analysis` skill from `${CLAUDE_PLUGIN_ROOT}/skills/rfp-analysis/SKILL.md` to understand the extraction framework.

### 2. Identify RFP Documents

List all files in the user's selected folder. Identify PDFs, Excel files, and Word documents.

### 3. Analyze Files

Read and analyze each file, **tracking source references for every extracted item**:
   - For PDFs: Read in 20-page batches, focus on Scope of Work, Terms of Reference, and Annexures. **Record the file name and page number for every piece of data extracted.**
   - For Excel (.xlsx): Use Python with openpyxl/pandas to extract BOQ tables, staffing tables, clarifications. **Record the file name, sheet name, and row number for every item.**
   - For Word (.docx): Read with pandoc or python-docx for BOQ narratives and scope details. **Record the file name and section/page for every item.**

### 4. Extract and Organize

Extract and organize into the framework defined in the skill. **Every item must carry its source reference** (see the Source Referencing section in the skill):
   - Project Overview (client, title, location, duration, deadline) — with source references
   - Consultancy Stages with durations — with source references
   - Scope by Discipline (each department's design and site responsibilities) — with source references
   - Staffing Requirements (positions, quantities, man-months, qualifications) — with a **Source column** in every table
   - Commercial Terms (payment, penalties, liability, warranty) — with a **Source column** in every table
   - Compliance Requirements — with source references

### 5. Present Findings

Present findings as a structured summary. Use tables for staffing **(include a Source column in every table)**. Flag ambiguities **with references to the conflicting sources**.

### 6. Save Output

Save the analysis as a markdown file in the user's folder named `RFP_Analysis_Summary.md`. **Include a Source Document Index at the top** listing every file analyzed, its type, page/sheet count, and key content areas.

If the user provides a specific file path as an argument, analyze only that file: $ARGUMENTS
