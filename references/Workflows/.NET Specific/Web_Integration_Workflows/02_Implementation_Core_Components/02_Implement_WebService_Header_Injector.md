# Workflow: Implement a WebService WCF Header Injector

This workflow step focuses on creating a mechanism to inject custom HTTP headers into outgoing SOAP requests.

## Prompt to Cline

```
"Create a new C# class file named `ApigeeKeyHeader.cs` in the appropriate data or utility project. This file should contain a WCF `IClientMessageInspector` and an `IEndpointBehavior`. The message inspector must add the `x-macys-apikey`, `brand`, and `division` HTTP headers to every outgoing request. The values for these headers should be passed into the endpoint behavior's constructor. Conditionally, if the `brand` header's value is 'custom', the inspector must also add the `targethost` and `targetport` headers. After creation, must validate that the file `ApigeeKeyHeader.cs` exists. If the file does not exist or the step fails, must log the failure in `clineworkspace/logs/limitation_step_2_Implement_WebService_Header_Injector_<datetimestamp>.md`."
```

## Validation

After Cline completes, validate that the file `ApigeeKeyHeader.cs` exists. If the file does not exist or the step fails, Cline will log the failure in `clineworkspace/logs/limitation_step_2_Implement_WebService_Header_Injector_<datetimestamp>.md`.

---
*Include: .clinerules\workflows\Web_Integration_Workflows\common_workflow_rules.md*
