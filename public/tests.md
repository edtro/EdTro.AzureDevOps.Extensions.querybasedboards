## Introduction

> Note: these testscripts are not all of the tests that are done, there are lot of other tests that are done on also on-premise environments. These tests can be easily executed on any online organization.

Preperation:
- Make sure to install/enable the current release of Query Based Boards;
- Make sure to install/enable the WIQL Playground extension;
- Create two teamprojects with the 'Azure DevOps Demo Generator'
    - The first should be based on 'Tailwind Traders' (=Agile) 
      - Add a new field 'Is Done' (Boolean) to the User Story
      - Add the existing field 'Blocked' to the User Story
      - Add an extra state called 'Test' (within the category 'In Progress' between states Active and Resolved)
      - Within the feature 'Purchase' there is one story for Iteration 2... and three for iteration 3. Add the first as a predeccessor dependency to those three.
    - The second should be based on 'SmartHotel360' (=Scrum)    

## Testcases

<!-- Start-->
<table>
<tr>
<td> Description </td> <td> Admin user </td> <td> Normal user </td>
</tr>

<!-- New Query-->
<tr>
<td colspan="3">

Create a new Query called 'All Work Items', like
```sql
SELECT
    [System.Id],
    [System.WorkItemType],
    [System.Title],
    [System.AssignedTo],
    [System.State],
    [System.Tags]
FROM workitems
WHERE
    [System.TeamProject] = @project
    AND [System.WorkItemType] <> ''
    AND [System.State] <> ''
ORDER BY [System.Id]
```
</td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if all columns are visible (there should be New*, Design*, Active, Resolved, Ready, In Planning, In Progress, Test, Closed*, Inactive* and Completed*)
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if the columns marked with * should be collapseable
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if is possible to move around different workitems of different types. Not all columns should be available for the e.g. Bug. 
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Within the Query Editor, descent the sorting on the WorkItem Id and sort back, check if the sorting in the board reflects this. Sort on an other column. Save the query every time after each modification.
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Open up the settings and use zoom option. Check every option and restore to the default, make sure to check if it is possible to move items.
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Open up the settings and turn on and off the 'dependency arrows'. And move arround the work items connected to the arrows. When the successor is moved to a 'higher' state than the predeccessor, the arrow should turn into color red.
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
With the 'dependency arrows' turned on, collapse a couple of columns. Check if the arrows are still correct. Move an item to a collapsed column, check if this works.
</td>
<td> OK? </td> <td>  OK? </td>
</tr>
<!-- End -->

<!-- New Query-->
<tr>
<td colspan="3">

Create a new Query called 'Stories or PBIs Multiple projects', like
```sql
SELECT
    [System.Id],
    [System.WorkItemType],
    [System.Title],
    [System.AssignedTo],
    [System.State],
    [System.Tags],
    [Microsoft.VSTS.Common.Priority]
FROM workitems
WHERE
    [System.WorkItemType] IN ('User Story', 'Product Backlog Item')
    AND [System.State] <> ''
    AND [System.TeamProject] IN ('Test-QBB-Agile', 'Test-QBB-Scrum')
ORDER BY [Microsoft.VSTS.Common.Priority]

```
</td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if the columns New*, Approved*, Committed, Active, Test, Resolved, Done* and Closed* are visible
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if the columns marked with * should be collapseable

</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New Query-->
<tr>
<td colspan="3">

**First play around in the Backlog with stack ranks, so these fields are filled in.**
Create a new Query called 'Stories', like
```sql
SELECT
    [System.Id],
    [System.WorkItemType],
    [System.Title],
    [System.AssignedTo],
    [System.State],
    [Microsoft.VSTS.Common.StackRank],
    [Microsoft.VSTS.Common.Priority],
    [Microsoft.VSTS.Common.Severity]
FROM workitems
WHERE
    [System.TeamProject] = @project
    AND [System.WorkItemType] IN ('User Story')
    AND [System.State] <> ''
ORDER BY 
    [Microsoft.VSTS.Common.StackRank],
    [Microsoft.VSTS.Common.Priority],
    [Microsoft.VSTS.Common.Severity]
```
</td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if the columns New, Active, Test, Resolved and Closed are visible (and within that exact order)
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Shuffle arround a couple of user stories within the backlog and see if the sorting is reflected within the board
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New Query-->
<tr>
<td colspan="3">

Create a new Query called 'Features with Stories', like
```sql
SELECT
    [System.Id],
    [System.WorkItemType],
    [System.Title],
    [System.AssignedTo],
    [System.State],
    [Microsoft.VSTS.Common.StackRank],
    [Microsoft.VSTS.Common.Priority],
    [Microsoft.VSTS.Common.Severity]
FROM workitemLinks
WHERE
    (
        [Source].[System.TeamProject] = @project
        AND [Source].[System.WorkItemType] IN ('Feature')
        AND [Source].[System.State] <> ''
    )
    AND (
        [System.Links.LinkType] = 'System.LinkTypes.Hierarchy-Forward'
    )
    AND (
        [Target].[System.TeamProject] = @project
        AND [Target].[System.WorkItemType] = 'User Story'
    )
ORDER BY 
    [Microsoft.VSTS.Common.StackRank],
    [Microsoft.VSTS.Common.Priority],
    [Microsoft.VSTS.Common.Severity],
    [System.Id]
MODE (MayContain)
```
</td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if the columns New*, Active, Test, Resolved and Closed* are visible (and within that exact order)
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if the columns marked with * should be collapseable
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if you can collapse each of the lanes and check if the collapse all works correctly
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Collapse all of the items and open up one feature that has no user stories. Open it up with clicking on the card and set the state to 'Closed'. This should be reflected in the board and when you collapse that lane, the Feature should be 'crossed out'.
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check when the lanes are collapsed, if the counters are correct. Per category (except for the closed) a counter is displayed. For the columns Active, Test and Resolved the counter 'Active' will be calculated.
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Move around the items within the columns and see if this is reflected within collapsed view.
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Turn on the 'Dependency Arrows' and check if these are correctly displayed. When a lane is collapsed, the arrow should not be shown.
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if zooming works
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- End -->
</table>

## Testing configurations

<!-- Start-->
<table>
<tr>
<td> Description </td> <td> Admin user </td> <td> Normal user </td>
</tr>

<!-- New Config for query-->
<tr>
<td colspan="3">

Create a new configuration for the Query called 'Stories', like
```json
{
   "columns":[
      {
         "name":"New",
         "title":"New/Backlog",
         "isBacklog":true
      },
      {
         "name":"Active",
         "title":"In Progress/Active"
      },
      {
         "name":"Test2",
         "title":"Testing (should not be visible)"
      },
      {
         "name":"Resolved",
         "title":"Resolved/Ready for release",
         "isBacklog":true
      },            
      {
         "name":"Closed",
         "title":"Completed/Done",
         "isBacklog":true
      }
   ]
}
```
</td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if all columns are visible (there should be New/Backlog*, In Progress/Active, Resolved/Ready for release* and Completed/Done*)
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if the columns marked with * should be collapseable
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Change the config so "Test2" will be "Test" so it will show up. Next change the title accordingly.
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New Config for query-->
<tr>
<td colspan="3">

Create a new configuration for the Query called 'Features with Stories', like
```json
{
   "columns":[
      {
         "name":"New",
         "title":"New/Backlog",
         "isBacklog":true
      },
      {
         "name":"Active",
         "title":"In Progress/Active"
      },
      {
         "name":"Test",
         "title":"Testing"
      },
      {
         "name":"Resolved",
         "title":"Resolved/Ready for release",
         "isBacklog":true
      },            
      {
         "name":"Closed",
         "title":"Completed/Done",
         "isBacklog":true
      }
   ]
}
```
</td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if all columns are visible (there should be New/Backlog*, In Progress/Active, Testing, Resolved/Ready for release* and Completed/Done*)
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- New line-->
</td>
<tr>
<td>
Check if the columns marked with * should be collapseable
</td>
<td> OK? </td> <td>  OK? </td>
</tr>

<!-- End -->
</table>
