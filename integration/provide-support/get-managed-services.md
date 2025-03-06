---
title: Get managed services for a customer by ID - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to get managed services for a customer by ID. The information in this article supports Azure Cloud Solution Provider (Azure CSP) integration.
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

# Get managed services for a customer by ID

Learn how to retrieve a list of all of a customer's services for which you have delegated admin permissions. For information about providing support for customers and escalating service requests to Microsoft, see [Azure Cloud Solution Provider support](../../support/azure-csp-support-overview.md). To get a list of managed services by using a customer ID, you can use one of the following options:

- PowerShell
- C#
- REST API

## PowerShell

```powershell
$customer = Get-PCCustomer -tenantid '<tenant id GUID>'
Get-PCManagedServices -tenantid $customer.id 
```

## C#

1. Use your **IAggregatePartner.Customers** collection and call the **ById()** method.
2. Call the **ManagedServices** property, followed by the **Get()** or the **GetAsync()** method.

```csharp
// IAggregatePartner partnerOperations;
// var selectedCustomerID as Customer;

ResourceCollection<ManagedService> managedServices = partnerOperations.Customers.ById(selectedCustomerId).ManagedServices.Get();
```


## REST API

### Request

|Method|Request URI|
|---|---|
|GET|{baseURL}/v1/customers/{customer-tenant-id}/managedservices HTTP/1.1|

**URI parameter**

The following query parameter is required for getting the customer's managed services.

|Name|Type|Description|
|---|---|---|
|customer-tenant-id|guid|A GUID that corresponds to the customer.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/managedservices HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 4ff57220-f17b-4d8f-8e09-78334c57ba00
MS-CorrelationId: 03d6064a-f048-4aee-8892-ed46dc5c8bee
```

### Response

If the request is successful, this method returns a collection of **Managed Service** objects in the response body.

```json
HTTP/1.1 200 OK
Content-Length: 10588
Content-Type: application/json
MS-CorrelationId: 03d6064a-f048-4aee-8892-ed46dc5c8bee
MS-RequestId: 4ff57220-f17b-4d8f-8e09-78334c57ba00
Date: Mon, 23 Nov 2015 18:02:12 GMT

{
    "totalCount": 2,
    "items": [{
        "id": "Exchange",
        "name": "Exchange",
        "groupName": "Office",
        "links": {
            "adminService": {
                "uri": "https://portal.office.com/Partner/BeginClientSession.aspx?CTID=<ctid>&CSDEST=Exchange&InitialDomain=<domain>&PrimaryDomain=<domain>",
                "method": "GET",
                "headers": []
            },
            "serviceHealth": {
                "uri": "https://portal.office.com/Partner/BeginClientSession.aspx?CTID=<ctid>&CSDEST=ServiceStatus",
                "method": "GET",
                "headers": []
            },
            "serviceTicket": {
                "uri": "https://portal.office.com/Partner/BeginClientSession.aspx?CTID=<ctid>&CSDEST=Support",
                "method": "GET",
                "headers": []
            }
        },
        "attributes": {
            "objectType": "ManagedService"
        }
    },
    {
        "id": "MicrosoftCommunicationsOnline",
        "name": "SkypeforBusiness",
        "groupName": "Office",
        "links": {
            "adminService": {
                "uri": "https://portal.office.com/Partner/BeginClientSession.aspx?CTID=<ctid>&CSDEST=MicrosoftCommunicationsOnline",
                "method": "GET",
                "headers": []
            },
            "serviceHealth": {
                "uri": "https://portal.office.com/Partner/BeginClientSession.aspx?CTID=<ctid>&CSDEST=ServiceStatus",
                "method": "GET",
                "headers": []
            },
            "serviceTicket": {
                "uri": "https://portal.office.com/Partner/BeginClientSession.aspx?CTID=<ctid>&CSDEST=Support",
                "method": "GET",
                "headers": []
            }
        },
        "attributes": {
            "objectType": "ManagedService"
        }
    }
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
