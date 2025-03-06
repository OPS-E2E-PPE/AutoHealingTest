---
title: Get a list of subscriptions by order - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to retrieve customer subscriptions from an order for Azure Cloud Solution Provider (Azure CSP) integration.
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

# Get a list of subscriptions by order for Azure CSP integration

Learn how to retrieve a collection of Azure Cloud Solution Provider (Azure CSP) subscription resources that correspond to an order. To retrieve a resource, you can use PowerShell, C#, or REST API commands.


## PowerShell guide

To retrieve all of the customer subscriptions from an order, enter the following command into PowerShell:

```powershell
$customer = Get-PCCustomer -tenantid '<tenant id GUID>'
Get-PCSubscription -tenantid $customer.id -orderid '<order id GUID>'
```


## C# SDK guide

To get a list of subscriptions by order, use your **IAggregatePartner.Customers** collection and call the **ById()** method. Then call the **Subscriptions** property, followed by the **ByOrderId()** method. Finish by calling **Get()** or **GetAsync()**.

```csharp
// IAggregatePartner partnerOperations;
// var selectedCustomerId as string;
// string orderID;

ResourceCollection<Subscription> customerSubscriptions = partnerOperations.Customers.ById(selectedCustomerId).Subscriptions.ByOrder(orderID).Get();
```


## REST API guide

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/customers/*{customer-tenant-id}*/subscriptions?order_id=*{id-for-order}* HTTP/1.1|

**URI parameter**

These are the *required* query parameters to get all the subscriptions.

|Name|Type|Description|
|---|---|---|
|*customer-tenant-id*|**guid**|A GUID that corresponds to the customer.|
|*id-for-order*|**guid**|A GUID that corresponds to the order.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/{customer-tenant-id}/subscriptions?order_id={id-for-order} HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 16fee928-dc2c-412f-adbb-871f68babf16
MS-CorrelationId: c49004b1-224f-4d86-a607-6c8bcc52cfdd
Connection: Keep-Alive
```

### Response

If the request is successful, this method returns a collection of **Subscription** resources in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 73754
Content-Type: application/json
MS-CorrelationId: c49004b1-224f-4d86-a607-6c8bcc52cfdd
MS-RequestId: 16fee928-dc2c-412f-adbb-871f68babf16
Date: Wed, 25 Nov 2015 05:50:45 GMT

{
    "totalCount": 37,
    "items": [{
        "id": "83ef9d05-4169-4ef9-9657-0e86b1eab1de",
        "friendlyName": "Myofferpurchase",
        "quantity": 1,
        "unitType": "none",
        "creationDate": "2015-11-25T06: 41: 12Z",
        "effectiveStartDate": "2015-11-24T08: 00: 00Z",
        "commitmentEndDate": "2016-12-12T08: 00: 00Z",
        "status": "active",
        "autoRenewEnabled": false,
        "billingType": "none",
        "contractType": "subscription",
        "links": {
            "offer": {
                "uri": "/v1/offers/0CCA44D6-68E9-4762-94EE-31ECE98783B9",
                "method": "GET",
                "headers": []
            },
            "entitlement": {
                "uri": "/entitlements?key=<key>",
                "method": "GET",
                "headers": []
            },
            "self": {
                "uri": "/subscriptions?key=<key>",
                "method": "GET",
                "headers": []
            }
        },
        "orderId": "{id-for-order}",
        "attributes": {
            "etag": "<etag>",
            "objectType": "Subscription"
        }
    }],
    "attributes": {
        "objectType": "Collection"
    }
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).

