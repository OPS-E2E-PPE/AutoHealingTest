---
title: Migrate an Azure subscriptions to another channel or partner | Microsoft Docs
description: Learn how to migrate an Azure subscription to another channel or partner.
services: azure-csp
author: KirillKotlyarenko
manager: narena
ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2019
ms.author: kirilk
---

# Azure Subscription Migration

## Migrate from on-premises workloads to Azure

If your customers have on-premises workloads, running on VMs or bare metal servers, you can migrate these workloads to Azure CSP. For more information, see [Migrate on-premises workloads to Azure CSP](migration-from-on-premises-to-azure-csp.md).

## Migrating existing Azure subscriptions

To migrate a subscription between offer types, use the following instructions below. These instructions could also be used with other Azure offers such as Azure in Open, and Visual Studio, etc.

### To migrate from EA/Pay-As-You-Go to CSP

1. Ensure that the source and target subscriptions are in the same Azure Active Directory (Azure AD) tenant.

    > [!NOTE]
    > You cannot change the Azure AD tenant for an Azure CSP subscription. The source Azure (EA / Pay-As-You-Go) subscriptions tenant would need to be changed. For more information on this process, see [Associate or add an Azure subscription to your Azure Active Directory tenant](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory).

2. The user account you are using to perform the migration will need [owner privileges](/azure/billing/billing-add-change-azure-subscription-administrator) on both subscriptions.

    > [!NOTE]
    > If you do not already have the appropriate [role based access control](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) owner privileges over the Azure CSP subscription, you will need to [work with the partner to get this assigned](/azure/cloud-solution-provider/customer-management/assign-permissions-to-azure-csp-subscription).

3. [Perform an assessment](ea-payg-to-azure-csp/ea-open-direct-assessment.md): Before you begin, assess all Azure resources that can be moved under the source subscription to Azure CSP.

    > [!IMPORTANT]
    > Some Azure resources cannot be moved between subscriptions. Please visit [services that cannot be moved](/azure/azure-resource-manager/resource-group-move-resources#services-that-cannot-be-moved) for the complete list.
    > Not all Azure services are supported in Azure CSP. Please visit the [Available Azure services in Azure CSP](/azure/cloud-solution-provider/overview/azure-csp-available-services) for more information.
    > As Azure CSP supports only Azure Resource Manager resources, any Azure resources on the source subscription that were created using Azure classic deployment model must be [migrated to Azure Resource Manager](ea-payg-to-azure-csp/ea-open-direct-asm-to-arm.md) before they can be moved.

4. After you've determined that all of the source subscription services are using the Azure Resource Manager model, [you can migrate the services from the source subscription to Azure CSP](ea-payg-to-azure-csp/ea-open-direct-arm-to-csp.md).

### To migrate from CSP to EA/Pay-As-You-Go

1. Ensure that the source and target subscriptions are in the same Azure Active Directory (Azure AD) tenant.

    > [!NOTE]
    > You cannot change the Azure AD tenant for an Azure CSP subscription. The target Azure (EA / Pay-As-You-Go) subscriptions tenant would need to be changed. For more information on this process, see [Associate or add an Azure subscription to your Azure Active Directory tenant](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory).

2. The user account you are using to perform the migration will need [owner privileges](/azure/billing/billing-add-change-azure-subscription-administrator) on both subscriptions.

    > [!NOTE]
    > If you do not already have the appropriate [role based access control](/azure/role-based-access-control/role-assignments-portal) owner privileges over the Azure CSP subscription, you will need to [work with the partner to get this assigned](/azure/cloud-solution-provider/customer-management/assign-permissions-to-azure-csp-subscription).

3. [Perform an assessment](/azure/cloud-solution-provider/migration/ea-payg-to-azure-csp/ea-open-direct-assessment): Before you begin, assess all Azure resources that can be moved under the CSP subscription to target subscription.

    > [!IMPORTANT]
    > Some Azure resources cannot be moved between subscriptions. Please visit [services that cannot be moved](/azure/azure-resource-manager/resource-group-move-resources#services-that-cannot-be-moved) for the complete list.

4. Continue to [migrate the services from the CSP subscription to the target subscription](/azure/azure-resource-manager/resource-group-move-resources#move-resources).

## Next steps

- [Review](../overview/azure-csp-available-services.md) what Azure services are available in Azure CSP.
- [Learn](../support/azure-csp-support-overview.md) how customer support works in the Azure CSP model.
- [Enroll](https://partnercenter.microsoft.com/partner/programs) in the Azure CSP program, and start making Azure business through Azure CSP.
