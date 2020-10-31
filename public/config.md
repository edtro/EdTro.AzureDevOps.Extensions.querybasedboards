## Using the configuration

Within the Teamprojects settings hub, you can find within the extensions section, the option to configure "Query Based Boards".

> Please note, that whenever you run a query and select the tab 'Show as board' or 'Show as taskboard', a configuration item is *automatically created* for the specific query. So first do this, before trying to configure the board for the query.

At the moment these options are implemented:
- setup columns
- splitup columns in doing/done **PREVIEW**
- setup swimlanes **PREVIEW**
- <del>display fields</del>

Unfortunately, I did not have the time to complete this documentation yet (or to create a user friendly UI). Sorry about that! But I have included the model of the actual configuration, see:

```javascript
export interface IConfigDataSetup {
    fields?: IConfigDataDisplayField[];
    columns?: IConfigDataColumn[];
    swimlanes?: IConfigDataSwimlanes;
}

export interface IConfigDataDisplayField {
    name: string;
    title: string;
}

export interface IConfigDataColumn {
    name: string;
    title: string;   
    isBacklog?: boolean;
    doingDone?: IConfigDataColumnDoingDone;
}

export interface IConfigDataColumnDoingDone {
    field: string;
    doingTitle: string;
    doingDefaultValue: boolean | number | string;
    doneTitle: string;
    doneValue: boolean | number | string;
}

export interface IConfigDataSwimlanes {
    field: string;
    defaultValue: boolean | number | string;
    values: IConfigDataSwimlaneFieldValue[];
}

export interface IConfigDataSwimlaneFieldValue {
    value: boolean | number | string;
    title: string;
}
```

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
