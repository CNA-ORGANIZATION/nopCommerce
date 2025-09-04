# Cline Workflow: Automated Project Setup for Knowledge Management

This document outlines a more streamlined procedure for setting up the `.clinerules`, `memory-bank`, and `clineworkspace` directories. Automating these steps helps Cline build and maintain a persistent knowledge base more efficiently.

## Step 1: Initial Setup Script

This single step replaces the manual creation of directories and `.gitignore` entries.

**Prompt to Cline:**
> "Please perform the initial project setup for knowledge management.
> 1.  Ensure the following directory structure exists at the project root, creating any missing directories one by one to ensure compatibility:
>     ```
>     .clinerules/
>     ├── memory-bank/
>     └── workflows/
>     clineworkspace/
>     ├── designs/
>     └── logs/
>     agent_reports/
>     ```
> 2.  Read the `.gitignore` file. If `.clinerules/`, `clineworkspace/`, and `agent_reports/` are not present, add them to the end of the file."

## Step 2: Import Analysis Reports

This step populates the `agent_reports` directory with existing analysis documents.

**Prompt to Cline:**
> "Please import the analysis reports.
> 1. Ask me for the local directory path where the analysis reports are stored, providing an option to skip this step.
> 2. If a path is provided, copy all files and subdirectories from it into the `agent_reports/` directory."

## Step 3: Import Custom Workflows

This step allows for adding generic or project-specific workflows to the project.

**Prompt to Cline:**
> "Please import any custom workflow files.
> 1. Ask me for the local directory path where the workflow files are stored, providing an option to skip this step.
> 2. If a path is provided, copy all files from it into the `.clinerules/workflows/` directory."

## Step 4: Generate and Populate Project Summaries

This step generates and populates the core knowledge base files after analyzing the repository. This should only be run once for a project, or when major changes have occurred.

**Prompt to Cline:**
> "Analyze the entire repository and generate the core project summaries.
> 1. Check if the following files exist first, and only create the ones that are missing.
> 2. After creating the files, analyze the project (e.g., by reading the README.md, project files, and source code) to populate the content.
>
> **Files to create and populate:**
>
> 1.  **`.clinerules/instructions.md`**: Create this file and populate the following sections:
>     *   **Important Notes**: (Use the standard template text)
>         *   **Analysis Reports:** Must refer this folder and related sub-directories \agent_reports\ for agentic ai reports analysis before responding to any prompt related to the repository. 
>         *   **Limitations:** Must record any failure of task along with its prompt in this file \clineworkspace\logs\limitation_<datetimestamp>.md
>         *   **Prompts Output:**: Must record all successful task's summary along with its prompt in this file \clineworkspace\logs\output_<datetimestamp>.md
>         *   **Old Code Record:** Must keep old code as commented-out with proper tags and regions, while working on the new changes instead of removing or replacing the same.
>         *   **New Code Changes:** Must keep proper comment tags on the new code changes with Jira Ticket Number, User, Functionality and Timestamp detail. When asking for the Jira Ticket Number and User Name, provide a prompt with an option to skip.
>     *   **Scope of the Project**: High-level project goals.
>     *   **Tech Stack**: Key languages, frameworks, and libraries.
>     *   **Best Practices**: Guidelines for coding, error handling, and security.
>
> 2.  **`.clinerules/memory-bank/application_summary.md`**: Create this file and populate it by describing the application's purpose, domain, core functionality, integrations, and business rules.
>
> 3.  **`.clinerules/memory-bank/technical_architecture_summary.md`**: Create this file and populate it by describing the architectural style, technology stack, design patterns, and key risks."

## Step 5: Log Task-Specific Output

This step is for ongoing use during development to log the results of specific tasks.

**Prompt to Cline:**
> "Must log the output of the current task. Create a new file in `clineworkspace/logs/` named `output_<YYYYMMDDHHMMSS>.md`. The file should contain a summary of my request and a log of the actions you took to fulfill it."

## Step 6: Log Workflow Efforts

This final step captures metadata about the execution of this setup workflow.

**Prompt to Cline:**
> "As the final step, please generate and populate the effort log for this workflow.
> 1. Create a file named `clineworkspace/logs/ClineSetupWorkflow_Efforts_<YYYYMMDDHHMMSS>.md`.
> 2. Populate the file with the following content, filling in the details based on the execution of this workflow. The 'Initial Prompt' section should contain the full text of the prompt that initiated this workflow. The start time should be the time the workflow was initiated.
> ```
> ## Cline Setup Workflow Log
> 
> **Initial Prompt:**
> ```
> [Insert the initial user prompt that triggered the workflow here]
> ```
> 
> **Execution Details:**
> - **Start Date & Time:**
> - **End Date & Time:**
> - **Total Efforts Time:**
> ```"

---

## Important Notes for Cline

*   **Idempotency**: When running setup steps, check for existing files and directories to avoid duplication or errors.
*   **Analyze First**: Before generating summaries, perform a thorough analysis of the entire repository to ensure the generated files are accurate and comprehensive.
*   **Ask for Clarification**: If the project is empty or lacks sufficient information to create a meaningful summary, inform the user and ask for more context.
