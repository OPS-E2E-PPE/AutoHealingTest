---
title: Get invoices - Azure Cloud Solution Provider (CSP) | Microsoft Docs
description: This article covers how to retrieve invoices by using the Microsoft Partner Center APIs.
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

# Get invoices

An invoice includes the adjustments and charges that customers incurred during the partner's billing cycle. This information can be used to generate an invoice for a single customer. You can programmatically obtain invoice details using the following options

- .NET SDK
- Java SDK
- PowerShell
- REST API

## .NET SDK

### Get invoice by identifier

To get an invoice by it's identifier, use your **IAggregatePartner.Invoices** collection and call the **ById()** function, and then call the **Get()** or **GetAsync()** function.

```csharp
// IAggregatePartner partnerOperations;
// string selectedInvoiceId;
var invoice = partnerOperations.Invoices.ById(selectedInvoiceId).Get();
```

### Get a collection of all invoices

To get a collection of invoices, use your **IAggregatePartner.Invoices** collection to call the **Query** method and build your collection.

```csharp
// IAggregatePartner partnerOperations;

var pagedInvoiceCollection = partnerOperations.Invoices.Query(QueryFactory.Instance.BuildIndexedQuery(5, 0));
```

### Get invoice line items

To get the reconciliation file information for a specific invoice, use your **IAggregatePartner.Invoices** collection and call the **ById()** function, and then call the **Get()** or **GetAsync()** function on the resulting object.

```csharp
// IPartner partnerOperations;
// string invoiceId;

var invoice = partnerOperations.Invoices.ById(invoiceId).Get();

foreach (var invoiceDetail in invoice.InvoiceDetails)
{...}
```

## Java SDK

### Get invoice by identifier

To get an invoice by it's identifier, use your **IPartner.getInvoices** function and call the **byId()** function, and then call the **get()** function.

```java
// IPartner partnerOperations;
// String selectedInvoiceId;
Invoice invoice = partnerOperations.getInvoices().byId(selectedInvoiceId).get();
```

### Get a collection of all invoices

To get a collection of invoices, use your **IPartner.getInvoices** function to call the **query** method and build your collection.

```java
// IPartner partnerOperations;

ResourceCollection<Invoice> pagedInvoiceCollection = partnerOperations.getInvoices().query(QueryFactory.getInstance().buildIndexedQuery(5, 0));
```

### Get invoice line items

To get the reconciliation file information for a specific invoice, use your **IPartner.getInvoices** function and call the **byId()** function, and then call the **get()** function on the resulting object.

```java
// IPartner partnerOperations;
// var invoiceId as string;

Invoice invoice = partnerOperations.getInvoices().byId(invoiceId).get;

foreach (var invoiceDetail in invoice.getInvoiceDetails())
{...}
```

## PowerShell

### Get invoice by ID

To retrieve a specific invoice by ID in PowerShell, enter the following command:

```powershell
# $invoiceId

$invoice = Get-PartnerInvoice -InvoiceId $invoiceId
```

### Get a collection of all invoices

To retrieve a list of all your invoices in PowerShell, enter the following command:

```powershell
Get-PartnerInvoice
```

### Get invoice line items

To retrieve the line items of a customer invoice in PowerShell, run the following command:

```powershell
# $invoiceId

Get-PartnerInvoiceLineItem -InvoiceId $invoiceId -BillingProvider '<provider>' -LineItemType '<line item type>'
```

## REST API

### Get invoice by ID

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/invoices/{invoice-id} HTTP/1.1|

**URI parameter**

To get the invoice, use the following query parameter:

|Name|Type|Description|
|---|---|---|
|**invoice-id**|**string**|The value allows the reseller to filter the results for a specific invoice.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/invoices/<invoice-id> HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 8ac25aa5-9537-4b6d-b782-aa0c8e979e99
MS-CorrelationId: 57eb2ca7-755f-450f-9187-eae1e75a0114
```

If the request is successful, this method returns an **InvoiceSummary** object in the response.

**Response example**

```http
HTTP/1.1 200 OK
Content-Length: 676
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 57eb2ca7-755f-450f-9187-eae1e75a0114
MS-RequestId: 8ac25aa5-9537-4b6d-b782-aa0c8e979e99
Date: Thu, 24 Mar 2016 05:22:14 GMT

{
    "id":<invoice-id>,
    "invoiceDate":"2015-08-11T00:00:00",
    "totalCharges":3822.14,
    "paidAmount":0.0,
    "currencyCode":"USD",
    "pdfDownloadLink":<pdfDownloadLink>,
    "invoiceDetails":[{
        "invoiceLineItemType":"billing_line_items",
        "billingProvider":"office",
        "links":{
            "self":{
                "uri":"/invoices/<invoice-id>/lineitems/Office/BillingLineItems",
                "method":"GET",
                "headers":[]
            }
        },
        "attributes":{
            "objectType":"InvoiceDetail"
        }
    }],
    "links":{
        "self":{
            "uri":"/invoices/<invoice-id>",
            "method":"GET",
            "headers":[]
        }
    },
    "attributes":{
        "objectType":"Invoice"
    }
}
```

### Get a collection of all invoices

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|{baseURL}/v1/invoices HTTP/1.1|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/invoices HTTP/1.1
Authorization: Bearer
Accept: application/json
MS-RequestId: 8ba3af44-dfcd-4aaa-b04c-53363bee0287
MS-CorrelationId: 57eb2ca7-755f-450f-9187-eae1e75a0114
```

If the request is successful, this method returns a collection of **InvoiceDetail** objects in the response.

```http
HTTP/1.1 200 OK
Content-Length: 5310
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 57eb2ca7-755f-450f-9187-eae1e75a0114
MS-RequestId: 8ba3af44-dfcd-4aaa-b04c-53363bee0287

Date: Thu, 24 Mar 2016 05:23:53 GMT

{
    "totalCount":9,
    "items":[{
        "id":"D070002EGD",
        "invoiceDate":"2016-03-11T00:00:00",
        "totalCharges":17383.43,
        "paidAmount":0.0,
        "currencyCode":"USD",
        "invoiceDetails":[{
            "invoiceLineItemType":"billing_line_items",
            "billingProvider":"office",
            "links":{
                "self":{
                    "uri":"/invoices/D070002EGD/lineitems/Office/BillingLineItems",
                    "method":"GET",
                    "headers":[]
                }
            },
            "attributes":{
                "objectType":"InvoiceDetail"
            }
        },
        {
            "invoiceLineItemType":"billing_line_items",
            "billingProvider":"azure",
            "links":{
                "self":{
                    "uri":"/invoices/D070002EGD/lineitems/Azure/BillingLineItems",
                    "method":"GET",
                    "headers":[]
                }
            },
            "attributes":{
                "objectType":"InvoiceDetail"
            }
        },
        {
            "invoiceLineItemType":"usage_line_items",
            "billingProvider":"azure",
            "links":{
                "self":{
                    "uri":"/invoices/D070002EGD/lineitems/Azure/UsageLineItems",
                    "method":"GET",
                    "headers":[]
                }
            },
            "attributes":{
                "objectType":"InvoiceDetail"
            }
        }],
        "links":{
            "self":{
                "uri":"/invoices/D070002EGD",
                "method":"GET",
                "headers":[]
            }
        },
        "attributes":{
            "objectType":"Invoice"
        }
    }],
    "links":{
        "self":{
            "uri":"/invoices",
            "method":"GET",
            "headers":[]
        }
    },
    "attributes":{
        "objectType":"Collection"
    }
}
```

### Get invoice line items

**Request syntax**

Use the first syntax to return a full list of every line item for a specific invoice. For large invoices, use the second syntax with a specified size and 0-based offset to return a paged list of line items.

|Method|Request URI|
|---|---|
|**GET**|{baseURL}/v1/invoices/{invoice-id}/lineitems/{billing-provider}/{invoice-line-item-type} HTTP/1.1|
|**GET**|{baseURL}/v1/invoices/{invoice-id}/lineitems/{billing-provider}/{invoice-line-item-type}?size={size}&offset=0 HTTP/1.1|

**URI parameter**

To get the invoice line items, use the following *required* parameters:

|Name|Type|Description|
|---|---|---|
|**invoice-id**|**string**|The value allows the reseller to filter the results for a specific invoice.|
|**billing-provider**|**string**|A billing-provider that allows the filtering of results for a specific service within the invoice.|
|**invoice-line-item-type**| |A value that allows the filtering of results by a specific type of line item.|
|**size**|**int**|The number of results to be displayed at one time.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/invoices/<invoice-id>/lineitems/<billing-provider>/<invoice-line-item-type>?size=<size>&offset=0 HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 646e3cfe-16a8-4b8f-8171-11a48a60ef28
MS-CorrelationId: 57eb2ca7-755f-450f-9187-eae1e75a0114
```

If the request is successful, this method returns an **InvoiceSummary** object in the response.

**Response example**

```http
HTTP/1.1 200 OK
Content-Length: 40019
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 57eb2ca7-755f-450f-9187-eae1e75a0114
MS-RequestId: 646e3cfe-16a8-4b8f-8171-11a48a60ef28
Date: Thu, 24 Mar 2016 05:26:38 GMT

{
    "totalCount":45,
    "items":[{
        "partnerId":<partnerId>,
        "customerId":<customerId>,
        "customerName":"COMPANYNAME",
        "mpnId":<mpnId>,
        "tier2MpnId":-1,
        "orderId":<orderId>,
        "subscriptionId":<subscriptionId>,
        "syndicationPartnerSubscriptionNumber":<subscriptionNumber>,
        "offerId":<offerId>,
        "durableOfferId":<durabeOfferId>,
        "offerName":"OFFICE 365 ENTERPRISE E3",
        "subscriptionStartDate":"2016-08-10T00:00:00",
        "subscriptionEndDate":"2016-08-10T00:00:00",
        "chargeStartDate":"2015-07-13T00:00:00",
        "chargeEndDate":"2015-08-09T00:00:00",
        "chargeType":"PURCHASE FEE",
        "unitPrice":0.0,
        "quantity":10,
        "amount":0.0,
        "totalOtherDiscount":0.0,
        "subtotal":0.0,"tax":0.0,
        "totalForCustomer":0.0,
        "currency":"USD",
        "invoiceLineItemType":"billing_line_items",
        "billingProvider":"office",
        "attributes":{
            "objectType":"LicenseBasedLineItem"
        }
    }],
    "links":{
        "self":{
            "uri":"/invoices/<invoice-id>/lineitems/<billing-provider>/<invoice-line-item-type>?size=<size>&offset=0",
            "method":"GET",
            "headers":[]
        }
    },
    "attributes":{
        "objectType":"Collection"
    }
}
```

## Next steps

- [Review](../available-apis-overview.md) what are the available APIs for Azure CSP integration.
- [Review](../integration-scenarios-list.md) the list of available Azure CSP Integration scenarios.