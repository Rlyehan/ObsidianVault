222022-08-19

Style: #CourseNotes 

Domain: #Security 

Tags: #Certification

# Configure and manage SQL database security

## Enable SQL database authentication
-   **SQL authentication** - With this authentication method, the user submits a user account name and associated password to establish a connection. This password is stored in the master database for user accounts linked to a login or stored in the database containing the user accounts not linked to a login.
-   **Azure Active Directory Authentication** - With this authentication method, the user submits a user account name and requests that the service use the credential information stored in Azure Active Directory.

-   A **login** is an individual account in the master database, to which a user account in one or more databases can be linked. With a login, the credential information for the user account is stored with the login.
-   A **user account** is an individual account in any database that may be but does not have to be linked to a login. With a user account that is not linked to a login, the credential information is stored with the user account.

**Authorization** to access data and perform various actions are managed using database roles and explicit permissions. Authorization refers to the permissions assigned to a user, and determines what that user is allowed to do. Authorization is controlled by your user account's database role memberships and object-level permissions. As a best practice, you should grant users the least privileges necessary. As a best practice, your application should use a dedicated account to authenticate. This way, you can limit the permissions granted to the application and reduce the risks of malicious activity in case the application code is vulnerable to a SQL injection attack. The recommended approach is to create a contained database user, which allows your app to authenticate directly to the database.


## Configure SQL database firewalls
Whenever possible, as a best practice, use database-level IP firewall rules to enhance security and to make your database more portable. Use server-level IP firewall rules for administrators and when you have several databases with the same access requirements, and you don't want to spend time configuring each database individually.


## Enable and monitor database auditing
You can use SQL database auditing to:

-   **Retain** an audit trail of selected events. You can define categories of database actions to be audited.
-   **Report** on database activity. You can use pre-configured reports and a dashboard to get started quickly with activity and event reporting.
-   **Analyze** reports. You can find suspicious events, unusual activity, and trends.


## Implement data discovery and classification

Data discovery and classification provides advanced capabilities built into Azure SQL Database for discovering, classifying, labeling and protecting sensitive data (such as business, personal data, and financial information) in your databases. Discovering and classifying this data can play a pivotal role in your organizational information protection stature. It can serve as infrastructure for:

-   Helping meet data privacy standards and regulatory compliance requirements.
-   Addressing various security scenarios such as monitoring, auditing, and alerting on anomalous access to sensitive data.
-   Controlling access to and hardening the security of databases containing highly sensitive data.

Data discovery and classification is part of the Advanced Data Security offering, which is a unified package for advanced Microsoft SQL Server security capabilities. You access and manage data discovery and classification via the central SQL Advanced Data Security portal.

Data discovery and classification introduces a set of advanced services and SQL capabilities, forming a SQL Information Protection paradigm aimed at protecting the data, not just the database:

-   **Discovery and recommendations** - The classification engine scans your database and identifies columns containing potentially sensitive data. It then provides you with an easier way to review and apply the appropriate classification recommendations via the Azure portal.
-   **Labeling** - Sensitivity classification labels can be persistently tagged on columns using new classification metadata attributes introduced into the SQL Server Engine. This metadata can then be utilized for advanced sensitivity-based auditing and protection scenarios.
-   **Query result set sensitivity** - The sensitivity of the query result set is calculated in real time for auditing purposes.
-   **Visibility** - You can view the database classification state in a detailed dashboard in the Azure portal. Additionally, you can download a report (in Microsoft Excel format) that you can use for compliance and auditing purposes, in addition to other needs.


## Dynamic data masking
### Dynamic data masking basics

You set up a dynamic data masking policy in the Azure portal by selecting the dynamic data masking operation in your SQL Database configuration blade or settings blade. This feature cannot be set by using portal for Azure Synapse

### Dynamic data masking policy

-   **SQL users excluded from masking** - A set of SQL users or AAD identities that get unmasked data in the SQL query results. Users with administrator privileges are always excluded from masking, and view the original data without any mask.
-   **Masking rules** - A set of rules that define the designated fields to be masked and the masking function that is used. The designated fields can be defined using a database schema name, table name, and column name.
-   **Masking functions** - A set of methods that control the exposure of data for different scenarios.

## Implement transparent data encryption
TDE performs real-time I/O encryption and decryption of the data at the page level. Each page is decrypted when it's read into memory and then encrypted before being written to disk. TDE encrypts the storage of an entire database by using a symmetric key called the Database Encryption Key (DEK). On database startup, the encrypted DEK is decrypted and then used for decryption and re-encryption of the database files in the SQL Server Database Engine process. DEK is protected by the TDE protector. TDE protector is either a service-managed certificate (service-managed transparent data encryption) or an asymmetric key stored in Azure Key Vault (customer-managed transparent data encryption).

## Deploy always encrypt features
Always Encrypted is a feature designed to protect sensitive data, such as credit card numbers or national identification numbers (for example, U.S. social security numbers), stored in Azure SQL Database or SQL Server databases. Always Encrypted allows clients to encrypt sensitive data inside client applications and never reveal the encryption keys to the Database Engine (SQL Database or SQL Server). As a result, Always Encrypted provides a separation between users who own the data (and can view it) and users who manage the data (but should have no access). **By ensuring on-premises database administrators, cloud database operators, or other high-privileged, but unauthorized users, cannot access the encrypted data**. Always Encrypted enables customers to confidently store sensitive data outside of their direct control. So, Always Encrypted allows organizations to encrypt data at rest and in use for storage in Azure, to enable delegation of on-premises database administration to third parties, or to reduce security clearance requirements for their own DBA staff.

Always Encrypted makes encryption transparent to applications. An Always Encrypted-enabled driver installed on the client computer achieves this by automatically encrypting and decrypting sensitive data in the client application. The driver encrypts the data in sensitive columns before passing the data to the Database Engine, and automatically rewrites queries so that the semantics to the application are preserved. Similarly, the driver transparently decrypts data, stored in encrypted database columns, contained in query results.



___
# References
[Configure and manage SQL database security - Learn | Microsoft Docs](https://docs.microsoft.com/en-us/learn/modules/sql-database-security/)