222022-08-12

Style: 

Domain: #Security 

Tags: #Azure 

# Azure API Managment Security

**Configuration Guidance**: Deploy Azure API Management inside an Azure Virtual Network (VNET), so it can access backend services within the network. The developer portal and API Management gateway can be configured to be accessible either from the Internet (External) or only within the Vnet (Internal).

-   External: the API Management gateway and developer portal are accessible from the public internet via an external load balancer. The gateway can access resources within the virtual network.
    -   [External Virtual Network Configuration](https://docs.microsoft.com/en-us/azure/api-management/api-management-using-with-vnet)
-   Internal: the API Management gateway and developer portal are accessible only from within the virtual network via an internal load balancer. The gateway can access resources within the virtual network.
    -   [Internal Virtual Network Configuraiton](https://docs.microsoft.com/en-us/azure/api-management/api-management-using-with-internal-vnet)

**Reference**: [Use a virtual network with Azure API Management](https://docs.microsoft.com/en-us/azure/api-management/virtual-network-concepts)



**Configuration Guidance**: Deploy network security groups (NSG) to your API Management subnets to restrict or monitor traffic by port, protocol, source IP address, or destination IP address. Create NSG rules to restrict your service's open ports (such as preventing management ports from being accessed from untrusted networks). Be aware that by default, NSGs deny all inbound traffic but allow traffic from virtual network and Azure Load Balancers.

Caution: When configuring an NSG on the API Management subnet, there are a set of ports that are required to be open. If any of these ports are unavailable, API Management may not operate properly and may become inaccessible.

**Note**: [Configure NSG rules for API Management](https://docs.microsoft.com/en-us/azure/api-management/api-management-using-with-vnet#configure-nsg-rules)

**Reference**: [Virtual network configuration reference: API Management](https://docs.microsoft.com/en-us/azure/api-management/virtual-network-reference)

**Configuration Guidance**: Use Azure Active Directory (Azure AD) as the default authentication method for API Management where possible.

-   Configure your Azure API Management Developer Portal to authenticate developer accounts by using Azure AD.
-   Configure your Azure API Management instance to protect your APIs by using the OAuth 2.0 protocol with Azure AD.

**Reference**: [Protect an API in Azure API Management using OAuth 2.0 authorization with Azure Active Directory](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-protect-backend-with-aad)




___
# References
[Azure security baseline for API Management | Microsoft Docs](https://docs.microsoft.com/en-us/security/benchmark/azure/baselines/api-management-security-baseline)