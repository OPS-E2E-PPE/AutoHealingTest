---
title: Assess migration of an existing Azure subscription to Azure CSP | Microsoft Docs
description: Learn how to assess an Azure subscription migration from Enterprise Agreement or pay-as-you go to Azure Cloud Solution Provider (Azure CSP).
services: azure-csp
author: KirillKotlyarenko
manager: narena
 
ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: kirilk
---

#  Assess migration of an existing Azure subscription to Azure CSP

Before you can migrate an existing Azure subscription to Azure Cloud Solution Provider (Azure CSP), it's a good idea to complete an assessment to analyze existing resources. An assessment can help you identify resources that might present roadblocks in the migration process.

## Migration assessment

1. Go to the [Azure Management Portal](https://portal.azure.com) and sign in with subscription owner credentials.

2. Go to *Subscriptions* menu and select *All resources*

3. Build a list of all resource types that exists in the subscription.

4. Check if there are any resource types, that are not available in Azure in CSP by looking up the list of [Azure services, available in CSP](../../overview/azure-csp-available-services.md). You can't migrate Azure resources, that are not available or not supported in CSP.

5. Check if you have any Classic virtual machines, virtual networks or storage accounts. You will need to upgrade them to Resource Manager resources prior to the migration.

6. Check if all resources that you want to migrate support [Azure resource move](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources#services-that-can-be-moved). Also check if [resource move limitations for virtual machines](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources#virtual-machines-limitations) apply in your case.

	> [!NOTE]
	> When you use Azure resource move, you need to move resources with all their dependencies at once. It means that if you want to move the VM, you also need to move all virtual networks and all storage accounts, attached to that virtual machine. 

## Next steps

- [Convert](ea-open-direct-asm-to-arm.md) resources from the Azure classic deployment model to Resource Manager.
- [Migrate](ea-open-direct-arm-to-csp.md) supported Resource Manager resources to Azure CSP.
