## Using the configuration

Within the Teamprojects settings hub, you can find within the extensions section, the option to configure "Query Based Boards". For each query a seperate configuration item is available, next to a **Global** configuration item (here you can define the settings/configs that should be used as default for all of the queries). 

> Please note, that whenever you run a query and select the tab 'Show as board' or 'Show as taskboard', a configuration item is *automatically created* for the specific query. So first do this, before trying to configure the board for the query.

At the moment these options are implemented:
- settings
- setup columns
- splitup columns in doing/done **PREVIEW**
- setup swimlanes **PREVIEW**
- display fields **PREVIEW**
- backlog/board tabs **PREVIEW**

Unfortunately, I did not have the time to complete this documentation yet (or to create a user friendly UI). Sorry about that! But I have included the model of the actual configuration, see:

```javascript
export interface IConfigDataSetup {
    settings?: IConfigDataSettings,
    backlogTabs?: IConfigDataBacklogTab[],
    fields?: IConfigDataDisplayField[],
    columns?: IConfigDataColumn[],
    swimlanes?: IConfigDataSwimlanes
}

export interface IConfigDataSettings {
    showFilter?: boolean,
    showArrows?: boolean,
    zoomLevel?: number, //0, 1, 2 or 3  
    showProject?: boolean
    showStoryPoints?: boolean,
    showTags?: boolean, 
}

export interface IConfigDataBacklogTab {
    tabNumber: string, //"1", "2", "3", "4" or "5"
    level: string, //"", "Epics", "Features", "Stories" or "Backlog items"
    teamId: string, //can also be "", next to the actual id
    queryId: string, //only the actual id
    title: string
}

export interface IConfigDataDisplayField {
    name: string,
    title: string,    
}

export interface IConfigDataColumn {
    name: string,
    title: string,   
    isBacklog?: boolean,
    doingDone?: IConfigDataColumnDoingDone
}

export interface IConfigDataColumnDoingDone {
    field: string,
    doingTitle: string,
    doingDefaultValue: boolean | number | string,
    doneTitle: string,
    doneValue: boolean | number | string,
}

export interface IConfigDataSwimlanes {
    field: string,
    defaultValue: boolean | number | string,
    values: IConfigDataSwimlaneFieldValue[],
}

export interface IConfigDataSwimlaneFieldValue {
    value: boolean | number | string,
    title: string,
}
```
<br/>

## Important notice
When you configure the query to split up the columns into Doing/Done and/or use the swimlanes, be aware that _this extension will also do updates to the workitem in other fields than just the 'System.State' field_.
We are using existing fields (or custom fields you have created yourself), but we only allow fields that are setup with "allowed values".

So make sure that:
* The REST api can retrieve the "allowed values", see examples:
  * _apis/wit/workitemtypes/Bug/fields/Microsoft.VSTS.CMMI.Blocked?$expand=All&api-version=5.1 (this is a text based picklist)
  * _apis/wit/workitemtypes/Bug/fields/Microsoft.VSTS.Common.Priority?$expand=All&api-version=5.1 (this is a number based picklist))
  * (also fields of the type boolean are allowed, regardless that these have no "allowed values" within, but obviously the allowed values are True or False, so this is implemented)
* Please mind uppercase and lowercase, it is case sensitive.

> NOTE: for obvious reasons **all** system fields are not allowed to be used e.g. `System.WorkItemType`, because the risk is too high or they are system protected. 

> NOTE #2: make sure to implement rules when you want to use the doing/done columns to reset the 'done' value to 'false' on state change.

## Tips
Here are some tips:
* Make sure the JSON is correct and can be parsed (please use a formatter like this one https://jsonformatter.curiousconcept.com/).
* When you modify a configuration, within the console of the browser (goto the development tools with `F12`) the actual parsed JSON is logged. So make sure to review this when there are any problems. I will be adding a feature that will show the serialized JSON according to the model above.
* When an error is shown, please review the details within the console of the browser (goto the development tools with `F12`).

## Examples
Here is an sample for just using columns (for a query with Epics and Features):
```json
{
   "columns":[
      {
         "name":"New",
         "title":"New/Backlog",
         "isBacklog":true
      },
      {
         "name":"In Progress",
         "title":"In Progress/Active"
      },
      {
         "name":"Test",
         "title":"Testing - will not be shown"
      },      
      {
         "name":"Done",
         "title":"Completed/Done",
         "isBacklog":true
      }
   ]
}
```
<br/>
NOTE: the "name" has to map to an actual state that is used within one of the work item types... so within this example the column "Test" will not be shown.


And here is an example with swimlanes and doing/done... but that has not been implemented yet.
```json
{
   "columns":[
      {
         "name":"New",
         "title":"Proposed",
         "isBacklog":true
      },
      {
         "name":"Active",
         "title":"Active - In development",
         "doingDone":{
            "field":"Custom.IsDone",
            "doingTitle":"Working on it...",
            "doingDefaultValue":false,
            "doneTitle":"Finished!",
            "doneValue":true
         }
      },
      {
         "name":"Test",
         "title":"Testing (this column will not be visible, because the 'name' does not map to a valid workitemtype state"
      },
      {
         "name":"Resolved",
         "title":"Resolved - Can be released"
      },
      {
         "name":"Closed",
         "title":"Completed",
         "isBacklog":true
      }
   ],
   "swimlanes":{
      "field":"Microsoft.VSTS.CMMI.Blocked",
      "defaultValue": "No",
      "values":[
         {
            "value":"Yes",
            "title":"Is Blocked"
         }
      ]
   }
}
```
<br/>

Here is an example of using default values for a setting and the setup of backlogtabs. This json is inserted into the 'Global' configuration.
```json
{
   "settings":{
      "showFilter":true
   },
   "backlogTabs":[
      {
         "tabNumber":"1",
         "level":"Epics",
         "teamId":"",
         "queryId":"b5de9cf7-20ff-4bdd-95ab-c181cbb93de2",
         "title":"Custom Board"
      },
      {
         "tabNumber":"1",
         "level":"Features",
         "teamId":"",
         "queryId":"7fa63f27-6fbc-4812-9dbf-8bc8ee185b0d",
         "title":"Custom Board"
      },
      {
         "tabNumber":"1",
         "level":"",
         "teamId":"",
         "queryId":"c28d86e3-a236-4e0b-ae84-8f8f173547e9",
         "title":"Custom Board"
      }
   ]
}
```
<br/>

> Please note: for the display fields the types Guid, History, Html and Identity are not supported.