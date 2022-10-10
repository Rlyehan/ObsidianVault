222022-08-09

Style: 

Domain: #Security 

Tags:

# Azure SQL Security

- Make use if virtual network rules to only allow access from selected subnets.
- Use SQL authentication only for an Admin access, normal access should be managed with AD, more here: [Azure Active Directory authentication - Azure SQL Database | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-aad-overview?view=azuresql)
- Make use of database roles to control and manage authorization on the DB, [Authorize server and database access using logins and user accounts - Azure SQL Database & SQL Managed Instance & Azure Synapse Analytics | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-sql/database/logins-create-manage?view=azuresql)

- Row-level security allows us to manage access down to the row level of the tables, [Row-Level Security - SQL Server | Microsoft Docs](https://docs.microsoft.com/en-us/sql/relational-databases/security/row-level-security)

- Make use of Advanced threat protection to help prevent SQL injection or other malicous attacks, [Configure Advanced Threat Protection - Azure SQL Database | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-sql/database/threat-detection-configure?view=azuresql)

- Our data is always encrypted in transit by default, it is best pratices to make sure we specify an encrypted connection and not trust server certificats.

- [Transparent data encryption - Azure SQL Database & SQL Managed Instance & Azure Synapse Analytics | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-sql/database/transparent-data-encryption-tde-overview?view=azuresql) Helps protect our data at rest.

- We should further secure our data with [Customer-managed transparent data encryption (TDE) - Azure SQL Database & SQL Managed Instance & Azure Synapse Analytics | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-sql/database/transparent-data-encryption-byok-overview?view=azuresql)

- [Always Encrypted - SQL Server | Microsoft Docs](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine)

- The dynamic data masling feature allows our SQL DBs to automatically detect sensitive data and mask it for non-priviliged users. [Dynamic data masking - Azure SQL Database | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-sql/database/dynamic-data-masking-overview?view=azuresql)

- Assign access rights to resources to Azure AD principals via group assignment: Create Azure AD groups, grant access to groups, and add individual members to the groups. In your database, create contained database users that map your Azure AD groups. To assign permissions inside the database, put the users that are associated with your Azure AD groups in database roles with the appropriate permissions.

	-   See the articles, [Configure and manage Azure Active Directory authentication with SQL](https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-aad-configure?view=azuresql) and [Use Azure AD for authentication with SQL](https://docs.microsoft.com/en-us/azure/azure-sql/database/authentication-aad-overview?view=azuresql).

-   Use [managed identities for Azure resources](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview).
    
    -   [System-assigned managed identity](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql)
    -   [User-assigned managed identity](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal)
    -   [Use Azure SQL Database from Azure App Service with managed identity (without code changes)](https://github.com/Azure-Samples/app-service-msi-entityframework-dotnet)
-   Use cert-based authentication for an application.
    
    -   See this [code sample](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/token).
-   Use Azure AD authentication for integrated federated domain and domain-joined machine (see section above).
    
    -   See the [sample application for integrated authentication](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/integrated).

-   Create and use custom roles with the exact permissions needed. Typical roles that are used in practice:
    
    -   Security deployment
    -   Administrator
    -   Developer
    -   Support personnel
    -   Auditor
    -   Automated processes
    -   End user


### Configure App Service for secure connections to SQL Database/SQL Managed Instance

**Best practices**:

-   For a simple Web App, connecting over public endpoint requires setting **Allow Azure Services** to ON.
    
-   [Integrate your app with an Azure Virtual Network](https://docs.microsoft.com/en-us/azure/app-service/overview-vnet-integration) for private data path connectivity to a managed instance. Optionally, you can also deploy a Web App with [App Service Environments (ASE)](https://docs.microsoft.com/en-us/azure/app-service/environment/intro).
    
-   For Web App with ASE or virtual network Integrated Web App connecting to a database in SQL Database, you can use [virtual network service endpoints and virtual network firewall rules](https://docs.microsoft.com/en-us/azure/azure-sql/database/vnet-service-endpoint-rule-overview?view=azuresql) to limit access from a specific virtual network and subnet. Then set **Allow Azure Services** to OFF. You can also connect ASE to a managed instance in SQL Managed Instance over a private data path.
    
-   Ensure that your Web App is configured per the article, [Best practices for securing platform as a service (PaaS) web and mobile applications using Azure App Service](https://docs.microsoft.com/en-us/azure/security/fundamentals/paas-applications-using-app-services).
    
-   Install [Web Application Firewall (WAF)](https://docs.microsoft.com/en-us/azure/web-application-firewall/ag/ag-overview) to protect your web app from common exploits and vulnerabilities.




___
# References
[Playbook for addressing common security requirements - Azure SQL Database & Azure SQL Managed Instance | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-sql/database/security-best-practice?view=azuresql)

[Security Overview - Azure SQL Database & Azure SQL Managed Instance | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-sql/database/security-overview?view=azuresql)
