---
title: Get subscriptions by partner MPN ID for Azure Cloud Solution Provider integration | Microsoft Docs
description: Learn how to get a customer's subscriptions by the partner's Microsoft Partner Network (MPN) ID for Azure Cloud Solution Provider (Azure CSP) integration.
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

# Get a list of a customer's subscriptions by using a Microsoft Partner Network ID

Learn how to use a partner's Microsoft Partner Network (MPN) ID to get a list of a customer's subscriptions that the partner gave to the customer. You can use the information for Azure Cloud Solution Provider (Azure CSP) integration. To get the list of subscriptions, you can use one of the following options:

- .NET SDK
- Java SDK
- PowerShell
- REST API

## .NET SDK

1. To identify the customer, use the **IAggregatePartner.Customers.ById** method with the customer identifier.
2. To get an interface for the collection operations of customer subscriptions, use the **Subscriptions** property.
3. To identify the partner and to get an interface for partner subscription operations, call the **ByPartner** method with the MPN ID.
4. To get the collection, call the **Get** or the **GetAsync** method.

```csharp
// IAggregatePartner partnerOperations;
// string customerId;
// string partnerMpnId;

var customerSubscriptionsByMpnId = partnerOperations.Customers.ById(customerId).Subscriptions.ByPartner(partnerMpnId).Get();
```

## Java SDK

1. To identify the customer, use the **IPartner.getCustomers().byId** function with the customer identifier.
2. To get an interface for the collection operations of customer subscriptions, use the **getSubscriptions** function.
3. To identify the partner and to get an interface for partner subscription operations, call the **byPartner** function with the MPN identifier.
4. To get the collection, call the **get** function.

```java
// IPartner partnerOperations;
// String customerId;
// String partnerMpnId;

ResourceCollection<Subscription> subscriptionsByMpnId = partnerOperations.getCustomers().byId(customerId).getSubscriptions().byPartner(partnerMpnId).get();
```

## PowerShell

Run the following command to get a list of subscriptions using the MPN identifier

```powershell
Get-PartnerCustomerSubscription -CustomerId $customerId -MpnId $partnerMpnId
```

Review the [Get-PartnerCustomerSubscription](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/docs/help/Get-PartnerCustomerSubscription.md) help for more information.

## REST API

**Request syntax**

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/customers/{customer-id}/subscriptions?mpn_id={mpn-id} HTTP/1.1|

**URI parameters**

|Name|Type|Description|
|---|---|---|
|customer-id|string|A GUID-format string that identifies the customer.
|mpn-id|int|An MPN ID that identifies the partner.


**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/c501c3c4-d776-40ef-9ecf-9cefb59442c1/subscriptions?mpn_id=4847383 HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: d0e38dfd-a2c5-4a14-ac06-12d30f0ec54e
MS-CorrelationId: e937630b-8341-4d70-8f73-450d32ee0189
X-Locale: en-US
Host: api.partnercenter.microsoft.com
Connection: Keep-Alive
```

**Response example**

If the request is successful, the response body contains the collection of subscription resources.

```http
HTTP/1.1 200 OK
Content-Length: 985
Content-Type: application/json; charset=utf-8
MS-CorrelationId: e937630b-8341-4d70-8f73-450d32ee0189
MS-RequestId: d0e38dfd-a2c5-4a14-ac06-12d30f0ec54e
MS-CV: LdFhumtx6Ea0Kl5Z.0
MS-ServerId: 101112202
Date: Thu, 13 Apr 2017 20:58:08 GMT

{
    "totalCount": 1,
    "items": [{
            "id": "42226ED6-070A-4E0F-B80C-4CDFB3E97AA7",
            "offerId": "DB2E705F-B82A-4024-A3D5-D88E12F2DB35",
            "offerName": "Intune Device",
            "friendlyName": "new offer purchase",
            "quantity": 5,
            "unitType": "Licenses",
            "creationDate": "2017-04-10T23:02:26.02Z",
            "effectiveStartDate": "2017-04-10T00:00:00Z",
            "commitmentEndDate": "2018-05-07T00:00:00Z",
            "status": "active",
            "autoRenewEnabled": true,
            "isTrial": false,
            "billingType": "license",
            "billingCycle": "monthly",
            "partnerId": "4847383",
            "contractType": "subscription",
            "links": {
                "offer": {
                    "uri": "/offers/DB2E705F-B82A-4024-A3D5-D88E12F2DB35?country=US",
                    "method": "GET",
                    "headers": []
                },
                "self": {
                    "uri": "/customers/c501c3c4-d776-40ef-9ecf-9cefb59442c1/subscriptions/42226ED6-070A-4E0F-B80C-4CDFB3E97AA7",
                    "method": "GET",
                    "headers": []
                }
            },
            "orderId": "3EDDCAC6-63B2-4C40-B0B6-F47E18301492",
            "attributes": {
                "etag": "eyJpZCI6IjQyMjI2ZWQ2LTA3MGEtNGUwZi1iODBjLTRjZGZiM2U5N2FhNyIsInZlcnNpb24iOjF9",
                "objectType": "Subscription"
            }
        }
    ],
    "attributes": {
        "objectType": "Collection"
    }
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md)