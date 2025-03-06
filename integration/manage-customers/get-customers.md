---
title: Search for customers - Azure Cloud Solution Provider | Microsoft Docs
description: This article covers the automated API and web UI methods for searching your customer list.
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

# Search for customers

This guide covers the automated API and web UI methods that are available for searching through your list of customers. The methods include isolating specific customers, applying search filters, and retrieving a bulk list of customers.

## Web UI

### Search for a customer by using the search filter

1. On the Microsoft Partner Center **Dashboard** menu, select **Customers**.
2. To search for a customer, enter the customer name or domain name in the search box.
3. To view the customer's Microsoft ID, associated subscriptions, and services quick links, select the down arrow at the right of the customer row.

### Get a list of all customers

1. On the **Dashboard** menu, select **Customers**.

2. Do one of the following:
    * To filter for the subset of data that you want to export, use the search filter, and then select **Export customers**.
    * To export the entire customer list, leave the search field blank, and then select **Export customers**.

      ![The "Export customers" link](media/export-customer-list-1.PNG)

    * *Indirect providers*: You can filter your customer list by indirect reseller. To do so, select **Filter by indirect reseller** from the list, and then select a reseller.

Partner Center converts the complete customer list into a .csv file and then uploads it to the default download folder on your computer. The data columns include the following:

  - **Microsoft ID**
  - **Company name**
  - **Primary domain name**
  - **Relationship**: The partner's business relationship with each listed customer

## PowerShell

### Get a customer by ID

To find a customer by ID, enter the following command:

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>'
```

### Get a list of all customers

To get a list of all customers, enter the following command:

```powershell
Get-PCCustomer
```

## C#

### Get a customer by ID

To get a customer by ID, use your **IAggregatePartner.Customers** collection, call the **ById()** method, then call the **Get()** or **GetAsync()** method.

```csharp
// IAggregatePartner partnerOperations;
// string customerIdToRetrieve;

Customer customerInfo = partnerOperations.Customers.ById(customerIdToRetrieve).Get();
```

### Get customers by search filter

To get a collection of customers that match a filter, do the following:

1. To create the filter, instantiate a **SimpleFieldFilter** object.
2. To pass a string that contains the **CustomerSearchField** and indicate the type of filter operation, choose the **FiledFilterOperation** enumeration.
3. To instantiate an **iQuery** object to pass to the query, call the **BuildSimpleQuery** method.  
Now it's time to execute the filter and get the result.
4. To get an interface to the partner's customer operations, use **IAggregatePartner.Customers**.
5. Call the **Query** or **QueryAsync** method.

    ```csharp
    IAggregatePartner partnerOperations;

    // Specify the partial string to filter by (to match Contoso).
    string searchPrefix = "cont"  

    // Create a simple field filter.
    var fieldFilter = new SimpleFieldFilter(
        CustomerSearchField.CompanyName.ToString(),
        FieldFilterOperation.StartsWith,
        searchPrefix);

    // Create an iQuery object to pass to the Query method.
    var myQuery = QueryFactory.Instance.BuildSimpleQuery(fieldFilter);

    // Get the collection of matching customers.
    var customers = partnerOperations.Customers.Query(myQuery);
    ```

### Get a list of all customers

To get a list of all customers, do the following:

1. To create an **IPartner** object, use your **IAggregatePartner.Customers** collection. 
2. Retrieve the customer list by using the **Query()** or **QueryAsync()** method. For instructions on creating a query, see the **QueryFactory** class.

```csharp
// IAggregatePartner partnerOperations;

var allCustomers = new List<Customer>();

// All the operations executed on this partner operation instance will share the same correlation Id but will differ in request Id
IPartner scopedPartnerOperations = partnerOperations.With(RequestContextFactory.Instance.Create(Guid.NewGuid()));

// Read customers into chunks of 40s
var customersBatch = scopedPartnerOperations.Customers.Query(QueryFactory.Instance.BuildIndexedQuery(40));
var customersEnumerator = scopedPartnerOperations.Enumerators.Customers.Create(customersBatch);
```

## REST API

### Get a customer by ID

**Request syntax**

|Method|Request URL|
|---|---|
|GET|*{baseURL}*/v1/customers/{customer-tenant-id} HTTP/1.1|

**URL parameter**

To get a specific customer, use the following query parameter:

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|The value is a GUID-formatted string that lets the reseller filter the results to find a specific customer that belongs to the reseller.|

**Request example**

```http
GET https://api.partnercenter.microsoft-ppe.com/v1/customers/<customer-tenant-id> HTTP/1.1  
Authorization: Bearer <token>
Accept: application/json
MS-CorrelationId: a176c585-b5de-4d65-824c-67a6deb45cd9  
MS-RequestId: 74ca1db9-df92-41c6-a362-a16433b0542b  
```

If the request is successful, this method returns a customer resource in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 1530
Content-Type: application/json; charset=utf-8
MS-CorrelationId: a176c585-b5de-4d65-824c-67a6deb45cd9
MS-RequestId: 74ca1db9-df92-41c6-a362-a16433b0542b
{
  "id": "eebd1b55-5360-4438-a11d-5c06918c3014",
  "commerceId": "99e6a635-48e7-424d-9059-c9db944e3c54",
  "companyProfile": {
    "tenantId": "eebd1b55-5360-4438-a11d-5c06918c3014",
    "domain": "abcdefgh1234.ccsctp.net",
    "companyName": "Fabrikam",
    "address": {
      "country": "US",
      "region": "wa",
      "city": "redmond",
      "addressLine1": "1 ms way",
      "postalCode": "98052",
      "phoneNumber": "1234567890"
    },
    "email": "a@a.com",
    "links": {
      "self": {
        "uri": "/customers/eebd1b55-5360-4438-a11d-5c06918c3014/profiles/company",
        "method": "GET",
        "headers": [

        ]
      }
    },
    "attributes": {
      "objectType": "CustomerCompanyProfile"
    }
  },
  "billingProfile": {
    "id": "eeada110-69d6-4cc9-b093-75feb7ca9d3f",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@fabrikam.com",
    "culture": "en-US",
    "language": "en",
    "companyName": "Fabrikam",
    "defaultAddress": {
      "country": "US",
      "city": "Redmond",
      "state": "WA",
      "addressLine1": "1 Microsoft Way",
      "postalCode": "98052",
      "firstName": "John",
      "lastName": "Doe",
      "phoneNumber": "1234567890"
    },
    "links": {
      "self": {
        "uri": "/customers/eebd1b55-5360-4438-a11d-5c06918c3014/profiles/billing",
        "method": "GET",
        "headers": [
        ]
      }
    },
    "attributes": {
      "etag": "-4242348048554929329",
      "objectType": "CustomerBillingProfile"
    }
  },
  "relationshipToPartner": "reseller",
  "allowDelegatedAccess": true,
  "customDomains": [
    "abcdefgh1234.ccsctp.net"
  ],
  "links": {
    "self": {
      "uri": "/customers/eebd1b55-5360-4438-a11d-5c06918c3014",
      "method": "GET",
      "headers": [

      ]
    }
  },
  "attributes": {
    "objectType": "Customer"
  }
}
```

### Get customers by using the search filter

**Request syntax**

|Method|Request URI|
|---|---|
|**GET**|*{baseURI}*/v1/customers?size={size}&filter={filter} HTTP/1.1|

**URL parameters**

|Name|Type|Description|
|---|---|---|
|size|int| *Optional*. The number of results to be displayed at one time.|
|filter|filter|The filter to apply to customers. This filter must be an encoded string.|

**Filter syntax**

Compose the filter parameter as a series of comma-separated, key/value pairs. Each key and value must be individually quoted and separated by a colon. The entire filter must be encoded.

An unencoded example looks like this: 
`?filter{"Field":"CompanyName","Value":"cont","Operator":"starts_with"}`

The following table describes the key/value pairs:

|Key|Value|
|---|---|
|Field|The field to filter. You can find the valid values in **CustomerSearchField**.|
|Value|The value to filter by. Ignore the case of the value.|
|Operator|The operator to apply. You can find the valid operators in **FieldFilterOperation**.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers?size=0&filter=%7B%22Field%22%3A%22CompanyName%22%2C%22Value%22%3A%22Cont%22%2C%22Operator%22%3A%22starts_with%22%7D HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 5ce66de5-eea9-486f-a11c-c852aa3d1502
MS-CorrelationId: a2a912ee-d595-47e2-97ae-1b0ae1efa13d
X-Locale: en-US
Host: api.partnercenter.microsoft.com
Connection: Keep-Alive
```

If the request is successful, this method returns a collection of matching customer resources in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 1839
Content-Type: application/json; charset=utf-8
MS-CorrelationId: a2a912ee-d595-47e2-97ae-1b0ae1efa13d
MS-RequestId: dfeda56c-1af5-43fc-a9c0-346b9e85dc96
MS-CV: n0lMNyJtaUC802pO.0
MS-ServerId: 202010223
Date: Fri, 24 Feb 2017 22:08:20 GMT

{
    "totalCount": 3,
    "items": [{
            "id": "c5757d70-06f3-4f23-8367-5a9e55019f94",
            "companyProfile": {
                "tenantId": "c5757d70-06f3-4f23-8367-5a9e55019f94",
                "domain": "contoso190.onmicrosoft.com",
                "companyName": "Contoso190",
                "links": {
                    "self": {
                        "uri": "/customers/c5757d70-06f3-4f23-8367-5a9e55019f94/profiles/company",
                        "method": "GET",
                        "headers": []
                    }
                },
                "attributes": {
                    "objectType": "CustomerCompanyProfile"
                }
            },
            "relationshipToPartner": "reseller",
            "links": {
                "self": {
                    "uri": "/customers/c5757d70-06f3-4f23-8367-5a9e55019f94",
                    "method": "GET",
                    "headers": []
                }
            },
            "attributes": {
                "objectType": "Customer"
            }
        }, {
            "id": "7b26b357-9ca3-48b8-a58e-4febe2662a5d",
            "companyProfile": {
                "tenantId": "7b26b357-9ca3-48b8-a58e-4febe2662a5d",
                "domain": "ContosoCorpCo.onmicrosoft.com",
                "companyName": "Contoso",
                "links": {
                    "self": {
                        "uri": "/customers/7b26b357-9ca3-48b8-a58e-4febe2662a5d/profiles/company",
                        "method": "GET",
                        "headers": []
                    }
                },
                "attributes": {
                    "objectType": "CustomerCompanyProfile"
                }
            },
            "relationshipToPartner": "reseller",
            "links": {
                "self": {
                    "uri": "/customers/7b26b357-9ca3-48b8-a58e-4febe2662a5d",
                    "method": "GET",
                    "headers": []
                }
            },
            "attributes": {
                "objectType": "Customer"
            }
        }, {
            "id": "bfbd6ef0-311f-47ec-bbd7-0fcb7846661b",
            "companyProfile": {
                "tenantId": "bfbd6ef0-311f-47ec-bbd7-0fcb7846661b",
                "domain": "contosocorpdemo.onmicrosoft.com",
                "companyName": "Contoso",
                "links": {
                    "self": {
                        "uri": "/customers/bfbd6ef0-311f-47ec-bbd7-0fcb7846661b/profiles/company",
                        "method": "GET",
                        "headers": []
                    }
                },
                "attributes": {
                    "objectType": "CustomerCompanyProfile"
                }
            },
            "relationshipToPartner": "reseller",
            "links": {
                "self": {
                    "uri": "/customers/bfbd6ef0-311f-47ec-bbd7-0fcb7846661b",
                    "method": "GET",
                    "headers": []
                }
            },
            "attributes": {
                "objectType": "Customer"
            }
        }
    ],
    "links": {
        "self": {
            "uri": "/customers?size=0&filter=%7B%22Field%22%3A%22Domain%22%2C%22Value%22%3A%22cont%22%2C%22Operator%22%3A%22starts_with%22%7D",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "Collection"
    }
}
```

### Get a list of all customers

**Request syntax**

|Method| Request URI|
|---|---|
|**GET**|{baseURL}/v1/customers?size={size} HTTP/1.1|

**URI parameter**

To get a list of customers, use the following query parameter:

|Name|Type|Description|
|---|---|---|
|**size**|**int**|The number of results to be displayed at one time.|

**Request example**

```http
GET https://api.partnercenter.microsoft.com/v1/customers?size=40 HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 3705fc6d-4127-4a87-bdba-9658f73fe019
MS-CorrelationId: b12260fb-82de-4701-a25f-dcd367690645
```

If the request is successful, this method returns a collection of customer resources in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 15650
Content-Type: application/json
MS-CorrelationId: b12260fb-82de-4701-a25f-dcd367690645
MS-RequestId: 3705fc6d-4127-4a87-bdba-9658f73fe019
Date: Fri, 20 Nov 2015 01:08:23 GMT

{
    "totalCount": 25,
    "items": [{
        "id": "b44bb1fb-c595-45b0-9e09-d657365580bf",
        "companyProfile": {
            "tenantId": "<guid>",
            "domain": "domain",
            "companyName": "companyName",
            "attributes": {
                "objectType": "CustomerCompanyProfile"
            }
        },
        "relationshipToPartner": "reseller",
        "attributes": {
            "objectType": "Customer"
        }
    },
    {
        "id": "45c44870-ef77-4fdd-b6fe-3dacb075cff2",
        "companyProfile": {
            "tenantId": "<guid>",
            "domain": "domain",
            "companyName": "companyName",
            "attributes": {
                "objectType": "CustomerCompanyProfile"
            }
        },
        "relationshipToPartner": "reseller",
        "attributes": {
            "objectType": "Customer"
        }
    }],
    "links": {
        "self": {
            "uri": "/v1/customers?size=40",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "Collection"
    }
}
```

## Next steps

* Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
* View a list of [Azure CSP integration scenarios](../integration-scenarios-list.md).