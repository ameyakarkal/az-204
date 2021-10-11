
# Azure API Management

> reference : <br/>
> - Pluralsight Course : [Microsoft Azure Developer: Implement API Management](https://app.pluralsight.com/library/courses/microsoft-azure-developer-implement-api-management/table-of-contents)


- components
    - API Gateway service
    - Administration portal
    - Developer portal 
- integrate api
- security
- caching

## API Gateway Service
- routes requests to backend service
- provides authentication
- provides caching
- provides rate limiting and quotas

### Products
- groups api together
- "packaging" for API
- open or closed(subscription , needs developer to get client id / client key) 

### Groups
- "manage" visibility of products

### Policy
- applied to API / Product / Api / Operation
- types
    - inbound : request received from client
    - backend : before forwarding to backend managed api
    - outbound: before sending response to client
    - onerror : after error is raised

:::mermaid
    classDiagram
    direction LR
    class Gateway {
        List<Group>
        List<Product>
        Policy
    }

    class Group{
        List<Product>
        List<Developer>
        AddProduct()
    }

    class Product{
       List<Api>
       Policy
       IsProtected
       Publish(Api)
    }

    class Api {
        List<Operation>
        List<Subscription>
        AddSubscription()
    }
    class Developer

    class Policy

    Group*--Product
    Group*--Developer
    Product*--Api

    Gateway*--Policy
    Product*--Policy
    Api*--Policy
    Operation*--Policy
    
:::

:::mermaid
    classDiagram
    direction TD

    class Policy

    Policy<|--AccessRestriction
    AccessRestriction<|--RateLimit
    AccessRestriction<|--TokenValidation
    AccessRestriction<|--Quota
    Quota<|--QuotaPerKey
    Quota<|--QuotaPerSubscription

    Policy<|--Behavior
    Behavior<|--MockResponse
    Behavior<|--ForwardRequest

    Policy<|--Transformation
    Policy<|--Caching
:::

