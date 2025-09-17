# Workflow: Integrate WebService Secure Credential Management

This workflow step focuses on integrating secure credential management for fetching sensitive values like API keys.

## Prompt to Cline

```
"First, inspect the solution to determine if it already integrates with CyberArk Safe.

-   **If CyberArk is NOT currently used:** Modify the application to retrieve the `ApigeeApiKey` directly from a local configuration key (e.g., `LocalApigeeApiKey_<ENV>`). Do not add new CyberArk integration logic.
-   **If CyberArk IS currently used:** In the project's existing security or vault interaction class (e.g., `CyberArkVault.cs`), create a new public method `GetApiKey(string environment)`. This method should:
    1.  Check for a configuration flag (e.g., `EnableSecureVault`) in `App.config`.
    2.  If enabled, use the existing SDK to fetch the secret from the vault. The secret's name should be based on a configuration key (e.g., `VaultApigeeApiKey_<ENV>`). Implement a retry mechanism for this call.
    3.  Implement a fail-safe mechanism if the primary retrieval fails (e.g., call a backup service or read from a local encrypted file).
    4.  If disabled, read the API key directly from a local configuration key for development and testing purposes."
After completion, must validate that the `GetApiKey` method has been created or the local configuration key has been added. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_3_Integrate_WebService_Credential_Management_<datetimestamp>.md`."
```

## Validation

After Cline completes, validate that the `GetApiKey` method has been created or the local configuration key has been added. If the validation fails, Cline will log the failure in `clineworkspace/logs/limitation_step_3_Integrate_WebService_Credential_Management_<datetimestamp>.md`.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
