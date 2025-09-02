## SOAP to REST Migration Recommendations: UPS Shipping Rate Computation and EuropaCheckVatService

### Introduction

This report provides detailed recommendations for migrating the UPS Shipping Rate Computation Method and the EuropaCheckVatService from SOAP to REST. Migrating to REST offers several benefits, including improved performance, simplified integrations, and reduced maintenance overhead. This report is based on the analysis performed in the `soap_to_rest_migration_strategy.md` report and the source code in the `source\nopCommerce` directory.

### Analysis

The analysis identified a SOAP client dependency in the `Nop.Services.csproj` file, indicating the presence of SOAP client implementations in the application. The `Nop.Plugin.Shipping.UPS` plugin, responsible for calculating shipping rates from UPS, and the `EuropaCheckVatService`, used for checking VAT numbers, were identified as likely candidates for SOAP integrations. It is recommended that the `UPSService` within the `Nop.Plugin.Shipping.UPS` plugin and the `EuropaCheckVatService` be rewritten to use modern REST APIs.

### Files Involved

The following files are involved in the SOAP to REST migration:

**UPS Shipping Rate Computation:**

-   `source\nopCommerce\src\Plugins\Nop.Plugin.Shipping.UPS\UPSComputationMethod.cs`: This file contains the `UPSComputationMethod` class, which implements the `IShippingRateComputationMethod` interface and is responsible for getting shipping rates from UPS.
-   `source\nopCommerce\src\Plugins\Nop.Plugin.Shipping.UPS\UPSSettings.cs`: This file defines the settings that are used by the UPS plugin, including the API endpoint and authentication credentials.
-   `source\nopCommerce\src\Plugins\Nop.Plugin.Shipping.UPS\Services\UPSService.cs`: This file contains the `UPSService` class, which is responsible for interacting with the UPS API.

**EuropaCheckVatService:**

-   `source\nopCommerce\src\Libraries\Nop.Services\Connected Services\EuropaCheckVatService\Reference.cs`: This file contains the generated SOAP client proxy for the EuropaCheckVatService.
-   (Inferred) A service class that uses the EuropaCheckVatService (e.g., `VatService.cs` or similar).

**Common:**

-   `source\nopCommerce\src\Nop.Services\Nop.Services.csproj`: This file contains the project dependencies, including the `System.ServiceModel.Http` dependency.

### Required Changes

#### Code Changes

**UPS Shipping Rate Computation:**

-   **`source\nopCommerce\src\Plugins\Nop.Plugin.Shipping.UPS\UPSComputationMethod.cs`**:
    -   Modify the constructor to inject the new REST API client instead of the `UPSService`.
    -   Update the `GetShippingOptionsAsync` method to call the new REST API client to get the shipping rates.
    -   Update the `GetShipmentTrackerAsync` method to use the new REST API client.
-   **`source\nopCommerce\src\Plugins\Nop.Plugin.Shipping.UPS\Services\UPSService.cs`**:
    -   Remove this file, as it is no longer needed.
    -   Create a new file, `UpsRestApiClient.cs`, to contain the REST API client.
    -   Implement methods in the `UpsRestApiClient.cs` file to get the shipping rates and track shipments using the UPS REST API.
-   **`source\nopCommerce\src\Plugins\Nop.Plugin.Shipping.UPS\UPSSettings.cs`**:
    -   Update the properties to store the REST API credentials, such as the client ID and client secret.

**EuropaCheckVatService:**

-   **`source\nopCommerce\src\Libraries\Nop.Services\Connected Services\EuropaCheckVatService\Reference.cs`**:
    -   Remove this file, as it is no longer needed.
-   **(Inferred) A service class that uses the EuropaCheckVatService (e.g., `VatService.cs` or similar):**
    -   Modify the service class to use a new REST API client for the EuropaCheckVatService.
    -   Create a new file, `EuropaCheckVatApiClient.cs`, to contain the REST API client.
    -   Implement methods in the `EuropaCheckVatApiClient.cs` file to check VAT numbers using the EuropaCheckVatService REST API.

**Common:**

-   **`source\nopCommerce\src\Nop.Services\Nop.Services.csproj`**:
    -   Remove the `System.ServiceModel.Http` dependency.

#### Configuration Changes

**UPS Shipping Rate Computation:**

-   Update the plugin's configuration page to allow the user to enter the REST API credentials, such as the client ID and client secret.
-   Update the plugin's configuration logic to use the new REST API endpoint.

**EuropaCheckVatService:**

-   Update the application's configuration to store the REST API endpoint for the EuropaCheckVatService.

### Implementation Tasks

#### Task 1: Create REST API Client for UPS (`UpsRestApiClient.cs`)

-   Create a new file named `UpsRestApiClient.cs` in the `source\nopCommerce\src\Plugins\Nop.Plugin.Shipping.UPS\Services` directory.
-   Implement methods to get the shipping rates and track shipments using the UPS REST API.
-   Use the `HttpClient` class to make calls to the UPS REST API endpoints.
-   Implement OAuth 2.0 authentication to get the access token.
-   Handle errors and exceptions.

```csharp
using System.Net.Http;
using System.Threading.Tasks;
using Nop.Plugin.Shipping.UPS.Domain;

namespace Nop.Plugin.Shipping.UPS.Services
{
    public class UpsRestApiClient
    {
        private readonly HttpClient _httpClient;
        private readonly UPSSettings _upsSettings;

        public UpsRestApiClient(HttpClient httpClient, UPSSettings upsSettings)
        {
            _httpClient = httpClient;
            _upsSettings = upsSettings;
        }

        public async Task<string> GetShippingRatesAsync(string request)
        {
            // Implement the logic to get the shipping rates from the UPS REST API.
            // Use the _httpClient to make the API call.
            // Handle the authentication and error handling.
            return await _httpClient.GetStringAsync(request);
        }

        public async Task<string> TrackShipmentAsync(string trackingNumber)
        {
            // Implement the logic to track the shipment using the UPS REST API.
            // Use the _httpClient to make the API call.
            // Handle the authentication and error handling.
            return await _httpClient.GetStringAsync(trackingNumber);
        }
    }
}
```

#### Task 2: Update `UPSComputationMethod.cs`

-   Modify the constructor to inject the `UpsRestApiClient` instead of the `UPSService`.
-   Update the `GetShippingOptionsAsync` method to call the `GetShippingRatesAsync` method of the `UpsRestApiClient` to get the shipping rates.
-   Update the `GetShipmentTrackerAsync` method to call the `TrackShipmentAsync` method of the `UpsRestApiClient` to track the shipment.

```csharp
using Nop.Core;
using Nop.Plugin.Shipping.UPS.Domain;
using Nop.Plugin.Shipping.UPS.Services;
using Nop.Services.Configuration;
using Nop.Services.Localization;
using Nop.Services.Plugins;
using Nop.Services.Shipping;
using Nop.Services.Shipping.Tracking;

namespace Nop.Plugin.Shipping.UPS;

/// <summary>
/// Represents UPS computation method
/// </summary>
public class UPSComputationMethod : BasePlugin, IShippingRateComputationMethod
{
    #region Fields

    private readonly ILocalizationService _localizationService;
    private readonly ISettingService _settingService;
    private readonly IWebHelper _webHelper;
    private readonly UpsRestApiClient _upsRestApiClient;

    #endregion

    #region Ctor

    public UPSComputationMethod(ILocalizationService localizationService,
        ISettingService settingService,
        IWebHelper webHelper,
        UpsRestApiClient upsRestApiClient)
    {
        _localizationService = localizationService;
        _settingService = settingService;
        _webHelper = webHelper;
        _upsRestApiClient = upsRestApiClient;
    }

    #endregion

    #region Methods

    /// <summary>
    ///  Gets available shipping options
    /// </summary>
    /// <param name="getShippingOptionRequest">A request for getting shipping options</param>
    /// <returns>
    /// A task that represents the asynchronous operation
    /// The task result contains the represents a response of getting shipping rate options
    /// </returns>
    public async Task<GetShippingOptionResponse> GetShippingOptionsAsync(GetShippingOptionRequest getShippingOptionRequest)
    {
        ArgumentNullException.ThrowIfNull(getShippingOptionRequest);

        if (!getShippingOptionRequest.Items?.Any() ?? true)
            return new GetShippingOptionResponse { Errors = new[] { "No shipment items" } };

        if (getShippingOptionRequest.ShippingAddress?.CountryId == null)
            return new GetShippingOptionResponse { Errors = new[] { "Shipping address is not set" } };

        // Call the GetShippingRatesAsync method of the UpsRestApiClient to get the shipping rates.
        var rates = await _upsRestApiClient.GetShippingRatesAsync(getShippingOptionRequest.ToString());

        return new GetShippingOptionResponse(); // Replace with actual implementation
    }

    /// <summary>
    /// Gets fixed shipping rate (if shipping rate computation method allows it and the rate can be calculated before checkout).
    /// </summary>
    /// <param name="getShippingOptionRequest">A request for getting shipping options</param>
    /// <returns>
    /// A task that represents the asynchronous operation
    /// The task result contains the fixed shipping rate; or null in case there's no fixed shipping rate
    /// </returns>
    public Task<decimal?> GetFixedRateAsync(GetShippingOptionRequest getShippingOptionRequest)
    {
        return Task.FromResult<decimal?>(null);
    }

    /// <summary>
    /// Get associated shipment tracker
    /// </summary>
    /// <returns>
    /// A task that represents the asynchronous operation
    /// The task result contains the shipment tracker
    /// </returns>
    public Task<IShipmentTracker> GetShipmentTrackerAsync()
    {
        return Task.FromResult<IShipmentTracker>(new UPSShipmentTracker(_upsRestApiClient));
    }

    /// <summary>
    /// Gets a configuration page URL
    /// </summary>
    public override string GetConfigurationPageUrl()
    {
        return $"{_webHelper.GetStoreLocation()}Admin/UPSShipping/Configure";
    }

    /// <summary>
    /// Install plugin
    /// </summary>
    /// <returns>A task that represents the asynchronous operation</returns>
    public override async Task InstallAsync()
    {
        //settings
        await _settingService.SaveSettingAsync(new UPSSettings
        {
            UseSandbox = true,
            CustomerClassification = CustomerClassification.StandardListRates,
            PickupType = PickupType.OneTimePickup,
            PackagingType = PackagingType.ExpressBox,
            PackingPackageVolume = 5184,
            PackingType = PackingType.PackByDimensions,
            PassDimensions = true,
            WeightType = "LBS",
            DimensionsType = "IN",
            RequestTimeout = UPSDefaults.RequestTimeout
        });

        //locales
        await _localizationService.AddOrUpdateLocaleResourceAsync(new Dictionary<string, string>
        {
            ["Enums.Nop.Plugin.Shipping.UPS.PackingType.PackByDimensions"] = "Pack by dimensions",
            ["Enums.Nop.Plugin.Shipping.UPS.PackingType.PackByOneItemPerPackage"] = "Pack by one item per package",
            ["Enums.Nop.Plugin.Shipping.UPS.PackingType.PackByVolume"] = "Pack by volume",
            ["Plugins.Shipping.UPS.Fields.AccountNumber"] = "Account number",
            ["Plugins.Shipping.UPS.Fields.AccountNumber.Hint"] = "Specify UPS account number (required to get negotiated rates).",
            ["Plugins.Shipping.UPS.Fields.AdditionalHandlingCharge"] = "Additional handling charge",
            ["Plugins.Shipping.UPS.Fields.AdditionalHandlingCharge.Hint"] = "Enter additional handling fee to charge your customers.",
            ["Plugins.Shipping.UPS.Fields.AvailableCarrierServices"] = "Carrier Services",
            ["Plugins.Shipping.UPS.Fields.AvailableCarrierServices.Hint"] = "Select the services you want to offer to customers.",
            ["Plugins.Shipping.UPS.Fields.ClientId"] = "Client ID",
            ["Plugins.Shipping.UPS.Fields.ClientId.Hint"] = "Specify UPS client ID.",
            ["Plugins.Shipping.UPS.Fields.ClientSecret"] = "Client secret",
            ["Plugins.Shipping.UPS.Fields.ClientSecret.Hint"] = "Specify UPS client secret.",
            ["Plugins.Shipping.UPS.Fields.CustomerClassification"] = "UPS Customer Classification",
            ["Plugins.Shipping.UPS.Fields.CustomerClassification.Hint"] = "Choose customer classification.",
            ["Plugins.Shipping.UPS.Fields.DimensionsType"] = "Dimensions type",
            ["Plugins.Shipping.UPS.Fields.DimensionsType.Hint"] = "Choose dimensions type (inches or centimeters).",
            ["Plugins.Shipping.UPS.Fields.InsurePackage"] = "Insure package",
            ["Plugins.Shipping.UPS.Fields.InsurePackage.Hint"] = "Check to insure packages.",
            ["Plugins.Shipping.UPS.Fields.PackagingType"] = "UPS Packaging Type",
            ["Plugins.Shipping.UPS.Fields.PackagingType.Hint"] = "Choose UPS packaging type.",
            ["Plugins.Shipping.UPS.Fields.PackingPackageVolume"] = "Package volume",
            ["Plugins.Shipping.UPS.Fields.PackingPackageVolume.Hint"] = "Enter your package volume.",
            ["Plugins.Shipping.UPS.Fields.PackingType"] = "Packing type",
            ["Plugins.Shipping.UPS.Fields.PackingType.Hint"] = "Choose preferred packing type.",
            ["Plugins.Shipping.UPS.Fields.PassDimensions"] = "Pass dimensions",
            ["Plugins.Shipping.UPS.Fields.PassDimensions.Hint"] = "Check if you want to pass package dimensions when requesting rates.",
            ["Plugins.Shipping.UPS.Fields.PickupType"] = "UPS Pickup Type",
            ["Plugins.Shipping.UPS.Fields.PickupType.Hint"] = "Choose UPS pickup type.",
            ["Plugins.Shipping.UPS.Fields.SaturdayDeliveryEnabled"] = "Saturday Delivery enabled",
            ["Plugins.Shipping.UPS.Fields.SaturdayDeliveryEnabled.Hint"] = "Check to get rates for Saturday Delivery options.",
            ["Plugins.Shipping.UPS.Fields.Tracing"] = "Tracing",
            ["Plugins.Shipping.UPS.Fields.Tracing.Hint"] = "Check if you want to record plugin tracing in System Log. Warning: The entire request and response will be logged (including Client Id/secret, AccountNumber). Do not leave this enabled in a production environment.",
            ["Plugins.Shipping.UPS.Fields.UseSandbox"] = "Use sandbox",
            ["Plugins.Shipping.UPS.Fields.UseSandbox.Hint"] = "Check to use sandbox (testing environment).",
            ["Plugins.Shipping.UPS.Fields.WeightType"] = "Weight type",
            ["Plugins.Shipping.UPS.Fields.WeightType.Hint"] = "Choose the weight type (pounds or kilograms).",
            ["Plugins.Shipping.UPS.Tracker.Arrived"] = "Arrived",
            ["Plugins.Shipping.UPS.Tracker.Booked"] = "Booked",
            ["Plugins.Shipping.UPS.Tracker.Delivered"] = "Delivered",
            ["Plugins.Shipping.UPS.Tracker.Departed"] = "Departed",
            ["Plugins.Shipping.UPS.Tracker.ExportScanned"] = "Export scanned",
            ["Plugins.Shipping.UPS.Tracker.NotDelivered"] = "Not delivered",
            ["Plugins.Shipping.UPS.Tracker.OriginScanned"] = "Origin scanned",
            ["Plugins.Shipping.UPS.Tracker.Pickup"] = "Pickup"
        });

        await base.InstallAsync();
    }

    /// <summary>
    /// Uninstall plugin
    /// </summary>
    /// <returns>A task that represents the asynchronous operation</returns>
    public override async Task UninstallAsync()
    {
        //settings
        await _settingService.DeleteSettingAsync<UPSSettings>();

        //locales
        await _localizationService.DeleteLocaleResourcesAsync("Enums.Nop.Plugin.Shipping.UPS");
        await _localizationService.DeleteLocaleResourcesAsync("Plugins.Shipping.UPS");

        await base.UninstallAsync();
    }

    #endregion
}
```

#### Task 3: Update `UPSSettings.cs`

-   Add properties for the REST API credentials, such as the client ID and client secret.

```csharp
using Nop.Core.Configuration;
using Nop.Plugin.Shipping.UPS.Domain;

namespace Nop.Plugin.Shipping.UPS;

/// <summary>
/// Represents settings of the UPS shipping plugin
/// </summary>
public class UPSSettings : ISettings
{
    /// <summary>
    /// Gets or sets the account number
    /// </summary>
    public string AccountNumber { get; set; }

    /// <summary>
    /// Gets or sets the client ID
    /// </summary>
    public string ClientId { get; set; }

    /// <summary>
    /// Gets or sets the client secret
    /// </summary>
    public string ClientSecret { get; set; }

    /// <summary>
    /// Gets or sets a value indicating whether to use sandbox environment
    /// </summary>
    public bool UseSandbox { get; set; }

    /// <summary>
    /// Gets or sets an amount of the additional handling charge
    /// </summary>
    public decimal AdditionalHandlingCharge { get; set; }

    /// <summary>
    /// Gets or sets UPS customer classification
    /// </summary>
    public CustomerClassification CustomerClassification { get; set; }

    /// <summary>
    /// Gets or sets a pickup type
    /// </summary>
    public PickupType PickupType { get; set; }

    /// <summary>
    /// Gets or sets packaging type
    /// </summary>
    public PackagingType PackagingType { get; set; }

    /// <summary>
    /// Gets or sets offered carrier services
    /// </summary>
    public string CarrierServicesOffered { get; set; }

    /// <summary>
    /// Gets or sets a value indicating whether Saturday Delivery enabled
    /// </summary>
    public bool SaturdayDeliveryEnabled { get; set; }

    /// <summary>
    /// Gets or sets a value indicating whether to insure packages
    /// </summary>
    public bool InsurePackage { get; set; }

    /// <summary>
    /// Gets or sets a value indicating whether to pass package dimensions
    /// </summary>
    public bool PassDimensions { get; set; }

    /// <summary>
    /// Gets or sets the packing package volume
    /// </summary>
    public int PackingPackageVolume { get; set; }

    /// <summary>
    /// Gets or sets packing type
    /// </summary>
    public PackingType PackingType { get; set; }

    /// <summary>
    /// Gets or sets a value indicating whether to record plugin tracing in log
    /// </summary>
    public bool Tracing { get; set; }

    /// <summary>
    /// Gets or sets package weight type (LBS or KGS)
    /// </summary>
    public string WeightType { get; set; }

    /// <summary>
    /// Gets or sets package dimensions type (IN or CM)
    /// </summary>
    public string DimensionsType { get; set; }

    /// <summary>
    /// Gets or sets a period (in seconds) before the request times out
    /// </summary>
    public int? RequestTimeout { get; set; }
}
```

#### Task 4: Remove `System.ServiceModel.Http` Dependency

-   Open the `source\nopCommerce\src\Nop.Services\Nop.Services.csproj` file.
-   Remove the following line:

```xml
<PackageReference Include="System.ServiceModel.Http" Version="8.1.0" />
```

#### Task 5: Create REST API Client for EuropaCheckVatService (`EuropaCheckVatApiClient.cs`)

-   Create a new file named `EuropaCheckVatApiClient.cs` in the `source\nopCommerce\src\Libraries\Nop.Services` directory.
-   Implement methods to check VAT numbers using the EuropaCheckVatService REST API.
-   Use the `HttpClient` class to make calls to the EuropaCheckVatService REST API endpoints.
-   Handle errors and exceptions.

```csharp
using System.Net.Http;
using System.Threading.Tasks;

namespace Nop.Services
{
    public class EuropaCheckVatApiClient
    {
        private readonly HttpClient _httpClient;

        public EuropaCheckVatApiClient(HttpClient httpClient)
        {
            _httpClient = httpClient;
        }

        public async Task<string> CheckVatNumberAsync(string countryCode, string vatNumber)
        {
            // Implement the logic to check the VAT number using the EuropaCheckVatService REST API.
            // Use the _httpClient to make the API call.
            // Handle the authentication and error handling.
            return await _httpClient.GetStringAsync($"https://europa.eu/vat/{countryCode}/{vatNumber}"); // Replace with actual endpoint
        }
    }
}
```

#### Task 6: Update the service class that uses the EuropaCheckVatService

-   Identify the service class that uses the EuropaCheckVatService (e.g., `VatService.cs` or similar).
-   Modify the constructor to inject the `EuropaCheckVatApiClient` instead of the generated SOAP client proxy.
-   Update the method that checks VAT numbers to call the `CheckVatNumberAsync` method of the `EuropaCheckVatApiClient`.

### Data Models

The data models that are used in the SOAP and REST services are different. The SOAP service uses XML-based data contracts, while the REST service uses JSON-based data contracts. You will need to create new C# classes that match the JSON request and response structures of the UPS Rating REST API and the EuropaCheckVatService REST API.

### Authentication

The SOAP service likely uses a static API key or username/password in the SOAP header. The REST service may use a different authentication mechanism, such as OAuth 2.0 or API keys. You will need to implement the appropriate authentication mechanism for each service.

### Risk Assessment

-   **High Risk**:
    -   **Rate Inconsistency**: The new REST API might calculate or return rates differently than the SOAP API, leading to incorrect shipping charges. Mitigation: Parallel testing and validation against the old API.
    -   **VAT Number Validation Failure**: The new REST API might not validate VAT numbers correctly, leading to incorrect tax calculations. Mitigation: Thorough testing and validation against the old API.
    -   **Authentication Failure**: Incorrect implementation of OAuth 2.0 or other authentication mechanisms could block all shipping rate calculations or VAT number validations. Mitigation: Thorough testing in a sandbox environment.
-   **Medium Risk**:
    -   **Performance Degradation**: The new API, while generally faster, could have higher latency for certain requests. Mitigation: Performance baselining and testing.
    -   **Breaking Changes**: If the REST API lacks certain features of the SOAP API, it could break business logic that depends on them. Mitigation: Thorough API analysis before implementation.
-   **Low Risk**:
    -   **Data Model Mismatches**: Minor differences in data fields between the XML and JSON contracts. Mitigation: Careful data mapping and validation.

### Action Items

**Immediate (1-2 days)**:

-   [ ] **Confirm Target APIs**: Confirm that UPS and EuropaCheckVatService provide modern REST APIs for shipping rate calculation and VAT number validation, and obtain their documentation and sandbox credentials.
-   [ ] **Analyze `UPSService` and `EuropaCheckVatService`**: Perform a detailed code review of the `UPSService` and the service class that uses the `EuropaCheckVatService` to fully document the existing SOAP operations and data contracts.

**Short-term (1-2 weeks)**:

-   [ ] **Develop REST Clients**: Implement the new `UpsRestApiClient` and `EuropaCheckVatApiClient` with appropriate authentication and methods for getting shipping rates and validating VAT numbers.
    -   [ ] Create `UpsRestApiClient.cs`
    -   [ ] Implement OAuth 2.0 authentication for UPS
    -   [ ] Implement methods for getting shipping rates for UPS
    -   [ ] Create `EuropaCheckVatApiClient.cs`
    -   [ ] Implement authentication for EuropaCheckVatService
    -   [ ] Implement methods for validating VAT numbers for EuropaCheckVatService
-   [ ] **Unit Test REST Clients**: Write unit tests for the new clients, mocking the HTTP responses from the APIs.

**Long-term (2-3 weeks)**:

-   [ ] **Integrate and Test**: Replace the SOAP clients with the new REST clients in `UPSComputationMethod` and the service class that uses the `EuropaCheckVatService`, and perform full integration and regression testing.
    -   [ ] Modify the constructor of `UPSComputationMethod.cs`
    -   [ ] Update the `GetShippingOptionsAsync` method in `UPSComputationMethod.cs`
    -   [ ] Update the `GetShipmentTrackerAsync` method in `UPSComputationMethod.cs`
    -   [ ] Modify the constructor of the service class that uses the `EuropaCheckVatService`
    -   [ ] Update the method that checks VAT numbers in the service class that uses the `EuropaCheckVatService`
-   [ ] **Deploy and Monitor**: Deploy the updated plugin and service to a staging environment for UAT and monitor performance and accuracy before a production release.
