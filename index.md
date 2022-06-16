<script src="https://unpkg.com/mermaid@9.1.2/dist/mermaid.min.js"></script>

# XYZ Corp

## Problem Summary
* XYZ Corp has existing Azure SQL Server schemas 
* Trading partners (customers) unable execute actions without direct involvement from XYZ Corp staff
    * Unnecessary overhead, loss of sales due to sluggish process for clients
    * Customer actions include:
        * Placing an order
        * Viewing order status
        * Updating an order
        * Searching catalog
        * Getting product details
        * Getting product inventory
* Don’t want to lose or transfer existing database
    * Prefer to stay within Azure
*  API must only be accessible by authenticated and authorized users
    * System must allow API to be disabled/enabled on an ad-hoc basis
* API calls return different data depending on authorization level
    * Customers can only view their own orders
    * Product details and product inventory return data columns based on authorization level

## Solution Summary
* Leverage existing Azure SQL Server schemas 
* Use [Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis) to handle authentication and authorization 
    * Transfer existing, on-premise Active Directory management to Azure 
* Use Azure API Management to handle several key components: 
    * User authorization via Azure Active Directory 
    * Ability to enable/disable entire API system 
    * Ensure proper caching and rate limiting 
    * Key generation and validation automatically handled by Azure 
* Creation of a set of 6 APIs to manage all customer actions: 
    * Placing an order
    * Viewing order status 
    * Updating an order
    * Searching catalog
    * Getting product details
    * Getting product inventory
* API backend written in [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/?view=aspnetcore-6.0), [published to Azure API Management](https://docs.microsoft.com/en-us/aspnet/core/tutorials/publish-to-azure-api-management-using-vs?view=aspnetcore-6.0) 
    * Error returns/Execution success statements 
    * Input validation
    * Query parameterization
* Development and deployment performed using CI/CD pipeline with sandbox testing environment to prevent issues on deployment 

## Mermaid
<div class="mermaid">
graph TD
  A[User] -->|Makes Request| B(Intercept Request on Azure API)
  B -->|Forwards Request With Validated API Key| C{API}
  C -->|Requests Data| D[SQL Server]
  C -->|Updates Data|D
  C-->|Adds New Data|D
  C-->|Returns Data|B
  B-->|Returns request/status to User|A    
</div>

## Mermaid
<div class="mermaid">
graph TD
    A[API] --> B[Key Validation/Routing]
    B-->|Partial Info, Individual orders|F{Customer}
    B-->|All Information| C{Admin}
</div>
