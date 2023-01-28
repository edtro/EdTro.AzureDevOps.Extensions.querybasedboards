# EdTro.AzureDevOps.Extensions.querybasedboards
Query Based Boards enables a user to visualize the result of work item queries as a board and track the dependencies.

This extension for Azure DevOps Services/Server is available on the markteplace [here](https://marketplace.visualstudio.com/items?itemName=realdolmen.EdTro-AzureDevOps-Extensions-QueryBasedBoards-Public)

View the detail documentation [here](public/details.md)

Please feel free to take a look at the [YouTube channel](https://www.youtube.com/channel/UCPhOzbTeOeNiy3-sIgE0U5g) that I have created for this extension.

## Information
This project is bootstrapped with [Create React App](https://github.com/facebook/create-react-app) and has the following dependencies:
* azure-devops-extension-sdk, see details [here](https://github.com/Microsoft/azure-devops-extension-sdk)
* azure-devops-extension-api, see detals [here](https://github.com/Microsoft/azure-devops-extension-api)
* azure-devops-ui (this is the new Formula Design System), see information [here](https://developer.microsoft.com/en-us/azure-devops/)
* react-dnd, see details [here](https://github.com/react-dnd/react-dnd/) and for a tutorial [here](https://react-dnd.github.io/react-dnd/docs/tutorial)
* react-dnd-multi-backend, see details [here](https://github.com/LouisBrunner/dnd-multi-backend/tree/master/packages/react-dnd-multi-backend)
* some components that where missing in the Formula Design System, I have used from Fluent UI, see details [here](https://developer.microsoft.com/en-us/fluentui) 
* lodash, see details [here](https://github.com/lodash/lodash)

Many thanks to these teams and people.

> NOTE: this the second version of this extension. The first version was created by a former colleague of ours, all the credits for coming up with this idea is going to him! This version is however rebuild completely, based on the new Azure DevOps extension libraries and the new Formula Design System. 
> The old and original project is still available on the marketplace [here](https://marketplace.visualstudio.com/items?itemName=realdolmen.querybasedboards)

Full list of dependencies:
``` json
{
  "dependencies": {
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^12.1.5",
    "@testing-library/user-event": "^13.5.0",
    "@types/jest": "^27.5.2",
    "@types/lodash": "^4.14.191",
    "@types/node": "^16.18.11",
    "@types/pluralize": "^0.0.29",
    "@types/react": "^16.8.6",
    "@types/react-dnd-html5-backend": "^3.0.2",
    "@types/react-dnd-multi-backend": "^5.0.0",
    "@types/react-dnd-touch-backend": "^0.5.0",
    "@types/react-dom": "^16.8.6",
    "azure-devops-extension-api": "^1.158.0",
    "azure-devops-extension-sdk": "^2.0.11",
    "azure-devops-ui": "^2.167.53",
    "lodash": "^4.17.21",
    "moment": "^2.29.4",
    "office-ui-fabric-react": "^7.203.0",
    "pluralize": "^8.0.0",
    "react": "^16.8.6",
    "react-dnd": "^10.0.2",
    "react-dnd-html5-backend": "^10.0.2",
    "react-dnd-multi-backend": "^5.0.1",
    "react-dnd-touch-backend": "^10.0.2",
    "react-dom": "^16.8.6",
    "react-scripts": "5.0.1",
    "typescript": "^4.9.4",
    "web-vitals": "^2.1.4"
  },
  "devDependencies": {
    "sass": "^1.57.1"
  }
}
```
