---
title: Get a customer's usage summary - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to get the usage summary for a customer's subscription for Azure Cloud Solution Provider (Azure CSP) integration.
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

# Get a usage summary for a customer's subscriptions

Learn how to get a **CustomerUsageSummary** resource that represents the customer's usage of a specific Azure service or resource during the current billing period. You can use one of the following options:

- .NET SDK
- Java SDK
- PowerShell
- REST API

## .NET SDK

1. Use your **IAggregatePartner.Customers** collection, and then call the **ById()** function.
2. Call the **UsageSummary** property.
3. Call the **Get()** or the **GetAsync()** function.

```csharp
// IAggregatePartner partnerOperations;
// var selectedCustomerId as string;

var usageSummary = partnerOperations.Customers.ById(selectedCustomerId).UsageSummary.Get();
```

## Java SDK

1. Use your **IAggregatePartner.getCustomers** function, and then call the **byId()** function.
2. Call the **getUsageSummary** function.
3. Call the **get()** function.

```java
// String customerId;

CustomerUsageSummary customerUsageSummary = partnerOperations.getCustomers().byId(customerId).getUsageSummary().get();
```

## PowerShell

Run the following command to get the customer's usage summary

```powershell
# $customerId

Get-PartnerCustomerUsageSummary -CustomerId $customerId
```

Review the [Get-PartnerCustomerUsageSummary](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/docs/help/Get-PartnerCustomerUsageSummary.md) help for more information.

## REST API

> [!IMPORTANT]
> [Azure Resource Usage API](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) is not available for Azure CSP Subscriptions. Resource Usage should be accessed through [Partner Center API](https://msdn.microsoft.com/library/partnercenter/mt651646.aspx) instead.

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/customers/{customer-tenant-id}/usagesummary HTTP/1.1|

**URI parameter**

The following query parameter is required for getting the customer's rated usage information.

|Name|Type|Description|
|---|---|---|
|customer-tenant-id|guid|A GUID that corresponds to the customer.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/{customer-tenant-id}/usagesummary HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: e128c8e2-4c33-4940-a3e2-2e59b0abdc67
MS-CorrelationId: 47c36033-af5d-4457-80a4-512c1626fac4
```

### Response

If the request is successful, this method returns a **CustomerUsageSummary** resource in the response body.

**Response example**

```http
HTTP/1.1 200 OK
Content-Length: 1120
Content-Type: application/json
MS-CorrelationId: 47c36033-af5d-4457-80a4-512c1626fac4
MS-RequestId: e128c8e2-4c33-4940-a3e2-2e59b0abdc67
Date: Fri, 26 Feb 2016 20:31:45 GMT

{
    budget":{
        "ammount":300.000000,
        "attributes":{
            "objectType":"SpendingBudget"
        }
    },
    "id":"65726577-C208-40FD-9735-8C85AC9CAC68",
    "name":"600 test",
    "billingStartDate":"2016-02-06T00:00:00-08:00",
    "billingEndDate":"2016-03-05T00:00:00-08:00",
    "totalCost":0.0,
    "currencyLocale":"en-US",
    "lastModifiedDate":"2016-02-26T09:42:54.5130558+00:00",
    "links":{
        "self":{
            "uri":"/customers/{customer-tenant-id}/usagesummary",
            "method":"GET",
            "headers":[]
        }
    },
    "attributes":{
        "objectType":"CustomerUsageSummary"
    }
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).