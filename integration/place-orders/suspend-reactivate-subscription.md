---
title: Suspend or reactivate a customer subscription - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to suspend or reactivate a customer's subscription for Azure Cloud Solution Provider (Azure CSP) integration.
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

# Suspend or reactivate a customer subscription for Azure CSP integration

You can suspend an Azure Cloud Solution Provider (Azure CSP) subscription due to overuse, possibility of fraud or misuse, or lack of payment. With Microsoft Partner Center, you can easily suspend and reactivate subscriptions. Use Web UI, PowerShell, C#, or REST API to suspend or reactivate a subscription.





## Web UI

### Suspend the subscription

1.  From the Partner Center **Dashboard** page, go to **Customers** > select the specific customer > **Subscriptions** > select the specific subscription.

2.  For **Status**, select **Suspended**, and then select **Submit**.

    ![Image](media/suspend-subscription-1.PNG "Set the status to suspended, then select Submit.")

### Reactivate the subscription

1.  From the Partner Center **Dashboard** page, go to **Customers** > select the specific customer > **Subscriptions** > select the specific subscription.

2.  For **Status**, select **Active**, and then select **Submit**.

    ![image](media/reactivate-suspended-subscription-1.PNG "Set the status to active, then select Submit.")

## PowerShell

### Suspend the subscription

To suspend a subscription by using PowerShell, enter the following command:

```powershell
$customer = Get-PCCustomer -tenantid '<tenant id GUID>'
$subscription = Get-PCSubscription -tenantid $customer.id -subscriptionid '<subscription id GUID>'
$subscription | Set-PCSubscription -tenantid $customer.id -status suspended 
```

### Reactivate the subscription

To reactivate a suspended subscription in PowerShell, enter the following command:

```powershell
$customer = Get-PCCustomer -tenantid '<tenant id GUID>'
$subscription = Get-PCSubscription -tenantid $customer.id -subscriptionid '<subscription id GUID>'
$subscription | Set-PCSubscription -tenantid $customer.id -status active
```

## C#

### Suspend the subscription

1. [Get the subscription](../manage-customers/get-subscriptions-by-id.md), and then change the subscription's **Status** property. 
2.  After the change is made, use your **IAggregatePartner.Customers** collection to call the **ById()** method.
3.  Call the **Subscriptions** property, followed by the **ById()** method.
4.  Call the **Patch()** method.



```csharp
// IAggregatePartner partnerOperations;
// var selectedCustomerId as string;
// Subscription selectedSubscription;

updatedSubscription = partnerOperations.Customers.ById(selectedCustomerId).Subscriptions.ById(selectedSubscription.Id).Patch(
   new Subscription()
   {
      Status = SubscriptionStatus.Suspended
   });
```

### Reactivate the subscription

1.  [Get the subscription](../manage-customers/get-subscriptions-by-id.md), and then change the subscription's **Status** property. For information on **Status** codes, see [SubscriptionStatus enumeration](https://msdn.microsoft.com/library/partnercenter/microsoft.store.partnercenter.models.subscriptions.subscriptionstatus.aspx).
2.  After the change is made, use your **IPartner.Customers** collection to call the **ById()** method.
3.  Call the **Subscriptions** property, followed by the **ById()** method.
4.  Call the **Update()** method.



```csharp
// IPartner partnerOperations;
// var selectedCustomer as Customer;
// var selectedSubscription as Subscription;

updatedSubscription = partnerOperations.Customers.ById(selectedCustomerId).Subscriptions.ById(selectedSubscription.Id).Patch(
   new Subscription()
   {
      Status = SubscriptionStatus.Active
   });
```

## REST API

### Suspend the subscription

**Request syntax**

|Method|Request URI|
|---|---|
|**PATCH**|*{baseURL}*/v1/customers/*{customer-tenant-id}*/subscriptions/*{id-for-subscription}* HTTP/1.1|

**URI parameter**

These are the *required* query parameters that are used to suspend a subscription.

|Name|Type|Description|
|---|---|---|
|*customer-tenant-id*|**guid**|A GUID that corresponds to the customer.|
|*id-for-subscription*|**guid**|A GUID that corresponds to the subscription.|

**Request body**

A full **Subscription** resource is required in the request body. Ensure that the **Status** property is updated.

**Request example**

```json
PATCH https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/subscriptions/<id-for-subscription> HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: ca7c39f7-1a80-43bc-90d8-ee7d1cad3831
MS-CorrelationId: ec8f62e5-1d92-47e9-8d5d-1924af105f2c
If-Match: <etag>
Content-Type: application/json
Content-Length: 1029
Expect: 100-continue
Connection: Keep-Alive

{
    "Id": "83ef9d05-4169-4ef9-9657-0e86b1eab1de",
    "FriendlyName": "nickname",
    "Quantity": 2,
    "UnitType": "none",
    "ParentSubscriptionId": null,
    "CreationDate": "2015-11-25T06:41:12Z",
    "EffectiveStartDate": "2015-11-24T08:00:00Z",
    "CommitmentEndDate": "2016-12-12T08:00:00Z",
    "Status": "suspended",
    "AutoRenewEnabled": false,
    "BillingType": "none",
    "PartnerId": null,
    "ContractType": "subscription",
    "OrderId": "6183db3d-6318-4e52-877e-25806e4971be",
    "Attributes": {
        "Etag": "<etag>",
        "ObjectType": "Subscription"
    }
}
```

If the response is successful, this method returns an updated **Subscription** resource in the response body.

**Response example**

```json
PATCH https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/subscriptions/<subscriptionID> HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-Contract-Version: v1
MS-RequestId: ca7c39f7-1a80-43bc-90d8-ee7d1cad3831
MS-CorrelationId: ec8f62e5-1d92-47e9-8d5d-1924af105f2c
Content-Type: application/json
Content-Length: 1029
Expect: 100-continue
Connection: Keep-Alive

{
    "Id": "83ef9d05-4169-4ef9-9657-0e86b1eab1de",
    "FriendlyName": "nickname",
    "Quantity": 2,
    "UnitType": "none",
    "ParentSubscriptionId": null,
    "CreationDate": "2015-11-25T06:41:12Z",
    "EffectiveStartDate": "2015-11-24T08:00:00Z",
    "CommitmentEndDate": "2016-12-12T08:00:00Z",
    "Status": "suspended",
    "AutoRenewEnabled": false,
    "BillingType": "none",
    "PartnerId": null,
    "ContractType": "subscription",
    "Links": {
        "Offer": {
            "Uri": "/v1/offers/0CCA44D6-68E9-4762-94EE-31ECE98783B9",
            "Method": "GET",
            "Headers": []
        },
        "Entitlement": {
            "Uri": "/entitlements?key=<key>",
            "Method": "GET",
            "Headers": []
        },
        "Self": {
            "Uri": "/subscriptions?key=<key>",
            "Method": "GET",
            "Headers": []
        }
    },
    "OrderId": "6183db3d-6318-4e52-877e-25806e4971be",
    "Attributes": {
        "Etag": "<etag>",
        "ObjectType": "Subscription"
    }
}
```

### Reactivate the subscription

**Request syntax**

|Method|Request URI|
|---|---|
|**PATCH**|*{baseURL}*/v1/customers/*{customer-tenant-id}*/subscriptions/*{id-for-subscription}* HTTP/1.1|

**URI parameter**

These are the *required* query parameters that are used to reactivate a subscription.

|Name|Type|Description|
|---|---|---|
|*customer-tenant-id*|**guid**|A GUID that corresponds to the customer.|
|*id-for-subscription*|**guid**|A GUID that corresponds to the subscription.|

**Request body**

A full **Subscription** resource is required in the request body. Ensure that the **Status** property is updated.

**Request example**

```json
PATCH https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/subscriptions/<id-for-subscription> HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: ca7c39f7-1a80-43bc-90d8-ee7d1cad3831
MS-CorrelationId: ec8f62e5-1d92-47e9-8d5d-1924af105f2c
Content-Type: application/json
Content-Length: 1029
Expect: 100-continue
Connection: Keep-Alive

{
    "Id": "83ef9d05-4169-4ef9-9657-0e86b1eab1de",
    "FriendlyName": "nickname",
    "Quantity": 2,
    "UnitType": "none",
    "ParentSubscriptionId": null,
    "CreationDate": "2015-11-25T06:41:12Z",
    "EffectiveStartDate": "2015-11-24T08:00:00Z",
    "CommitmentEndDate": "2016-12-12T08:00:00Z",
    "Status": "active",
    "AutoRenewEnabled": false,
    "BillingType": "none",
    "PartnerId": null,
    "ContractType": "subscription",
    "OrderId": "6183db3d-6318-4e52-877e-25806e4971be",
    "Attributes": {
        "Etag": "<etag>",
        "ObjectType": "Subscription"
    }
}
```

If the response is successful, this method returns updated **Subscription** resource properties in the response body.

**Example response**

```json
PATCH https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/subscriptions/<subscriptionID> HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-Contract-Version: v1
MS-RequestId: ca7c39f7-1a80-43bc-90d8-ee7d1cad3831
MS-CorrelationId: ec8f62e5-1d92-47e9-8d5d-1924af105f2c
Content-Type: application/json
Content-Length: 1029
Expect: 100-continue
Connection: Keep-Alive

{
    "Id": "83ef9d05-4169-4ef9-9657-0e86b1eab1de",
    "FriendlyName": "nickname",
    "Quantity": 2,
    "UnitType": "none",
    "ParentSubscriptionId": null,
    "CreationDate": "2015-11-25T06:41:12Z",
    "EffectiveStartDate": "2015-11-24T08:00:00Z",
    "CommitmentEndDate": "2016-12-12T08:00:00Z",
    "Status": "active",
    "AutoRenewEnabled": false,
    "BillingType": "none",
    "PartnerId": null,
    "ContractType": "subscription",
    "Links": {
        "Offer": {
            "Uri": "/v1/offers/0CCA44D6-68E9-4762-94EE-31ECE98783B9",
            "Method": "GET",
            "Headers": []
        },
        "Entitlement": {
            "Uri": "/entitlements?key=<key>",
            "Method": "GET",
            "Headers": []
        },
        "Self": {
            "Uri": "/subscriptions?key=<key>",
            "Method": "GET",
            "Headers": []
        }
    },
    "OrderId": "6183db3d-6318-4e52-877e-25806e4971be",
    "Attributes": {
        "Etag": "<etag>",
        "ObjectType": "Subscription"
    }
}
```

## Next steps
- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
