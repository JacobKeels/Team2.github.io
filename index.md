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
