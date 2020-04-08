## Using the configuration

At the moment these options are implemented:
- setup colunmns
- <del>splitup columns in doing/done</del>
- <del>setup swimlanes</del>

Unfortunately, I did not have the time to complete this documentation yet. But I have included the model of the actual configuration, see:

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
    doingDefaultValue: any;
    doneTitle: string;
    doneValue: any;
}

export interface IConfigDataSwimlanes {
    field: string;
    columnSpanFrom: string;
    columnSpanTo: string;
    values: IConfigDataSwimlaneFieldValue[];
}

export interface IConfigDataSwimlaneFieldValue {
    value: any;
    title: string;
}
```

And here is a sample:
```json
{
   "columns":[
      {
         "name":"New",
         "title":"New - Proposed",
         "isBacklog":true
      },
      {
         "name":"Active",
         "title":"Active - In development",
         "doingDone":{
            "field":"System.BoardColumnDone",
            "doingTitle":"Working on it...",
            "doingDefaultValue":false,
            "doneTitle":"Finished!",
            "doneValue":true
         }
      },
      {
         "name":"Test",
         "title":"Testing"
      },
      {
         "name":"Resolved",
         "title":"Resolved..."
      },
      {
         "name":"Closed",
         "title":"Completed",
         "isBacklog":true
      }
   ],
   "swimlanes":{
      "field":"Microsoft.VSTS.CMMI.Blocked",
      "columnSpanFrom":"Active",
      "columnSpanTo":"Resolved",
      "values":[
         {
            "value":"Yes",
            "title":"Is Blocked"
         }
      ]
   }
}
```
