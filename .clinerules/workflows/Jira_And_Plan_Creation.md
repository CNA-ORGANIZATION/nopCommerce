# Workflow: Jira Integration and Plan Creation

This workflow covers the initial steps of a development task, including selecting a JIRA User Story and creating a detailed implementation plan.

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
