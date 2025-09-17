# Workflow: Configuration, Testing, and Cleanup

This workflow covers the tasks of updating the application's configuration, creating unit and integration tests, and cleaning up old dependencies.

---

## Workflow Steps

### 1. Update Configuration

Modify the application's configuration to support the new REST service.

**Prompt to Cline:**
> "Update the relevant configuration files (e.g., `plugin.json`, `appsettings.json`, or custom configuration pages) for the migrated feature.
> 1.  Remove old configuration settings related to the SOAP endpoint URL and credentials.
> 2.  Add new settings for the REST API base URL.
> 3.  Add new settings for the required credentials (e.g., Client ID, Client Secret, API Key), ensuring they are linked to a secure storage mechanism.
> After completion, must validate that the configuration has been updated with the new settings. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_8_Update_Configuration_<datetimestamp>.md`."

### 2. Create Unit Tests for the New REST Client

To ensure the new REST client is reliable and functions as expected, create a comprehensive suite of unit tests.

**Prompt to Cline:**
> "Before creating unit test cases, check if a unit test project already exists. If not, create a new Unit Test project and add it to the existing solution. Then, navigate to the appropriate test project and create a new test class for the REST client (e.g., `UpsRestApiClientTests.cs`).
> 1.  Write unit tests for the new REST client, ensuring all new code changes are covered. Use a mocking framework (e.g., `Moq`) to mock `IHttpClientFactory` and `HttpMessageHandler`.
> 2.  Simulate various API responses (success, failure, different status codes, empty/malformed JSON) to test the client's parsing logic and error handling.
> 3.  Ensure that all public methods in the new client are covered by unit tests.
> 4.  Validate that the tests are added to the test suite and pass successfully. If the validation fails, log the failure in `clineworkspace/logs/limitation_step_9_Create_Unit_Tests_<datetimestamp>.md`."

### 3. Create and Update Integration and Contract Tests

Ensure the new implementation integrates correctly with the application and maintains functional parity with the old one.

**Prompt to Cline:**
> "Update the test project for the modified code.
> 1.  Update integration tests to validate the end-to-end flow with the new REST client, preferably against a sandbox environment of the target API.
> 2.  Ensure that all integrated APIs are covered by integration tests. For example, if the service communicates with both a VIES and an HMRC API, there should be tests for both.
> 3.  **Crucially, create contract tests:** Compare the outputs from the new REST implementation with the outputs from the old SOAP implementation using the same set of inputs to ensure functional parity and prevent business logic regressions (e.g., ensuring shipping rates are identical).
> After completion, must validate that the new and updated tests have been added to the test suite and that all APIs are covered. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_10_Update_Integration_Tests_<datetimestamp>.md`."

### 4. Clean Up Old Dependencies

Once the new implementation is verified, remove the legacy code and dependencies.

**Prompt to Cline:**
> "After confirming the new implementation is working correctly and all tests are passing, remove the old SOAP client code and its dependencies.
> 1.  Delete the old SOAP service client files.
> 2.  If no other components in the solution use it, remove the package reference to the SOAP client library (e.g., `System.ServiceModel.Http`) from the relevant `.csproj` file.
> After completion, must validate that the old files and package references have been removed. If the validation fails, must log the failure in `clineworkspace/logs/limitation_step_11_Cleanup_Dependencies_<datetimestamp>.md`."
