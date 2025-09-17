## General Guidelines

*   **Mainframe Environment:** Must refer to the [Mainframe Environment Confluence page](https://macystech.atlassian.net/wiki/spaces/MZ1/pages/878150627/Mainframe+Environment) to validate any mainframe details used in the application.
*   **Failure Logging:** During task execution, if a step fails, must record the details in a new file at `clineworkspace/logs/limitation_step_<step_number>_<step_name>_<datetimestamp>.md`. The content should follow this template:
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
*   **Old Code Record:** Must keep old code as commented-out using `#region Old Code` and `#endregion` blocks for clear tracking.
*   **New Code Changes:** All new code must be commented with a standard header indicating the Jira Ticket, User, functionality, and a timestamp. Must ask user to share Jira Ticket Number and User Name before applying changes.

## Important Notes for Cline

*   **Ask for Clarification**: If at any point a step is unclear or the project structure is different from what is assumed in the prompts, do not guess. Use the `ask_followup_question` tool to ask for more specific information. For example:
    *   "I cannot find a `SecureVault.cs` file. What is the name of the class that handles security and credential management in this project?"
    *   "The prompt mentions a data layer project. Could you please specify the name of that project?"

*   **Analyze Existing Code**: Before creating new files or modifying existing ones, briefly analyze the surrounding code to match the existing coding style, naming conventions, and architectural patterns.

*   **Verify File Paths**: When asked to create a file, ensure the specified path is correct for the current project structure. If a `clineworkspace\designs` or `clineworkspace` folder does not exist, you are permitted to create it.
