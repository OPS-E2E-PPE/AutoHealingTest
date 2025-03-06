---
title: View a customer's deleted users - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to view a customer's deleted users. The information in this article supports Azure Cloud Solution Provider (Azure CSP) integration.
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

# View a customer's deleted users

When a user account is deleted, the user state is set to "inactive." The user account remains in an inactive state for 30 days. After 30 days, the account and its associated data is purged. Purged data cannot be recovered.

Within the 30-day window, deleted accounts can be restored. For more information about restoring a deleted user, see [Manage user accounts](manage-user-accounts.md).

Inactive user accounts do not return as part of the user collection. You must query them separately. You can use one of the following options:

- PowerShell
- C#
- REST API

## PowerShell

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>'

$user = Get-PCCustomerUser -TenantId $customer.id -deleted | ? id -EQ '<user id>'
```

## C#

To retrieve a list of deleted users, construct a query that filters for customer users whose status is set to inactive: 

1. Create the filter by instantiating a **SimpleFieldFilter** object by using the parameters shown in the following code example.
2. Create the query by using the **BuildIndexedQuery** method. If you do not want paged results, you can use the **BuildSimpleQuery** method instead.
3. To identify the customer, use the **IAggregatePartner.Customers.ById** method with the customer ID.
4. To send the request, call the **Query** method.

```csharp
// IAggregatePartner partnerOperations;
// int customerUserPageSize;

// Create a filter for users whose status is inactive (deleted).
var filter = new SimpleFieldFilter("UserState", FieldFilterOperation.Equals, "Inactive");

// Build a paged query.
var simpleQueryWithFilter = QueryFactory.Instance.BuildIndexedQuery(customerUserPageSize, 0, filter);

// Send the request.
var customerUsers = partnerOperations.Customers.ById(selectedCustomerId).Users.Query(simpleQueryWithFilter);
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/customers/{customer-id}/users?size={size}&filter={filter} HTTP/1.1|

**URI parameters**

Use the following path and query parameters when you create the request.

|Name|Type|Description|
|---|---|---|
|customer-id|guid|A GUID-format customer ID that identifies the customer.|
|size|int|(Optional) The number of results to display at one time.|
|filter|filter|The query that filters the user search. To retrieve deleted users, you must include and encode the following string: {"Field":"UserState","Value":"Inactive","Operator":"equals"}.|

**Request example**

```json
GET https://api.partnercenter.microsoft.com/v1/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/users?size=500&filter=%7B%22Field%22%3A%22UserState%22%2C%22Value%22%3A%22Inactive%22%2C%22Operator%22%3A%22equals%22%7D HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: c11feb95-55d2-45b6-9d1b-74b55d2221fb
MS-CorrelationId: 2b4ab588-f48c-4874-b479-a61895e107b2
X-Locale: en-US
Host: api.partnercenter.microsoft.com
```

### Response

If the request is successful, this method returns a collection of **CustomerUser** resources in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 802
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 690b34ca-07c8-4f8a-ab13-f22a50594a43
MS-RequestId: 1187f9ad-02b4-4d96-b668-7cf3d289467b
MS-CV: 3TLmR9gz6EaCVCjR.0
MS-ServerId: 101112616
Date: Fri, 20 Jan 2017 19:13:14 GMT

{
    "totalCount": 1,
    "items": [{
            "usageLocation": "US",
            "id": "a45f1416-3300-4f65-9e8d-f123b397a4ea",
            "userPrincipalName": "e83763f7f2204ac384cfcd49f79f2749@dtdemocspcustomer005.onmicrosoft.com",
            "firstName": "Ferdinand",
            "lastName": "Filibuster",
            "displayName": "Ferdinand",
            "userDomainType": "none",
            "state": "inactive",
            "softDeletionTime": "2017-01-20T00:33:34Z",
            "links": {
                "self": {
                    "uri": "/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/users/a45f1416-3300-4f65-9e8d-f123b397a4ea",
                    "method": "GET",
                    "headers": []
                }
            },
            "attributes": {
                "objectType": "CustomerUser"
            }
        }
    ],
    "links": {
        "self": {
            "uri": "/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/users?size=500&filter=%7B%22Field%22%3A%22UserStatus%22%2C%22Value%22%3A%22Inactive%22%2C%22Operator%22%3A%22equals%22%7D",
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