---
title: Migrate Azure subscriptions from Pay-As-You-Go to Azure CSP | Microsoft Docs
description: Learn how to migrate Azure subscriptions from Pay-As-You-Go to Azure CSP.
services: azure-csp
author: KirillKotlyarenko
manager: narena
ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2019
ms.author: kirilk
---

# Migrate Azure subscriptions from Pay-As-You-Go to Azure CSP

Here is how to assess and migrate existing Azure subscriptions from the Pay-As-You-Go program (sometimes called Azure Direct) to the Azure Cloud Solution Provider (Azure CSP) program. Same process also applies to Azure subscriptions in Open channel.

## Process overview

Overall, the migration process comprises the following steps:

1. Sign in to [Partner Center portal](http://partnercenter.microsoft.com) using CSP partner credentials and [attach an existing customer tenant](../customer-management/add-existing-customer.md).

2. [Perform an assessment](ea-payg-to-azure-csp/ea-open-direct-assessment.md): Assess Azure resources in the Pay-As-You-Go subscription that you want to move to Azure CSP. This helps you determine what you can and can't move to Azure CSP, and what moving a service entails.

    > [!NOTE]
    > Some Azure services are not available in Azure CSP. It's better to know as early as possible whether these services need to be restructured or cannot be migrated.

3. [Upgrade Azure classic deployment model to Azure Resource Manager](ea-payg-to-azure-csp/ea-open-direct-asm-to-arm.md): If a customer is still using resources that they created by using the Azure classic deployment model, the resources must be transferred to the Azure Resource Manager model before you move them to Azure CSP. This includes any virtual machines, virtual networks, and storage accounts that were created by using the Azure classic deployment model.

    >[!NOTE]
    > Azure CSP supports only Azure Resource Manager services. Azure classic deployment services cannot be migrated to Azure CSP.

4. [Move resources from target subscription to Azure CSP](ea-payg-to-azure-csp/ea-open-direct-arm-to-csp.md): When all of the customer's services are using the Azure Resource Manager model, you can perform the final step of migrating them to an Azure CSP subscription.

## Next steps

- Review the [Azure subscription migration process from the Enterprise Agreement to Azure CSP](migration-from-ea-to-csp.md).
- Learn [how to migrate customers from on-premises resources to Azure CSP](migration-from-on-premises-to-azure-csp.md).
