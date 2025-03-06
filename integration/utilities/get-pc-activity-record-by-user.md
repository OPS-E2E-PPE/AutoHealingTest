---
title: Get a record of Partner Center activity by user - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to get a record of Microsoft Partner Center activity by user. The information in this article supports Azure Cloud Solution Provide (Azure CSP) integration.
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

# Get a record of Partner Center activity by user

Learn how to get detailed information about changes that have been made to customer accounts and subscriptions over a period of time. You can use the information for auditing or for analytical purposes. To get a record of Microsoft Partner Center activity by user, you can use one of the following options:

- Included information
- C#
- REST API

## Included information

The following actions are included in the log, along with the user account that made the changes, and the date.

|Actions in Partner Center|Actions via the Partner Center SDK|
|---|---|
|<ul><li>Update subscriptions</li><li>Update orders</li><li>Update an Azure spending budget</li><li>Upgrade subscriptions</li></ul>|<ul><li>Create, update, or delete customer accounts</li><li>Update subscriptions</li><li>Update orders</li><li>Update an Azure spending budget</li><li>Upgrade subscriptions</li></ul> Note that changes made by using the CREST API are not included in the log.|

## C#

To retrieve a record of Partner Center activity by user, use **IPartner.AuditRecords** to get a collection that enumerates the actions.

```csharp
//IPartner ScopedPartnerOperations;
//var filter;
//var startDate;

//Setting up the filter, per the code sample.
var filter = new SimpleFieldFilter(AuditRecordSearchField.CompanyName.ToString(), FieldFilterOperation.Substring, "00");

var collection = scopedPartnerOperations.AuditRecords.Query(startDate.Date, query: QueryFactory.Instance.BuildSimpleQuery(filter: filter));
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/auditrecords?startDate=yyyy-mm-dd HTTP/1.1|
|GET|{baseURL}/v1/auditrecords?startDate=yyyy-mm-dd&endDate=yyyy-mm-dd HTTP/1.1|
|GET|{baseURL}/v1/auditrecords?startDate=yyyy-mm-dd&endDate=yyyy-mm-dd&filter={"Field":"CompanyName","Value":"Central","Operator":0} HTTP/1.1|

**URI parameter**

Use the following query parameter search *only* for actions that affect a single customer.

|Name|Type|Description|
|---|---|---|
|CompanyName|string|The exact name of the customer, as listed in Partner Center.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/auditrecords?startDate=2016-05-02 
Authorization: Bearer <AUTH-TOKEN>
Accept: application/json
MS-Contract-Version: v1
MS-RequestId: be82a8ba-4a53-49f7-8313-b033c058687e
MS-CorrelationId: 648a26a4-a63e-459f-844b-4f29d7913353
Content-Length: 0
Content-Type: application/json
```

### Response

If the request is successful, this method returns a set of activities that meet the filter requirements.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 1346
Content-Type: application/json; charset=utf-8
Server: Microsoft-HTTPAPI/2.0
MS-CorrelationId: 648a26a4-a63e-459f-844b-4f29d7913353
MS-RequestId: be82a8ba-4a53-49f7-8313-b033c058687e
Date: Tue, 10 May 2016 19:00:10 GMT

{
    "totalCount": 1,
    "items": [{
            "customerId": "49bd2a7e-0776-487d-99f6-e7536fb82049",
              "customerName": "Customer",
              "userPrincipalName": "user@domain.onmicrosoft.com",
              "applicationId": "87fe183e-041c-4cf7-b221-c065d9582cde",
              "resourceType": "order",
              "resourceNewValue": "\"{\\\"Id\\\":null,\\\"ReferenceCustomerId\\\":\\\"49bd2a7e-0776-487d-99f6-e7536fb82049\\\",\\\"LineItems\\\":[{\\\"LineItemNumber\\\":0,\\\"OfferId\\\":\\\"5C9FD4CC-EDCE-44A8-8E91-07DF09744609\\\",\\\"SubscriptionId\\\":null,\\\"ParentSubscriptionId\\\":null,\\\"FriendlyName\\\":\\\"Office 365 Business\\\",\\\"Quantity\\\":2,\\\"PartnerIdOnRecord\\\":null,\\\"Attributes\\\":{\\\"ObjectType\\\":\\\"OrderLineItem\\\"}}],\\\"CreationDate\\\":null,\\\"Attributes\\\":{\\\"ObjectType\\\":\\\"Order\\\"}}\"",
              "operationType": "create_order",
              "operationDate": "2016-05-10T18:34:37.5980461Z",
              "operationStatus": "succeeded",
              "originalCorrelationId": "94588c3f-5e41-4dbd-986f-1d5146453a46",
              "sessionId": "9c75b432-a7b3-4772-bff1-c9d44b190b98",
              "attributes": {
                  "objectType": "AuditRecord"
            }
        }
    ],
      "links": {
        "self": {
            "uri": "/auditrecords?startDate=2016-05-02&filter={" Field ":" CompanyName "," Value ":" Central "," Operator ":" equals "}",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "Collection"
    }
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
