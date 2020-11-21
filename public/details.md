**Query Based Boards** enables a user to visualize the result of work item queries as a board and track the dependencies

> LICENSE: This is a free showcase project to show the possibilities of creating extensions for Azure DevOps Server/Services and the Formula Design System (ref: https://developer.microsoft.com/en-us/azure-devops/). It is provided by **Inetum-Realdolmen** 'as-is' under the general MIT license issued within the support page (on GitHub). So feel free to install it, try it out and use it in any of your organizations. But do this at your own risk, no guarantees and/or no warrentees of any kind are provided.

> NOTE: This is the second version of this extension. The first version was created by a former colleague of ours, all the credits for coming up with this idea is going to him! This version is however rebuild completely, based on the new Azure DevOps extension libraries and the new Formula Design System. The old and original project is still available on the marketplace (ref: https://marketplace.visualstudio.com/items?itemName=realdolmen.querybasedboards).

> IMPORTANT: Not all of the features that are available within the standard out of the box boards are implemented within this extension or in the exact same way. However this extension will provide several features that are not available (yet) within the standard.


## Introduction
Here is a [link](https://youtu.be/tod0S2QXO-E) to an introduction video on YouTube to get you started (or _click on the picture below_, it takes less than 4 minutes to watch):
[![screenshot](img/youtube.png)](https://youtu.be/tod0S2QXO-E)

> Please feel free to subscribe to the channel and watch other movies regarding the possibilities of this extension.  When this version was released, only the introduction video was available, but more will follow soon. 

## Known limitations
* This extension is currently only available on Azure DevOps Services and Azure DevOps Server (demands the API version 5.0 at least);
* Be aware: this extension is created and supported for "on-line" first (like many other extensions). For "on-premises" instances there is limited support;
* Only 'flat' and 'one-hop' queries are implemented;
* The columns are only based on the field `System.State`;
* Only tested and validated on Chrome, FireFox and Edge (the new Chronium based version) for a regular non-touch device;
* Only tested and validated on Safari (IPhone) for a touch device;
* A couple of features are still in 'preview'.

(please review: https://github.com/edtro/EdTro.AzureDevOps.Extensions.querybasedboards/issues)

## Feedback
Please feel free to leave a behind your feedback within the Q & A section. We love to hear from you.

## Changelog

| Version | Description |
|---------|-------------| 
| 0.20326 | Added the filterbar, the ability to add extra fields to a work item card through the settings and the option to configure settings per query and globally.<br/><br/>Again added two **PREVIEW** features: <br/>1. add extra fields to a work item card through the configuration<br/> 2. and the option to add tabs to the Backlog/Boards hub.<br/><br/>_Please note: these items are still in preview, so please report questions/bugs etc. to contribute on these features, but please do not expect immediate response and/or follow up._|
| 0.20308 | Solved a bug regarding cross-project relations/dependencies that they were not shown (not displayed as arrows). |
| 0.20305 | Solved a bug regarding the configured 'Backlog' columns (the collapse icon was still shown based on the StateCategory like the auto columns), did some rebranding and added some extra guidance when dragging an item (the columns, where it is allowed to drag the item to, are highlighted and the original position is marked).<br/><br/>Also added two **PREVIEW** features: <br/>1. the option to use the 'As Of' date, that will allow you to browse back into time (for example: how many items where active yesterday?)<br/>2. added the ability to configure swimlanes and split columns into doing/done (to use this, please take a look at the section regarding the configuration hub). <br/> <br/> _Please note: these items are still in preview, so please report questions/bugs etc. to contribute on these features, but please do not expect immediate response and/or follow up._|
| 0.20140 | Solved a bug regarding the state categories. States within the category 'Resolved' where not taken into account, but this is resolved. So when you do not use the column configurations, make sure that the states you expect to show up as a column are setup as 'Proposed', 'InProgress', 'Resolved' or 'Completed'.|
| 0.20134 | Solved a bug regarding a loading error within the configuration hub when private queries where used.|
| 0.20122 | Enabled hovering cards within a collapsed column and refactored the div grid to use areas.|
| 0.20109 | Solved a minor bug regarding an invalid hex color code returned by the Rest API when using the 'old' xml process definitions.|
| 0.20104 | Added the Config hub, implemented configuration of the columns and also added the option to collapse the backlog columns.|
| 0.20096 | Solved some minor bugs, optimized the calculation of the arrow paths and implemented the support for touch devices.|
| 0.20065 | Improved the styling of the arrows and added zoom level slider.|
| 0.20064 | Solved minor bugs for on-prem.|
| 0.20062 | Initial version.|

If this project inspires you and/or you have specific requirements that are not not implemented within the standard and/or this project, please feel free to contact us, so we can see how we can help.

Created by **Inetum-Realdolmen**, please contact us at: https://www.realdolmen.com/en/solution/microsoft-application-lifecycle-management
<br/>
![screenshot](img/inetum-realdolmen-becomes.png)
<br/>

