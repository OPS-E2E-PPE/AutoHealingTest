---
title: Get a customer's orders - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to get a customer's orders for Azure Cloud Solution Provider (Azure CSP) integration.
ervices: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: kirilk
---

# Get a customer's orders

Learn how to retrieve a list of all of a customer's orders. You can use one of the following options:

- PowerShell
- C#
- REST API

## PowerShell

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>'

Get-PCOrder -TenantId $customer.id
```

## C#

1. Use your **IAggregatePartner.Customers** collection and call the **ById()** method.
2. Call the **Orders** property.
3. Call the **Get()** or the **GetAsync()** method.

```csharp
// IAggregatePartner partnerOperations;
// var selectedCustomerID as String;

var orders = partnerOperations.Customers.ById(selectedCustomerId).Orders.Get();
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/customers/{customer-tenant-id}/orders HTTP/1.1|

**URI parameter**

Use the following query parameter to get all orders.

|Name|Type|Description|
|---|---|---|
|customer-tenant-id|guid|A GUID-format customer tenant ID. The reseller can use the customer tenant ID to filter results for a specific customer that is associated with the reseller.|

**Request example**

```
GET https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/orders HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: b1317092-f087-471e-a637-f66523b2b94c
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
```

### Response

If the request is successful, this method returns a collection of **Order** resources in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 31942
Content-Type: application/json
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
MS-RequestId: b1317092-f087-471e-a637-f66523b2b94c
Date: Mon, 23 Nov 2015 22:00:25 GMT

{
    "totalCount": 24,
    "items": [{
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
        "creationDate": "2015-10-08T10: 42: 36.54-07: 00",
        "attributes": {
            "etag": "<etag>",
            "objectType": "Order"
        }
    },
    {
        "id": "b8fe2ddb-6e85-4f8b-a5ba-06c20ce6d6c5",
        "referenceCustomerId": "<customer-tenant-id>",
        "lineItems": [{
            "lineItemNumber": 0,
            "offerId": "4271AA57-4129-477E-BA88-0A276D4A1619",
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
        "creationDate": "2015-10-09T21: 53: 18.83-07: 00",
        "attributes": {
            "etag": "<etag>",
            "objectType": "Order"
        }
    }
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).