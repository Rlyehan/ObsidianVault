222022-08-19

Style: 

Domain: #Security 

Tags: #Azure 

# Azure Functions Security

### Function access keys

Functions lets you use keys to make it harder to access your HTTP function endpoints during development. Unless the HTTP access level on an HTTP triggered function is set to `anonymous`, requests must include an API access key in the request.

While keys provide a default security mechanism, you may want to consider additional options to secure an HTTP endpoint in production. For example, it's generally not a good practice to distribute shared secret in public apps. If your function is being called from a public client, you may want to consider implementing another security mechanism. To learn more, see [Secure an HTTP endpoint in production](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook-trigger#secure-an-http-endpoint-in-production).

When you renew your function key values, you must manually redistribute the updated key values to all clients that call your function.

#### Enable App Service Authentication/Authorization

The App Service platform lets you use Azure Active Directory (AAD) and several third-party identity providers to authenticate clients. You can use this strategy to implement custom authorization rules for your functions, and you can work with user information from your function code. To learn more, see [Authentication and authorization in Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/overview-authentication-authorization) and [Working with client identities](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook-trigger#working-with-client-identities).

#### Use Azure API Management (APIM) to authenticate requests

APIM provides a variety of API security options for incoming requests. To learn more, see [API Management authentication policies](https://docs.microsoft.com/en-us/azure/api-management/api-management-authentication-policies). With APIM in place, you can configure your function app to accept requests only from the IP address of your APIM instance. To learn more, see [IP address restrictions](https://docs.microsoft.com/en-us/azure/azure-functions/ip-addresses#ip-address-restrictions).

#### Managed identities

A managed identity from Azure Active Directory (Azure AD) allows your app to easily access other Azure AD-protected resources such as Azure Key Vault. The identity is managed by the Azure platform and does not require you to provision or rotate any secrets. For more about managed identities in Azure AD, see [Managed identities for Azure resources](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview).

Your application can be granted two types of identities:

-   A **system-assigned identity** is tied to your application and is deleted if your app is deleted. An app can only have one system-assigned identity.
-   A **user-assigned identity** is a standalone Azure resource that can be assigned to your app. An app can have multiple user-assigned identities.

Managed identities can be used in place of secrets for connections from some triggers and bindings. See [Identity-based connections](https://docs.microsoft.com/en-us/azure/azure-functions/security-concepts?tabs=v4#identity-based-connections).

For more information, see [How to use managed identities for App Service and Azure Functions](https://docs.microsoft.com/en-us/azure/app-service/overview-managed-identity?toc=/azure/azure-functions/toc.json).

#### Restrict CORS access

[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) is a way to allow web apps running in another domain to make requests to your HTTP trigger endpoints. App Service provides built-in support for handing the required CORS headers in HTTP requests. CORS rules are defined on a function app level.

While it's tempting to use a wildcard that allows all sites to access your endpoint, this defeats the purpose of CORS, which is to help prevent cross-site scripting attacks. Instead, add a separate CORS entry for the domain of each web app that must access your endpoint.

#### Key Vault references

While application settings are sufficient for most many functions, you may want to share the same secrets across multiple services. In this case, redundant storage of secrets results in more potential vulnerabilities. A more secure approach is to a central secret storage service and use references to this service instead of the secrets themselves.

[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/overview) is a service that provides centralized secrets management, with full control over access policies and audit history. You can use a Key Vault reference in the place of a connection string or key in your application settings. To learn more, see [Use Key Vault references for App Service and Azure Functions](https://docs.microsoft.com/en-us/azure/app-service/app-service-key-vault-references?toc=/azure/azure-functions/toc.json).

### Deploy your function app in isolation

Azure App Service Environment (ASE) provides a dedicated hosting environment in which to run your functions. ASE lets you configure a single front-end gateway that you can use to authenticate all incoming requests. For more information, see [Configuring a Web Application Firewall (WAF) for App Service Environment](https://docs.microsoft.com/en-us/azure/app-service/environment/integrate-with-application-gateway).

### Use a gateway service

Gateway services, such as [Azure Application Gateway](https://docs.microsoft.com/en-us/azure/application-gateway/overview) and [Azure Front Door](https://docs.microsoft.com/en-us/azure/frontdoor/front-door-overview) let you set up a Web Application Firewall (WAF). WAF rules are used to monitor or block detected attacks, which provide an extra layer of protection for your functions. To set up a WAF, your function app needs to be running in an ASE or using Private Endpoints (preview). To learn more, see [Using Private Endpoints](https://docs.microsoft.com/en-us/azure/app-service/networking/private-endpoint).

### NS-1: Implement security for internal traffic

**Guidance**: When you deploy Azure Functions resources, create or use an existing virtual network. Ensure that all Azure virtual networks follow an enterprise segmentation principle that aligns with the business risks. Any system that might incur higher risk for the organization should be isolated within its own virtual network and sufficiently secured with a network security group (NSG) and/or Azure Firewall.

Use Microsoft Defender for Cloud Adaptive Network Hardening to recommend network security group configurations that limit ports and source IPs based with the reference to external network traffic rules.

Azure Functions has two primary ways to deploy resources within a network context. A function app may be created on the Elastic Premium plan with service endpoints or private endpoints for inbound requests virtual network integration with forced tunneling for outbound requests. A function app may also be deployed completely within a virtual network by running in an App Service Environment. When operating in these modes, networking rules and restrictions still need to be set for dependencies such as the storage account used by Functions, as well as any event or data sources.

Use Microsoft Sentinel to discover the use of legacy insecure protocols such as SSL/TLSv1, SMBv1, LM/NTLMv1, wDigest, Unsigned LDAP Binds, and weak ciphers in Kerberos.

Function apps are created by default to support TLS 1.2 as a minimum version, but an app can be configured with a lower version through a configuration setting. HTTPS is not required of incoming requests by default, but this can also be set via a configuration setting, at which point any HTTP request will be automatically redirected to use HTTPS.

Certain event sources consumed by an app may require opening of additional ports to or from the network, but in the default case this is not necessary.

Function apps can be configured with IP restrictions for inbound requests. These are set via a priority list of Allow or Deny rules over IP blocks, virtual network subnets, or service tags.

-   [Azure Functions networking options](https://docs.microsoft.com/en-us/azure/azure-functions/functions-networking-options)
-   [Tutorial: Integrate Azure Functions with an Azure virtual network by using private endpoints](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-vnet)
-   [Integrate your app with an Azure virtual network](https://docs.microsoft.com/en-us/azure/app-service/web-sites-integrate-with-vnet)
-   [Azure Functions Premium plan](https://docs.microsoft.com/en-us/azure/azure-functions/functions-premium-plan)
-   [Introduction to the App Service Environments](https://docs.microsoft.com/en-us/azure/app-service/environment/intro)
-   [Set up access restrictions](https://docs.microsoft.com/en-us/azure/app-service/app-service-ip-restrictions)
-   [Using private endpoints for Azure Functions](https://docs.microsoft.com/en-us/azure/app-service/networking/private-endpoint)
-   [Tutorial: Establish Azure Functions private site access](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-private-site-access)



___
# References
[Securing Azure Functions | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-functions/security-concepts?tabs=v4)

[Azure security baseline for Azure Functions | Microsoft Docs](https://docs.microsoft.com/en-us/security/benchmark/azure/baselines/functions-security-baseline)