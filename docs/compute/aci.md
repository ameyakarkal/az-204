# Azure Container Registry | Container Instances
## Docker
### Docker File Structure
- FROM
- COPY 
- WORKDIR
- ENTRYPOINT
- RUN

## Azure Container Registry
- structure
    - sku
    - region
    - metadata
        - name
- authentication
    - roles
        - roles with access to ARM: owner|contributor|reader
            - assigned to users or SP
        - roles with access to just ACR : AcrPush  | AcrPull | AcrDelete
            - assigned to SP used in build tools
    - azure active directory
        - user 
        - service principal
        - ACR admin account : disabled by default
    - headless authentication (using azure active directory authtication) used build tools
- push
    - using docker
        - get loginServerName (dns for ACR)
        - `docker tag web:v1 $ACR_LOGIN_SERVER_URI/web:v1
        - `docker push $ACR_LOGIN_SERVER_URI/web:v1
    - acr task : build and push image to acr.
        - `az acr build --image web:v1 --registry $ACR_NAME . # dockerfile in current dir

## Azure Container Instances
- structure
    - region
    - meta
        - name
    - service
        - restart policy
        - os : windows | linux
        - grouped together
        - cpu : default 1.0
        - memory : default 1.5 (in gb)
        - azure files for backend
    - running an image
        - image from ACR
            - using PORTAL: needs admin user enabled ON ACR
            - using CLI: 
                - image from public registry
                    - same as image from ACR  without the SP details
                - image from ACR
                    - loginServer for ACR
                    - username of SP with `acrpull` access on ACR
                    - password of SP with `acrpul` access on ACR 
```bash
# create SP
ACR='name-azure-container-registry'
ACR_ID=$(az acr show \
    --name $ACR \
    --query id \
    --output tsv)
ACR_SP_NAME='name-of-sp'

# this is the password used to login into ACR
ACR_SP_PWD = $(az ad create-for-rbac \
    --name $ACR_SP_NAME \
    --query password \
    --role acrpull
    --scope $ACR_ID
    --output tsv)
# this is the username used to login into ACR
ACR_SP_USERNAME= $(az ad sp show \
    --id $ACR_SP_NAME
    --query id
    --output tsv)

# run container on ACI 
az container create \
--resource-group 'resource-group-name' \ 
--name 'name-of-aci' \
# your website will be available on wesbite-name.azureac.io
--dns-name-label 'website-name' \
# image details
--image 'image:tag'
--port 80
# acr details
--registry-login-server 'login-server'
--registry-username 'sp-username'
--registry-password 'sp-password'

```
    
