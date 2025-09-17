# Workflow: Apply the WebService WCF Behavior and Use the API Key

This workflow step focuses on modifying the application code to use the new components.

## Prompt to Cline

```
"Refactor the application to retrieve the API key at startup.
1. In a central application startup class or helper, call the `GetApigeeApiKey` method upon initialization and store the key in a static variable or singleton instance.
2. In every method that calls a WCF service, pass the retrieved API key.
3. Before calling the service operation, instantiate and add the custom endpoint behavior to the WCF client's endpoint, passing the API key and any other required header values from the configuration."
After completion, must validate that the application code has been refactored to use the new components. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_5_Apply_WebService_Behavior_<datetimestamp>.md`."
```

## Validation

After Cline completes, validate that the application code has been refactored to use the new components. If the validation fails, Cline will log the failure in `clineworkspace/logs/limitation_step_5_Apply_WebService_Behavior_<datetimestamp>.md`.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
