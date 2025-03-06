---
title: Get pricing for Microsoft Azure - Azure Cloud Solution Provider (CSP) | Microsoft Docs
description: This article covers how to get an Azure rate card with real-time pricing for an Azure offer.
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2018
ms.author: kirilk
---

# Get pricing for Microsoft Azure

Azure pricing is dynamic, and it changes frequently. Prices also differ by market and currency. The Azure rate card takes these facts into consideration and predicts the monthly bill for the partner and customers. This guide will show you how to get Azure rate card details programmatically using the following options

- .NET SDK
- Java SDK
- PowerShell
- REST API

## .NET SDK

To obtain the Azure rate card, call the **IAzureRateCard.Get** method to return an **AzureRateCard** resource that contains the Azure prices.

```csharp
// IAggregatePartner partnerOperations;

var azureRateCard = partnerOperations.RateCards.Azure.Get();
```

## Java SDK

To obtain the Azure rate card, call the **IPartner.getRateCards().getAzure** function to return an **AzureRateCard** resource that contains the Azure prices.

```java
// IPartner partnerOperations;

AzureRateCard azureRateCard = partnerOperations.getRateCards().getAzure().get("" , "");
```

## PowerShell

To retrieve an Azure rate card by using PowerShell, run the following commands. The command to run differs depending on whether you want a rate card for the default context or a specific currency and region.

```powershell
Get-PartnerAzureRateCard

# Gets the Azure rate card details in the specified currency and region.
Get-PartnerAzureRateCard -Currency 'USD' -Region 'US'

# Gets the Azure rate card details for Azure Partner Shared Services
Get-PartnerAzureRateCard -SharedServices
```

Review the [get-PartnerAzureRateCard](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/docs/help/Get-PartnerAzureRateCard.md) help for more information.

## REST API

> [!IMPORTANT]
> [Azure Resource RateCard API](https://docs.microsoft.com/azure/billing/billing-usage-rate-card-overview) is not available for Azure CSP subscriptions. Azure Rate Card should be accessed through [Partner Center API](https://msdn.microsoft.com/library/partnercenter/mt774619.aspx) instead.

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/ratecards/azure&currency={currency}&region={region}|

**URI parameters**

You can use these parameters to retrieve a specific currency or region. The parameters are *optional*. If you leave them blank, they default to the currency or country that's set in the partner profile.

|Name|Type|Description|
|---|---|---|
|**currency**|**string**|The three-letter ISO code for the currency in which the resource rates are provided.|
|**region**|**string**|The two-letter ISO country/region code that indicates the market where the offer is purchased.|

If you include the optional **X-Locale** header in the request, it determines the language that's used for the response details.

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/ratecards/azure HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 07ced227-3f32-4eeb-8062-f0bef849a9bc
MS-CorrelationId: a687bc47-8d08-4b78-aff6-5a59aa2055c2
X-Locale: en-US
Host: api.partnercenter.microsoft.com
Connection: Keep-Alive
```

### Response

If the request is successful, it returns an Azure rate card resource.

```http
HTTP/1.1 200 OK
Content-Length: 1545508
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 57b25659-fc00-4215-87e7-2b09bac6845d
MS-RequestId: 870118d0-adbb-41a3-82d2-a3d45ade3c73
MS-CV: CYBB8PXMsEukJBIn.0
MS-ServerId: 201021413
Date: Wed, 01 Feb 2017 00:13:45 GMT

 {
    "locale": "en-US",
    "currency": "USD",
    "isTaxIncluded": false,
    "meters": [{
            "id": "4b836326-7e19-46e6-8bce-1b19bb6cd91e",
            "name": "Unlimited Data - 1 Gbps",
            "rates": {
                "0": 7395.0
            },
            "tags": [],
            "category": "Networking",
            "subcategory": "ExpressRoute",
            "region": "Zone 2",
            "unit": "Connections",
            "includedQuantity": 0.0,
            "effectiveDate": "2015-09-01T00:00:00Z"
        }, {
            "id": "1e8f6d9f-8b40-4c97-80cc-cff87a290a93",
            "name": "Compute Hours",
            "rates": {
                "0": 3.9729
            },
            "tags": [],
            "category": "Cloud Services",
            "subcategory": "Standard_L16 Cloud Services",
            "region": "AU East",
            "unit": "1 Hour",
            "includedQuantity": 0.0,
            "effectiveDate": "2016-09-01T00:00:00Z"
        }, {
            "id": "7a2639ce-ae47-4413-9837-6b4f4b78be3d",
            "name": "Compute Hours",
            "rates": {
                "0": 0.1122
            },
            "tags": [],
            "category": "Virtual Machines",
            "subcategory": "Standard_D1_v2 VM (Windows)",
            "region": "BR South",
            "unit": "Hours",
            "includedQuantity": 0.0,
            "effectiveDate": "2017-01-01T00:00:00Z"
        }
    ],
    "offerTerms": [{
            "name": "Overage discount",
            "discount": 0.15,
            "excludedMeterIds": ["53cc0061-0fe2-4249-bf62-e1008c811f5c", "c82dbd27-c978-43a7-ad41-525a90d8962b"],
            "effectiveDate": "2014-01-01T00:00:00"
        }
    ],
    "attributes": {
        "objectType": "AzureRateCard"
    }
}
```

## Next steps

- [Review](../available-apis-overview.md) what are the available APIs for Azure CSP integration.
- [Review](../integration-scenarios-list.md) the list of available Azure CSP Integration scenarios.