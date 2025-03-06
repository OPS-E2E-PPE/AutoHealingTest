---
title: Get company profile info - Azure Cloud Solution Provider | Microsoft Docs
description: This article covers how to retrieve and update billing or business profile information for a customer on your Azure CSP account.
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: kirilk
---

# Get company profile information

This guide covers how to retrieve and update billing or business profile information for a customer on your Azure Cloud Solution Provider (Azure CSP) account.

## Web UI

1. On the Partner Center **Dashboard** page, select **Customers** > *customer name* > **Account**. 

    * The customer's company information is displayed under **Company Info**.  
    * The customer's billing information is displayed under **Bill-to info**.

2. To update the billing information, select **Update**.

    ![The customer billing Update link](media/update-billing-info-1.PNG)

3. After you've made your changes, select **Submit**.

    ![image2](media/update-billing-info-2.PNG)

## PowerShell

### Get a company profile

To retrieve a customer's company profile, enter the following commands:

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>'

Get-PCCustomerCompanyProfile -TenantId $customer.id
```

### Get a billing profile

To retrieve a customer's billing profile, enter the following commands:

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>>'

Get-PCCustomerBillingProfile -TenantId $customer.id
```

### Update a billing profile

To update a customer's billing profile, enter the following commands:

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>'

$customerBillingProfile = Get-PCCustomerBillingProfile -TenantId $customer.id

$customerBillingProfile.firstName = '<first name>'
$customerBillingProfile.lastName = '<last name>'
$customerBillingProfile.email = '<email>'

Set-PCCustomerBillingProfile -TenantId $customer.id -BillingProfile $customerBillingProfile
```

## C#

### View a company profile

1. To get the company profile for a customer, call the **IAggregatePartner.Customers.ById** method with the customer ID to identify the customer.

2. To access the customer's Company property, get the customer's **ICustomerProfileCollection** interface from the **Profiles** property.

3. Get the **ICustomerReadonlyProfile** interface from the **ICustomerProfileCollection.Company** property, and then call its **Get()** or **GetAsync()** method.

```csharp
// IAggregatePartner partnerOperations;
// var selectedCustomerID as string;

var companyProfile = partnerOperations.Customers.ById(selectedCustomerId).Profiles.Company.Get();
```

### View a billing profile

1. To get a customer's billing profile, use your **IPartner.Customers** collection, and then call the **ById()** method. 
2. Call the **Profiles** property, followed by the **Billing** property. 
3. Call the **Get()** or **GetAsync()** method.

```csharp
// IAggregatePartner partnerOperations;
// var selectedCustomerId as string;

var billingProfile = partnerOperations.Customers.ById(selectedCustomerId).Profiles.Billing.Get();
```

### Update a billing profile

To update a customer's billing profile, do the following:

1. Retrieve the billing profile, and then update the properties as necessary.
2. Retrieve your **IPartner.Customers** collection, and then call the **ById()** method.
3. Call the **Profiles** property, followed by the **Billing** property.
4. Call the **Update()** or **UpdateAsync()** method.

```csharp
// IAggregatePartner partnerOperations;
// var selectedCustomerId as string;

var billingProfile = partnerOperations.Customers.ById(selectedCustomerId).Profiles.Billing.Get();

// Apply changes to profile;

billingProfile = partnerOperations.Customers.ById(selectedCustomerId).Profiles.Billing.Update(billingProfile);
```

## REST API

### View a company profile

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/customers/{customer-tenant-id}/profiles/company HTTP/1.1|

**URI parameter**

To get the company profile, use the following query parameter:

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|A GUID-formatted string that allows the reseller to filter the results for a specific customer that belongs to the reseller.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/profiles/company HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 0b6f039c-e4b5-4b9e-bdac-b39077bb60da
MS-CorrelationId: ffa9174c-dbcb-47de-b70d-10e640a7f1b4
X-Locale: en-US
Host: api.partnercenter.microsoft.com
Connection: Keep-Alive
```

If the request is successful, this method returns information in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 488
Content-Type: application/json; charset=utf-8
MS-CorrelationId: ffa9174c-dbcb-47de-b70d-10e640a7f1b4
MS-RequestId: 0b6f039c-e4b5-4b9e-bdac-b39077bb60da
MS-CV: /e74N8OrkE29ycwZ.0
MS-ServerId: 101112202
Date: Wed, 04 Jan 2017 19:48:51 GMT

{
    "tenantId": "4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04",
    "domain": "dtdemocspcustomer005.onmicrosoft.com",
    "companyName": "DT Demo CSP Customer 005",
    "address": {
        "country": "US",
        "region": "WA",
        "city": "Redmond ",
        "addressLine1": "1 Microsoft Way",
        "postalCode": "98052",
        "phoneNumber": "4155551212"
    },
    "email": "daniel@hotmail.com.tw",
    "links": {
        "self": {
            "uri": "/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/profiles/company",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "CustomerCompanyProfile"
    }
}
```

### View a billing profile

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURL}*/v1/customers/{customer-tenant-id}/profiles/billing HTTP/1.1| 

**URI parameter**

This query parameter is *required* for getting the billing profile.

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|A GUID-formatted string that indicates a customer.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/profiles/billing HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: a5581a74-2778-4e34-9172-18baa4877081
MS-CorrelationId: 51d521b3-62db-4682-b75d-fb8ab09113b2
```

If the request is successful, this method returns a collection of **Profile** resources in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 1206
Content-Type: application/json
MS-CorrelationId: 51d521b3-62db-4682-b75d-fb8ab09113b2
MS-RequestId: a5581a74-2778-4e34-9172-18baa4877081
Date: Mon, 23 Nov 2015 18:13:43 GMT

{
    "id": "<billing-profile-id>",
    "firstName": "FirstName",
    "lastName": "LastName",
    "email": "email@sample.com",
    "culture": "en-US",
    "language": "en",
    "companyName": "CompanyName",
    "defaultAddress": {
        "country": "US",
        "city": "Redmond",
        "state": "WA",
        "addressLine1": "1 Microsoft Way",
        "postalCode": "98052",
        "firstName": "FirstName",
        "lastName": "LastName",
        "phoneNumber": "4255555555"
    },
    "links": {
        "self": {
            "uri": "/v1/customers/<customer-tenant-id>/profiles/billing",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "etag": "<etag>",
        "objectType": "CustomerBillingProfile"
    }
}
```

### Update a billing profile

**Request syntax**

|Method|Request URI|
|---|---|
|**PUT**|*{baseURL}*/v1/customers/{customer-tenant-id}/profiles/billing HTTP/1.1|

**URI parameter**

To update the billing profile, use the following query parameter:

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|The value is a GUID-formatted string that allows the reseller to filter the results for a specific customer that belongs to the reseller.|

**If-Match**: An ETag is required for concurrency detection.

**Request example**

```json
PUT https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/profiles/billing HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: ff85f13a-eb65-4b8e-9b67-05beb0565410
MS-CorrelationId: ff1b757d-cfaf-463a-b48b-0f96d05e95d7
Content-Type: application/json
Content-Length: 639
Expect: 100-continue

{
    "Id": "a58ceba5-60ac-48e4-a0bc-2a855811807a",
    "FirstName": "FirstName",
    "LastName": "LastName",
    "Email": "email@sample.com",
    "Culture": "en-US",
    "Language": "en",
    "CompanyName": "CompanyName",
    "DefaultAddress": {
        "Country": "US",
        "Region": null,
        "City": "Redmond",
        "State": "WA",
        "AddressLine1": "One Microsoft Way",
        "AddressLine2": null,
        "PostalCode": "98052",
        "FirstName": "FirstName",
        "LastName": "LastName",
        "PhoneNumber": "4255555555"
    },
    "Links": {
        "Self": {
            "Uri": "/v1/customers/<customer-tenant-id>/profiles/billing",
            "Method": "GET",
            "Headers": []
        }
    },
    "Attributes": {
        "Etag": "<etag>",
        "ObjectType": "CustomerBillingProfile"
    }
}
```


If the request is successful, this method returns updated **Profile** resource properties in the response body. This call requires an ETag for concurrency detection.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 1210
Content-Type: application/json
MS-CorrelationId: ff1b757d-cfaf-463a-b48b-0f96d05e95d7
MS-RequestId: ff85f13a-eb65-4b8e-9b67-05beb0565410
Date: Mon, 23 Nov 2015 18:20:43 GMT

{
    "id": "a58ceba5-60ac-48e4-a0bc-2a855811807a",
    "firstName": "FirstName",
    "lastName": "LastName",
    "email": "email@sample.com",
    "culture": "en-US",
    "language": "en",
    "companyName": "companyName",
    "defaultAddress": {
        "country": "US",
        "city": "Redmond",
        "state": "WA",
        "addressLine1": "One Microsoft Way",
        "postalCode": "98052",
        "firstName": "FirstName",
        "lastName": "LastName",
        "phoneNumber": "4255555555"
    },
    "links": {
        "self": {
            "uri": "/v1/customers/<customer-tenant-id>/profiles/billing",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "etag": "<etag>",
        "objectType": "CustomerBillingProfile"
    }
}
```

## Next steps

* Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
* View a list of [Azure CSP integration scenarios](../integration-scenarios-list.md).