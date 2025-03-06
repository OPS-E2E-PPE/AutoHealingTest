---
title: Create user accounts in Partner Center - Azure Cloud Solution Provider | Microsoft Docs
description: This article covers how to retrieve a user account for a customer by providing an ID.
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

# Get a user account by ID

This guide covers how to retrieve a specific user account for a customer by providing an ID.

## PowerShell

To retrieve a specific user account, enter the following commands:

```powershell
$user = Get-PCCustomerUser -TenantId '<customer identifier>' -UserId '<user identifier>'
```

## C#

To retrieve a user account for a customer, do the following:

1. To identify the customer, call the **IAggregatePartner.Customers.ById** method with the customer ID. 
2. To retrieve the specific user, call the **Users.ById** method. 
3. To retrieve the user account, call the **Users. Get** or **GetAsync** method.

```csharp
// IAggregatePartner partnerOperations;
// string selectedCustomerId;
// string selectedCustomerUserId;

// Get customer user detail.
var customerUsers = partnerOperations.Customers.ById(selectedCustomerId).Users.ById(selectedCustomerUserId).Get();
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/customers/{customer-tenant-id}/users/{user-id} HTTP/1.1|

**URI parameter**

To identify the correct customer and user, use the following URI parameters:

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|A GUID-formatted string that allows the reseller to filter the results for a specific customer that belongs to the reseller.|
|**user-id**|**guid**|A GUID-formatted string that belongs to a single user account.|

**Request example**

```
GET https://api.partnercenter.microsoft.com/v1/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/users/a9ef48bb-8758-4590-a312-d4a47bfaded4 HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: c1f673cb-655c-45a7-8a6b-257a0a006f4b
MS-CorrelationId: 24a631eb-a110-49dc-8325-99d4b196774c
X-Locale: en-US
Host: api.partnercenter.microsoft.com
```

### Response

If the request is successful, this method returns the user account for the customer.

**Response example**

```
HTTP/1.1 200 OK
Content-Length: 432
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 24a631eb-a110-49dc-8325-99d4b196774c
MS-RequestId: c1f673cb-655c-45a7-8a6b-257a0a006f4b
MS-CV: uWM1EGU7+0aI2MvV.0
MS-ServerId: 020021921
Date: Wed, 21 Dec 2016 22:59:10 GMT

{
    "usageLocation": "US",
    "id": "a9ef48bb-8758-4590-a312-d4a47bfaded4",
    "userPrincipalName": "Daniel@dtdemocspcustomer005.onmicrosoft.com",
    "firstName": "Daniel",
    "lastName": "Tsai",
    "displayName": "Daniel Tsai",
    "userDomainType": "none",
    "state": "active",
    "links": {
        "self": {
            "uri": "/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/users/a9ef48bb-8758-4590-a312-d4a47bfaded4",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "CustomerUser"
    }
}
```

## Next steps

* Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
* View a list of [Azure CSP integration scenarios](../integration-scenarios-list.md).