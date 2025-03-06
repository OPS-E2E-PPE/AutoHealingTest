---
title: Create an order for a customer of an indirect reseller - Azure Cloud Solution Provider| Microsoft Docs
description: Learn how to create an order for a customer of an indirect reseller. The information in this article supports Azure Cloud Solution Provider (Azure CSP) integration.
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

# Create an order for a customer of an indirect reseller

Learn how to create an order for a customer of an indirect reseller. You can use one of the following options:

- C#
- REST API

For information about creating a basic customer order, see [Add subscriptions to a customer account](add-subscriptions.md). 

## C#

1. Get a collection of the indirect resellers that have a relationship with the signed-in partner.
2. Set a local variable to the item in the collection that matches the indirect reseller ID. Do this so that you can access the reseller's **MpnId** property when you create the order.
3. To record the customer, instantiate an **Order** object. Set the **ReferenceCustomerID** property to the customer identifier.
4. Create a list of **OrderLineItem** objects. Assign the list to the order's **LineItems** property. Each order line item contains the purchase information for one offer. Be sure that you populate the **ParterIdOnRecord** property in each line item with the Microsoft Partner Network (MPN) ID of the indirect reseller. You must have at least one order line item.
5. To get an interface for order operations:
    1. To identify the customer, call the **IAggregatePartner.Customers.ById** method with the customer ID.
    2. To retrieve the interface, use the **Orders** property.
6. To create the order, call the **Create** or the **CreateAsync** method.

```csharp
// IAggregatePartner partnerOperations;
// string customerId;
// string offerId;
// string indirectResellerId;

// Get a list of the indirect resellers that have a relationship with the signed-in partner.
var indirectResellers = partnerOperations.Relationships.Get(PartnerRelationshipType.IsIndirectCloudSolutionProviderOf);

// In the collection, find the matching reseller.
var selectedIndirectReseller = (indirectResellers != null && indirectResellers.Items.Any()) ?
    indirectResellers.Items.FirstOrDefault(reseller => reseller.Id.Equals(indirectResellerId, StringComparison.OrdinalIgnoreCase)) :
    null;

// Prepare the order. Populate the PartnerIdOnRecord with the reseller's Microsoft Partner Network ID. 
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

**URI parameter**

The following parameter is required for identifying the customer.

|Name|Type|Description|
|---|---|---|
|customer-id|string|A GUID-format string that identifies the customer.|

**Request body**

The following table describes the **Order** properties in the request body.

|Name|Type|Required?|Description|
|---|---|---|---|
|id|string|N|An order identifier that is supplied when the order is successfully created.|
|referenceCustomerId|string|Y|The customer identifier.|
|billingCycle|string|N|The frequency with which the partner is billed for this order. The default is **Monthly**. The frequency is applied when the order is successfully created. Supported values are the member names found in **BillingCycleType**.|
|lineItems|array of objects|Y|An array of **OrderLineItem** resources.|
|creationDate|string|N|The date the order was created, in date/time format. The value for **creationDate** is applied when the order is successfully created.|
|attributes|object|N|Contains `ObjectType`:`Order`.|

The following table describes the **OrderLineItem** properties in the request body.

|Name|Type|Required?|Description|
|---|---|---|---|
|lineItemNumber|int|Y|Each line item in the collection has a unique line number, counting up from 0 to *count*-1.|
|offerId|string|Y|The offer identifier.|
|subscriptionId|string|N|The subscription identifier.|
|parentSubscriptionId|string|N|The ID of the parent subscription in an add-on offer. Applies to PATCH only.|
|friendlyName|string|N|The display name for the subscription, which is defined by the partner. You can use the display name to help disambiguate subscriptions.|
|quantity|int|Y|The number of licenses for a license-based subscription. (Required only for license-based subscriptions.)|
|partnerIdOnRecord|string|N|When an indirect provider places an order on behalf of an indirect reseller, populate this field *only* with the Microsoft Partner Network (MPN) ID of the **indirect reseller** (never use the MPN ID of the indirect provider). This ensures proper accounting for incentives.|
|attributes|object|N|Contains `ObjectType`:`OrderLineItem`.| 

**Request example**

```json
POST https://api.partnercenter.microsoft.com/v1/customers/c501c3c4-d776-40ef-9ecf-9cefb59442c1/orders HTTP/1.1
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
    "ReferenceCustomerId": "c501c3c4-d776-40ef-9ecf-9cefb59442c1",
    "BillingCycle": "unknown",
    "LineItems": [{
            "LineItemNumber": 0,
            "OfferId": "DB2E705F-B82A-4024-A3D5-D88E12F2DB35",
            "SubscriptionId": null,
            "ParentSubscriptionId": null,
            "FriendlyName": "New offer purchase.",
            "Quantity": 5,
            "PartnerIdOnRecord": "4847383",
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
MS-ServerId: 020021921
Date: Mon, 10 Apr 2017 23:02:24 GMT

﻿ {
    "id": "3eddcac6-63b2-4c40-b0b6-f47e18301492",
    "referenceCustomerId": "c501c3c4-d776-40ef-9ecf-9cefb59442c1",
    "billingCycle": "monthly",
    "lineItems": [{
            "lineItemNumber": 0,
            "offerId": "DB2E705F-B82A-4024-A3D5-D88E12F2DB35",
            "subscriptionId": "42226ED6-070A-4E0F-B80C-4CDFB3E97AA7",
            "friendlyName": "New offer purchase.",
            "quantity": 5,
            "partnerIdOnRecord": "4847383",
            "links": {
                "subscription": {
                    "uri": "/customers/c501c3c4-d776-40ef-9ecf-9cefb59442c1/subscriptions/42226ED6-070A-4E0F-B80C-4CDFB3E97AA7",
                    "method": "GET",
                    "headers": []
                }
            }
        }
    ],
    "creationDate": "2017-04-10T16:02:25.983-07:00",
    "links": {
        "self": {
            "uri": "/customers/c501c3c4-d776-40ef-9ecf-9cefb59442c1/orders/3eddcac6-63b2-4c40-b0b6-f47e18301492",
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
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).