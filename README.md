# EdTro.AzureDevOps.Extensions.querybasedboards
Query Based Boards enables a user to visualize the result of work item queries as a board and track the dependencies.

This extension for Azure DevOps Services/Server is available on the markteplace [here](https://marketplace.visualstudio.com/items?itemName=realdolmen.EdTro-AzureDevOps-Extensions-QueryBasedBoards-Public)

View the detail documentation [here](public/details.md)

## Information
This project is bootstrapped with Fabric UI React, see details [here](https://developer.microsoft.com/en-us/fluentui#/get-started/web#option-1-quick-start)
And has the following dependencies:
* azure-devops-extension-sdk, see details [here](https://github.com/Microsoft/azure-devops-extension-sdk)
* azure-devops-extension-api, see detals [here](https://github.com/Microsoft/azure-devops-extension-api)
* azure-devops-ui (this is the new Formula Design System), see information [here](https://developer.microsoft.com/en-us/azure-devops/)
* react-dnd, see details [here](https://github.com/react-dnd/react-dnd/) and for a tutorial [here](https://react-dnd.github.io/react-dnd/docs/tutorial)
* react-dnd-multi-backend, see details [here](https://github.com/LouisBrunner/dnd-multi-backend/tree/master/packages/react-dnd-multi-backend)

Many thanks to these teams and people.

> NOTE: this the second version of this extension. The first version was created by a former colleague of ours, all the credits for coming up with this idea is going to him! This version is however rebuild completely, based on the new Azure DevOps extension libraries and the new Formula Design System. 
> The old and original project is still available on the marketplace [here](https://marketplace.visualstudio.com/items?itemName=realdolmen.querybasedboards)


Full list of dependencies:
``` json
{
  "dependencies": {
    "@types/lodash": "^4.14.165",
    "@types/react-dnd-html5-backend": "^3.0.2",
    "@types/react-dnd-multi-backend": "^5.0.0",
    "@types/react-dnd-touch-backend": "^0.5.0",
    "azure-devops-extension-api": "^1.158.0",
    "azure-devops-extension-sdk": "^2.0.11",
    "azure-devops-ui": "^2.167.0",
    "lodash": "^4.17.21",
    "moment": "^2.29.1",
    "office-ui-fabric-react": "^7.0.0",
    "react": "^16.8.6",
    "react-dnd": "^10.0.2",
    "react-dnd-html5-backend": "^10.0.2",
    "react-dnd-multi-backend": "^5.0.0",
    "react-dnd-touch-backend": "^10.0.2",
    "react-dom": "^16.8.6"
  }
}
```
