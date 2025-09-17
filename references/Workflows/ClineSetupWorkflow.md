# Cline Workflow: Automated Project Setup for Knowledge Management

This document outlines a more streamlined procedure for setting up the `.clinerules`, `memory-bank`, and `clineworkspace` directories. Automating these steps helps Cline build and maintain a persistent knowledge base more efficiently.

## Step 1: Initial Setup Script

This single step replaces the manual creation of directories and `.gitignore` entries.

**Prompt to Cline:**
> "Please perform the initial project setup for knowledge management.
> 1.  Ensure the following directory structure exists at the project root, creating any missing directories:
>     ```
>     .clinerules/
>     ├── memory-bank/
>     └── workflows/
>     clineworkspace/
>     ├── designs/
>     └── logs/
>     agent_reports/
>     ```
>     *Note for PowerShell Users:* When creating directories, use `New-Item -ItemType Directory -Force -Path "path/to/directory"` for each path to ensure idempotency and avoid errors if directories already exist. For example:
>     ```powershell
>     New-Item -ItemType Directory -Force -Path ".clinerules"
>     New-Item -ItemType Directory -Force -Path ".clinerules/memory-bank"
>     New-Item -ItemType Directory -Force -Path ".clinerules/workflows"
>     New-Item -ItemType Directory -Force -Path "clineworkspace"
>     New-Item -ItemType Directory -Force -Path "clineworkspace/designs"
>     New-Item -ItemType Directory -Force -Path "clineworkspace/logs"
>     New-Item -ItemType Directory -Force -Path "agent_reports"
>     ```
> 2.  Ensure `.clinerules/`, `clineworkspace/`, and `agent_reports/` are all present in the `.gitignore` file. If not, please add them."
> After completion, validate that all directories and `.gitignore` entries exist. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_1_Initial_Setup_Script_<datetimestamp>.md`."

## Step 2: Import Analysis Reports

This step populates the `agent_reports` directory with existing analysis documents.

**Prompt to Cline:**
> "Please import the analysis reports.
> 1. Prompt me for the local directory path ask where the analysis reports are stored along with an option to skip this step.
> 2. If a path is provided, copy all files and subdirectories from it into the `agent_reports/` directory."
> After completion, if a path was provided, validate that the files have been copied. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_2_Import_Analysis_Reports_<datetimestamp>.md`."

## Step 3: Import Custom Workflows

This step allows for adding generic or project-specific workflows to the project.

**Prompt to Cline:**
> "Please import any custom workflow files.
> 1. Prompt me for the local directory path ask where the workflow files are stored, along with an option to skip this step.
> 2. If a path is provided, copy all files from it into the `.clinerules/workflows/` directory."
> After completion, if a path was provided, validate that the files have been copied. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_3_Import_Custom_Workflows_<datetimestamp>.md`."

## Step 4: Generate Project Summaries

This step generates the core knowledge base files after analyzing the repository. This should only be run once for a project, or when major changes have occurred.

**Prompt to Cline:**
> "Analyze the entire repository and generate the core project summaries with this prompt "Read the project's main config files and come up with rules about the tech stack along with version details, making sure not to use incompitilble packages and libraries, and enforce project standard best practices". Check if the following files exist first, and only create the ones that are missing:
>
> 1.  **`.clinerules/instructions.md`**: Create this file with the following sections and content:
>     *   **Important Notes**:
>         *   **Mainframe Environment:** Must refer the confluence page link: https://macystech.atlassian.net/wiki/spaces/MZ1/pages/878150627/Mainframe+Environment to validate the active mainframe details used in the application.
>         *   **Analysis Reports:** Must refer this folder and related sub-directories \agent_reports\ for agentic ai reports analysis before responding to any prompt related to the repository. 
>         *   **Limitations:** Must record any failure of task along with its prompt in this file \clineworkspace\limitation.md
>         *   **Prompts Output:**: Must record all successful task's summary along with its prompt in this file \clineworkspace\logs\output_<datetimestamp>.md
>         *   **Old Code Record:** Must keep old code as commented-out with proper tags and regions, while working on the new changes instead of removing or replacing the same.
>         *   **New Code Changes:** Must keep proper comment tags on the new code changes with Jira Ticket Number, User, Functionality and Timestamp detail. When asking for the Jira Ticket Number and User Name, provide a prompt with an option to skip.
>     *   **Scope of the Project**: High-level project goals.
>     *   **Tech Stack**: Key languages, frameworks, and libraries.
>     *   **Best Practices**: Guidelines for coding, error handling, and security.
>
> 2.  **`.clinerules/memory-bank/application_summary.md`**: Create this file describing the application's purpose, domain, core functionality, integrations, and business rules.
>
> 3.  **`.clinerules/memory-bank/technical_architecture_summary.md`**: Create this file describing the architectural style, technology stack, design patterns, and key risks."
> After completion, validate that the specified files have been created. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_4_Generate_Project_Summaries_<datetimestamp>.md`."

## Step 5: Log Task-Specific Output

This step is for ongoing use during development to log the results of specific tasks.

**Prompt to Cline:**
> "Must log the output of the current task. Create a new file in `clineworkspace/logs/` named `output_<YYYYMMDDHHMMSS>.md`. The file should contain a summary of my request and a log of the actions you took to fulfill it."
> After completion, validate that the output log file has been created. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_5_Log_Task-Specific_Output_<datetimestamp>.md`."

## Step 6: Log Workflow Efforts

This final step captures metadata about the execution of this setup workflow.

**Prompt to Cline:**
> "As the final step, please generate and populate the effort log for this workflow.
> 1. Create a file named `clineworkspace/logs/ClineSetupWorkflow_Efforts_<YYYYMMDDHHMMSS>.md`.
> 2. Populate the file with the following content, filling in the details based on the execution of this workflow:
> ```
> Prompt Details: 
> Tokens Details: <Prompt Tokens> | <Completion Tokens> | <Tokens reads from cache>
> Current Tokens used in this request: 
> Start Date & Time:
> End Date & Time:
> Total Efforts Time:
> ```"
> After completion, validate that the effort log file has been created. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_6_Log_Workflow_Efforts_<datetimestamp>.md`."

---

## General Guidelines

*   **Failure Logging:** During task execution, if a step fails, record the details in a new file at `clineworkspace/logs/limitation_step_<step_number>_<step_name>_<datetimestamp>.md`. The content should follow this template:
    ```markdown
    # Limitation Log

    - **Step Number:** <Step Number>
    - **Step Name:** <Step Name>
    - **Timestamp:** <Timestamp>
    - **Prompt:**
      ```
      <Full Prompt Used>
      ```
    - **Failure Details:**
      <Detailed explanation of the failure>
    ```

## Important Notes for Cline

*   **Idempotency**: When running setup steps, check for existing files and directories to avoid duplication or errors.
*   **Analyze First**: Before generating summaries, perform a thorough analysis of the entire repository to ensure the generated files are accurate and comprehensive.
*   **Ask for Clarification**: If the project is empty or lacks sufficient information to create a meaningful summary, inform the user and ask for more context.
