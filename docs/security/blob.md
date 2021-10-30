# Azure 
## References
- [Microsoft Azure Developer : Authentication and Authorization](https://app.pluralsight.com/library/courses/microsoft-azure-developer-implement-user-authentication-authorization/table-of-contents)

## Role Assignment
- Define "security principal" has "role" over "Scope"
- Deny assignment precides over everything else
- Key Concepts involved
    - Security Principal
        - user
        - groups
        - service principal
        - managed identity
    - Role Definition
        - actions
        - data-actions
        - not-actions
        - not-data-actions
    - Scope : limit scope of "access"

```
# hierarchy in azure
- management group
    - subcription
        - resource group
            - resource
```
## Blob storage
- Components requiring security
    - Management Plane (RBAC): managing account. SP / Users who have access to the account
    - Data plane : managing access to document / blob in account
    - Encryption : encrypt data/blobs stored in account
- Shared Keys
    - associated to root account
    - rotate keys
    - least privilege
- Shared Access Signature Token
    - built on top of shared keys
    - attached to specific permissions
    - attached to specific time period
    - user delegated SAS
        - attached to Azure AD
    - service SAS
        - secured with storage account key
        - specific to storage service (queue|table|blob|file)
    - account SAS
        - secured with storage account key
        - specific to one or more storage service (queue|table|blob|file)
    - type / kinds of SAS
        - adhoc : generated per requirement
        - structure
            - url                       : uri
            - signed-version            : version
            - signed-service            : azure service that generated this token qt : queue and table
            - signed-resource-type      : type of service e.g s = storage
            - signed-protocol           : protocol used e.g http
            - signed-start/signed-expiry: defines time period for token
            - signature                 : hmac generated using key
        - stored access policy
            - setup on container of the resource
            - available as service level SAS
            - structure
                - signed-resource   : 
                - signed-i          : name of the policy
                - signature         : hmac generated using key
- RBAC
    - Azure AD
