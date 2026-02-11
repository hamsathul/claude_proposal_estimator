---
description: Analyze client RFP/BOQ files and extract scope by discipline
allowed-tools: Read, Glob, Grep, Bash, Task, TodoWrite
---

Analyze the client RFP/BOQ package to extract a structured scope of work breakdown.

## Steps

1. First, read the `rfp-analysis` skill from `${CLAUDE_PLUGIN_ROOT}/skills/rfp-analysis/SKILL.md` to understand the extraction framework.

2. List all files in the user's selected folder. Identify PDFs, Excel files, and Word documents.

3. Read and analyze each file:
   - For PDFs: Read in 20-page batches, focus on Scope of Work, Terms of Reference, and Annexures
   - For Excel (.xlsx): Use Python with openpyxl/pandas to extract BOQ tables, staffing tables, clarifications
   - For Word (.docx): Read with pandoc or python-docx for BOQ narratives and scope details

4. Extract and organize into the framework defined in the skill:
   - Project Overview (client, title, location, duration, deadline)
   - Consultancy Stages with durations
   - Scope by Discipline (each department's design and site responsibilities)
   - Staffing Requirements (positions, quantities, man-months, qualifications)
   - Commercial Terms (payment, penalties, liability, warranty)
   - Compliance Requirements

5. Present findings as a structured summary. Use tables for staffing. Flag ambiguities.

6. Save the analysis as a markdown file in the user's folder named `RFP_Analysis_Summary.md`.

If the user provides a specific file path as an argument, analyze only that file: $ARGUMENTS
