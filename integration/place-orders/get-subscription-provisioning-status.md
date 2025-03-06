---
title: Get a customer's subscription provisioning status - Azure Cloud Solution Provider | Microsoft Docs
description: This article covers how to retrieve the subscription provisioning status for a customer subscription by using the REST API.
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

# Get subscription provisioning status

This guide covers how to retrieve the subscription provisioning status for a customer subscription by using the REST API. The status is updated every 15 minutes.

## Request

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/customers/{customer-id}/subscriptions/{subscription-id}/provisioningstatus HTTP/1.1|

**URI parameters**

To identify the customer and subscription, use the following *required* path parameters:

|Name|Type|Subscription|
|---|---|---|
|**customer-id**|**guid**|A GUID-formatted string that identifies the customer.|
|**subscription-id**|**guid**|A GUID-formatted string that identifies the subscription.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/0c39d6d5-c70d-4c55-bc02-f620844f3fd1/subscriptions/34828C05-C16C-4D6F-9CFC-4D2650EF19A1/provisioningstatus HTTP/1.1
Accept: application/json, text/plain, */*
Authorization: Bearer <token>
MS-RequestId: d0e38dfd-a2c5-4a14-ac06-12d30f0ec54e
MS-CorrelationId: e937630b-8341-4d70-8f73-450d32ee0189
X-Locale: en-US
Host: api.partnercenter.microsoft.com
```

## Response

If the request is successful, the response body contains a **SubscriptionProvisioningStatus** resource.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 177
Content-Type: application/json; charset=utf-8
MS-CorrelationId: e937630b-8341-4d70-8f73-450d32ee0189
MS-RequestId: d0e38dfd-a2c5-4a14-ac06-12d30f0ec54e
MS-CV: InswEQre402koceL.0
MS-ServerId: 030020344
Date: Thu, 20 Apr 2017 19:23:39 GMT

﻿ {
    "skuId": "6FD2C87F-B296-42F0-B197-1E91E994B900",
    "status": "success",
    "quantity": 5,
    "endDate": "2018-05-10T00:00:00Z",
    "attributes": {
        "objectType": "SubscriptionProvisioningStatus"
    }
}
```
## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- View a list of [Azure CSP integration scenarios](../integration-scenarios-list.md). 