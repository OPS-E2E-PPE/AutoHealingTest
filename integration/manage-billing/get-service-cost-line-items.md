---
title: Get a customer's service cost line items - Azure Cloud Solution Provider| Microsoft Docs
description: Learn how to get a customer's service cost line items for Azure Cloud Solution Provider (Azure CSP) integration.
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: kirilk
---

# Get a customer's service cost line items

Learn how to get a list of a customer's service cost line items for a specific billing period. You can use one of the following options:

- C#
- REST API

## C#

1. To identify the customer, call the **IAggregatePartner.Customers.ById** method. 
2. To get an interface for collection operations for customer service costs, use the **ServiceCosts** property. 
3. To return a value for **IServiceCostsCollection**, call the **ByBillingPeriod** method with a member of the billing period enumerator.
4. To get the customer’s service cost line items, use the **IServiceCostsCollection.LineItems.Get** or the **GetAsync** method.

```csharp
// IAggregatePartner partnerOperations;
// string selectedCustomerId;

var serviceCostsSummary = partnerOperations.Customers.ById(selectedCustomerId).ServiceCosts.ByBillingPeriod(ServiceCostsBillingPeriod.MostRecent).LineItems.Get();
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/customers/{customer-id}/servicecosts/{billing-period}/lineitems HTTP/1.1|

**URI parameter**

The following table lists the parameters that are required for identifying a customer.

|Name|Type|Description|
|---|---|---|
|customer-id|guid|A GUID-format customer ID that identifies the customer.|
|billing-period|string|The billing period. The only supported value is **MostRecent**. The string is not case-sensitive.|

```http
GET https://api.partnercenter.microsoft.com/v1/customers/65726577-c208-40fd-9735-8c85ac9cac68/servicecosts/mostrecent/lineitems HTTP/1.1
Authorization: Bearer <authorization token>
Accept: application/json
MS-RequestId: e6a3b6b2-230a-4813-999d-57f883b60d38
MS-CorrelationId: a687bc47-8d08-4b78-aff6-5a59aa2055c2
X-Locale: en-US
Host: api.partnercenter.microsoft.com
```

### Response

If the request is successful, the response body contains a **ServiceCostLineItem** resource that provides information about the customer's service costs.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 2148
Content-Type: application/json; charset=utf-8
MS-CorrelationId: a687bc47-8d08-4b78-aff6-5a59aa2055c2
MS-RequestId: e6a3b6b2-230a-4813-999d-57f883b60d38
MS-CV: gPPoyNX1X0asAAcw.0
MS-ServerId: <server ID>
Date: Fri, 02 Dec 2016 18: 54: 38 GMT

{
    "attributes": {
        "objectType": "Collection"
    },
    "items":
    [{
            "afterTaxTotal": 0.0,
            "chargeType": "PURCHASE FEE",
            "currencyCode": "USD",
            "currencySymbol": "$",
            "customerId": "<customer ID>",
            "customerName": "<customer name>",
            "endDate": "2016-01-11T00:00:00",
            "offerId": "<offer ID>",
            "offerName": "Project for Office 365 (Government Pricing)",
            "orderId": "<order ID>",
            "pretaxTotal": 0.0,
            "quantity": 1.0,
            "resellerMPNId": "-1",
            "startDate": "2015-12-15T00:00:00",
            "subscriptionFriendlyName": "Project Pro for Office 365 (Government Pricing)",
            "subscriptionId": "<subscription ID>",
            "tax": 0.0,
            "unitPrice": 0.0
        }, {
            "afterTaxTotal": 17.219999999999999,
            "chargeType": "CYCLE FEE",
            "currencyCode": "USD",
            "currencySymbol": "$",
            "customerId": "<customer ID>",
            "customerName": "<customer name>",
            "endDate": "2016-02-11T00:00:00",
            "offerId": "<offer ID>",
            "offerName": "Project for Office 365 (Government Pricing)",
            "orderId": "<order ID>",
            "pretaxTotal": 17.219999999999999,
            "quantity": 1.0,
            "resellerMPNId": "-1",
            "startDate": "2016-01-12T00:00:00",
            "subscriptionFriendlyName": "Project Pro for Office 365 (Government Pricing)",
            "subscriptionId": "<subscription ID>",
            "tax": 0.0,
            "unitPrice": 17.219999999999999
        }
    ],
    "links": {
        "self": {
            "headers": [],
            "method": "GET",
            "uri": "/customers/<customer ID>/servicecosts/MostRecent/lineitems"
        }
    },
    "totalCount": 2
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).