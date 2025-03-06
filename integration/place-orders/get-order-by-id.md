---
title: Get an order by ID - Azure Cloud Solution Provider | Microsoft Docs
description: This article covers how to retrieve an Order resource that matches the provided customer and order ID.
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

# Get an order by ID

This guide covers how to retrieve an **Order** resource that matches the provided customer and order ID.

## PowerShell

To retrieve an order, enter the following commands:

```powershell
$customer = Get-PCCustomer -tenantid '<tenant id GUID>'
Get-PCOrder -tenantid $customer.id -orderid '<order id guid>'
```

## C#

To get a customer's order by ID, do the following:
1. Use your **IAggregatePartner.Customers** collection, and call the **ById()** method. 
2. Call the **Orders** property, and then call the **ByID()** method again. 
3. Call the **Get()** or **GetAsync()** method.

```csharp
// IAggregatePartner partnerOperations;
// string selectedCustomerId;
// string selectedOrderId;

var order = partnerOperations.Customers.ById(selectedCustomerId).Orders.ById(selectedOrder.Id).Get();
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/customers/{customer-tenant-id}/orders/{id-for-order} HTTP/1.1|

**URI parameter**

To get an order by ID, use the following *required* query parameters:

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|A GUID that corresponds to the customer.|
|**id-for-order**|**guid**|A GUID that corresponds to the order.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/orders/<id-for-order> HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 0e5fc923-8e3c-4560-9100-ce7283c3e081
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
Connection: Keep-Alive
```

### Response

If the request is successful, this method returns an **Order** resource in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 1252
Content-Type: application/json
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
MS-RequestId: 0e5fc923-8e3c-4560-9100-ce7283c3e081
Date: Mon, 23 Nov 2015 22:02:59 GMT

{
    "id": "d6595733-265f-4918-a62e-026e64bc8384",
    "referenceCustomerId": "<customer-tenant-id>",
    "lineItems": [{
        "lineItemNumber": 0,
        "offerId": "031C9E47-4802-4248-838E-778FB1D2CC05",
        "friendlyName": "nickname",
        "quantity": 1,
        "links": {
            "subscription": {
                "uri": "/subscriptions?key=<key>",
                "method": "GET",
                "headers": []
            }
        }
    }],
    "status": "none",
    "creationDate": "2015-10-08T10:42:36.54-07:00",
    "attributes": {
        "etag": "<etag>",
        "objectType": "Order"
    }
}
```
## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- View a list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
