## Importing configurations
As being a DevOps enthusiast myself... I wanted to provide an option to automatically script the configurations for this extension. So when you create automatically teams and queries, you also can setup the Query Based Boards configurations for them.

However there is one big limitation: I am not able to create a webservice within Azure DevOps (and certainly not one that can execute the source code within my extension).

So I am not able to provide a solution that can by completely automated. This procedure just allows you pre-load configs in a seperate document collection. **You just have to run the Configuration page, to start the actual import!**

> Remark: I think it will be fairly easy to implement a fully automated flow, while using a UI testing tool like Selenium. 
> The only things that have to be done are: 
> - start a browser session (with js support), 
> - navigate to Azure DevOps, 
> - login as a user, 
> - navigate to the configuration hub URL, 
> - possibly use a retry after a timeout of 10 secs 
> - and just wait for the client to process the imports (this can be checked by the API).

Secondly: you will have to use the **not-documented** API that is provided by Microsoft. So I will be doing a bit here, within this document. Because it is not documented, I have to mention: **USE IT AT YOUR OWN RISK**

### The API base URL

For online:<br/>
`https://extmgmt.dev.azure.com/{organization}/_apis/ExtensionManagement/InstalledExtensions/realdolmen/EdTro-AzureDevOps-Extensions-QueryBasedBoards-Public/Data/Scopes/Default/Current/Collections/querybasedboards-import/Documents?api-version=3.2-preview1`

For onpremise:<br/>
`https://{yourserverurl}/tfs/{collection}/_apis/ExtensionManagement/InstalledExtensions/realdolmen/EdTro-AzureDevOps-Extensions-QueryBasedBoards-Public/Data/Scopes/Default/Current/Collections/querybasedboards-import/Documents?api-version=3.2-preview1`

When you run this url, you will get the LIST of all exisiting documents within this collection.


### List documents
Method: `GET`

Response Status: `200 - OK`

Response body (within a collection):

| Name      | Type   | Description |
|-----------|--------|-------------|
| id        | string | This can contain the name of the project (this will be the 'global' configuration) or the queryid. |
| title     | string | You can give this document a meaningfull title, it is not used by the actual import. |
| json      | string | The stringified json, you normally provide within the UI. | 
| project   | string | The name of the project. |
| token     | string | If the document has been processed, this is specified. |
| __etag    | number | System field used for versioning. |


The first time you will get the following exception:
```json
{
  "$id": "1",
  "innerException": null,
  "message": "%error=\"1660002\";%:The collection does not exist\r\n%error=\"1660002\";%:The collection does not exist",
  "typeName": "Microsoft.VisualStudio.Services.ExtensionManagement.WebApi.DocumentCollectionDoesNotExistException, Microsoft.VisualStudio.Services.ExtensionManagement.WebApi",
  "typeKey": "DocumentCollectionDoesNotExistException",
  "errorCode": 0,
  "eventId": 3000
}
```
<br/>

### Create document
Method: `POST` 

Request body:

| Name      | Type   | Description |
|-----------|--------|-------------|
| id        | string | This can contain the name of the project (this will be the 'global' configuration) or the queryid. |
| title     | string | You can give this document a meaningfull title, it is not used by the actual import. |
| json      | string | The stringified json, you normally provide within the UI. | 
| project   | string | The name of the project. |

> All these fields within the request body are MANDATORY. You will have to provide a valid string value for these fields.

Response Status: `201 - Created`

Response body:

| Name      | Type   | Description |
|-----------|--------|-------------|
| id        | string | This can contain the name of the project (this will be the 'global' configuration) or the queryid. |
| title     | string | You can give this document a meaningfull title, it is not used by the actual import. |
| json      | string | The stringified json, you normally provide within the UI. | 
| project   | string | The name of the project. |
| token     | string | If the document has been processed, this is specified. |
| __etag    | number | System field used for versioning. |


Example for creating a json request body for the global config
```powershell
$project = "Test-Agile"
$globalConfig = [PSCustomObject]@{
    "settings" = [PSCustomObject]@{
        "showFilter" = $true
        "zoomLevel" = 0
    } 
}

$importGlobalConfig = [PSCustomObject]@{
    "id" = $project
    "title" = "import global for $project"
    "json" = ConvertTo-Json $globalConfig -Compress
    "project" = $project
}

ConvertTo-Json $importGlobalConfig
```

Example for creating a json request body for a query
```powershell
$project = "Test-Agile"
$queryId = "73744df6-6bed-4125-afb1-2007f83722b4"
$queryConfig = [PSCustomObject]@{
    "settings" = [PSCustomObject]@{
        "showFilter" = $false
        "zoomLevel" = 2
    } 
}

$importQueryConfig = [PSCustomObject]@{
    "id" = $queryId
    "title" = "import $queryId for $project"
    "json" = ConvertTo-Json $queryConfig -Compress
    "project" = $project
}

ConvertTo-Json $importQueryConfig
```
<br/>

### Other methods
There are also an update method and a delete method implemented by Microsoft that you can use on these documents.