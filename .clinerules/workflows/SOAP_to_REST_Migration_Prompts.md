# Prompts for SOAP to REST Migration using Modular Workflows

This document provides a step-by-step series of prompts to guide the user through a SOAP to REST migration using the modular workflows.

---

## Part 1: Jira Integration and Plan Creation

This part of the process involves selecting a JIRA User Story and creating a detailed implementation plan.

**Prompt to Cline:**
> "Start the SOAP to REST migration process by using the `Jira_And_Plan_Creation.md` workflow. First, select a JIRA User Story, and then create a detailed implementation plan based on that story."

---

## Part 2: Core Development and Refactoring

This part of the process covers the core development tasks, including identifying the SOAP service, building the application, creating a new REST client, implementing authentication, and refactoring the application.

**Prompt to Cline:**
> "Now that the planning is complete, proceed with the core development and refactoring using the `Core_Development_And_Refactoring.md` workflow. Identify the SOAP integration, ensure the application builds, develop the new REST client, implement authentication, and refactor the application to use the new client."

---

## Part 3: Configuration, Testing, and Cleanup

This part of the process involves updating the configuration, creating tests, and cleaning up old dependencies.

**Prompt to Cline:**
> "With the core development done, continue with the `Configuration_Testing_And_Cleanup.md` workflow. Update the configuration, create unit and integration tests for the new client, and clean up the old SOAP dependencies."

---

## Part 4: Documentation, Finalization, and Logging

This is the final part of the process, which includes generating documentation, performing a final build and test run, updating JIRA, and logging the results.

**Prompt to Cline:**
> "To complete the migration, execute the `Documentation_Finalization_And_Logging.md` workflow. Generate the technical documentation, run a final build and test suite, update the JIRA ticket, and log the successful completion of the entire process."
