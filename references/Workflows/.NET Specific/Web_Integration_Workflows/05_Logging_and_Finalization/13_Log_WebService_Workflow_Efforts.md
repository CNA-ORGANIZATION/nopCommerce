# Workflow: Log WebService Workflow Efforts

This final workflow step captures metadata about the execution of this setup workflow.

## Prompt to Cline

```
"As the final step, must generate and populate the effort log for this workflow.
1. Create a file named `clineworkspace/logs/ApigeeWebServiceMigration_Efforts_<YYYYMMDDHHMMSS>.md`.
2. Populate the file with the following content, filling in the details based on the execution of this workflow:
```
# Apigee WebService Migration Workflow Effort Log

## Prompt & Token Details
**Prompt Details:**
**Tokens Details:** <Prompt Tokens> | <Completion Tokens> | <Tokens read from cache>
**Current Tokens used in this request:**

## Overall Timing
**Start Date & Time:**
**End Date & Time:**
**Total Efforts Time:**

## Time per Step
- **Step 1 (Identify WebService Integrations):** <Time>
- **Step 2 (Implement WebService Header Injector):** <Time>
- **Step 3 (Integrate WebService Credential Management):** <Time>
- **Step 4 (Update WebService Configuration):** <Time>
- **Step 5 (Apply WebService Behavior and Use API Key):** <Time>
- **Step 6 (Create and Update WebService Unit Tests):** <Time>
- **Step 7 (Validate WebService Build Environment):** <Time>
- **Step 8 (Validate WebService Project Files):** <Time>
- **Step 9 (Validate WebService Code Files):** <Time>
- **Step 10 (Update WebService Configuration with Header Details):** <Time>
- **Step 11 (Generate WebService Technical Documentation):** <Time>
- **Step 12 (Build the WebService Application):** <Time>
```"
```

## Validation

After Cline completes, validate that the effort log file has been created.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
