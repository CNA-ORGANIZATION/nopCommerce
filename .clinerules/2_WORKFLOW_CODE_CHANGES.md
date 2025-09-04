# Generic Cline Workflow: Migrating SOAP Services to REST

This document provides a standardized, generic workflow for migrating existing SOAP-based web service integrations to modern RESTful APIs. It includes steps for analysis, code implementation, configuration, testing, and cleanup.

## Prerequisites

-   An application with one or more existing SOAP service integrations (e.g., using WCF or other SOAP client libraries).
-   Access to the target REST API documentation, including endpoint URLs, authentication mechanisms (e.g., OAuth 2.0, API Key), and data contracts (JSON schemas).
-   Access to a sandbox or testing environment for the target REST API.

---

## Workflow Steps

### 1. Select JIRA User Story

Before starting development, identify and select an active JIRA User Story to associate with the changes.

**Prompt to Cline:**
> "Search for all active JIRA User Stories for development activities only and must not include with status `DONE`, `CANCELED` or `OUT OF SCOPE` ones. Present a list of these User Stories to the user and prompt them to select one to proceed with the development. In case of a JIRA MCP Server connection issue, retry the operation up to 3 times. Upon selection, assign the selected user story to the current user and change the status as `In Progress` in JIRA. If no active User Stories are found, inform the user and ask if they want to proceed without a JIRA ticket. Store the selected JIRA ticket number for future reference in comments. If the step fails, log the failure in `clineworkspace/logs/limitation_step_1_Select_Jira_Story_<datetimestamp>.md`."

### 2. Create Detailed Implementation Plan

Based on the selected JIRA User Story, create a detailed implementation plan. This involves thoroughly checking the user story's steps, sub-tasks, and any attached documents.

**Prompt to Cline:**
> "Based on the selected JIRA User Story, analyze its description, sub-tasks, and any attached files and also thoroughly review the entire codebase for any additional code recommendations related to the selected user story to be included in the implementation plan. Based upon the complete analysis, must create a detailed implementation plan and present it to the user for approval. The plan should outline the files to be created/modified, the classes and methods to be implemented, and the expected testing approach. Download the complete implementation plan and the attachements (if available) in the selected JIRA User Story in `clineworkspace/designs/Detailed_Code_Implementation_Plan_<datetimestamp>.md`. If the step fails, log the failure in `clineworkspace/logs/limitation_step_2_Create_Implementation_Plan_<datetimestamp>.md`."

### 3. Identify and Analyze SOAP-based Integrations

Before making any code changes, identify and analyze the existing SOAP integrations to understand their function and scope.

**Prompt to Cline:**
> "Review implementation plan from the previous step's output and all analysis reports in the `agent_reports` directory and search the entire codebase for dependencies on SOAP client libraries (e.g., `System.ServiceModel.Http`) and their usage. Identify all active SOAP integrations that are candidates for migration to REST. Present a list of these services and a high-level migration plan, including complexity and effort estimates, to the user for confirmation. After user confirmation, validate that the list of services to be migrated is not empty. If it is, or the step fails, log the failure in `clineworkspace/logs/limitation_step_3_Identify_Integrations_<datetimestamp>.md`."

### 4. Application Build and Package Restoration

Before starting any development, ensure the application is in a buildable state and all dependencies are restored.

**Prompt to Cline:**
> "Execute a build of the entire solution to confirm that there are no compilation errors and restore any missing packages. After completion, must validate that the build was successful. If the build fails, must log the failure in `clineworkspace/logs/limitation_step_4_Application_Build_<datetimestamp>.md`."

### 5. Develop a REST API Client

Create a new, modern client to communicate with the target REST API.

**Prompt to Cline:**
> "For the identified service (e.g., UPS Shipping Service), create a new C# class file for a REST API client (e.g., `UpsRestApiClient.cs`) in the appropriate project. This client should:
> 1. Use `IHttpClientFactory` to create and manage `HttpClient` instances.
> 2. Implement methods for the required operations (e.g., fetching shipping rates). If the service integrates with multiple APIs, ensure the client can handle requests to each API.
> 3. Define C# POCO classes for the JSON request and response data models based on the API documentation. Use `Newtonsoft.Json` for deserialization.
> 4. Include robust error handling for HTTP status codes, network exceptions, and JSON error responses.
> After creation, must validate that the new client file exists and uses `IHttpClientFactory`. If the file does not exist or the validation fails, must log the failure in `clineworkspace/logs/limitation_step_5_Develop_REST_Client_<datetimestamp>.md`."

### 6. Implement REST API Authentication

Integrate the authentication mechanism required by the new REST API.

**Prompt to Cline:**
> "Implement the required authentication mechanism for the target REST API in the new client.
> -   **If OAuth 2.0 is required:** Implement the client credentials flow (or other appropriate flow) to obtain an access token. Add logic to securely store client credentials, cache the access token, and refresh it upon expiration.
> -   **If an API Key is required:** Add logic to include the API key in the request headers or query parameters as specified by the API documentation.
> Sensitive credentials (Client ID, Secret, API Key) must be retrieved from a secure configuration source. After completion, must validate that the authentication logic has been added to the REST client. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_6_Implement_Authentication_<datetimestamp>.md`."

### 7. Refactor Application to Use the New REST Client

Update the application's business logic to replace the old SOAP client with the new REST client.

**Prompt to Cline:**
> "Refactor the application code that currently calls the old SOAP service to use the new REST client.
> 1.  Update the dependency injection configuration to register the new REST client interface and implementation.
> 2.  In the consumer class (e.g., `UPSComputationMethod.cs`), replace the calls to the old SOAP client with calls to the new REST client's methods.
> 3.  Implement a data transformation layer or mapping logic to convert the application's internal data models to the new REST client's request models and vice-versa.
> 4.  Update `try-catch` blocks to handle `HttpRequestException` and check for non-success HTTP status codes instead of SOAP-specific exceptions like `FaultException`.
> After completion, must validate that the consumer class has been successfully refactored. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_7_Refactor_Application_<datetimestamp>.md`."

### 8. Update Configuration

Modify the application's configuration to support the new REST service.

**Prompt to Cline:**
> "Update the relevant configuration files (e.g., `plugin.json`, `appsettings.json`, or custom configuration pages) for the migrated feature.
> 1.  Remove old configuration settings related to the SOAP endpoint URL and credentials.
> 2.  Add new settings for the REST API base URL.
> 3.  Add new settings for the required credentials (e.g., Client ID, Client Secret, API Key), ensuring they are linked to a secure storage mechanism.
> After completion, must validate that the configuration has been updated with the new settings. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_8_Update_Configuration_<datetimestamp>.md`."

### 9. Create Unit Tests for the New REST Client

To ensure the new REST client is reliable and functions as expected, create a comprehensive suite of unit tests.

**Prompt to Cline:**
> "Before creating unit test cases, check if a unit test project already exists. If not, create a new Unit Test project and add it to the existing solution. Then, navigate to the appropriate test project and create a new test class for the REST client (e.g., `UpsRestApiClientTests.cs`).
> 1.  Write unit tests for the new REST client, ensuring all new code changes are covered. Use a mocking framework (e.g., `Moq`) to mock `IHttpClientFactory` and `HttpMessageHandler`.
> 2.  Simulate various API responses (success, failure, different status codes, empty/malformed JSON) to test the client's parsing logic and error handling.
> 3.  Ensure that all public methods in the new client are covered by unit tests.
> 4.  Validate that the tests are added to the test suite and pass successfully. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_9_Create_Unit_Tests_<datetimestamp>.md`."

### 10. Create and Update Integration and Contract Tests

Ensure the new implementation integrates correctly with the application and maintains functional parity with the old one.

**Prompt to Cline:**
> "Update the test project for the modified code.
> 1.  Update integration tests to validate the end-to-end flow with the new REST client, preferably against a sandbox environment of the target API.
> 2.  Ensure that all integrated APIs are covered by integration tests. For example, if the service communicates with both a VIES and an HMRC API, there should be tests for both.
> 3.  **Crucially, create contract tests:** Compare the outputs from the new REST implementation with the outputs from the old SOAP implementation using the same set of inputs to ensure functional parity and prevent business logic regressions (e.g., ensuring shipping rates are identical).
> After completion, must validate that the new and updated tests have been added to the test suite and that all APIs are covered. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_10_Update_Integration_Tests_<datetimestamp>.md`."

### 11. Clean Up Old Dependencies

Once the new implementation is verified, remove the legacy code and dependencies.

**Prompt to Cline:**
> "After confirming the new implementation is working correctly and all tests are passing, remove the old SOAP client code and its dependencies.
> 1.  Delete the old SOAP service client files.
> 2.  If no other components in the solution use it, remove the package reference to the SOAP client library (e.g., `System.ServiceModel.Http`) from the relevant `.csproj` file.
> After completion, must validate that the old files and package references have been removed. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_11_Cleanup_Dependencies_<datetimestamp>.md`."

### 12. Generate Technical Documentation

Document the changes for future reference.

**Prompt to Cline:**
> "Based on all the changes made, create two documents:
> 1. A comprehensive technical design document covering the high-level design, detailed implementation of the new REST client, authentication mechanism, configuration changes, and an overview of tests. Save this as `TechnicalDesign_RestMigration.md` in `clineworkspace\designs`.
> 2. A Technical Architecture Design document that details the old vs. new architecture, highlighting the benefits of the new changes. Save this as `TechArchitecture_Old_vs_New.md` in `clineworkspace\designs`.
> After creation, must validate that both files exist. If either file does not exist or the step fails, must log the failure in `clineworkspace/logs/limitation_step_12_Generate_Documentation_<datetimestamp>.md`."

### 13. Build the Application

Ensure the application builds successfully with all the new changes.

**Prompt to Cline:**
> "Execute a build of the entire solution to confirm that there are no compilation errors. After completion, must validate that the build was successful. If the build fails, must log the failure in `clineworksspace/logs/limitation_step_13_Build_the_Application_<datetimestamp>.md`."

### 14. Test Unit Test Cases

Test all the newly created Unit Test Cases and generate a consolidated Test Execution Summary.

**Prompt to Cline:**
> "Run only newly created unit tests for the solution. After the test run is complete, generate a consolidated Test Execution Summary report in markdown format for newly created unit test cases only. The report should include the total number of tests passed, failed, and skipped, along with a list of any failed tests and their error messages. Save this report to `clineworkspace/logs/NewlyCreated_UnitTests_Execution_Summary_<datetimestamp>.md`. If the test run fails or the report generation fails, log the failure in `clineworkspace/logs/limitation_step_14_Test_Unit_Tests_<datetimestamp>.md`."

> "Run all unit tests (except newly created ones) for the solution. After the test run is complete, generate a consolidated Test Execution Summary report in markdown format. The report should include the total number of tests passed, failed, and skipped, along with a list of any failed tests and their error messages. Save this report to `clineworkspace/logs/Existing_UnitTests_Execution_Summary_<datetimestamp>.md`. If the test run fails or the report generation fails, log the failure in `clineworkspace/logs/limitation_step_14_Test_Unit_Tests_<datetimestamp>.md`."

### 15. Update Memory Bank

Update the memory bank to create a persistent record of the migration.

**Prompt to Cline:**
> "Create a new file in the memory bank to summarize the migration. The file should be named `.clinerules/memory-bank/migrations/MIGRATION_<JiraTicket>_<datetimestamp>.md`. Populate this file with the JIRA ticket number, a brief description of the migrated service, and a list of the key files that were created or modified during the workflow. If the file creation fails, log the failure in `clineworkspace/logs/limitation_step_15_Update_Memory_Bank_<datetimestamp>.md`."

### 16. Update JIRA Ticket

Update the selected JIRA ticket with a high-level progress update.

**Prompt to Cline:**
> "Post a comment to the selected JIRA ticket with a high-level bulleted summary of the workflow completion. The comment should not include any sensitive details. Before posting, present the comment to the user for validation and allow for any last-minute changes. If the user approves, post the comment to JIRA and also show prompt to user to chose one of the other available status for the selected JIRA user story to update accordingly on user's selection. If the update fails, log the failure in `clineworkspace/logs/limitation_step_16_Update_JIRA_Ticket_<datetimestamp>.md`."

### 17. Log Successful Workflow Output

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

### 18. Log Workflow Metadata and Validate

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

---

## General Guidelines

*   **Failure Logging:** During task execution, if a step fails, must record the details in a new file at `clineworkspace/logs/limitation_step_<step_number>_<step_name>_<datetimestamp>.md`.
*   **Old Code Record:** Must keep old code as commented-out with proper tags and regions, while working on the new changes instead of removing or replacing the same.
*   **New Code Changes:** Must keep proper comment tags on the new code changes with Jira Ticket Number, User, Functionality and Timestamp detail. When asking for the Jira Ticket Number and User Name, provide a prompt with an option to skip.

## Important Notes for Cline

*   **Ask for Clarification**: If at any point a step is unclear or the project structure is different from what is assumed, use the `ask_followup_question` tool to ask for more specific information.
*   **Analyze Existing Code**: Before creating new files or modifying existing ones, analyze the surrounding code to match the existing coding style, naming conventions, and architectural patterns.
*   **Verify File Paths**: When asked to create a file, ensure the specified path is correct for the current project structure. If a required folder does not exist, you are permitted to create it.
*   **User Intervention**: If you encounter a repetitive error that you cannot resolve, please prompt the user for intervention.
