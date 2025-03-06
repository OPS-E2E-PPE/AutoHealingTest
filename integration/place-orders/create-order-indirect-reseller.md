---
title: Create an order for a customer of an indirect reseller - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to create an order for a customer of an indirect reseller.
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

# Create an order for a customer of an indirect reseller

Learn how to create an order for a customer of an indirect reseller. You can use one of the following options:

* C#
* REST API

## C#

To create an order for a customer of an indirect reseller, do the following:
1. Get a collection of the indirect resellers that have a relationship with the signed-in partner. 
2. Set a local variable to the item in the collection that matches the indirect reseller ID. This setting helps you access the reseller's **MpnId** property when you create the order.
3. To record the customer, instantiate an **Order** object, and then set the **ReferenceCustomerID** property to the customer identifier.
4. Create a list of **OrderLineItem** objects, and then assign the list to the order's **LineItems** property. Each order line item contains the purchase information for one offer. Be sure to populate the **ParterIdOnRecord** property in each line item with the MPN ID of the indirect reseller. You must have at least one order line item.
5. To identify the customer, obtain an interface to order operations by calling the **IAggregatePartner.Customers.ById** method with the customer ID, and then retrieve the interface from the **Orders** property.
6. To create the order, call the **Create** or **CreateAsync** method.

```csharp
// IAggregatePartner partnerOperations;
// string customerId;
// string offerId;
// string indirectResellerId;

// Get the indirect resellers with a relationship to the signed-in partner.
var indirectResellers = partnerOperations.Relationships.Get(PartnerRelationshipType.IsIndirectCloudSolutionProviderOf);

// Find the matching reseller in the collection.
var selectedIndirectReseller = (indirectResellers != null && indirectResellers.Items.Any()) ?
    indirectResellers.Items.FirstOrDefault(reseller => reseller.Id.Equals(indirectResellerId, StringComparison.OrdinalIgnoreCase)) :
    null;

// Prepare the order and populate the PartnerIdOnRecord with the reseller's Microsoft Partner Network ID. 
var order = new Order()
{
    ReferenceCustomerId = customerId,
    LineItems = new List<OrderLineItem>()
    {
        new OrderLineItem()
        {
            OfferId = offerId,
            FriendlyName = "New offer purchase.",
            Quantity = 5,
            PartnerIdOnRecord = selectedIndirectReseller != null ? selectedIndirectReseller.MpnId : null
        }
    }
};

// Place the order.
var createdOrder = partnerOperations.Customers.ById(customerId).Orders.Create(order);
```

## REST API

### Request

|Method|Request URI|
|---|---|
|POST|{baseURL}/v1/customers/{customer-id}/orders HTTP/1.1|

**URI parameters**

To identify the customer, use the following *required* parameter:

|Name|Type|Description|
|---|---|---|
|customer-id|string|A GUID-formatted string that identifies the customer.|

**Request body**

The request body contains the following **Order** properties:

|Name|Type|Required?|Description|
|---|---|---|---|
|id|string|N|The order identifier that is supplied when the order is created successfully.|
|referenceCustomerId|string|Y|The customer identifier.|
|billingCycle|string|N|The frequency with which the partner is billed for this order. The default setting is *monthly*, and it is applied when the order is created successfully. The supported values are the member names found in **BillingCycleType**. Note that annual billing is not yet supported, but support is coming soon.|
|lineItems|array of objects|Y|An array of **OrderLineItem** resources.|
|creationDate|string|N|The date the order was created, in date-time format. The setting is applied when the order is created successfully.|
|attributes|object|N|Contains the **Order** object type.|

The request body contains the following **OrderLineItem** object properties:

|Name|Type|Required?|Description|
|---|---|---|---|
|lineItemNumber|int|Y|Each line item in the collection gets a unique line number, counting up from 0 to count-1.|
|offerId|string|Y|The offer identifier.|
|subscriptionId|string|N|The subscription identifier.|
|parentSubscriptionId|string|N|The ID of the parent subscription in an add-on offer. Applies to PATCH only.|
|friendlyName|string|N|The friendly name of the subscription defined by the partner to help disambiguate.|
|quantity|int|Y*|The number of licenses for a license-based subscription. <br>*Required only for license-based subscriptions.|
|partnerIdOnRecord|string|N|When an indirect provider places an order on behalf of an indirect reseller, populate this field with the MPN ID of the *indirect reseller only*, never the ID of the indirect provider. This approach ensures proper accounting for incentives.|
|attributes|object|N|Contains the **OrderLineItem** object type.| 

**Request example**

```json
POST https://api.partnercenter.microsoft.com/v1/customers/<customer ID>/orders HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 02109f46-3ff2-4be4-9f37-b2eb6d58d542
MS-CorrelationId: 85195ae6-3de5-4978-abd4-7be2fbfe4c84
X-Locale: en-US
Content-Type: application/json
Host: api.partnercenter.microsoft.com
Content-Length: 410
Expect: 100-continue

{
    "Id": null,
    "ReferenceCustomerId": "<customer ID>",
    "BillingCycle": "unknown",
    "LineItems": [{
            "LineItemNumber": 0,
            "OfferId": "<offer ID>",
            "SubscriptionId": null,
            "ParentSubscriptionId": null,
            "FriendlyName": "New offer purchase.",
            "Quantity": 5,
            "PartnerIdOnRecord": "<partner ID>",
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

### Response

If the request is successful, the response body contains the populated **Order** resource.

```json
HTTP/1.1 201 Created
Content-Length: 831
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 85195ae6-3de5-4978-abd4-7be2fbfe4c84
MS-RequestId: 02109f46-3ff2-4be4-9f37-b2eb6d58d542
MS-CV: Nd3Oum/L5EywtKQK.0
MS-ServerId: <server ID>
Date: Mon, 10 Apr 2017 23:02:24 GMT

﻿ {
    "id": "<order ID>",
    "referenceCustomerId": "<customer ID>",
    "billingCycle": "monthly",
    "lineItems": [{
            "lineItemNumber": 0,
            "offerId": "<offer ID>",
            "subscriptionId": "<subscription ID>",
            "friendlyName": "New offer purchase.",
            "quantity": 5,
            "partnerIdOnRecord": "<partner ID>",
            "links": {
                "subscription": {
                    "uri": "/customers/<customer ID>/subscriptions/<subscription ID>",
                    "method": "GET",
                    "headers": []
                }
            }
        }
    ],
    "creationDate": "2017-04-10T16:02:25.983-07:00",
    "links": {
        "self": {
            "uri": "/customers/<customer ID>/orders/<subscription ID>",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "etag": "eyJpZCI6IjNlZGRjYWM2LTYzYjItNGM0MC1iMGI2LWY0N2UxODMwMTQ5MiIsInZlcnNpb24iOjF9",
        "objectType": "Order"
    }
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- View a list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
