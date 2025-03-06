---
title: Create user accounts in Partner Center | Microsoft Docs
description: Review scenario for Azure CSP Integration.
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

# Get a list of user roles for a customer

This guide covers how to retrieve a list of roles that users of a particular customer hold. You can accomplish this task with the C# or REST APIs.

## Retrieve roles by using C#

To retrieve all the directory roles for a specified customer, take the following steps:

1. Retrieve the specified customer ID. Then use the **IAggregatePartner.Customers** collection followed by the **ById() method.**
2. Call the **DirectoryRoles** property, followed by the **Get()** or **GetAsync()** method.

```csharp
// string selectedCustomerId;
// IAggregatePartner partnerOperations;

var directoryRoles = partnerOperations.Customers.ById(selectedCustomerId).DirectoryRoles.Get();
```

To retrieve a list of customer users that have a given role, take the following steps:

1. Retrieve the specified customer ID and the directory role ID.
2. Use the **IAggregatePartner.Customers** collection to call the **ById()** method.
3. Call the **DirectoryRoles property**, followed by the **ById()** method.
4. Call the **UserMembers** property,  followed by the **Get()** or **GetAsync()** method.

```csharp
// string selectedCustomerId;
// IAggregatePartner partnerOperations;
// string selectedDirectoryRoleId;

var userMembers = partnerOperations.Customers.ById(selectedCustomerId).DirectoryRoles.ById(selectedDirectoryRoleId).UserMembers.Get();
```

## Retrieve roles by using the REST API

### Request syntax

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/customers/{customer-tenant-id}/users/{user-id}/directoryroles HTTP/1.1|
|GET|{baseURL}/v1/customers/{customer-tenant-id}/directoryroles HTTP/1.1|
|GET|{baseURL}/v1/customers/{customer-tenant-id}/directoryroles/{role-ID}/usermembers|

### Request parameters

|Name|Type|Description|
|---|---|---|
|customer-tenant-id|GUID|Allows the reseller to filter the results for a given customer that belongs to the reseller.|
|user-id|GUID|Belongs to a single user account.|
|role-id|GUID|Belongs to a type of role. You can get these IDs by querying all the directory roles for a customer, across all user accounts (as described earlier in the second scenario).|

### Request example

```
GET https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/users/<user-id>/directoryroles HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: b1317092-f087-471e-a637-f66523b2b94c
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
```

### Response example

If the request is successful, this method returns a list of the roles that are associated with the given user account.

```
HTTP/1.1 200 OK
Content-Length: 31942
Content-Type: application/json
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
MS-RequestId: b1317092-f087-471e-a637-f66523b2b94c
Date: June 24 2016 22:00:25 PST

{
      "totalCount": 2,
      "items": [
        {
          "name": "Helpdesk Administrator",
          "id": "729827e3-9c14-49f7-bb1b-9608f156bbb8",
          "attributes": { "objectType": "DirectoryRole" }
        },
        {
          "name": "User Account Administrator",
          "id": "fe930be7-5e62-47db-91af-98c3a49a38b1",
          "attributes": { "objectType": "DirectoryRole" }
        }
      ],
      "attributes": { "objectType": "Collection" }
}
```

## Next steps

* Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
* See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).