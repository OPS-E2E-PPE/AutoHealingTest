---
title: Get customer utilization records - Azure Cloud Solution Provider (CSP) | Microsoft Docs
description: This article covers how to retrieve the utilization records of a customer's Azure subscription for a specified period.
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: kirilk
---

# Get a customer's utilization records for Azure

The Azure Utilization record contains details about the utilization of an Azure subscription resource. If you are a cloud service provider partner who owns the billing relationship for your customers' Azure subscriptions, you can use this resource to provide a scalable way to track usage incurred on the subscriptions in order to send an invoice to your customers at the end of every billing cycle. This guide will show you how to get Azure Utilization records using the following options

- .NET SDK
- Java SDK
- PowerShell
- REST API

## .NET SDK

To obtain the Azure utilization records, you first need a customer identifier and a subscription identifier. You then call the **IAzureUtilizationCollection.Query** function to return a **ResourceCollection** that contains the utilization records. Because the resource collection is paged, you must then obtain an Azure utilization record enumerator to traverse the utilization pages.

```csharp
// IPartner partner;

// Retrieve the utilization records for the last year in pages of 100 records.
var utilizationRecords = partner.Customers[customerId].Subscriptions[subscriptionId].Utilization.Azure.Query(
    DateTimeOffset.Now.AddYears(-1),
    DateTimeOffset.Now,
    size: 100);

// Create an Azure utilization enumerator, which helps you traverse the utilization pages.
var utilizationRecordEnumerator = partner.Enumerators.Utilization.Azure.Create(utilizationRecords);

while (utilizationRecordEnumerator.HasValue)
{
    //
    // Insert code here to work with this page.
    //

    // Get the next page.
    utilizationRecordEnumerator.Next();
}
```

## Java SDK

To obtain the Azure utilization records, you first need a customer identifier and a subscription identifier. You then call the **IAzureUtilizationCollection.query** function to return a **ResourceCollection** that contains the utilization records. Because the resource collection is paged, you must then obtain an Azure utilization record enumerator to traverse the utilization pages.

```java
// IPartner partnerOperations;

ResourceCollection<AzureUtilizationRecord> utilizationRecords = partnerOperations.getCustomers()
    .byId(customerId).getSubscriptions().byId(subscriptionId)
    .getUtilization().getAzure().query(
        new DateTime().minusYears(1),
        new DateTime(),
        AzureUtilizationGranularity.Daily,
        true,
        100);

// Create an Azure utilization enumerator which will aid us in traversing the utilization pages
IResourceCollectionEnumerator<ResourceCollection<AzureUtilizationRecord>> utilizationRecordEnumerator =
    partnerOperations.getEnumerators().getUtilization().getAzure().create(utilizationRecords);

while (utilizationRecordEnumerator.hasValue())
{
    //
    // Insert code here to work with this page.
    //

    // get the next page
    utilizationRecordEnumerator.next();
}
```

## PowerShell

To retrieve the utilization records of a customer's Azure subscription for a specified period, run the following PowerShell command:

```powershell
# $customerId;
# $subscriptionId;

Get-PartnerCustomerSubscriptionUtilization -CustomerId $customerId -SubscriptionId $subscriptionId -StartDate (Get-Date).AddYears(-1).ToUniversalTime() -Granularity Hourly -ShowDetails
```

## REST API

### Request

**Request syntax**

| Method  |                                                                                        Request URI                                                                                        |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **GET** | *{baseURL}*/v1/customers/{customer-tenant-id}/subscriptions/{subscription-id}/utilizations/azure?start_time={start-time}&end_time={end-time}&granularity={granularity}&show_details={True} |

**URI parameters**

Use the following parameters to get the utilization records.

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|The customer ID.|
|**subscription-id**|**guid**|The subscription ID.|
|**start_time**|**DateTimeOffset**|The start of the time range when the utilization was metered in the billing system.|
|**end_time**|**DateTimeOffset**|The end of the time range when the utilization was metered in the billing system.|
|**granularity**|**string**|*Optional*. Defines the granularity of usage aggregations. Options are *daily* and *hourly*. Defaults to *daily*.|
|**show_details**|**bool**|*Optional*. Specifies whether to get the instance-level usage details. Defaults to *true*.|
|**size**|**int**|*Optional*. Specifies the number of aggregations returned by a single API call. Defaults to *1000*, the maximum.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/65726577-c208-40fd-9735-8c85ac9cac68/subscriptions/87F4B92F-A490-485E-AD34-5B70CBA4AF74/utilizations/azure?start_time=2015-12-02T10:54:34Z&end_time=2016-12-02T10:54:34Z&granularity=Daily&show_details=True&size=100 HTTP/1.1
Authorization: Bearer <authorization token>
Accept: application/json
MS-RequestId: e6a3b6b2-230a-4813-999d-57f883b60d38
MS-CorrelationId: a687bc47-8d08-4b78-aff6-5a59aa2055c2
X-Locale: en-US
Host: api.partnercenter.microsoft.com
```

### Response

If the request is successful, this method returns a collection of Azure utilization record resources in the response body.

**Response example**

```http
HTTP / 1.1 200 OK
Content - Length: 51145
Content - Type: application / json;
charset = utf - 8
MS - CorrelationId: a687bc47 - 8d08 - 4b78 - aff6 - 5a59aa2055c2
MS - RequestId: e6a3b6b2 - 230a - 4813 - 999d - 57f883b60d38
MS - CV: gPPoyNX1X0asAAcw.0
MS - ServerId: 101112202
Date: Fri, 02 Dec 2016 18: 54: 38 GMT

{
    "totalCount": 2,
    "items": [
        {
            "usageStartTime": "2015-11-30T16:00:00-08:00",
            "usageEndTime": "2015-12-01T16:00:00-08:00",
            "resource": {
                "id": "505db374-df8a-44df-9d8c-13c14b61dee1",
                "name": "Standard Small App Service Hours",
                "category": "Azure App Service",
                "subcategory": ""
            },
            "quantity": 2.0,
            "unit": "Hours",
            "infoFields": {
                "meteredRegion": "North Central US",
                "meteredService": "Azure App Service",
                "meteredServiceType": "abtintest",
                "project": "JBlack2-NorthCentralUSwebspace"
            },
            "attributes": {
                "objectType": "AzureUtilizationRecord"
            }
        },
        {
            "usageStartTime": "2015-11-30T16:00:00-08:00",
            "usageEndTime": "2015-12-01T16:00:00-08:00",
            "resource": {
                "id": "c0f5cb45-6fb1-41c9-8545-72ad400d9da4",
                "name": "Free App Service",
                "category": "Azure App Service",
                "subcategory": ""
            },
            "quantity": 0.002688,
            "unit": "Apps",
            "infoFields": {
                "meteredRegion": "North Central US",
                "meteredService": "Azure App Service",
                "meteredServiceType": "abtintest",
                "project": "abtintest"
            },
            "attributes": {
                "objectType": "AzureUtilizationRecord"
            }
        }
    ],
	"links": {
		"self": {
			"uri": "customers/65726577-c208-40fd-9735-8c85ac9cac68/subscriptions/87F4B92F-A490-485E-AD34-5B70CBA4AF74/utilizations/azure?start_time=2015-12-02T00:00:00Z&end_time=2016-12-02T00:00:00Z&granularity=Daily&show_details=True&size=100",
			"method": "GET",
			"headers": []
		},
		"next": {
			"uri": "customers/65726577-c208-40fd-9735-8c85ac9cac68/subscriptions/87F4B92F-A490-485E-AD34-5B70CBA4AF74/utilizations/azure?seek_operation=Next",
			"method": "GET",
			"headers": [{
					"key": "MS-ContinuationToken",
					"value": "/1f5ccb06-8e36-4a74-a74c-fcaa9e87e26a/usage-records?entitlement_id=87f4b92f-a490-485e-ad34-5b70cba4af74&reported_start_time=2015-12-02+00%3a00%3a00Z&reported_end_time=2016-12-02+00%3a00%3a00Z&count=100&show_details=True&error_if_not_processed=True&granularity=Daily&continuation_token=eyJyIjoiMjAxNTEyMTYtNjM1ODU4MjA4MDAwMDAwMDAwIiwiaSI6IldNR2hrYWhKU1VBeS9CZkFnSDNKSlhCZERZVEVmY3Z5NWQvOHFmSEthWHdiNi9Wd2VvZWVscVltL0Rtbys3QUU0NGVUZmFvc2ZMTG5GM0hScW92WkhnPT0iLCJzIjoiaFVrUGVacz0ifQ%3d%3d"
				}
			]
		}
	},
	"attributes": {
		"objectType": "Collection"
	}
}
```

## Next steps

- [Review](../available-apis-overview.md) what are the available APIs for Azure CSP integration.
- [Review](../integration-scenarios-list.md) the list of available Azure CSP Integration scenarios.
