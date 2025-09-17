# Workflow: Documentation, Finalization, and Logging

This workflow covers the final steps of the development process, including generating documentation, building and testing the application, updating JIRA, and logging the workflow's output.

---

## Workflow Steps

### 1. Generate Technical Documentation

Document the changes for future reference.

**Prompt to Cline:**
> "Based on all the changes made, create two documents:
> 1. A comprehensive technical design document covering the high-level design, detailed implementation of the new REST client, authentication mechanism, configuration changes, and an overview of tests. Save this as `TechnicalDesign_RestMigration.md` in `clineworkspace\designs`.
> 2. A Technical Architecture Design document that details the old vs. new architecture, highlighting the benefits of the new changes. Save this as `TechArchitecture_Old_vs_New.md` in `clineworkspace\designs`.
> After creation, must validate that both files exist. If either file does not exist or the step fails, must log the failure in `clineworkspace/logs/limitation_step_12_Generate_Documentation_<datetimestamp>.md`."

### 2. Build the Application

Ensure the application builds successfully with all the new changes.

**Prompt to Cline:**
> "Execute a build of the entire solution to confirm that there are no compilation errors. After completion, must validate that the build was successful. If the build fails, must log the failure in `clineworksspace/logs/limitation_step_13_Build_the_Application_<datetimestamp>.md`."

### 3. Test Unit Test Cases

Test all the newly created Unit Test Cases and generate a consolidated Test Execution Summary.

**Prompt to Cline:**
> "Run only newly created unit tests for the solution. After the test run is complete, generate a consolidated Test Execution Summary report in markdown format for newly created unit test cases only. The report should include the total number of tests passed, failed, and skipped, along with a list of any failed tests and their error messages. Save this report to `clineworkspace/logs/NewlyCreated_UnitTests_Execution_Summary_<datetimestamp>.md`. If the test run fails or the report generation fails, log the failure in `clineworkspace/logs/limitation_step_14_Test_Unit_Tests_<datetimestamp>.md`."

> "Run all unit tests (except newly created ones) for the solution. After the test run is complete, generate a consolidated Test Execution Summary report in markdown format. The report should include the total number of tests passed, failed, and skipped, along with a list of any failed tests and their error messages. Save this report to `clineworkspace/logs/Existing_UnitTests_Execution_Summary_<datetimestamp>.md`. If the test run fails or the report generation fails, log the failure in `clineworkspace/logs/limitation_step_14_Test_Unit_Tests_<datetimestamp>.md`."

### 4. Update Memory Bank

Update the memory bank to create a persistent record of the migration.

**Prompt to Cline:**
> "Create a new file in the memory bank to summarize the migration. The file should be named `.clinerules/memory-bank/migrations/MIGRATION_<JiraTicket>_<datetimestamp>.md`. Populate this file with the JIRA ticket number, a brief description of the migrated service, and a list of the key files that were created or modified during the workflow. If the file creation fails, log the failure in `clineworkspace/logs/limitation_step_15_Update_Memory_Bank_<datetimestamp>.md`."

### 5. Update JIRA Ticket

Update the selected JIRA ticket with a high-level progress update.

**Prompt to Cline:**
> "Post a comment to the selected JIRA ticket with a high-level bulleted summary of the workflow completion. The comment should not include any sensitive details. Before posting, present the comment to the user for validation and allow for any last-minute changes. If the user approves, post the comment to JIRA and also show prompt to user to chose one of the other available status for the selected JIRA user story to update accordingly on user's selection. If the update fails, log the failure in `clineworkspace/logs/limitation_step_16_Update_JIRA_Ticket_<datetimestamp>.md`."

### 6. Log Successful Workflow Output

Log a summary of the successful task execution.

**Prompt to Cline:**
> "As a final step, you must record a summary of all the tasks executed in a checklist format for each completed step.
> 1. Create a file named `clineworkspace/logs/output_<datetimestamp>.md`, replacing `<datetimestamp>` with the current timestamp in `YYYYMMDDHHMMSS` format.
> 2. Populate the file with a summary of the task, the initial prompt, and a checklist of the completed steps, followed by a final confirmation. For example:
>    ```markdown
>    # Workflow Execution Summary
>
>    **Initial Prompt:** <User's initial prompt>
>
>    ## Completed Steps
>    - [x] Step 1: Select JIRA User Story
>    - [x] Step 2: Create Detailed Implementation Plan
>    - ...
>
>    **Result:** The SOAP to REST migration workflow was completed successfully.
>    ```
> After creation, must validate that the output log file exists. If the file does not exist or the step fails, must log the failure in `clineworkspace/logs/limitation_step_17_Log_Output_<datetimestamp>.md`."

### 7. Log Workflow Metadata and Validate

This final step captures metadata about the execution of this workflow and validates its creation.

**Prompt to Cline:**
> "As the final step, must generate and populate the metadata log for this workflow.
> 1. Create a file named `clineworkspace/logs/WebServiceChanges_Metadata_<YYYYMMDDHHMMSS>.md`.
> 2. Populate the file with the following content, filling in the details based on the execution of this workflow:
> ```markdown
> # Web Service Migration Workflow Metadata
>
> ## Execution Details
> **Initial Prompt:** <User's initial prompt for the entire task>
> **JIRA Ticket:** <Selected JIRA Ticket Number>
>
> ## Overall Timing
> **Start Timestamp:** <Timestamp when the workflow began>
> **End Timestamp:** <Timestamp when the workflow concluded>
> **Total Duration:** <Calculated duration from start and end timestamps>
>
> ## Completed Steps and Efforts
> - [x] Step 1: Select JIRA User Story - **Effort:** <Time taken>
> - [x] Step 2: Create Detailed Implementation Plan - **Effort:** <Time taken>
> - [x] Step 3: Identify and Analyze SOAP-based Integrations - **Effort:** <Time taken>
> - [x] Step 4: Application Build and Package Restoration - **Effort:** <Time taken>
> - [x] Step 5: Develop a REST API Client - **Effort:** <Time taken>
> - [x] Step 6: Implement REST API Authentication - **Effort:** <Time taken>
> - [x] Step 7: Refactor Application to Use the New REST Client - **Effort:** <Time taken>
> - [x] Step 8: Update Configuration - **Effort:** <Time taken>
> - [x] Step 9: Create Unit Tests for the New REST Client - **Effort:** <Time taken>
> - [x] Step 10: Create and Update Integration and Contract Tests - **Effort:** <Time taken>
> - [x] Step 11: Clean Up Old Dependencies - **Effort:** <Time taken>
> - [x] Step 12: Generate Technical Documentation - **Effort:** <Time taken>
> - [x] Step 13: Build the Application - **Effort:** <Time taken>
> - [x] Step 14: Test Unit Test Cases - **Effort:** <Time taken>
> - [x] Step 15: Update Memory Bank - **Effort:** <Time taken>
> - [x] Step 16: Update JIRA Ticket - **Effort:** <Time taken>
> - [x] Step 17: Log Successful Workflow Output - **Effort:** <Time taken>
> - [x] Step 18: Log Workflow Metadata and Validate - **Effort:** <Time taken>
> ```
> After creation, must validate that the metadata log file exists. If the file does not exist or the step fails, must log the failure in `clineworkspace/logs/limitation_step_18_Log_Metadata_<datetimestamp>.md`."
