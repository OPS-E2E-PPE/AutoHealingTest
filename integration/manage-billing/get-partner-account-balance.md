---
title: Get a partner's current account balance in Azure Cloud Solution Provider (CSP) | Microsoft Docs
description: This article covers how to retrieve the current account balance of a partner.
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2087
ms.author: kirilk
---

# Get a partner's current account balance

This guide covers how to retrieve the current account balance of a partner.

## C# guide

To retrieve your reseller account balance, do the following:

1. Use your **IAggregatePartner.Invoices** collection.
2. Call the **Summary** property. 
3. Call the **Get** function.
4. Call the **BalanceAmount** property.

```csharp
// IAggregatePartner scopedPartnerOperations;

var invoiceSummary = scopedPartnerOperations.Invoices.Summary.Get();

Console.Out.WriteLine("Current Account Balance:  {0:C}", invoiceSummary.BalanceAmount);
```

## REST API guide

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/invoices/summary HTTP/1.1|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/invoices/summary HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: a45e6643-1caf-4429-8f90-07c03d85bc2b
MS-CorrelationId: 57eb2ca7-755f-450f-9187-eae1e75a0114
Connection: Keep-Alive
```

### Response

If the request is successful, this method returns an **InvoiceSummary** object in the response.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 256
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 57eb2ca7-755f-450f-9187-eae1e75a0114
MS-RequestId: a45e6643-1caf-4429-8f90-07c03d85bc2b
Date: Thu, 24 Mar 2016 05:21:01 GMT

{
    "balanceAmount":68059.03,
    "currencyCode":"USD",
    "accountingDate":"2016-03-11T00:00:00",
    "firstInvoiceCreationDate":"2015-07-11T00:00:00",
    "links":{
        "self":{
            "uri":"/invoices/summary",
            "method":"GET",
            "headers":[]
        }
    },
    "attributes":{
        "objectType":"InvoiceSummary"
    }
}
```

## Next steps

- [Review](../available-apis-overview.md) what are the available APIs for Azure CSP integration.
- [Review](../integration-scenarios-list.md) the list of available Azure CSP Integration scenarios.