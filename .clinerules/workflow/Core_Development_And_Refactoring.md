# Workflow: Core Development and Refactoring

This workflow covers the core development and refactoring tasks, including identifying legacy code, building the application, developing a new client, implementing authentication, and refactoring the application to use the new client.

---

## Workflow Steps

### 1. Identify and Analyze SOAP-based Integrations

Before making any code changes, identify and analyze the existing SOAP integrations to understand their function and scope.

**Prompt to Cline:**
> "Review implementation plan from the previous step's output and all analysis reports in the `agent_reports` directory and search the entire codebase for dependencies on SOAP client libraries (e.g., `System.ServiceModel.Http`) and their usage. Identify all active SOAP integrations that are candidates for migration to REST. Present a list of these services and a high-level migration plan, including complexity and effort estimates, to the user for confirmation. After user confirmation, validate that the list of services to be migrated is not empty. If it is, or the step fails, log the failure in `clineworkspace/logs/limitation_step_3_Identify_Integrations_<datetimestamp>.md`."

### 2. Application Build and Package Restoration

Before starting any development, ensure the application is in a buildable state and all dependencies are restored.

**Prompt to Cline:**
> "Execute a build of the entire solution to confirm that there are no compilation errors and restore any missing packages. After completion, must validate that the build was successful. If the build fails, must log the failure in `clineworkspace/logs/limitation_step_4_Application_Build_<datetimestamp>.md`."

### 3. Develop a REST API Client

Create a new, modern client to communicate with the target REST API.

**Prompt to Cline:**
> "For the identified service (e.g., UPS Shipping Service), create a new C# class file for a REST API client (e.g., `UpsRestApiClient.cs`) in the appropriate project. This client should:
> 1. Use `IHttpClientFactory` to create and manage `HttpClient` instances.
> 2. Implement methods for the required operations (e.g., fetching shipping rates). If the service integrates with multiple APIs, ensure the client can handle requests to each API.
> 3. Define C# POCO classes for the JSON request and response data models based on the API documentation. Use `Newtonsoft.Json` for deserialization.
> 4. Include robust error handling for HTTP status codes, network exceptions, and JSON error responses.
> After creation, must validate that the new client file exists and uses `IHttpClientFactory`. If the file does not exist or the validation fails, must log the failure in `clineworkspace/logs/limitation_step_5_Develop_REST_Client_<datetimestamp>.md`."

### 4. Implement REST API Authentication

Integrate the authentication mechanism required by the new REST API.

**Prompt to Cline:**
> "Implement the required authentication mechanism for the target REST API in the new client.
> -   **If OAuth 2.0 is required:** Implement the client credentials flow (or other appropriate flow) to obtain an access token. Add logic to securely store client credentials, cache the access token, and refresh it upon expiration.
> -   **If an API Key is required:** Add logic to include the API key in the request headers or query parameters as specified by the API documentation.
> Sensitive credentials (Client ID, Secret, API Key) must be retrieved from a secure configuration source. After completion, must validate that the authentication logic has been added to the REST client. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_6_Implement_Authentication_<datetimestamp>.md`."

### 5. Refactor Application to Use the New REST Client

Update the application's business logic to replace the old SOAP client with the new REST client.

**Prompt to Cline:**
> "Refactor the application code that currently calls the old SOAP service to use the new REST client.
> 1.  Update the dependency injection configuration to register the new REST client interface and implementation.
> 2.  In the consumer class (e.g., `UPSComputationMethod.cs`), replace the calls to the old SOAP client with calls to the new REST client's methods.
> 3.  Implement a data transformation layer or mapping logic to convert the application's internal data models to the new REST client's request models and vice-versa.
> 4.  Update `try-catch` blocks to handle `HttpRequestException` and check for non-success HTTP status codes instead of SOAP-specific exceptions like `FaultException`.
> After completion, must validate that the consumer class has been successfully refactored. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_7_Refactor_Application_<datetimestamp>.md`."
