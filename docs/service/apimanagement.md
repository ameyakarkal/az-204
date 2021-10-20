
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
    - inbound : execute when request is received from client
    - backend : execute before forwarding to backend managed api
    - outbound: execute before sending response to client
    - onerror : execute after error is raised
- access restriction
    - rate limit by key | by subscription
    - authentication
    - usage quota / bandwidth quota
    - header policies
- advanced 
    - mock response
    - call backend service
    - change http method
    - retry endpoints
    - add logging / tracing
- transformation
    - content-type : XML to JSON or JSON to XML
    - replace content in the body
- caching
    - 
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
       IsProtected
       List<Api>
       List<Subscription>
       Policy
       Publish(Api)
       AddSubscription(Developer)
    }

    class Api {
        List<Operation>
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

