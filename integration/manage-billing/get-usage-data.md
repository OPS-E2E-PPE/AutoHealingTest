---
title: Get subscription usage data - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to get subscription usage data for Azure Cloud Solution Provider (Azure CSP) integration.
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: kirilk
---

# Get subscription usage data

You can get a collection resource that contains a list of services within a customer's subscription and their associated rated usage information. This guide will show you how to get the subscription usage data using the following options

- .NET SDK
- Java SDK
- PowerShell
- REST API

## .NET SDK

To get a subscription's resource usage information, use your **IAggregatePartner.Customers** collection and call the **ById()** function. Then call the **Subscriptions** property, as well as **UsageRecords**, then the **Resources** property. Finish by calling the **Get()** or **GetAsync()** function.

```csharp
// IAggregatePartner partnerOperations;
// string selectedCustomerId;
// string selectedSubscriptionId;

var usageRecords = partnerOperations.Customers.ById(selectedCustomerId).Subscriptions.ById(selectedSubscriptionId).UsageRecords.Resources.Get();
```

## Java SDK

To get a subscription's resource usage information, use your **IAggregatePartner.getCustomers** function and call the **byId()** function. Then call the **getSubscriptions** function, as well as **getUsageRecords**, then the **getResources** function. Finish by calling the **Get()** function.

```java
// IPartner partnerOperations;
// String customerId;
// String subscriptionId;

 ResourceCollection<SubscriptionMonthlyUsageRecord> usageRecords = partnerOperations.getCustomers().byId(customerId).getSubscriptions().byId(subscriptionId).getUsageRecords().get();
```

## PowerShell

To get a subscription resource usage information, run the following command

```powershell
# $customerId
# $subscriptionId

Get-PartnerCustomerSubscriptionUsage -CustomerId $customerId -SubscriptionId $subscriptionId
```

Review the [Get-PartnerCustomerSubscriptionUsage](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/docs/help/Get-PartnerCustomerSubscriptionUsage.md) help for more information.

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/customers/{customer-tenant-id}/subscriptions/{id-for-subscription}/usagerecords/resources HTTP/1.1|

**URI parameter**

The following table lists the query parameters that are required for getting the rated usage information.

|Name|Type|Description|
|---|---|---|
|customer-tenant-id|guid|A GUID that corresponds to the customer.|
|id-for-subscription|guid|A GUID that corresponds to the subscription.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/{customer-tenant-id}/subscriptions/{id-for-subscription}/usagerecords/resources HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 65b26053-37d0-4303-9fd1-46ad8012bcb6
MS-CorrelationId: 47c36033-af5d-4457-80a4-512c1626fac4
```

### Response
If the request is successful, this method returns a collection of **AzureResourceMonthlyUsageRecord** resources in the response body.

**Response example**

```http
HTTP/1.1 200 OK
Content-Length: 12014
Content-Type: application/json
MS-CorrelationId: 648a26a4-a63e-459f-844b-4f29d7913353
MS-RequestId: be82a8ba-4a53-49f7-8313-b033c058687e
Date: Tue, 10 Nov 2015 19:09:59 GMT

{
    "totalCount":20,
    "items":[{
        "category":"Storage",
        "subcategory":"LOCALLY REDUNDANT",
        "quantityUsed":0.151287527825352,
        "unit":"GB",
        "id":"2a2419c0-cefe-46b2-8004-8eb002ad606c",
        "name":"Azure Resource 1",
        "totalCost":0.195779159290613,
        "currencyLocale":"en-US",
        "attributes":{
            "objectType":"AzureResourceMonthlyUsageRecord"
        }
    },
    {
        "category":"Remote App",
        "subcategory":"Remote App",
        "quantityUsed":0.932546524299563,
        "unit":"GB",
        "id":"7e4099c8-2b3d-41a6-a1bd-d5cf315989b2",
        "name":"Azure Resource 2",
        "totalCost":0.920983775016379,
        "currencyLocale":"en-US",
        "attributes":{
            "objectType":"AzureResourceMonthlyUsageRecord"
        }
    }],
    "links":{
        "self":{
            "uri":"/v1/customers/<customer-tenant-id>/subscriptions/<id-for-subscription>%20/usagerecords",
            "method":"GET",
            "headers":[]
        }
    },
    "attributes":{
        "objectType":"Collection"
    }
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).