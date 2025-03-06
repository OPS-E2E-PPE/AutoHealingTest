---
title: Get an offer by ID - Azure Cloud Solution Provider | Microsoft Docs
description: This article covers how to retrieve an Offer resource that matches the provided offer ID.
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

# Get an offer by ID

This guide covers how to retrieve an **Offer** resource that matches the provided offer ID.

## PowerShell

To retrieve a specific offer from Microsoft Partner Center by using PowerShell, enter the following command:

```powershell
Get-PCOffer -countryid '<country two digits id>' -offerid '<offer id GUID>'
```

## C#

To find a specific offer by ID, do the following:
1. Use your **IAggregatePartner.Offers** collection, establish the country with a call to **ByCountry()**, and then call the **ByID()** method. 
2. Call the **Get()** or **Get Async()** method.

```csharp
// IAggregatePartner partnerOperations;

//retrieve the offer ID
var offers = state[FeatureSamplesApplication.OffersKey] as List<Offer>;

var offerDetails = partnerOperations.Offers.ByCountry("US").ById(offers[0].Id).Get();
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/offers/{offer-id}?country={country-id} HTTP/1.1|

**URI parameter**

For the URI request, use the following *required* parameters:

|Name|Type|Description|
|---|---|---|
|**offer-id**|**guid**|A GUID that corresponds to the offer.|
|**country-id**|**string**|The country/region ID.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/offers/<offer-id>?country=<country-id> HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: ac943950-ba3d-47a0-bd2a-c5617a7fefe8
MS-CorrelationId: 7c1f6619-c176-4040-a88f-2c71f3ba4533
X-Locale: <locale-id>
Connection: Keep-Alive
```

### Response

If the request is successful, this method returns an **Offer** resource in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 1918
Content-Type: application/json
MS-CorrelationId: 7c1f6619-c176-4040-a88f-2c71f3ba4533
MS-RequestId: ac943950-ba3d-47a0-bd2a-c5617a7fefe8
Date: Mon, 23 Nov 2015 23:13:01 GMT

{
    "id": "031C9E47-4802-4248-838E-778FB1D2CC05",
    "name": "Office 365 Business Premium",
    "description": "For businesses with 1 to 300 users that need the latest desktop version of Office,
                    plus anywhere access to email, filesharing, and online conferencing.",
    "minimumQuantity": 1,
    "maximumQuantity": 300,
    "rank": 56,
    "uri": "/3c95518e-8c37-41e3-9627-0ca339200f53/Offers/031C9E47-4802-4248-838E-778FB1D2CC05",
    "locale": "en-us",
    "country": "US",
    "category": {
        "id": "SmallBusiness_Key",
        "name": "Small Business",
        "rank": 30,
        "locale": "en-us",
        "country": "US",
        "attributes": {
            "objectType": "OfferCategory"
        }
    },
    "prerequisiteOffers": [],
    "isAddOn": false,
    "isAvailableForPurchase": true,
    "billing": "license",
    "isAutoRenewable": true,
    "product": {
        "id": "f245ecc8-75af-4f8e-b61f-27d8114de5f3",
        "name": "Office 365 Business Premium",
        "unit": "Licenses"
    },
    "unitType": "Licenses",
    "links": {
        "learnMore": {
            "uri": "http: //g.microsoftonline.com/0BXPS00en/909",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "Offer"
    }
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- View a list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
