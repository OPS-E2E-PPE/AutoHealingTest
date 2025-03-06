---
title: Verify a partner's Microsoft Partner Network profile - Azure Cloud Solution Provider | Microsoft Docs
description: Verify a partner's Microsoft Partner Network (MPN) profile for Azure Cloud Solution Provider (Azure CSP) integration.
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

# Get and verify a partner's Microsoft Partner Network profile

Learn how to get a partner's Microsoft Partner Network (MPN) profile, and verify an MPN ID. You can use one of the following options:

- .NET SDK
- Java SDK
- PowerShell
- REST API

## .NET SDK

### Verify

To verify a partner's MPN ID:

1. To get an interface for partner profile collection operations, use the **IAggregatePartner.Profiles** property.
2. To get an interface for MPN profile operations, use the **MpnProfile** property.
3. To retrieve the MPN profile, call the **Get** or the **GetAsync** method with the MPN ID.

If you omit the MPN ID from the **Get** or **GetAsync** call, the request attempts to retrieve the MPN profile of the partner who currently is authenticated.

```csharp
// IAggregatePartner partnerOperations;
// string partnerMpnId;

var partnerProfile = partnerOperations.Profiles.MpnProfile.Get(partnerMpnId);
```

### Retrieve

To get a partner's MPN profile:

1. Use your **IAggregatePartner.Profiles** collection.
2. Call the **MpnProfile** property.
3. Call the **Get()** or the **GetAsync()** method.

```csharp
// IAggregatePartner partnerOperations;

mpnProfile = partnerOperations.Profiles.MpnProfile.Get();
```

## Java SDK

### Verify

To verify a partner's MPN ID:

1. To get an interface for partner profile collection operations, use the **IPartner.getProfiles** function.
2. To get an interface for MPN profile operations, use the **getMpnProfile** function.
3. To retrieve the MPN profile, call the **get** function with the MPN identifier.

If you omit the MPN ID from the **get**, the request attempts to retrieve the MPN profile of the partner who currently is authenticated.

```java
// IPartner partnerOperations;
// String partnerMpnId;

MpnProfile mpnProfile = partnerOperations.getProfiles().getMpnProfile().get(partnerMpnId);
```

### Retrieve

To get a partner's MPN profile:

1. Use your **IPartner.getProfiles** collection.
2. Call the **getMpnProfile** function.
3. Call the **get()** function.

```java
// IPartner partnerOperations;

MpnProfile mpnProfile = partnerOperations.getProfiles().getMpnProfile().get();
```

## PowerShell

Run the following command to get the MPN profile. If the *-MpnId* parameter is omitted the MPN profile of the partner who currently is authenticated will be returned.

```powershell
Get-PartnerMpnProfile
```

Review the [Get-PartnerMpnProfile](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/docs/help/Get-PartnerMpnProfile.md) help for more information.

## REST API

### Verify

**Request syntax**

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/profiles/mpn?mpnId={mpn-id} HTTP/1.1|

**URI parameter**

|Name|Type|Description|
|---|---|---|
|mpn-id|int|An MPN ID that identifies the partner. If you don't include this value, the profile of the partner who currently is signed in is retrieved.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/profiles/mpn?mpnId=9999999 HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 560df6b9-6e53-4954-aed7-133477ac1194
MS-CorrelationId: e937630b-8341-4d70-8f73-450d32ee0189
X-Locale: en-US
MS-PartnerCenter-Client: Partner Center .NET SDK
Host: api.partnercenter.microsoft.com
Connection: Keep-Alive
```

**Response example**

If the request is successful, the response body contains the **MpnProfile** resource for the partner.

**Success**

```http
HTTP/1.1 200 OK
Content-Length: 159
Content-Type: application/json; charset=utf-8
MS-CorrelationId: e937630b-8341-4d70-8f73-450d32ee0189
MS-RequestId: e39e0ddf-3fd0-4b7e-bb4e-8aebe242d3ee
MS-CV: s2GvkNgZsUSadxQX.0
MS-ServerId: 030011719
Date: Thu, 13 Apr 2017 18:13:40 GMT

{
    "mpnId": "4391507",
    "profileType": "MpnProfile",
    "links": {
        "self": {
            "uri": "/profiles/mpn",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "MpnProfile"
    }
}
```

**Failure**

```http
HTTP/1.1 404 Not Found
Content-Length: 124
Content-Type: application/json; charset=utf-8
MS-CorrelationId: e937630b-8341-4d70-8f73-450d32ee0189
MS-RequestId: 560df6b9-6e53-4954-aed7-133477ac1194
MS-CV: sLRFZMWm+EKuL47u.0
MS-ServerId: 102030524
Date: Thu, 13 Apr 2017 18:26:51 GMT

{
    "code": 3000,
    "description": "Partner Organization with partner_id 9999999 could not be found",
    "data": [],
    "source": "PartnerFD"
}
```

### Retrieve

**Request syntax**

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/profiles/mpn HTTP/1.1|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/profiles/mpn HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 76879323-92d1-437e-90dd-c84dbb9f7dec
MS-CorrelationId: cb9f3209-d020-4bf9-871c-e1f1c75348f8
Connection: Keep-Alive
```

**Response example**

If the request is successful, this method returns an **MpnProfile** object in the response body.

```json
HTTP/1.1 200 OK
Content-Length: 177
Content-Type: application/json; charset=utf-8
MS-CorrelationId: cb9f3209-d020-4bf9-871c-e1f1c75348f8
MS-RequestId: 76879323-92d1-437e-90dd-c84dbb9f7dec
Date: Mon, 21 Mar 2016 05:51:29 GMT

{
    "mpnId":"<mpnID>",
    "profileType":"MpnProfile",
    "links":{
        "self":{
            "uri":"/profiles/mpn",
            "method":"GET",
            "headers":[]
        }
    },
    "attributes":{
        "objectType":"MpnProfile"
    }
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).