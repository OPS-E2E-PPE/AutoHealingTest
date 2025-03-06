---
title: Update partner profiles - Azure Cloud Solution Provider (CSP) | Microsoft Docs
description: This article covers how to change your partner business profile, billing profile, and support profile by using the web UI, C#, PowerShell, or the REST API.
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

# Update partner profiles in your CSP account

A profile with respect to the Cloud Solution Provider program is a set of information that provides additional information as contact details or a website address. Each reseller has the following profiles associated with their account

- Billing profile
- Legal business profile
- Organizational profile
- Support profile

In addition to the Partner Dashboard these profiles can be updated using the following options

- .NET SDK
- Java SDK
- PowerShell
- REST API

## Billing profile

The partner billing profile determines where Microsoft sends CSP invoices. It is important to keep the information up to date so that you receive all billing invoices.

This guide covers how to update your billing profile when necessary.

### .NET SDK

To update a partner billing profile, retrieve the existing profile. Once you have updated the profile, use your **IAggregatePartner.Profiles** collection and call the **BillingProfile** property. Finally, call the **Update()** function.

```csharp
// IAggregatePartner partnerOperations;

BillingProfile existingBillingProfile = partnerOperations.Profiles.BillingProfile.Get();

// update the profile with a purchase order number
existingBillingProfile.PurchaseOrderNumber = new Random().Next(9000, 10000).ToString(CultureInfo.InvariantCulture);

BillingProfile updatedPartnerBillingProfile = partnerOperations.Profiles.BillingProfile.Update(existingBillingProfile);
```

### Java SDK

To update a partner billing profile, retrieve the existing profile. Make the necessary modifications to the partner billing profile. Finally, call the **update()** function.

```java
// IPartner partnerOperations;

BillingProfile billingProfile = partnerOperations.getProfiles().getBillingProfile().get();

Random random = new Random();
billingProfile.setPurchaseOrderNumber( String.valueOf( random.nextInt( 1000 ) + 9000 ) );

BillingProfile updateBillingProfile = partnerOperations.getProfiles().getBillingProfile().update( billingProfile );
```

### PowerShell

Run the following command to update the billing profile

```powershell
Set-PartnerBillingProfile -AddressLine1 '700 Bellevue Way NE' -City 'Bellevue' -EmailAddress 'jdoe@contoso.com' -FirstName 'Jonathan' -LastName 'Doe' -PhoneNumber '425-555-5555' -PostalCode '98004' -PurchaseOrderNumber '1234' -State 'WA' -TaxId '0123'
```

Review the [Set-PartnerBillingProfile](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/docs/help/Set-PartnerLegalProfile.md) help for more information.

### REST API

**Request syntax**

|Method|Request URI|
|---|---|
|**PUT**|*{baseURL}*/v1/profiles/billing HTTP/1.1|

**Request example**

```http
PUT https://api.partnercenter.microsoft.com/v1/profiles/billing HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 9231559e-ed95-41e4-810b-e754ae32dc2f
MS-CorrelationId: cb9f3209-d020-4bf9-871c-e1f1c75348f8
Content-Length: 613
Expect: 100-continue

{
    "CompanyName":"TEST_TEST_BugBash1",
    "Address":{
        "Country":"US",
        "Region":null,
        "City":"Redmond",
        "State":"WA",
        "AddressLine1":"1 Microsoft Way",
        "AddressLine2":"","PostalCode":"98052",
        "FirstName":null,
        "LastName":null,
        "PhoneNumber":null
    },
    "PrimaryContact":{
        "FirstName":"Test",
        "LastName":"Customer",
        "Email":null,
        "PhoneNumber":""
    },
    "PurchaseOrderNumber":"9888",
    "TaxId":<TaxId>,
    "BillingCurrency":"USD",
    "Links":{
        "Self":{
            "Uri":"/profiles/billing",
            "Method":"GET","Headers":[]
        }
    },
    "Attributes":{
        "Etag":<etag>,
        "ObjectType":"BillingProfile"
    }
}
```

**Response example**

If the request is successful, this method returns a **BillingProfile** object in the response body.

```http
HTTP/1.1 200 OK
Content-Length: 568
Content-Type: application/json; charset=utf-8
MS-CorrelationId: cb9f3209-d020-4bf9-871c-e1f1c75348f8
MS-RequestId: 9231559e-ed95-41e4-810b-e754ae32dc2f
Date: Mon, 21 Mar 2016 05:47:16 GMT

{
    "CompanyName":"TEST_TEST_BugBash1",
    "Address":{
        "Country":"US",
        "Region":null,
        "City":"Redmond",
        "State":"WA",
        "AddressLine1":"1 Microsoft Way",
        "AddressLine2":"","PostalCode":"98052",
        "FirstName":null,
        "LastName":null,
        "PhoneNumber":null
    },
    "PrimaryContact":{
        "FirstName":"Test",
        "LastName":"Customer",
        "Email":null,
        "PhoneNumber":""
    },
    "PurchaseOrderNumber":"9888",
    "TaxId":<TaxId>,
    "BillingCurrency":"USD",
    "Links":{
        "Self":{
            "Uri":"/profiles/billing",
            "Method":"GET","Headers":[]
        }
    },
    "Attributes":{
        "Etag":<etag>,
        "ObjectType":"BillingProfile"
    }
}
```

## Business profile

The partner business profile contains legal business-contact information about your business. For this reason, if you change this information in Partner Center, Microsoft might contact your business for verification. This guide covers how to update your partner business profile.

### .NET SDK

To update the partner legal business profile, first instantiate a **LegalBusinessProfile** object and populate it with the existing profile. For more information, see [Get the partner legal](https://docs.microsoft.com/partner-center/develop/get-legal-business-profile) business profile. Then, update the properties that you need to change. The following code example illustrates changing the address and primary contact phone numbers.

Next, get an interface to the partner profile operations collection from the **IAggregatePartner.Profiles** property. Then, retrieve the value of the **LegalBusinessProfile** property to get an interface to legal business profile operations. Finally, call the **Update** or **UpdateAsync** method with the changed object to update the profile.

```csharp
// IAggregatePartner partnerOperations;

BillingProfile existingBillingProfile = partnerOperations.Profiles.BillingProfile.Get();

// update the profile with a purchase order number
existingBillingProfile.PurchaseOrderNumber = new Random().Next(9000, 10000).ToString(CultureInfo.InvariantCulture);

BillingProfile updatedPartnerBillingProfile = partnerOperations.Profiles.BillingProfile.Update(existingBillingProfile);
```

### Java SDK

To update the partner legal business profile, first instantiate a **LegalBusinessProfile** object and populate it with the existing profile. Then, update the properties that you need to change. The following code example illustrates changing the address and phone number.

Next, get an interface to the partner profile operations collection from the **IIPartner.getProfiles** function. Then, retrieve the result of the **getLegalBusinessProfile** function to get an interface to legal business profile operations. Finally, call the **update** function with the changed object to update the profile.

```java
// IPartner partnerOperations;

LegalBusinessProfile legalBusinessProfile = partnerOperations.getProfiles().getLegalBusinessProfile().get();

// Change the address phone number.
Address address = legalBusinessProfile.getAddress();
address.setPhoneNumber("4255550110");

// Apply changes to the profile.
LegalBusinessProfile updateLegalBusinessProfile = partnerOperations.getProfiles().getLegalBusinessProfile().update( legalBusinessProfile );
```

### PowerShell

Run the following command to update the partner legal business profile

```powershell
Set-PartnerLegalProfile -PhoneNumber '4255550110'
```

Review the [Set-PartnerLegalProfile](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/docs/help/Set-PartnerLegalProfile.md) help for more information.

### REST API

**Request syntax**

|Method|Request URI|
|---|---|
|**PUT**|*{baseURL}*/v1/profiles/legalbusiness HTTP/1.1|

**Request body**

The legal business-profile resource.

**Request example**

```http
PUT https://api.partnercenter.microsoft.com/v1/profiles/legalbusiness HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 4549ac0c-0f1d-4d8f-b02f-6d36fadcccee
MS-CorrelationId: aa2a0d8c-7a41-470f-97ae-b82e6c338ee4
X-Locale: en-US
Content-Type: application/json
Host: api.partnercenter.microsoft.com
Content-Length: 806
Expect: 100-continue

{
    "CompanyName": "Lucerne Publishing",
    "Address": {
        "Country": "US",
        "Region": null,
        "City": "Redmond",
        "State": "WA",
        "AddressLine1": "123 Main Street",
        "AddressLine2": "",
        "PostalCode": "98052",
        "FirstName": "Gena",
        "LastName": "Soto",
        "PhoneNumber": "4255550110"
    },
    "PrimaryContact": {
        "FirstName": "Gena",
        "LastName": "Soto",
        "Email": "gena@lucernepublishing.com",
        "PhoneNumber": "4255550110"
    },
    "CompanyApproverAddress": {
        "Country": "US",
        "Region": null,
        "City": "Redmond",
        "State": "WA",
        "AddressLine1": "123 Main Street",
        "AddressLine2": "",
        "PostalCode": "98052",
        "FirstName": null,
        "LastName": null,
        "PhoneNumber": null
    },
    "CompanyApproverEmail": "gena@lucernepublishing.com",
    "VettingStatus": "authorized",
    "VettingSubStatus": "none",
    "Links": {
        "Self": {
            "Uri": "/profiles/legalbusiness",
            "Method": "GET",
            "Headers": []
        }
    },
    "Attributes": {
        "ObjectType": "LegalBusinessProfile"
    }
}
```

**Response example**

If the request is successful, the response body contains the updated **LegalBusinessProfile**.

```http
HTTP/1.1 200 OK
Content-Length: 1157
Content-Type: application/json; charset=utf-8
MS-CorrelationId: aa2a0d8c-7a41-470f-97ae-b82e6c338ee4
MS-RequestId: 4549ac0c-0f1d-4d8f-b02f-6d36fadcccee
MS-CV: KZLU42qJ4EObO75q.0
MS-ServerId: 030020643
Date: Tue, 21 Mar 2017 22:03:15 GMT

{
    "companyName": "Lucerne Publishing",
    "address": {
        "country": "US",
        "city": "Redmond",
        "state": "WA",
        "addressLine1": "123 Main Street",
        "addressLine2": "",
        "postalCode": "98052",
        "firstName": "Gena",
        "lastName": "Soto",
        "phoneNumber": "4255550110"
    },
    "primaryContact": {
        "firstName": "Gena",
        "lastName": "Soto",
        "email": "gena@lucernepublishing.com",
        "phoneNumber": "4255550110"
    },
    "companyApproverAddress": {
        "country": "US",
        "city": "Redmond",
        "state": "WA",
        "addressLine1": "123 Main Street",
        "addressLine2": "",
        "postalCode": "98052"
    },
    "companyApproverEmail": "gena@lucernepublishing.com",
    "vettingStatus": "authorized",
    "vettingSubStatus": "none",
    "profileType": "LegalBusinessProfile",
    "links": {
        "self": {
            "uri": "/profiles/legalbusiness",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "LegalBusinessProfile"
    }
}
```

## Support profile

If your customers call Microsoft looking for support, Microsoft will point them back to the website, email, and phone contact information that's listed in this support profile.

This guide covers how to update your support profile if necessary.

### .NET SDK

To update your support profile, first [get your support profile](https://docs.microsoft.com/partner-center/develop/get-support-profile) and make any changes you wish. Then, use your **IPartnerOperations.Profiles** collection. Call the **SupportProfile** property, followed by the **Update()** or **UpdateAsync()** method.

```csharp
// IAggregatePartner partnerOperations;

SupportProfile supportProfile = partnerOperations.Profiles.SupportProfile.Get();

// Update the profile
SupportProfile newSupportProfile = new SupportProfile
{
   Email = supportProfile.Email,
   Website = supportProfile.Website,
   Telephone = new Random().Next(10000000, 99999999).ToString(CultureInfo.InvariantCulture)
};

SupportProfile updatedSupportProfile = partnerOperations.Profiles.SupportProfile.Update(newSupportProfile);
```

### PowerShell

Run the following command to update the partner support profile

```powershell
Set-PartnerSupportProfile -SupportEmail 'support@contoso.com' -SupportPhoneNumber '4255555555' -SupportWebsite 'https://www.microsoft.com'
```

Review the [Set-PartnerSupportProfile](https://github.com/Microsoft/Partner-Center-PowerShell/blob/master/docs/help/Set-PartnerSupportProfile.md) help for more information.

### REST API

**Request syntax**

|Method|Request URI|
|---|---|
|**PUT**|{baseURL}/v1/profiles/supportprofile HTTP/1.1|

**Request body**

Enter the full support-profile resource in the request body.

**Request example**

```http
PUT https://api.partnercenter.microsoft.com/v1/profiles/supportprofile HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 603f3cd9-01b8-48f2-b65d-855a246f5bfd
MS-CorrelationId: 20604323-50bf-4738-9968-c5486ab32be0
Content-Type: application/json
Content-Length: 167
Expect: 100-continue

{
    "Email": "email@sample.com",
    "Telephone": "4255555555",
    "Website": "www.microsoft.com",
    "ProfileType": "support_profile",
    "Attributes": {
        "ObjectType": "PartnerSupportProfile"
    }
}
```

**Response example**

If the request is successful, this method returns updated **SupportProfile** object properties in the response body.

```http
HTTP/1.1 200 OK
Content-Length: 502
Content-Type: application/json
MS-CorrelationId: 20604323-50bf-4738-9968-c5486ab32be0
MS-RequestId: 603f3cd9-01b8-48f2-b65d-855a246f5bfd
Date: Wed, 25 Nov 2015 07:16:18 GMT

{
    "email": "email@sample.com",
    "telephone": "4255555555",
    "website": "www.microsoft.com",
    "profileType": "support_profile",
    "links": {
        "self": {
            "uri": "/v1/profiles/support",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "PartnerSupportProfile"
    }
}
```

## Next steps

- [Review](../available-apis-overview.md) what are the available APIs for Azure CSP integration.
- [Review](../integration-scenarios-list.md) the list of available Azure CSP Integration scenarios.
