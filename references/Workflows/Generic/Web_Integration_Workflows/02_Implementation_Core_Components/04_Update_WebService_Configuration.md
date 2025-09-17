# Workflow: Update WebService Configuration (`App.config`)

This workflow step focuses on modifying the application's configuration file to support the new integration.

## Prompt to Cline

```
"Update the configuration file (e.g., `App.config` or `Web.config`) in the main application project and any associated unit test projects. Make the following changes:
1. Add a flag to enable/disable the secure vault integration (e.g., `EnableSecureVault`).
2. Add keys for the vault connection parameters (e.g., `VaultAppID`, `VaultSafe`).
3. Add keys for the vault secret name and the local fallback API key.
4. Add keys for all required Apigee headers.
5. In the `system.serviceModel` section, update the `endpoint` address for each migrated service to point to the new Apigee URL."
After completion, must validate that the configuration file has been updated with the new keys and endpoint addresses. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_4_Update_WebService_Configuration_<datetimestamp>.md`."
```

## Validation

After Cline completes, validate that the configuration file has been updated with the new keys and endpoint addresses. If the validation fails, Cline will log the failure in `clineworkspace/logs/limitation_step_4_Update_WebService_Configuration_<datetimestamp>.md`.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
