## Executive Summary

This report outlines a comprehensive test data strategy for the nopCommerce application, tailored to mainframe-style integration testing scenarios as requested. The analysis of the codebase reveals a modern, plugin-based architecture rather than direct mainframe integrations. However, analogous patterns exist that can be mapped to the required integration types (MFT, API, Messaging, Database).

The strategy details the creation of realistic, environment-specific test data for key business workflows such as product management, order processing, and customer data synchronization. It covers data requirements for integration, regression, and end-to-end testing, with a strong emphasis on data volume variations, boundary conditions, and error scenarios. The plan also includes procedures for automated data generation, PII masking for non-production environments, and data quality assurance to ensure robust and reliable testing.

## Analysis

### Test Data Strategy for Mainframe Distributed Apps Integration

This document provides a comprehensive test data strategy for ensuring the quality and reliability of integrations within the nopCommerce application, framed through the lens of Mainframe Distributed Apps (MDA) patterns.

### 1. Integration Testing Data

This data is designed to validate individual integration points, ensuring each component correctly processes data and communicates with its counterparts.

#### MFT (File Transfer) Integration Test Data

**Evidence**: The nopCommerce system includes robust export/import functionality, primarily managed by the `ExportManager` and `ImportManager` in the `Nop.Services` project. These services handle data transfer via XLSX and XML files, which is analogous to MFT.

**Business Purpose**: This data supports bulk product updates, order exports for external fulfillment systems, and customer data migration.

**File-based Data Requirements:**

```markdown
### Product Catalog Export/Import (XLSX)
**File Type**: Product Master Data
**Format**: XLSX with defined columns (Name, SKU, Price, StockQuantity, etc.)
**Volume Requirements**:
- Small Dataset: 50 products (~20KB)
- Medium Dataset: 5,000 products (~2MB)
- Large Dataset: 50,000 products (~20MB)
- Stress Dataset: 250,000 products (~100MB)

**File Naming Convention**: `products_export_YYYY-MM-DD-HH-MM-SS.xlsx`
**Sample Record Layout (Columns)**:
`ProductId`, `ProductType`, `ParentGroupedProductId`, `VisibleIndividually`, `Name`, `ShortDescription`, `FullDescription`, `Vendor`, `ProductTemplate`, `ShowOnHomepage`, `MetaKeywords`, `MetaDescription`, `MetaTitle`, `AllowCustomerReviews`, `SKU`, `ManufacturerPartNumber`, `Gtin`, `IsGiftCard`, `GiftCardType`, `OverriddenGiftCardAmount`, `RequireOtherProducts`, `RequiredProductIds`, `AutomaticallyAddRequiredProducts`, `IsDownload`, `DownloadId`, `UnlimitedDownloads`, `MaxNumberOfDownloads`, `DownloadActivationType`, `HasSampleDownload`, `SampleDownloadId`, `HasUserAgreement`, `UserAgreementText`, `IsRecurring`, `RecurringCycleLength`, `RecurringCyclePeriod`, `RecurringTotalCycles`, `IsRental`, `RentalPriceLength`, `RentalPricePeriod`, `IsShipEnabled`, `IsFreeShipping`, `ShipSeparately`, `AdditionalShippingCharge`, `DeliveryDate`, `IsTaxExempt`, `TaxCategory`, `ManageInventoryMethod`, `StockQuantity`, `DisplayStockAvailability`, `DisplayStockQuantity`, `MinStockQuantity`, `LowStockActivity`, `NotifyAdminForQuantityBelow`, `BackorderMode`, `AllowBackInStockSubscriptions`, `OrderMinimumQuantity`, `OrderMaximumQuantity`, `AllowedQuantities`, `DisableBuyButton`, `DisableWishlistButton`, `AvailableForPreOrder`, `PreOrderAvailabilityStartDateTimeUtc`, `CallForPrice`, `Price`, `OldPrice`, `ProductCost`, `CustomerEntersPrice`, `MinimumCustomerEnteredPrice`, `MaximumCustomerEnteredPrice`, `BasepriceEnabled`, `BasepriceAmount`, `BasepriceUnit`, `BasepriceBaseAmount`, `BasepriceBaseUnit`, `MarkAsNew`, `Published`, `CategoryIds`, `ManufacturerIds`, `Picture1`, `Picture2`, `Picture3`

**Environment-Specific File Locations**:
- **DEV**: `wwwroot/files/exportimport/dev_products.xlsx`
- **TEST**: `wwwroot/files/exportimport/test_products.xlsx`
- **STAGE**: `wwwroot/files/exportimport/stage_products.xlsx`

**Test Scenarios and Required Files**:
1.  **Valid File Processing**: Standard product file with all valid data types.
2.  **Invalid Record Handling**: File with 10% invalid records (e.g., text in `Price` column, non-existent `CategoryId`).
3.  **Empty File Processing**: A file with only a header row to test error handling.
4.  **Large File Processing**: 100MB file to test performance and memory usage during import.
5.  **Duplicate SKU Handling**: File with intentional duplicate SKUs to test update vs. insert logic.
6.  **Special Character Handling**: File with Unicode characters in product names and descriptions.
```

#### Apigee/API Integration Test Data

**Evidence**: The codebase contains numerous plugins for external API integrations, such as `Nop.Plugin.Payments.PayPalCommerce`, `Nop.Plugin.Shipping.UPS`, and `Nop.Plugin.Tax.Avalara`. These serve as excellent proxies for API gateway integrations. We will use the `UPS` shipping calculator as our example.

**Business Purpose**: This data is used to test real-time rate calculation with third-party shipping providers, a critical function for e-commerce checkout.

```json
### UPS Shipping Rate API Test Data
**API Endpoint**: (Simulated via `UPSComputationMethod.cs`)
**Authentication**: API Key, Username, Password

**Valid Request Payload (Conceptual)**:
{
  "Origin": {
    "CountryCode": "US",
    "PostalCode": "10001"
  },
  "Destination": {
    "CountryCode": "US",
    "PostalCode": "90210"
  },
  "Package": {
    "Weight": "5.5",
    "Dimensions": { "Length": "10", "Width": "8", "Height": "4" }
  },
  "Services": ["Ground", "NextDayAir"]
}

**Valid Response Payload (Conceptual)**:
{
  "Provider": "UPS",
  "Rates": [
    {
      "Service": "Ground",
      "Amount": 15.50,
      "DeliveryDays": 5
    },
    {
      "Service": "NextDayAir",
      "Amount": 45.75,
      "DeliveryDays": 1
    }
  ]
}

**Error Response Payload (Invalid Postal Code)**:
{
  "Error": "Invalid Destination Postal Code",
  "ErrorCode": "UPS-1001"
}
```

**API Test Data Scenarios**:

```markdown
### Authentication Test Data
**Valid Credentials**:
- TEST Environment: User: `[TEST_UPS_USER]`, Pass: `[TEST_UPS_PASS]`, Key: `[TEST_UPS_KEY]`

**Invalid Credentials**:
- Invalid Key: `[INVALID_UPS_KEY]`
- Incorrect Password: `[INCORRECT_PASSWORD]`

### Rate Limiting Test Data
**Normal Load**: 10 requests/minute
**Burst Load**: 200 requests/minute (to test potential throttling)
```

#### Kafka (Messaging) Integration Test Data

**Evidence**: While no direct Kafka integration exists, nopCommerce uses a robust internal eventing system (`IConsumer<T>`, `EventPublisher`). We will model test data for this system as if it were publishing to Kafka, using key business events.

**Business Purpose**: This data supports decoupled, asynchronous processes such as sending order confirmations, updating reporting databases, and notifying fulfillment centers.

```json
### Order and Customer Event Messages
**Topic**: `production-order-events`
**Partition Strategy**: By `customerId`
**Schema Version**: v1.0

**OrderPlacedEvent**:
{
  "messageId": "MSG-20241201-001",
  "eventType": "ORDER_PLACED",
  "timestamp": "2024-12-01T14:30:22.123Z",
  "source": "Nop.Web",
  "data": {
    "orderId": 12345,
    "orderGuid": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
    "customerId": 54321,
    "orderTotal": 199.99,
    "currency": "USD",
    "shippingMethod": "Ground",
    "itemCount": 3
  },
  "metadata": {
    "correlationId": "CORR-WEB-20241201-001"
  }
}

**CustomerRegisteredEvent**:
{
  "messageId": "MSG-20241201-002",
  "eventType": "CUSTOMER_REGISTERED",
  "timestamp": "2024-12-01T15:00:00.000Z",
  "source": "Nop.Web",
  "data": {
    "customerId": 54322,
    "email": "[REDACTED_EMAIL]",
    "firstName": "Jane",
    "lastName": "Doe",
    "registrationType": "Standard"
  },
  "metadata": {
    "correlationId": "CORR-WEB-20241201-002"
  }
}
```

#### AlloyDB (PostgreSQL) Integration Test Data

**Evidence**: The project supports PostgreSQL via the `Npgsql` driver in `Nop.Data.csproj`. The data model is defined by entities in `Nop.Core.Domain`.

**Business Purpose**: This data forms the core transactional database for the application, storing all customer, product, and order information.

**Database Test Datasets:**

```sql
### Customer Test Data (AlloyDB/PostgreSQL)
-- Small Dataset: 1,000 customers
-- Medium Dataset: 100,000 customers
-- Large Dataset: 1,000,000 customers

CREATE TABLE public."Customer" (
    "Id" integer NOT NULL,
    "Username" character varying(1000),
    "Email" character varying(1000),
    "FirstName" character varying(1000),
    "LastName" character varying(1000),
    "Active" boolean NOT NULL,
    "Deleted" boolean NOT NULL,
    "CreatedOnUtc" timestamp without time zone NOT NULL,
    "LastActivityDateUtc" timestamp without time zone NOT NULL,
    -- Other columns omitted for brevity
    PRIMARY KEY ("Id")
);

-- Sample customer records for testing
INSERT INTO public."Customer" ("Id", "Username", "Email", "FirstName", "LastName", "Active", "Deleted", "CreatedOnUtc", "LastActivityDateUtc") VALUES
(1, 'testuser1', '[REDACTED_EMAIL_1]', 'John', 'Doe', true, false, '2023-01-15 10:00:00', '2023-11-30 11:00:00'),
(2, 'testuser2', '[REDACTED_EMAIL_2]', 'Jane', 'Smith', true, false, '2023-02-20 12:00:00', '2023-11-29 13:00:00'),
(3, NULL, '[REDACTED_EMAIL_3]', 'Guest', 'User', true, false, '2023-03-25 14:00:00', '2023-11-28 15:00:00');

### Product Test Data (AlloyDB/PostgreSQL)
CREATE TABLE public."Product" (
    "Id" integer NOT NULL,
    "Name" character varying(400) NOT NULL,
    "Sku" character varying(400),
    "Price" numeric(18,4) NOT NULL,
    "StockQuantity" integer NOT NULL,
    "Published" boolean NOT NULL,
    "Deleted" boolean NOT NULL,
    -- Other columns omitted for brevity
    PRIMARY KEY ("Id")
);

-- Sample product records
INSERT INTO public."Product" ("Id", "Name", "Sku", "Price", "StockQuantity", "Published", "Deleted") VALUES
(1, 'Laptop Pro 15', 'LP15-2023', 1299.99, 50, true, false),
(2, 'Wireless Mouse', 'WM-BLK', 29.99, 200, true, false),
(3, 'Mechanical Keyboard', 'MK-RGB', 89.50, 0, true, false);
```

#### Oracle Database Integration Test Data

**Evidence**: No direct evidence of Oracle integration was found in the project's data providers or dependencies. The primary supported databases are MS SQL, MySQL, and PostgreSQL.

**Strategy**: A test data strategy for Oracle would mirror the AlloyDB (PostgreSQL) strategy. The key focus would be on:
1.  **Schema Differences**: Creating test data that exercises any data types or constraints unique to Oracle (e.g., `NVARCHAR2`, `CLOB`).
2.  **PL/SQL Validation**: If stored procedures were used, specific datasets would be required to test each procedure's logic, branches, and exception handling.
3.  **Batch Job Data**: Creating input data for any Oracle-specific batch jobs, similar to the MFT file strategy.

Without concrete implementation, a detailed data plan cannot be generated.

### 2. Regression and End-to-End Testing Data

This data is designed to simulate complete business workflows, ensuring that changes in one area do not negatively impact others.

```markdown
### End-to-End Business Process Test Data
**Process**: New Customer Onboarding with First Purchase

**Required Data Components**:
1.  **Customer Application Data**:
    -   Personal Info: `FirstName`, `LastName`, `Email: [UNIQUE_EMAIL]`, `Password`
    -   Address Info: `BillingAddress`, `ShippingAddress`
2.  **Product Data**:
    -   An in-stock product (e.g., `SKU: WM-BLK`, `StockQuantity: > 10`)
    -   An out-of-stock product (e.g., `SKU: MK-RGB`, `StockQuantity: 0`)
3.  **Payment Data**:
    -   Valid test credit card numbers (e.g., from a test gateway like Stripe or Braintree).
    -   Invalid/expired credit card numbers for failure testing.
4.  **Discount Data**:
    -   An active discount coupon code (e.g., `SAVE10`).

**Expected Data Flow & State Changes**:
1.  **Customer Registration**: A new record is created in the `Customer` table.
2.  **API/Event**: A `CustomerRegisteredEvent` is published.
3.  **Shopping Cart**: `ShoppingCartItem` records are created for the in-stock product. An attempt to add the out-of-stock product fails.
4.  **Checkout**: `Order` and `OrderItem` records are created with `OrderStatus: Pending`.
5.  **Payment API**: A request is sent to the payment gateway API with the correct order total.
6.  **Order Update**: Upon successful payment, `OrderStatus` changes to `Processing` and `PaymentStatus` to `Paid`.
7.  **API/Event**: An `OrderPlacedEvent` is published.
8.  **Inventory Update**: The `StockQuantity` for `SKU: WM-BLK` is decremented.
9.  **Order Export (MFT)**: The new order appears in the next daily order export file.
```

### 3. Environment-Specific Test Data Management

| Environment | Purpose | Data Volume | Refresh Policy | PII Masking |
| :--- | :--- | :--- | :--- | :--- |
| **DEV** | Unit & Feature Dev | ~10% of Prod | Weekly | Full PII Masking |
| **TEST** | Integration & E2E | ~50% of Prod | Daily | Partial PII Masking |
| **STAGE** | UAT & Performance | ~80-100% of Prod | On-demand | Minimal (non-PII only) |

### 4. Test Data Generation and Management

**Automated Data Generation**:
-   Utilize libraries like `Faker` (in C# tests or standalone scripts) to generate realistic customer names, addresses, and emails.
-   Create SQL scripts with loops and random functions to populate tables with large volumes of data for performance testing, as demonstrated in the AlloyDB section.

**Data Masking and Privacy**:
-   Implement SQL `UPDATE` scripts to be run after a database restore to non-production environments. These scripts should replace sensitive data with placeholders.
-   **Example Masking SQL**:
    ```sql
    -- Mask customer emails and names in a non-production environment
    UPDATE public."Customer"
    SET
        "Email" = 'user' || "Id" || '@example.com',
        "FirstName" = 'FirstName' || "Id",
        "LastName" = 'LastName' || "Id"
    WHERE "Id" > 0; -- Safety clause
    ```

### 5. Test Data Quality Assurance

**Data Validation Procedures**:
-   **Completeness**: All required fields for a given business process are populated.
-   **Accuracy**: Data formats are correct (e.g., emails, phone numbers).
-   **Consistency**: Referential integrity is maintained (e.g., `Order.CustomerId` exists in the `Customer` table).
-   **Security**: No production PII exists in DEV or TEST environments.

**Monitoring**:
-   Implement automated checks post-data-refresh to validate record counts and key relationships.
-   Monitor test data quality metrics, such as the rate of test failures caused by bad data.

## Assumptions Made
- The project's export/import functionality is a suitable analogue for MFT.
- The project's external plugin integrations (like UPS) are suitable analogues for Apigee/API gateway integrations.
- The internal event bus is a suitable analogue for Kafka-style messaging.
- The existing PostgreSQL support is a direct match for the AlloyDB requirement.
- No direct Oracle integration exists in the current codebase.

## Open Questions
- What are the specific performance SLAs for file import/export operations?
- Are there any third-party systems that consume the exported files, and what are their data format requirements?
- What are the latency and throughput requirements for the business eventing system?

## Confidence Level
**Overall Confidence**: High

**Rationale**: The nopCommerce codebase is well-structured with clear data models and business logic encapsulation. While direct mainframe patterns are absent, the application's e-commerce nature provides strong, analogous integration points (payments, shipping, data export) that map well to the requested test data strategy. The explicit support for PostgreSQL makes the AlloyDB data strategy straightforward to define. The primary assumption is the mapping of conceptual mainframe patterns to existing application features.

**Evidence**:
- **Data Models**: `Nop.Core.Domain` directory contains all entity definitions.
- **Data Providers**: `Nop.Data.DataProviders` shows support for `MsSql`, `MySql`, and `PostgreSql`.
- **File Exports**: `Nop.Services.ExportImport.ExportManager.cs` details the logic for exporting data to XLSX.
- **API Integrations**: The `Plugins` directory contains numerous examples of external API clients (e.g., `Nop.Plugin.Shipping.UPS`).
- **Eventing**: The `Nop.Services.Events` namespace and `IConsumer` interface define the publish-subscribe mechanism.

## Action Items
**Immediate**:
-   [ ] Develop data generation scripts (SQL or C#) for creating baseline customer and product data sets for DEV and TEST environments.
-   [ ] Create a masked and anonymized snapshot of the STAGE database to serve as a golden copy for performance testing.

**Short-term**:
-   [ ] Implement automated data quality checks to run after every test data refresh cycle.
-   [ ] Build a library of invalid and edge-case data files (XLSX) for negative testing of the import feature.

**Long-term**:
-   [ ] Investigate using containerized databases (e.g., PostgreSQL in Docker) within the CI/CD pipeline to provide ephemeral, isolated data for every test run.

## Risk Assessment
-   **High Risk**: Data integrity for financial transactions. Test data must cover complex scenarios including refunds, partial payments, and currency conversions.
-   **Medium Risk**: Performance degradation during bulk data imports. Large-scale test data (100k+ products) is essential to identify bottlenecks.
-   **Low Risk**: Inconsistencies in non-critical data like log entries. While important, the business impact is lower.