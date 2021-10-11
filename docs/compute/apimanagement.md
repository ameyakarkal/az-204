
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


:::mermaid
    classDiagram
    direction LR
    class Group{
        List<Product>
        List<Developer>
        AddProduct()
    }

    class Product{
       List<Api>
       IsProtected
       Publish(Api)
    }

    class Api {
        List<Operation>
        List<Subscription>
        AddSubscription()
    }
    class Developer

    Group*--Product
    Group*--Developer
    Product*--Api
:::