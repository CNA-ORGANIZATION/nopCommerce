# Workflow: Generate WebService Technical Documentation

This workflow step focuses on documenting the changes after the core implementation and testing are complete.

## Prompt to Cline

```
"Based on all the changes made, create a comprehensive technical design document. The document should cover the high-level design, detailed implementation of the header injection and secure credential retrieval, a summary of all configuration changes in `App.config`, and an overview of the new unit tests. Save this as `TechnicalDesign_ApigeeIntegration.md` in the `clineworkspace\designs` folder. After creation, must validate that the file `TechnicalDesign_ApigeeIntegration.md` exists. If the file does not exist or the step fails, must log the failure in `clineworkspace/logs/limitation_step_11_Generate_WebService_Technical_Documentation_<datetimestamp>.md`."
```

## Validation

After Cline completes, validate that the file `TechnicalDesign_ApigeeIntegration.md` exists. If the file does not exist or the step fails, Cline will log the failure in `clineworkspace/logs/limitation_step_11_Generate_WebService_Technical_Documentation_<datetimestamp>.md`.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
