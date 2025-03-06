---
title: Verify domain availability - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to verify domain availability. The information in this article supports Azure Cloud Solution Provider (Azure CSP) integration.
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

# Verify domain availability

Learn how to determine whether a domain is available for a customer to use. You can use one of the following options:

- PowerShell
- C#
- REST API

## PowerShell

```powershell
$domainname = '<name>'
$domain = $domainname+'.onmicrosoft.com'
Get-PCDomainAvailability -domain $domain
```

## C#

1. To get an interface for domain operations, call **IAggregatePartner.Domains**.
2. Call the **ByDomain** method with the domain to check. This retrieves an interface for the operations that are available for a specific domain.
3. To see if the domain already exists, call the **Exists** method.

```csharp
// IAggregatePartner partnerOperations;
// const string domain = "contoso.onmicrosoft.com";  

bool result = partnerOperations.Domains.ByDomain(domain).Exists();
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|HEAD|{baseURL}/v1/domains/{domain} HTTP/1.1|

**URI parameter**

The following query parameter is required for verifying domain availability.

|Name|Type|Description|
|---|---|---|
|domain|string|A string that identifies the domain to check.

**Request example**

```http
HEAD https://api.partnercenter.microsoft.com/v1/domains/contoso.onmicrosoft.com HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: cf5b00d6-9240-431c-a973-cc06c904e5bf
MS-CorrelationId: ec57501a-a4c3-45ee-ab2b-da4250545fc9
X-Locale: en-US
Host: api.partnercenter.microsoft.com
Connection: Keep-Alive
```

### Response

If the domain exists, it is not available for use. A response status code 200 OK is returned. 

If the domain is not found, it is available for use. A response status code 404 Not Found is returned.

**Response example when the domain is in use**

```http
HTTP/1.1 200 OK
Content-Length: 0
MS-CorrelationId: ec57501a-a4c3-45ee-ab2b-da4250545fc9
MS-RequestId: cf5b00d6-9240-431c-a973-cc06c904e5bf
MS-CV: 7UXAHds8J0mNUCSp.0
MS-ServerId: 201022015
Date: Tue, 31 Jan 2017 22:22:35 GMT
```

**Response example when the domain is available**

```http
HTTP/1.1 404 Not Found
Content-Length: 0
MS-CorrelationId: 54770745-17f0-433c-bd7b-0265e5b38f98
MS-RequestId: 1169a4cd-3be7-4e29-9cb3-0f78ffa2e91e
MS-CV: RRmc+bEw9U2e97CC.0
MS-ServerId: 202010406
Date: Tue, 31 Jan 2017 22:36:01 GMT
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
