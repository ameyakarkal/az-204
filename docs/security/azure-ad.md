## References
- [Microsoft Azure Developer : Implement User Authentication and Authorization](https://app.pluralsight.com/library/courses/microsoft-azure-developer-implement-user-authentication-authorization/table-of-contents)

## Microsoft Identity Platform
- Authentication Service
- Open Source Libraries
- Application Management tools

- Authentication service
    - Azure AD
    - AD Connect    : on premise product to sync on premise
    - ADFS          :  
- Open source libraries
    - ADAL : deprecation
    - MSAL : microsoft authentication library
    - Microsoft.Identity.Web
        - specific for dotnet core
- Application Management tools
    - Authorization : what are you allowed to access
    - Consent : allowing apps to access , admins allowing users to allow apps to access
    - Logs 


## Authentication : who you are ?
### Identity
- WSFed / SAML
    - based on redirection.
    - not suitable for mobile
- OAuth
    - not a protocol
    - user deletes access to an app.
    - Open ID Connect builds on top of OAuth and assigns an identity

### Open ID Connect

:::mermaid
    sequenceDiagram;
    participant client as client
    participant api as api
    participant AD as Azure AD
    participant TAPI as Third API  

    client-->>api: Access Token
    api-->>AD: Access Token
    AD-->>api: onBehalf(token)
    api-->>TAPI: get data
    TAPI-->>api: data
:::

### Tokens
> register app -> portal -> app-registration
- token configuration (lets you configure what a token should carry)
- api permissions 
    - delegated permission      : app uses the user identity to access data
    - application permission    : app accesses data as itself, NOT the logged in user
- kinds / type
    - access token
        - identity
        - what you wish to do
        - authorized
        - api's responisibility : look/unpack/make decision
    - id token
        - user identity
    - refresh token
        - get another access token


## Authorization : what can you do ?
- decisions of authorization can be based on
    - Groups : the group a user belongs can determine if the person/app has access to resource
    - Custom Claim : value of a claim in the ID token can be used to determine access to resource
    - App role : defined at the resource level. user/apps are added these roles
- if you want properties to appear in the access token
    - setup on Api
- if you want properties to appear in the id token
    - setup on App
### Group Claim
App is requesting a token for Api : with group claim
1. create group "Group"
2. add "App" to "Group"
3. "App" -> Permissions -> add "Api" to suggest that "App has access to Api"
4. "App" -> Secret -> create secret
5. request token for "Api" as client "App"
6. Api -> token configuration -> Add Group Claims and select Security Groups
7. request token for "Api" as client "App" and groups show up in the token generated

### Token Claim
App is requesting a token for Api : with optional claim
1. "App" -> Permissions -> add "Api" to suggest that "App has access to Api"
2. "App" -> Secret -> create secret
3. request token for "Api" as client "App"
4. Api -> token configuration -> Add optional token Claims and select Access token -> country property
5. request token for "Api" as client "App" and shows country in token generated

### App role based token
1. "Api" -> App role -> create app role "Admin"
2. "App" -> permissions -> when adding permissions to "App" select role "Admin"
3. "App" -> Secret -> create secret
4. request token for "Api" as client "App"
5. request token for "Api" as client "App" shows roles in token generated
