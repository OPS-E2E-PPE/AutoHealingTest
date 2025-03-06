---
title: Delete a customer account from an integration sandbox - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to delete a customer account from an integration sandbox. The information in this article supports Azure Cloud Solution Provider (Azure CSP) integration.
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

# Delete a customer account from an integration sandbox

Learn how to remove a customer account from the Testing in Production integration sandbox. You can use one of the following options:

- C#
- REST API

## C#

1. Sign in to the sandbox by using your sandbox credentials.
2. Use your **IParnter.Customer** collection and call the **ById()** method.
3. Call the **Delete** method.

```csharp
// IPartnerCredentials tipAccountCredentials;
// string customerTenantId;

IPartner tipAccountPartnerOperations = PartnerService.Instance.CreatePartnerOperations(tipAccountCredentials);

tipAccountPartnerOperations.Customers.ById(customerTenantId).Delete();
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|DELETE|{baseURL}/v1/customers/{customer-tenant-id} HTTP/1.1|

**URI parameter**

The following query parameter is required for deleting an integration sandbox account.

|Name|Type|Description|
|---|---|---|
|customer-tenant-id|guid|A GUID-format customer tenant ID that refers to a specific customer.|

**Request example**

```http
DELETE https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id> HTTP/1.1
Accept: application/json
MS-RequestId: 655890ba-4d2b-4d09-a95f-4ea1348686a5
MS-CorrelationId: 1438ea3d-b515-45c7-9ec1-27ee0cc8e6bd
Content-Length: 0
```

### Response

If the request is successful, this method returns an empty response.

**Response example**

```http
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 1438ea3d-b515-45c7-9ec1-27ee0cc8e6bd
MS-RequestId: 655890ba-4d2b-4d09-a95f-4ea1348686a5
Date: Wed, 16 Mar 2016 00:43:02 GMT
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
