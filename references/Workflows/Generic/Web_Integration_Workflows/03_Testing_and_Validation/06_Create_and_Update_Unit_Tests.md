# Workflow: Create and Update Unit Tests

This workflow step focuses on ensuring all the changes are covered by unit tests.

## Prompt to Cline

```
"First, check if the solution already contains a Unit Test project.

-   **If a test project does NOT exist:** Create a new Unit Test Project, add it to the solution, and configure it to test the main application.
-   **If a test project already exists:** Proceed with updating the existing test suite.

Add or update unit tests to validate the new changes.
1.  Create new tests to verify the new logic, including any fail-safe mechanisms.
2.  Add mock data or dummy input files to the project for each service affected by the changes to ensure consistent test data and keep those generated dummy input files under a 'DummyData' folder in the Unit Test Project.
3.  Update existing integration tests to incorporate any new method signatures or configurations, confirming they work as expected."
After completion, must validate that the unit tests have been created or updated. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_6_Create_and_Update_Unit_Tests_<datetimestamp>.md`."
```

## Validation

After Cline completes, validate that the unit tests have been created or updated. If the validation fails, Cline will log the failure in `clineworkspace/logs/limitation_step_6_Create_and_Update_Unit_Tests_<datetimestamp>.md`.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
