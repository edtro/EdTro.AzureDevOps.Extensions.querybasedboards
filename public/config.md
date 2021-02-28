# Getting started
## The YouTube channel
Recently I have started a YouTube channel for this extension, to provide you instruction videos regarding the possibilities of this extension.
Please make sure to check it out @ https://www.youtube.com/channel/UCPhOzbTeOeNiy3-sIgE0U5g. 

## Using the configuration

Within the Teamprojects settings hub, you can find within the extensions section, the option to configure "Query Based Boards". For each query a seperate configuration item is available, next to a **Global** configuration item (here you can define the settings/configs that should be used as default for all of the queries). 

> Please note, that whenever you run a query and select the tab 'Show as board' or 'Show as taskboard', a configuration item is *automatically created* for the specific query. So first do this, before trying to configure the board for the query.

At the moment these options are implemented:
- settings
- setup columns
- splitup columns in doing/done **PREVIEW**
- setup swimlanes **PREVIEW**
- display fields **PREVIEW**
- style Rules **PREVIEW**
- backlog/board tabs **PREVIEW**

Unfortunately, I did not have the time to complete this documentation yet (or to create a user friendly UI). Sorry about that! But I have included the model of the actual configuration, see:

```javascript
export interface IConfigDataSetup {
    settings?: IConfigDataSettings,
    backlogTabs?: IConfigDataBacklogTab[],
    fields?: IConfigDataDisplayField[],
    columns?: IConfigDataColumn[],
    swimlanes?: IConfigDataSwimlanes,
    styleRules?: IConfigDataStylingRule[] //you can setup multiple rules, takes the first rule that can be applied
}

export interface IConfigDataSettings {
    showFilter?: boolean,
    showArrows?: boolean,
    zoomLevel?: number, //0, 1, 2 or 3  
    showProject?: boolean,
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
    type?: number, //0=standard; 1=treepath; 2=tags; 3=treepathCustom
    filter?: boolean, //true=will allow this field (when it is text based) to be shown within the quick filters (only the first two will be used)
}

export interface IConfigDataColumn {
    name: string,
    title: string,   
    isBacklog?: boolean,
    isCollapsed?: boolean, //backlog=true
    width?: number, //backlog=false && between 1 and 5
    wipLimit?: number, //backlog=false && if >= 0 then count and limit will be displayed 
    doingDone?: IConfigDataColumnDoingDone,
    parentTransitionMapping?: IConfigDataColumnParentTransitionMapping
}

export interface IConfigDataColumnDoingDone {
    field: string, //no system fields like 'System.xyz'
    doingTitle: string,
    doingDefaultValue: boolean | number | string,
    doneTitle: string,
    doneValue: boolean | number | string,
    unsupportedAPIFields?: string[]; //like the name suggest: USING THIS IS UNSUPPORTED... try at your own risk. Use the correct fieldnames, like: WEF_{ID-CODE}_Kanban.Column.Done (note: only works when field='System.BoardColumnDone')
}

export interface IConfigDataColumnParentTransitionMapping {
    parentToNameAll: string, //the 'state' value of the parent, when a 'state' is transitioned to this 'state' (or column)
    parentToName: string, //the 'state' value of the parent, when a 'state' is transitioned to this 'state' (or column)
    allChildren: boolean //are all other children within this 'state' (or column)
}

export interface IConfigDataSwimlanes {
    field: string, //no system fields like 'System.xyz'
    defaultValue: boolean | number | string,
    values: IConfigDataSwimlaneFieldValue[],
}

export interface IConfigDataSwimlaneFieldValue {
    value: boolean | number | string,
    title: string,
}

export interface IConfigDataStylingRule {        
    field: string; // at this moment only the operator 'EQUAL' is implemented (and this will always the default)
    value: string;
    color: string; //use the HTML Hexcolor values like #RRGGBB
}

export interface IConfigDataImport {
    id: string,
    title: string, 
    json: string,
    project: string
}

```
<br/>

Please be aware of the restraints that I have put into the comments.

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


And here is an example with swimlanes, columns splitup into doing/done, a custom width, wiplimits and backlog columns that are collapsed by default:
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
         "wipLimit":5,
         "width": 2,
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
         "title":"Resolved - Can be released",
         "wipLimit":7
      },
      {
         "name":"Closed",
         "title":"Completed",
         "isBacklog":true,
         "isCollapsed": true
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

An example for the Parent Transition Mappings (this **PREVIEW** feature is great for simple rollup scenario's/flows):
```json
{
     "columns":[
      {
         "name":"New",
         "title":"New",
         "isBacklog":true,
         "parentTransitionMapping": 
         {
             "parentToName":"Active",
             "parentToNameAll":"New",
             "allChildren": true
         } 
      },
      {
         "name":"Active",
         "title":"Active",
         "parentTransitionMapping": 
         {
             "parentToName":"Active",
             "parentToNameAll":"Active",
             "allChildren": false
         } 
      },      
      {
         "name":"Closed",
         "title":"Closed",
         "isBacklog":true,
         "parentTransitionMapping": 
         {
             "parentToName":"Active",
             "parentToNameAll":"Closed",
             "allChildren": true
         }  
      }
   ]
}
```
<br/>

Here is an example of using default values for a setting and the setup of backlogtabs. This json is inserted into the 'Global' configuration (you can setup 'settings' and 'backlogTabs' globally only).
```json
{
   "settings":{
      "showFilter":true
   },
   "backlogTabs":[
      {
         "tabNumber":"1",
         "level":"Epics",
         "teamId":"", // add the GUID of your team here, but you can leave it empty
         "queryId":"00000000-0000-0000-0000-000000000000", // add the GUID of your query here
         "title":"Custom Board"
      },
      {
         "tabNumber":"1",
         "level":"Features",
         "teamId":"", // add the GUID of your team here, but you can leave it empty
         "queryId":"00000000-0000-0000-0000-000000000000", // add the GUID of your query here
         "title":"Custom Board"
      },
      {
         "tabNumber":"1",
         "level":"", // you can leave the level empty, please be aware of the sortorder of your json. Because the config that is found first, that matches the criteria, will be used.
         "teamId":"", // add the GUID of your team here, but you can leave it empty
         "queryId":"00000000-0000-0000-0000-000000000000", // add the GUID of your query here
         "title":"Custom Board"
      }
   ]
}
```
<br/>

And finally an example of adding extra fields on the workitem cards:
```json
{
   "fields":[
      {
         "name":"System.AreaPath",
         "title":"Area",
         "filter": true       
      },
      {
         "name":"System.IterationPath",
         "title":"Sprint"       
      },
      {
         "name":"Microsoft.VSTS.Common.Priority",
         "title":"Priority"       
      },
      {
         "name":"System.Tags",
         "title":"Tags",
         "filter": true
      },
      {
         "name":"System.Parent",
         "title":"Parent"
      }
   ],
   "styleRules":[
      {
         "field":"Microsoft.VSTS.Common.Priority",
         "value":1,
         "color":"#FF0000"       
      },
     {
         "field":"Microsoft.VSTS.Common.Priority",
         "value":2,
         "color":"#FFA500"       
      }  
   ]
} 
```
> Please note: for the display fields the types Guid, History, Html and Identity are not supported.

While adding the 'System.Tags' to the 'Fields' will not enable you to display the field (you should use the settings), but it will enable you to filter on this field
It is also possible to filter on Areas and Iterations (and on all other text-based fields... unfortunately numbers/guid/etc... are not supported)

When you have added the field, it is also possible to add a styling rule.