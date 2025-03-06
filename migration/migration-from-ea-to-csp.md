---
title: Migrate an Azure subscription from Microsoft Enterprise Agreement to Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to migrate an Azure subscription from Microsoft Enterprise Agreement to Azure Cloud Solution Provider (Azure CSP).
services: azure-csp
author: KirillKotlyarenko
manager: narena
 
ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: kirilk
---

# Migrate an Azure subscription from Enterprise Agreement to Cloud Solution Provider

Learn how to assess and then complete the migration of a customer's existing Azure subscription from Microsoft Enterprise Agreement or Microsoft Server and Cloud Enrollment (SCE) to the Azure Cloud Solution Provider (Azure CSP) program.

## Process overview

Migrating an Azure subscription from Enterprise Agreement to CSP involves three major steps:

1. Ensure that source Azure EA subscription and target Azure CSP subscription are within the same tenant (directory). Refer to [partner EA to CSP](ea-payg-to-azure-csp/partner-ea-to-csp.md) or [customer EA to CSP](ea-payg-to-azure-csp/customer-ea-to-csp.md) migration procedure accordingly.

2. [Perform an assessment](ea-payg-to-azure-csp/ea-open-direct-assessment.md): Before you begin, assess all Azure resources in all subscriptions in the Enterprise Agreement account that you want to move to Azure CSP. This helps you determine what you can and can't move to Azure CSP, and what moving a service entails.

    > [!NOTE]
    > Some Azure services are not available in Azure CSP. It's better to know as early as possible whether these services need to be restructured or cannot be migrated.

3. [Upgrade Azure classic deployment model to Azure Resource Manager](ea-payg-to-azure-csp/ea-open-direct-asm-to-arm.md): If a customer is still using resources that they created by using the Azure classic deployment model, the resources must be transferred to the Azure Resource Manager model before you move them to Azure CSP. This includes any virtual machines, virtual networks, and storage accounts that were created by using the Azure classic deployment model.

    >[!NOTE]
    > Azure CSP supports only Azure Resource Manager services. Azure classic deployment services cannot be migrated to Azure CSP.

4. [Move resources from target subscription to Azure CSP](ea-payg-to-azure-csp/ea-open-direct-arm-to-csp.md): When all of the customer's services are using the Azure Resource Manager model, you can perform the final step of migrating them to an Azure CSP subscription.

## Next steps

- Review the process of [Azure subscription migration from other channels](migration-from-payg-to-csp.md), including from Pay-As-You-Go (PAYG) or the open model.
- Learn how to [migrate customers from on-premises to Azure CSP](migration-from-on-premises-to-azure-csp.md).
