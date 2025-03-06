---
title: Azure CSP API integration options | Microsoft Docs
description: Learn about Azure CSP API integration options in the Azure CSP business model.
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2018
ms.author: kirilk
---

# Azure CSP API integration options

This article provides an overview of the available APIs that are used to integrate the Azure Cloud Solution Provider (Azure CSP) system with already-existing systems. The systems might manage customer accounts, place orders, manage subscriptions, or handle support requests.

## Partner Center APIs

With the use of these APIs, you can perform just about any task in a script-based format without having to touch the Partner Center UI directly. They interact with the Partner Center interface, not Azure directly.

### Partner Center .NET SDK

The Partner Center .NET SDK simplifies the interaction with Partner Center API by simplifying network operations and token management. Partners can leverage the .NET SDK to design custom integration with any of their systems. More information regarding the supported scenarios can be found at [Partner Center SDK Scenarios](https://docs.microsoft.com/partner-center/develop/scenarios).

### Partner Center Java SDK

The Partner Center Java SDK provides an SDK to interact with Microsoft's Partner Center service. This enables partners to perform the Partner Center operations programmatically or through their own portals. The Java SDK is the latest addition to existing portfolio of REST APIs and the .NET SDK.

### PowerShell

The Partner Center PowerShell module enables PowerShell to interact with Microsoft' Partner Center service. This enables partners to quickly build script to automate Partner Center operations.

> [!IMPORTANT]
> Partner Center PowerShell module is an open source project, and it is only supported through the open source community.

More information can be found in the [Partner Center PowerShell](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/README.md) read me. To learn how to use the PowerShell module to check out the [management options](../overview/azure-csp-management-options.md).

### REST API

The Partner Center REST API helps Azure CSP partners integrate their existing systems with the Partner Center by using common REST API mechanics. By submitting REST requests to the proper endpoints, you can perform many of the Partner Center functions through this API.

The major advantage of this API is that it is highly compatible. If your internal system doesn't work with C#, REST can perform many of the same functions.

For more information, see the [REST API reference](https://msdn.microsoft.com/library/partnercenter/mt667943.aspx), and [download the Swagger document](https://apidocs.microsoft.com/services/partnercenter).

## Azure APIs

The following APIs bypass Partner Center completely and interact directly with Azure.

### Resource Manager API

Azure Resource Manager is the deployment model, replacing Azure Service Management (ASM). Although you can perform many functions of Resource Manager through the portal UI, these functions can also be performed automatically through the Resource Manager API. These functions include, but are not limited to, configuring Azure Virtual Machines and other Azure services, generating reports for debugging and other tasks, and assigning policies inside Azure subscriptions.

The infrastructure of an application may be made up of many components: virtual machines, storage accounts, web applications, or third-party services, just to name a few. These are separate components but are frequently seen as parts of a single entity that should be deployed, managed, and monitored as a group.

With Resource Manager, you can work with the resources in your solution as a group. You can deploy, update, or delete all the resources for your solution in a single, coordinated operation. You use a template for deployment. The template can work for different environments such as testing, staging, and production. Resource Manager provides security, auditing, and tagging features to help you manage your resources after deployment.

For more information about Resource Manager, see [the Resource Manager documentation](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

Resource Manager uses Azure SDKs in multiple languages and platforms to allow for control through whatever platform works best for your solution. Each SDK is generated from open-source RESTful API specifications. The tools used to create the SDKs are open-source and freely available. The available SDKs are as follows:

> [!Note]
> If the SDK you're investigating does not have the functionality you want, you can call directly to the Azure REST API instead.

- [.NET](https://github.com/Azure/azure-sdk-for-net)
- [Java](https://github.com/Azure/azure-sdk-for-java)
- [Node.js](https://github.com/Azure/azure-sdk-for-node)
- [PHP](https://github.com/Azure/azure-sdk-for-php)
- [Python](https://github.com/Azure/azure-sdk-for-python)
- [Ruby](https://github.com/Azure/azure-sdk-ruby)

For more information about the Resource Manager functions and features, see [the Resource Manager website](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

> [!IMPORTANT]
> The Partner Center API/SDK should be used to be obtain rate card and utilization records for Azure CSP subscription. The native Azure billing APIs like RateCard and Usage API cannot be used because Microsoft.Commerce provider is not available for Azure CSP subscriptions.

### Graph API

With the Microsoft Graph API, you can interact with the data of millions of users in the Microsoft cloud. Use it to connect to a wealth of resources, relationships, and intelligence, all through a single endpoint.

Microsoft Graph is a network of resources and the relationships between those resources. By defining the relationships, you can connect resources and perform actions through the Graph API.

These relationships also bring valuable insights to your solution. For example, you can see the subscriptions that a user can access, or get a list of the people who are most relevant to a user.

To learn more about the Graph API, [see the developer documentation](https://developer.microsoft.com/graph/docs/concepts/overview).

## Next steps

- [Review](integration-scenarios-list.md) the list of available Azure CSP Integration scenarios.
- [Review](../overview/azure-csp-management-options.md) management options for Azure CSP.
- [Review](../billing/azure-csp-billing-overview.md) how billing works in the Azure CSP model.