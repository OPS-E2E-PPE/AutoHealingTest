---
title: Validate a customer address - Azure Cloud Solution Provider | Microsoft Docs
description: Learn how to validate a customer address. The information in this article supports Azure Cloud Solution Provider (Azure CSP) integration.
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

# Validate a customer address

Learn how to validate an address by using the address validation API. You can use one of the following options:

- PowerShell
- C#
- REST API

## PowerShell

To validate an address by using PowerShell, you have two options. You can use this command:

    ```powershell
    $address = New-PCAddress -AddressLine1 '<string>' -AddressLine2 '<string>' -City '<string>' -State '<string>' -PostalCode '<string>' -country 'two-digit country code' -region '<string>'
    Test-PCAddress -Address <DefaultAddress> 
    ```

Or, you can use this command:

    ```powershell
    Test-PCAddress -AddressLine1 '<string>' -AddressLine2 '<string>' -City '<string>' -State '<string>' -PostalCode '<string>' -country 'two-digit country code' -region '<string>'
    ```

## C#

1. Instantiate a new **Address** object and populate it with the address you want to validate.
2. Retrieve an interface for **Validations** operations from the **IAggregatePartners.Validations** property.
3. Call the **IsAddressValid** method with the address object. 

```csharp
// Create an address to validate.
Address address = new Address()
{
    AddressLine1 = "One Microsoft Way",
    City = "Redmond",
    State = "WA",
    PostalCode = "98052",    
    Country = "US"
};

// Validate the address.
bool result = partnerOperations.Validations.IsAddressValid(address);

// If the address is valid, the result should equal true.
Console.WriteLine("Result: " + result.ToString());

// The following is an example that causes address validation to fail.
try
{
    // Change to an invalid postal code for this address.
    address.PostalCode = "98007";

    // Validate the address.
    result = partnerOperations.Validations.IsAddressValid(address);

    Console.WriteLine("ERROR: The code should have thrown an exception - BadRequest(400).");
}
catch (PartnerException exception)
{
    if (exception.ErrorCategory == PartnerErrorCategory.BadInput)
    {
        Console.WriteLine(exception.ErrorCategory.ToString());
        Console.WriteLine("Exception:");
        Console.WriteLine("Message: {0}", exception.Message);
    }
    else
    {
        throw;
    }
}
```

## REST API

### Request

**Request syntax**

|Method|Request URI|
|---|---|
|POST|{baseURL}/v1/validations/address HTTP/1.1|

**Request body**

The following properties are present in the request body.

|Name|Type|Description|
|---|---|---|
|addressline1|string|The first line of the address.|
|addressline2|string|(Optional) The second line of the address.|
|city|string|The city.|
|state|string|The state.|
|postalcode|string|The postal code.|
|country|string|The two-character ISO alpha-2 country code.|

**Request example**

```json
POST https://api.partnercenter.microsoft.com/v1/validations/address HTTP/1.1
Content-Type: application/json
Authorization: Bearer <token> 
Accept: application/json
MS-RequestId: 0b30452a-8be2-4b8b-b25b-2d4850f4345f
MS-CorrelationId: 8a853a1a-b0e6-4cb0-ae87-d6dd32ac3a0c
X-Locale: en-US
Host: api.partnercenter.microsoft.com
Content-Length: 129

{
    AddressLine1: "One Microsoft Way",
    City: "Redmond",
    State: "WA",
    PostalCode: "98052",
    Country: "US"
}
```

### Response

If the request is successful, the method returns a status code 200, as demonstrated in the success example.

If the request fails, the method returns a status code 400, as demonstrated in the failure example. The response body contains a JSON payload that has additional information about the error.

**Response succeeded example**

```http
HTTP/1.1 200 OK
Content-Length: 0
MS-CorrelationId: 8a853a1a-b0e6-4cb0-ae87-d6dd32ac3a0c
MS-RequestId: 0b30452a-8be2-4b8b-b25b-2d4850f4345f
MS-CV: IqhjoWVyq0Kl81dO.0
MS-ServerId: 030011719
Date: Mon, 13 Mar 2017 23:56:12 GMT
```

**Response failed example**

```json
HTTP/1.1 400 Bad Request
Content-Length: 418
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 8a853a1a-b0e6-4cb0-ae87-d6dd32ac3a0c
MS-RequestId: 0b30452a-8be2-4b8b-b25b-2d4850f4345f
MS-CV: pdlItMyvtkmGHDWt.0
MS-ServerId: 101112012
Date: Tue, 14 Mar 2017 01:57:55 GMT

{
    "code": 2007,
    "description": "{\"code\":\"60071\",\"reason\":\"ZipCityInvalid - Details: Field - 'City' is corrected from OldValue: 'Redmond' to NewValue: 'BELLEVUE'.\",\"corrected_address\":{\"country\":\"US\",\"region\":\"WA\",\"city\":\"BELLEVUE\",\"address_line1\":\"One Microsoft Way\",\"postal_code\":\"98007\"},\"object_type\":\"AddressValidation\",\"resource_status\":\"Active\"}",
    "data": [],
    "source": "PartnerFD"
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).
