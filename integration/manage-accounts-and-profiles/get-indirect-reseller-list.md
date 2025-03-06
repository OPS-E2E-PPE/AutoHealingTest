---
title: Get a list of indirect resellers - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to get a list of indirect resellers in a scenario for Azure Cloud Solution Provider (Azure CSP) integration. 
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2018
ms.author: kirilk
---

# Get a list of indirect resellers

Learn how to get a list of your indirect resellers. In addition to the Partner Dashboard you can use one of the following options:

- .NET SDK
- Java SDK
- PowerShell
- REST API

## .NET SDK

1. To establish an interface for relationship collection operations, use the **partnerOperations.Relationships** property.
2. Call the **Get** or the **Get_Async** method.
3. To identify the relationship type, pass a member of the **PartnerRelationshipType** enumeration. To retrieve indirect resellers, you must use **IsIndirectCloudSolutionProviderOf**.

```csharp
// IAggregatePartner partnerOperations;

var indirectResellers = partnerOperations.Relationships.Get(PartnerRelationshipType.IsIndirectCloudSolutionProviderOf);
```

## Java SDK

1. To establish an interface for relationship collection operations, use the **partnerOperations.getRelationships** function.
2. Call the **get** function.
3. To identify the relationship type, pass a member of the **PartnerRelationshipType** enumeration. To retrieve indirect resellers, you must use **ISINDIRECTCLOUDSOLUTIONPROVIDEROF**.

```java
// IPartner partnerOperations;

ResourceCollection<PartnerRelationship> indirectResellers = partnerOperations.getRelationships().get(PartnerRelationshipType.ISINDIRECTCLOUDSOLUTIONPROVIDEROF);
```

## PowerShell

Run the following command to get a list of indirect resellers

```powershell
Get-PartnerIndirectReseller
```

Review the [Get-PartnerIndirectReseller](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/docs/help/Get-PartnerIndirectReseller.md) help for more information.

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|GET|   {baseURL}/v1/relationships?relationship_type=IsIndirectCloudSolutionProviderOf HTTP/1.1|

**URI parameters**

The following table lists the query parameters that are required for identifying the relationship type.

|Name|Type|Description|
|---|---|---|
|relationship_type|string|<ul><li>The string representation of a member name from the **PartnerRelationshipType** enumeration.</li><li> If the partner is signed in as a provider and you want a list of the indirect resellers with whom the provider has established a relationship, use **IsIndirectCloudSolutionProviderOf**. </li><li>If the partner is signed in as a reseller and you want a list of the indirect providers with whom the user has established a relationship, use **IsIndirectResellerOf**.</li></ul>|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/relationships?relationship_type=IsIndirectCloudSolutionProviderOf HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 144391a4-fb06-41ae-b684-3308ce4706bd
MS-CorrelationId: 72524ef8-81aa-4141-a049-45a4fece5d84
X-Locale: en-US
Host: api.partnercenter.microsoft.com
```

### Response

If the request is successful, the response body contains a collection of **PartnerRelationship** resources that identify the resellers.

**Response example**

```http
HTTP/1.1 200 OK
Content-Length: 298
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 72524ef8-81aa-4141-a049-45a4fece5d84
MS-RequestId: 144391a4-fb06-41ae-b684-3308ce4706bd
MS-CV: b21Ll1miM0yFMPQQ.0
MS-ServerId: 030020643
Date: Wed, 05 Apr 2017 21:08:44 GMT

{
    "totalCount": 2,
    "items": [{
            "id": "484e548c-f5f3-4528-93a9-c16c6373cb59",
            "name": "First Up Consultants",
            "relationshipType": "is_indirect_cloud_solution_provider_of",
            "state": "Active",
            "mpnId": "4847383",
            "location": "US",
            "attributes": {
                "objectType": "PartnerRelationship"
            }
        }, {
            "id": "b01b1487-b36e-4e6d-9b5e-0b58974c4b28",
            "name": "ReleCloud",
            "relationshipType": "is_indirect_cloud_solution_provider_of",
            "state": "Active",
            "mpnId": "4847433",
            "location": "BR",
            "attributes": {
                "objectType": "PartnerRelationship"
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
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).