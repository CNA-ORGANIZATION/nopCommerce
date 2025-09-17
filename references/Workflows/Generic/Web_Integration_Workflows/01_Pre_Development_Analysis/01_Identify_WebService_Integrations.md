# Workflow: Identify Active Mainframe Web Service Integrations

This workflow step focuses on identifying active mainframe web service integrations within the application, primarily by reviewing analysis reports.

## Prompt to Cline

```
"Review all the analysis reports located in the `agent_reports` directory to identify all active mainframe service integrations. If more details needed, then search the codebase for exact WCF service clients (classes inheriting from `System.ServiceModel.ClientBase`) to identify any additional active mainframe service integrations. Suggest the most appropriate implementation plan accordingly. Present a list of active mainframe services and the suggested implementation plan to the user for confirmation before proceeding. After the user confirms, must validate that the identified services list is not empty. If it is empty or the step fails, must log the failure in `clineworkspace/logs/limitation_step_1_Identify_WebService_Integrations_<datetimestamp>.md`."
```

## Validation

After Cline completes, validate that the identified services list is not empty. If it is empty or the step fails, Cline will log the failure in `clineworkspace/logs/limitation_step_1_Identify_WebService_Integrations_<datetimestamp>.md`.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
