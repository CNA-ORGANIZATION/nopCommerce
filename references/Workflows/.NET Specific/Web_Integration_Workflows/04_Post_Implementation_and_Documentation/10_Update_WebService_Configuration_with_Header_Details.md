# Workflow: Update WebService Configuration with Header Details

This workflow step focuses on prompting the user for the final header values and updating the configuration files.

## Prompt to Cline

```
"Ask the user to provide the production-ready values for the newly introduced Apigee headers (`x-macys-apikey`, `brand`, `division`, `targethost`, `targetport`). Update the respective configuration files (`Web.config`, `app.config`, etc.) with these values. After completion, must validate that the configuration files have been updated with the production-ready values. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_10_Update_WebService_Configuration_with_Header_Details_<datetimestamp>.md`."
```

## Validation

After Cline completes, validate that the configuration files have been updated with the production-ready values. If the validation fails, Cline will log the failure in `clineworkspace/logs/limitation_step_10_Update_WebService_Configuration_with_Header_Details_<datetimestamp>.md`.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
