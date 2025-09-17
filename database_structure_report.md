# nopCommerce Database Structure Report

**1. Supported Database Types:**

*   The application supports SQL Server, MySQL, and PostgreSQL.
*   Version requirements for each database are not explicitly specified in the code, but the `MsSqlDataProvider` uses `SqlServerVersion.v2012`, suggesting a minimum SQL Server version.
*   Connection string formats are determined by the specific data provider (e.g., `SqlConnectionStringBuilder` for SQL Server, `MySqlConnectionStringBuilder` for MySQL, `NpgsqlConnectionStringBuilder` for PostgreSQL).
*   Database-specific implementations and limitations:
    *   MySQL does not support backup/restore operations through the data provider.
    *   SQL Server's hashing function is limited to 8000 bytes.
    *   PostgreSQL uses sequences for identity columns.

**2. Database Schemas Present:**

*   Default schema names are not explicitly defined in the code.
*   Custom schema support is not evident in the data provider implementations.
*   Schema naming conventions:
    *   Table names generally follow PascalCase (e.g., `Product`, `Category`).
    *   Column names generally follow PascalCase, but some older tables might use snake_case (legacy underscore).
    *   Foreign key naming conventions: `FK_{ForeignTable}_{ForeignColumn}_{PrimaryTable}_{PrimaryKey}` (truncated and hashed for MySQL).
    *   Index naming conventions: `IX_{TargetTable}_{TargetColumn}` (truncated and hashed for MySQL).
*   Multi-tenant schema support is not explicitly implemented.

**3. Complete Table List:**

Here's a list of tables organized by functional areas, based on the entity definitions in `src/Libraries/Nop.Core/Domain`:

*   **Customer Management Tables:**
    *   `Customer`: Stores customer information (Id, CustomerGuid, Username, Email, FirstName, LastName, etc.).
    *   `CustomerAttribute`: Stores customer attribute definitions (Id, Name).
    *   `CustomerAttributeValue`: Stores customer attribute values (Id, CustomerAttributeId, Name, Value).
    *   `CustomerCustomerRoleMapping`: Maps customers to customer roles (CustomerId, CustomerRoleId).
    *   `CustomerRole`: Stores customer role information (Id, Name, SystemName).
    *   `ExternalAuthenticationRecord`: Stores external authentication information (Id, CustomerId, ExternalIdentifier, OAuthToken, OAuthAccessToken).
    *   `RewardPointsHistory`: Stores reward points history for customers (Id, CustomerId, Points, PointsBalance, Message).
    *   `CustomerPassword`: Stores customer passwords (Id, CustomerId, Password, PasswordFormat, PasswordSalt, CreatedOnUtc).
*   **Product Catalog Tables:**
    *   `Product`: Stores product information (Id, Name, ShortDescription, FullDescription, Price, Sku, etc.).
    *   `Category`: Stores category information (Id, Name, Description, ParentCategoryId, PictureId, etc.).
    *   `ProductCategory`: Maps products to categories (ProductId, CategoryId, IsFeaturedProduct, DisplayOrder).
    *   `Manufacturer`: Stores manufacturer information (Id, Name, Description, PictureId, etc.).
    *   `ProductManufacturer`: Maps products to manufacturers (ProductId, ManufacturerId, IsFeaturedProduct, DisplayOrder).
    *   `ProductTag`: Stores product tag information (Id, Name).
    *   `ProductProductTagMapping`: Maps products to product tags (ProductId, ProductTagId).
    *   `ProductAttribute`: Stores product attribute definitions (Id, Name).
    *   `ProductAttributeValue`: Stores product attribute values (Id, ProductAttributeMappingId, Name, PriceAdjustment, WeightAdjustment, IsPreSelected, DisplayOrder).
    *   `ProductAttributeMapping`: Maps products to product attributes (Id, ProductId, ProductAttributeId, TextPrompt, IsRequired, AttributeControlTypeId, DisplayOrder).
    *   `ProductAttributeCombination`: Stores product attribute combinations (Id, ProductId, Sku, Price, StockQuantity).
    *   `ProductAttributeCombinationPicture`: Maps product attribute combinations to pictures (ProductAttributeCombinationId, PictureId).
    *   `SpecificationAttribute`: Stores specification attribute definitions (Id, Name, SpecificationAttributeGroupId).
    *   `SpecificationAttributeOption`: Stores specification attribute options (Id, SpecificationAttributeId, Name, DisplayOrder).
    *   `ProductSpecificationAttribute`: Maps products to specification attributes (ProductId, SpecificationAttributeOptionId, AttributeTypeId, CustomValue, AllowFiltering, ShowOnProductPage, DisplayOrder).
    *   `ReviewType`: Stores review types (Id, Name, Description, DisplayOrder, IsRequired).
    *   `ProductReviewReviewTypeMapping`: Maps product reviews to review types (ProductReviewId, ReviewTypeId, Rating).
    *   `TierPrice`: Stores tier prices for products (Id, ProductId, StoreId, CustomerRoleId, Quantity, Price).
    *   `RelatedProduct`: Stores related products (Id, ProductId1, ProductId2, DisplayOrder).
    *   `BackInStockSubscription`: Stores back-in-stock subscriptions (Id, ProductId, CustomerId, CreatedOnUtc, SentEmail).
    *   `ProductWarehouseInventory`: Stores product warehouse inventory (Id, ProductId, WarehouseId, StockQuantity, ReservedQuantity).
    *   `ProductVideo`: Stores product videos (Id, ProductId, VideoId, DisplayOrder).
    *   `Download`: Stores downloadable files (Id, DownloadGuid, Filename, Extension, MimeType, DownloadBinary).
    *   `Picture`: Stores picture files (Id, MimeType, SeoFilename, AltAttribute, TitleAttribute, IsNew).
    *   `PictureBinary`: Stores picture binary data (Id, PictureId, BinaryData).
    *   `ProductPicture`: Stores product picture mappings (Id, ProductId, PictureId, DisplayOrder).
*   **Order Management Tables:**
    *   `Order`: Stores order information (Id, OrderGuid, CustomerId, BillingAddressId, ShippingAddressId, OrderTotal, PaymentMethodSystemName, etc.).
    *   `OrderItem`: Stores order item information (Id, OrderId, ProductId, Quantity, UnitPriceInclTax, PriceInclTax, DiscountAmountInclTax, AttributesXml).
    *   `Shipment`: Stores shipment information (Id, OrderId, TrackingNumber, ShippedDateUtc, DeliveryDateUtc, etc.).
    *   `ShipmentItem`: Stores shipment item information (Id, ShipmentId, OrderItemId, Quantity).
    *   `RecurringPayment`: Stores recurring payment information (Id, CustomerId, InitialOrderId, CycleLength, CyclePeriod, TotalCycles, IsActive, etc.).
    *   `RecurringPaymentHistory`: Stores recurring payment history (Id, RecurringPaymentId, OrderId, CreatedOnUtc, CycleNumber, RecurringPaymentCycleStatus).
    *   `GiftCard`: Stores gift card information (Id, GiftCardGuid, Amount, IsGiftCardActivated, GiftCardCouponCode, RecipientName, RecipientEmail, SenderName, SenderEmail, IsRecipientNotified, CreatedOnUtc).
    *   `GiftCardUsageHistory`: Stores gift card usage history (Id, GiftCardId, UsedWithOrderId, UsedValue, CreatedOnUtc).
    *   `ReturnRequest`: Stores return request information (Id, CustomerId, OrderId, ReasonForReturn, RequestedAction, CustomerComments, StaffNotes, ReturnRequestStatusId, CreatedOnUtc).
    *   `OrderNote`: Stores order notes (Id, OrderId, Note, DisplayToCustomer, CreatedOnUtc).
*   **Inventory Tables:**
    *   `ProductWarehouseInventory`: Stores product inventory information for each warehouse (ProductId, WarehouseId, StockQuantity, ReservedQuantity).
*   **Content Management Tables:**
    *   `Topic`: Stores topic information (Id, SystemName, IsSystemTopic, IncludeInTopMenu, IsAccessibleWhenStoreIsClosed, DisplayOrder, AvailableStartDateTimeUtc, AvailableEndDateTimeUtc, Title, Body, MetaKeywords, MetaDescription, MetaTitle, LimitedToStores, SubjectToAcl, Published, CreatedOnUtc, UpdatedOnUtc).
    *   `BlogPost`: Stores blog post information (Id, LanguageId, Title, Short, Full, MetaKeywords, MetaDescription, MetaTitle, AllowComments, Tags, StartDateUtc, EndDateUtc, Published, CreatedOnUtc).
    *   `NewsItem`: Stores news item information (Id, LanguageId, Title, Short, Full, MetaKeywords, MetaDescription, MetaTitle, AllowComments, Published, CreatedOnUtc).
*   **Security & Permissions Tables:**
    *   `AclRecord`: Stores access control list (ACL) records (Id, EntityId, EntityName, CustomerRoleId).
    *   `PermissionRecord`: Stores permission records (Id, Name, SystemName, Category).
    *   `PermissionRecordCustomerRoleMapping`: Maps permission records to customer roles (PermissionRecordId, CustomerRoleId).
*   **Localization Tables:**
    *   `Language`: Stores language information (Id, Name, LanguageCulture, UniqueSeoCode, FlagImageFileName, Rtl, Published, DisplayOrder).
    *   `LocaleStringResource`: Stores localized string resources (Id, LanguageId, ResourceName, ResourceValue).
    *   `LocalizedProperty`: Stores localized property values (Id, EntityId, LanguageId, LocaleKeyGroup, LocaleKey, LocaleValue).
*   **System Configuration Tables:**
    *   `Setting`: Stores configuration settings (Id, Name, Value, StoreId).
*   **Logging & Audit Tables:**
    *   `ActivityLogType`: Stores activity log type information (Id, SystemKeyword, Name, Enabled).
    *   `ActivityLog`: Stores activity log entries (Id, ActivityLogTypeId, CustomerId, Comment, CreatedOnUtc, IpAddress, EntityId, EntityName).
    *   `Log`: Stores log entries (Id, LogLevelId, ShortMessage, FullMessage, CustomerId, PageUrl, IpAddress, CreatedOnUtc).
*   **Other Supporting Tables:**
    *   `UrlRecord`: Stores URL slugs for SEO (Id, EntityId, EntityName, Slug, LanguageId, IsActive).
    *   `ScheduleTask`: Stores scheduled task information (Id, Name, Seconds, Type, Enabled, StopOnError, LastStartUtc, LastEndUtc, LastSuccessUtc).
    *   `TaxCategory`: Stores tax category information (Id, Name, DisplayOrder).
    *   `Currency`: Stores currency information (Id, Name, CurrencyCode, DisplayLocale, Rate, DisplayOrder, Published).
    *   `MeasureDimension`: Stores measure dimension information (Id, Name, SystemKeyword, Ratio).
    *   `MeasureWeight`: Stores measure weight information (Id, Name, SystemKeyword, Ratio).
    *   `StateProvince`: Stores state/province information (Id, CountryId, Name, Abbreviation).
    *   `Download`: Stores download information (Id, DownloadGuid, Filename, Extension, MimeType, DownloadBinary).
    *   `Picture`: Stores picture information (Id, MimeType, SeoFilename, AltAttribute, TitleAttribute, IsNew).
    *   `PictureBinary`: Stores picture binary data (Id, PictureId, BinaryData).
    *   `ProductPicture`: Stores product picture mappings (Id, ProductId, PictureId, DisplayOrder).
    *   `Video`: Stores video information (Id, VideoUrl).

**4. Entity Relationship Diagram Information:**

Due to the complexity of the database schema, a complete ER diagram is difficult to represent in text. However, here are some key relationships:

*   **Customer - Order (One-to-Many):** A customer can have multiple orders.
*   **Order - OrderItem (One-to-Many):** An order can have multiple order items.
*   **Product - OrderItem (One-to-Many):** A product can be in multiple order items.
*   **Product - ProductCategory (Many-to-Many):** Products and categories have a many-to-many relationship, represented by the ProductCategory table.
*   **Product - ProductTag (Many-to-Many):** Products and product tags have a many-to-many relationship, represented by the ProductProductTagMapping table.
*   **Product - ProductAttribute (One-to-Many):** A product can have multiple product attributes.
*   **Category - Category (Hierarchical):** Categories can have parent categories.
*   **Product - SpecificationAttributeOption (Many-to-Many):** Products and specification attribute options have a many-to-many relationship, represented by the ProductSpecificationAttribute table.
*   **Customer - Address (One-to-Many):** A customer can have multiple addresses, and can have a billing and shipping address.
*   **Discount - Category (Many-to-Many):** Discounts and categories have a many-to-many relationship, represented by the DiscountCategoryMapping table.
*   **Discount - Product (Many-to-Many):** Discounts and products have a many-to-many relationship, represented by the DiscountProductMapping table.
*   **Discount - Manufacturer (Many-to-Many):** Discounts and manufacturers have a many-to-many relationship, represented by the DiscountManufacturerMapping table.

**5. Dependency Diagram:**

Creating a complete dependency diagram would be very complex. However, here's a simplified view:

*   **Independent Tables:**
    *   `Country`
    *   `Currency`
    *   `Language`
    *   `TaxCategory`
    *   `ActivityLogType`
    *   `PermissionRecord`
    *   `TopicTemplate`
    *   `CategoryTemplate`
    *   `ManufacturerTemplate`
    *   `ShippingMethod`
    *   `DeliveryDate`
    *   `ProductAvailabilityRange`
    *   `RecurringProductCyclePeriod`
    *   `RentalPricePeriod`
    *   `Download`
    *   `Video`
    *   `EmailAccount`
*   **Level 1 Dependencies:** (Tables that depend only on independent tables)
    *   `StateProvince` (depends on `Country`)
    *   `Product` (depends on `Vendor`, `TaxCategory`, `ProductType`, `DownloadActivationType`, `GiftCardType`, `ManageInventoryMethod`, `RecurringProductCyclePeriod`, `RentalPricePeriod`)
    *   `Category` (depends on `CategoryTemplate`, `Picture`)
    *   `Manufacturer` (depends on `Picture`)
    *   `LocaleStringResource` (depends on `Language`)
    *   `Setting` (depends on `Store`)
*   **Level 2+ Dependencies:** (Tables with multiple levels of dependencies)
    *   `Order` (depends on `Customer`, `Address`, `PaymentMethod`, `ShippingMethod`)
    *   `OrderItem` (depends on `Order`, `Product`)
    *   `ProductCategory` (depends on `Product`, `Category`)
    *   `ProductManufacturer` (depends on `Product`, `Manufacturer`)
    *   `ProductSpecificationAttribute` (depends on `Product`, `SpecificationAttributeOption`)
    *   `ShoppingCartItem` (depends on `Customer`, `Product`)
    *   `ActivityLog` (depends on `ActivityLogType`, `Customer`)
    *   `AclRecord` (depends on `CustomerRole`)

**6. Special Database Features:**

*   **Custom Indexes:** The `Indexes.cs` file defines a number of custom indexes to improve query performance.
*   **Stored Procedures/Functions/Triggers/Views/Partitioning Strategies/Caching Mechanisms:** No evidence of these features was found in the analyzed files.
*   **Migration and Versioning Strategies:** The application uses FluentMigrator for database schema migrations. The migration classes are located in the `src/Libraries/Nop.Data/Migrations` directory.

**7. Naming Conventions & Compatibility:**

*   Table naming patterns: PascalCase (e.g., `Product`, `Category`).
*   Column naming patterns: PascalCase, but some older tables might use snake_case (legacy underscore).
*   Foreign key naming conventions: `FK_{ForeignTable}_{ForeignColumn}_{PrimaryTable}_{PrimaryKey}` (truncated and hashed for MySQL).
*   Index naming conventions: `IX_{TargetTable}_{TargetColumn}` (truncated and hashed for MySQL).
*   Backward compatibility is handled by the `NameCompatibilityManager` class, which provides mappings for older table and column names.

This report provides a comprehensive overview of the database structure based on the analyzed files. Please note that this is a static analysis and might not reflect the actual database schema in its entirety.
