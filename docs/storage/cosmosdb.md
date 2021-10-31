# Azure CosmosDB
## Reference 
[Microsoft Azure Developer : Develop Solutions with CosmosDB](https://app.pluralsight.com/library/courses/microsoft-azure-developer-develop-solutions-cosmos-db-storage/table-of-contents)

## Agenda
- data / api
- consistency level
- partitioning
- change feed notification
---
- low latency
- high SLA
- multi-region replication

## properties
- structure
    - account
        - database
            - container (similar to table)
                - items ()

```bash
# create account
az cosmodb create --name 'cosmodb-account-name' --resource group 'rg'

# create database
az cosmodb sql create \
--name 'dbname' \
--account-name 'cosmodb-account-name'

# create container
az cosmodb sql container create \
--name 'container-name' \
--database-name 'dbname' \
--account-name 'cosmodb-account-name' \
# specifiy partitioning key
--partition-key-path '/id'
```

- supported api
    - sql
    - cassendra
    - mongoDB
    - gremlin
    - azure table

- throughput
    - measured in RU. 1 RU = read 1kb from container
    - provisioned at the container level
    - provisioned vs serverless
        - provisioned : always on
            - manual scaling
            - autoscaling : increase provisioned RUs
- consistency level
    - specified at account level
    - override per request when making the request
    - stong / bounded require twice the RU for the same operation
    - STRONG CONSISTENCY
        - guarantees client always reads most recent change. similar to DB
        - latency : HIGHEST
        - throughput : LOWEST
        - availability : LOWEST
    - BOUNDED STALENESS
        - guarantees client always reads recent change with a expected lag measured by version or time
    - SESSION CONSISTENCY
        - guarantees user can read its own recent changes
    - CONSISTENCY PREFIX CONSISTENCY
        - writes are made in the same order
    - EVENTUAL : 
        - no guarante of order
        - latency : LOWEST
        - throughput : HIGHEST
        - availability : HIGHEST
- partitioning
    - throughput is distributed across partitions equally
    - logical  (say shard)
        - max limit 20 gb
        - one or more logical partition mapped to physical 
    - physical        
    - partition key 
    - replica set