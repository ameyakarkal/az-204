# Azure Resource Management Template

## Structure
- apiVersion      : use it to specify the version of the ARM template
- contentVersion  : use it for versioning the template itself
- parameters      : these help to make abstract and make templates reusable
- variables       : hold values that help simplify the template definition
- resource        : the actual artifacts that will be generated when the template is deployed
    - dependsOn   : defines list of resources that need to be setup before *this* resource is created
- functions       : system and user defined functions

```json
/* arm-template.json */
{
  "parameters" : {
      "param_01": {
        "type" : "string" // value of this parameter comes from parameters.json file
      }
  }
}
/* parameters.json */
{
   "param_01":{
      "value" : "01"
   }
}
```

## deployment

```powershell
New-AzResourceGroupDeployment `
  -Name "deployment-01" `
  -ResourceGroupName "eastus2-app" `
  -TemplateFile "./path-to-template" `
  -TemplateParameterFile "./path-to-parameters"
```
