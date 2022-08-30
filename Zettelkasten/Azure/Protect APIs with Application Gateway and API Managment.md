222022-08-12

Style: 

Domain:

Tags:

# Protect APIs with Application Gateway and API Managment



Example Architecture Overview:

![[Pasted image 20220812115352.png]]

### Workflow

-   Application Gateway sets up a URL redirection mechanism that sends the request to the proper [backend pool](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-components#backend-pools), depending on the URL format of the API call:
    
    -   URLs formatted like `api.<some-domain>/external/*` can reach the back end to interact with the requested APIs.
        
    -   Calls formatted as `api.<some-domain>/*` go to a dead end, which is a back-end pool with no target.
        
-   API Management accepts and properly maps internal calls, which come from resources in the same Azure virtual network, under `api.<some-domain>/internal/*`.
    
-   Finally, at the API Management level, APIs are set up to accept calls under the following patterns:
    
    -   `api.<some-domain>/external/*`
    -   `api.<some-domain>/internal/*`
    
    In this scenario, API Management uses two types of IP addresses, public and private. Public IP addresses are for internal communication on port 3443, and for runtime API traffic in the external virtual network configuration. When API Management sends a request to a public internet-facing back end, it shows a public IP address as the origin of the request. For more information, see [IP addresses of API Management service in VNet](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-ip-addresses#ip-addresses-of-api-management-service-in-vnet).
    
-   A rule at the Application Gateway level properly redirects users under `portal.<some-domain>/*` to the developer portal, so that developers can manage APIs and their configurations from both internal and external environments.

### Recommendations
-   To communicate with private resources in the back end, Application Gateway and API Management must be in the same virtual network as the resources. Before implementing the solution, set up a virtual network for your resources. The solution creates subnets for Application Gateway and API Management.
    
-   The private, internal deployment model allows API Management to connect to an existing virtual network, making it reachable from the inside of that network context. To enable this feature, deploy either the **Developer** or **Premium**API Management tiers.
    
-   Application Gateway requires PFX certificates for SSL termination. Make sure these certificates are in place before you implement the solution.
    
-   Manage certificates and passwords in [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/basic-concepts).
    
-   To personalize interactions with the services, you can use [CNAME entries](https://docs.microsoft.com/en-us/azure/dns/dns-web-sites-custom-domain).
    
-   You can use other services to deliver the same level of firewall and Web Application Firewall (WAF) protection:
    
    -   [Azure Front Door](https://docs.microsoft.com/en-us/azure/frontdoor/front-door-overview)
    -   [Azure Firewall](https://docs.microsoft.com/en-us/azure/firewall/overview)




___
# References
[Protect APIs with Azure Application Gateway and Azure API Management - Azure Reference Architectures | Microsoft Docs](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/apis/protect-apis)