# Web App Service
## App Service Plan
- App Service Plan
    - Shared    : infrastructure is shared with other
        - Basic
        - Standard
        - Premium
    - Isolated App Service Environment
    
## Custom Domain
- Allows to set up a custom domain. 
- Domain validation
    - create a record set CNAME with alias to xx.azurewebsite.net
    - you can also create a txt record asuid.xx.azurewebsite.net with a value of custom domain verification id
- TSL/SSL settings
    - create private certificate 
        - upload existing certificate
        - import from key vault
        - create a managed private certificate
    - add a TSL/SSL certificate binding that uses the certificate created; SNI Server Name Indication Type can be used if multiple app services use the same certificate
- Logging
    - app logging
    - server logging
    - error trace
    - trace logging

:::mermaid
    classDiagram

    class App

    class AppServicePlan
    {
        SKU PlanType
    }

 
:::