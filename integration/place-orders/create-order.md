---
title: Create an order - Azure Cloud Solution Provider | Microsoft Docs
description: This article covers how to create an order for a customer by using the Microsoft Partner Center APIs.
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


# Create an order

This guide covers how to create an order for a customer by using the Microsoft Partner Center APIs.

## PowerShell

To create an order in PowerShell, enter the following commands:

```powershell
# Get a customer
$customer = Get-PCCustomer -tenantid '<tenant id GUID>'
# Get offer
$offer = Get-PCOffer -countryid '<country two digits id>' -offerid '<offer id GUID>'
# Create the OrderLineItem
$lineItems = @()
$lineItems += [OrderLineItem]::new()
$lineItems[0].LineItemNumber = 0
$lineItems[0].FriendlyName = '<friendly name>'
$lineItems[0].OfferId = $offer.id
$lineItems[0].Quantity = <quantity>
# Send order
New-PCOrder -tenantid $customer.id -LineItems $lineItems
```

## C#

To create an order for a customer in C#, do the following:
1. To record the customer, instantiate an **Order** object, and then set the **ReferenceCustomerID** property to the customer ID to record the customer. 
2. Create a list of **OrderLineItem** objects, and then assign the list to the order's **LineItems** property. Each order line item contains the purchase information for one offer. You must have at least one order line item.
3. To identify the customer, obtain an interface to order operations by calling the **IAggregatePartner.Customers.ById** method with the customer ID, and then retrieve the interface from the **Orders** property.
4. To create the order, call the **Create** or **CreateAsync** method.

```csharp
IAggregatePartner partnerOperations;
string customerId;
string offerId;

var order = new Order()
{
    ReferenceCustomerId = customerId,
    LineItems = new List<OrderLineItem>()
    {
        new OrderLineItem()
        {
            OfferId = offerId,
            FriendlyName = "new offer purchase",
            Quantity = 5
        }
    }
};

var createdOrder = partnerOperations.Customers.ById(customerId).Orders.Create(order);
```

## REST API

**Request syntax**

|Method|Request URI|
|---|---|
|POST|{baseURL}/v1/customers/{customer-id}/orders HTTP/1.1|

**URI parameters**

|Name|Type|Required|Description|
|---|---|---|---|
|**customer-id**|string|A GUID-formatted string that identifies the customer.|

**Request body**

The request body contains the following **Order** properties:

|Name|Type|Required?|Description|
|---|---|---|---|
|id|string|N|The order identifier that is supplied when the order is created successfully.|
|referenceCustomerId|string|Y|The customer identifier.|
|billingCycle|string|N|The frequency with which the partner is billed for this order. The default is *monthly*, and it is applied when the order is created successfully. Supported values are the member names found in **BillingCycleType**. Note that annual billing is not yet supported.|
|lineItems|array of objects|Y|An array of **OrderLineItem** resources.|
|creationDate|string|N|The date when the order was created, in date-time format. It is applied when the order is created successfully.|
|attributes|object|N|Contains the **Order** object type.|

The request body contains the following **OrderLineItem** object properties:

|Namne|Type|Required?|Description|
|---|---|---|---|
|lineItemNumber|int|Y|Each line item in the collection gets a unique line number, counting up from 0 to count-1.|
|offerId|string|Y|The offer identifier.|
|subscriptionId|string|N|The subscription identifier.|
|parentSubscriptionId|string|N|The ID of the parent subscription in an add-on offer. Applies to PATCH only.|
|friendlyName|string|N|The friendly name of the subscription, as defined by the partner to help disambiguate.|
|quantity|int|Y*|The number of licenses for a license-based subscription. <br>*Required only for license-based subscriptions.|
|partnerIdOnRecord|string|N*|When an indirect provider places an order on behalf of an indirect reseller, populate this field with the MPN ID of the *indirect reseller only*, never the ID of the indirect provider. This approach ensures proper accounting for incentives.|
|attributes|object|N|Contains the **OrderLineItem** object type.| 

**Request example**

```json
POST https://api.partnercenter.microsoft.com/v1/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/orders HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 57870501-203b-468e-8a63-078a3826d8ec
MS-CorrelationId: 9c272436-538d-4dd4-a421-c811e004784c
X-Locale: en-US
Content-Type: application/json
Host: api.partnercenter.microsoft.com
Content-Length: 405
Expect: 100-continue
Connection: Keep-Alive

{
    "Id": null,
    "ReferenceCustomerId": "4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04",
    "BillingCycle": "unknown",
    "LineItems": [{
            "LineItemNumber": 0,
            "OfferId": "84A03D81-6B37-4D66-8D4A-FAEA24541538",
            "SubscriptionId": null,
            "ParentSubscriptionId": null,
            "FriendlyName": "new offer purchase",
            "Quantity": 5,
            "PartnerIdOnRecord": null,
            "Attributes": {
                "ObjectType": "OrderLineItem"
            }
        }
    ],
    "CreationDate": null,
    "Attributes": {
        "ObjectType": "Order"
    }
}
```

If the request is successful, the response body contains the populated **Order** resource.


**Response example**

```json
HTTP/1.1 201 Created
Content-Length: 801
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 9c272436-538d-4dd4-a421-c811e004784c
MS-RequestId: 57870501-203b-468e-8a63-078a3826d8ec
MS-CV: VcfS+fqdQUW8Nap6.0
MS-ServerId: 030020525
Date: Thu, 30 Mar 2017 17:43:08 GMT

﻿ {
    "id": "074bd849-9106-405c-9923-fa061839d487",
    "referenceCustomerId": "4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04",
    "billingCycle": "monthly",
    "lineItems": [{
            "lineItemNumber": 0,
            "offerId": "84A03D81-6B37-4D66-8D4A-FAEA24541538",
            "subscriptionId": "1DA3295E-C59A-476C-A7E7-D7E981FE79BE",
            "friendlyName": "new offer purchase",
            "quantity": 5,
            "links": {
                "subscription": {
                    "uri": "/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/subscriptions/1DA3295E-C59A-476C-A7E7-D7E981FE79BE",
                    "method": "GET",
                    "headers": []
                }
            }
        }
    ],
    "creationDate": "2017-03-30T10:43:07.157-07:00",
    "links": {
        "self": {
            "uri": "/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/orders/074bd849-9106-405c-9923-fa061839d487",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "etag": "eyJpZCI6IjA3NGJkODQ5LTkxMDYtNDA1Yy05OTIzLWZhMDYxODM5ZDQ4NyIsInZlcnNpb24iOjF9",
        "objectType": "Order"
    }
}
```
## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- View a list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
