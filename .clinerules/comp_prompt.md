# nopCommerce Database Structure Analysis Prompt
 
I need you to analyze the nopCommerce codebase and provide a comprehensive database structure report. Please examine the following key files and directories to extract detailed information:
 
## **Files to Analyze (in priority order):**
 
### **1. Data Provider Files (Database Support)**

- `DataProviders/MsSqlDataProvider.cs`

- `DataProviders/MySqlDataProvider.cs` 

- `DataProviders/PostgreSqlDataProvider.cs`

- `DataProviders/BaseDataProvider.cs`

- `INopDataProvider.cs`

- `DataProviderType.cs`
 
### **2. Entity Mapping/Builder Files (Table Structures)**

Analyze ALL files in these directories:

- `Mapping/Builders/Catalog/*.cs`

- `Mapping/Builders/Customers/*.cs`

- `Mapping/Builders/Orders/*.cs`

- `Mapping/Builders/Common/*.cs`

- `Mapping/Builders/Directory/*.cs`

- `Mapping/Builders/Discounts/*.cs`

- `Mapping/Builders/Forums/*.cs`

- `Mapping/Builders/Localization/*.cs`

- `Mapping/Builders/Logging/*.cs`

- `Mapping/Builders/Media/*.cs`

- `Mapping/Builders/Messages/*.cs`

- `Mapping/Builders/News/*.cs`

- `Mapping/Builders/Security/*.cs`

- `Mapping/Builders/Shipping/*.cs`

- `Mapping/Builders/Stores/*.cs`

- `Mapping/Builders/Tax/*.cs`

- `Mapping/Builders/Topics/*.cs`

- `Mapping/Builders/Vendors/*.cs`
 
### **3. Migration Files (Schema Information)**

- `Migrations/Installation/SchemaMigration.cs`

- `Migrations/Installation/Indexes.cs`

- Any `Migrations/UpgradeTo*/SchemaMigration.cs` files

- `NopMigrationAttribute.cs`
 
### **4. Core Infrastructure Files**

- `EntityRepository.cs`

- `Mapping/NopMappingSchema.cs`

- `Mapping/NameCompatibilityManager.cs`

- `Mapping/BaseNameCompatibility.cs`

- `IRepository.cs`
 
### **5. Domain Entity Files (for relationships)**

From the Core project, examine key entities:

- `Catalog/Product.cs`

- `Customers/Customer.cs`

- `Orders/Order.cs`

- `Orders/OrderItem.cs`

- `Orders/ShoppingCartItem.cs`

- `Common/Address.cs`

- And other major entity files
 
## **Analysis Requirements:**
 
Please provide a detailed report with the following sections:
 
### **1. Supported Database Types**

- List all database engines supported

- Version requirements for each database

- Connection string formats

- Any database-specific implementations or limitations
 
### **2. Database Schemas Present**

- Default schema names used

- Any custom schema support

- Schema naming conventions

- Multi-tenant schema support (if any)
 
### **3. Complete Table List**

Organize tables by functional areas:

- **Customer Management Tables**

- **Product Catalog Tables** 

- **Order Management Tables**

- **Inventory Tables**

- **Content Management Tables**

- **Security & Permissions Tables**

- **Localization Tables**

- **System Configuration Tables**

- **Logging & Audit Tables**

- **Other Supporting Tables**
 
For each table, provide:

- Table name

- Primary purpose

- Key columns (at least primary key and major foreign keys)

- Any special naming conventions or compatibility issues
 
### **4. Entity Relationship Diagram Information**

Create a comprehensive ER diagram description including:

- **Core entities and their relationships**

- **One-to-Many relationships** (Customer → Orders, Product → OrderItems, etc.)

- **Many-to-Many relationships** (Products ↔ Categories, etc.)

- **Foreign key constraints**

- **Lookup/reference tables**

- **Inheritance relationships** (if any)
 
Present this as:

- Text-based relationship descriptions

- Grouped by functional domains

- Include cardinality information
 
### **5. Dependency Diagram**

Show table dependencies in layers:

- **Independent tables** (no foreign key dependencies)

- **Level 1 dependencies** (tables that depend only on independent tables)

- **Level 2+ dependencies** (tables with multiple levels of dependencies)

- **Circular dependencies** (if any)
 
### **6. Special Database Features**

Document any:

- **Custom indexes** and their purposes

- **Stored procedures** or functions

- **Triggers** (if any)

- **Views** (if any)

- **Partitioning strategies**

- **Caching mechanisms**

- **Migration and versioning strategies**
 
### **7. Naming Conventions & Compatibility**

- Table naming patterns

- Column naming patterns  

- Foreign key naming conventions

- Index naming conventions

- Any backward compatibility considerations
 
## **Output Format:**
 
Please structure your response as a comprehensive technical document with:

1. **Executive Summary** of the database architecture

2. **Detailed sections** for each requirement above

3. **Visual representations** where possible (ASCII diagrams, tables, etc.)

4. **Technical notes** about implementation details

5. **Recommendations** for developers working with this database
 
## **Additional Context:**
 
Based on my research, nopCommerce:

- Uses Linq2DB as ORM (since v4.30)

- Uses FluentMigrator for schema migrations

- Supports 126+ tables in default installation

- Has mixed naming conventions (legacy underscore + modern PascalCase)

- Implements INameCompatibility for backward compatibility

- Uses IRepository<TEntity> pattern for data access
 
Please be thorough and provide actionable technical information that would help developers understand and work with the nopCommerce database structure.
 