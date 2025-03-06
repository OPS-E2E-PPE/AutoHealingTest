---
title: Manage user accounts in Azure CSP| Microsoft Docs
description: Review scenario for Azure CSP integration.
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

# Manage user accounts

This guide covers the various actions you take to manage a customer's user accounts in Partner Center, including *adding, deleting, updating*, and *restoring* user accounts.

## Web UI method

### Create users by using the web UI method

With the Partner Center Web portal, you can upload users either manually (one at a time) or in bulk with a .csv file. Instructions for both methods are described here.

### Create users one at a time by using the web UI method

1. From the Partner Center **Dashboard**, select **Customers**. Next, select the specific customer, and then select **Users and Licenses**.
2. Select the **Add User** button.

   ![Click the add user button](media/add-user-accounts-1.PNG)

3. Enter the user's first and last name (optional), display name as it will be shown in Partner Center, on microsoft.com email address, and location.
4. Select the user permissions.
5. When you're finished, select **Save**.

   ![Select the "Save" button after all information has been entered](media/add-user-accounts-2.PNG)

6. Make sure to copy the temporary password information for the new user.

>[!IMPORTANT]
>Ths temporary password information won't be available later.

### Create users in bulk by using the web UI method

Before you can upload users in bulk, the following criteria must be met:

- You must have global administrator permissions for the account.
- Each user must have a unique email address in the customer's email domain(s).
- A maximum of 100 users can be uploaded at a time. Batches of more than 100 users must be split across multiple uploads.
- All users must be in the same location.
- Only the necessary data should be entered. *Extra data causes the upload to fail.*

1. Create a comma-separated value (.csv) data file with the data that's described in the following section. Then save the file.
2. From the **Dashboard** menu, select **Customers**. Then choose a customer from the list.
3. Go to **Users and licenses**, and then select **Upload users**.
4. Under **Upload user info**, select **Browse**, and then choose the .csv file from step 1.
5. Select **Validate**.

   > [!NOTE]
   > Most account creation errors are caused by data file issues, including missing information, malformed or duplicate email addresses, or too many records in the file. Ensure that the data is accurate before uploading it.

6. After the data has been validated, select the geographic **Location** for the new users, and then select **Save**.
7. Download the temporary password information for the users. 

>[!IMPORTANT]
>This temporary password information won't be available later and must be downloaded immediately.

**CSV data file parameters**

The following data must be present in the data file.

|Column name|Description|Limitation|
|---|---|---|
|First name|User's first name (optional)|50-character limit|
|Last name|User's last name (optional)|50-character limit|
|Display name|Name that's displayed in Partner Center|50-character limit|
|Email|User's business email address at customer company|Each user must have a unique email address|
|Status update| An update that indicates whether the new user record was successfully created|*Must be left empty*|

### Update a user account by using the web UI method

1. On the **Dashboard** screen, go to **Customers**. Select the customer's name, and then select **Users and Licenses**. Then select the user's name.

   ![Update a user account](media/update-user-account-1.PNG)

2. Here the user's information can be updated, including name, display name, email, location, and permissions. After you make the changes, select **Submit**.

   ![Submit the user information](media/update-user-account-2.PNG)

### Delete a user account by using the web UI method

1. From the **Dashboard** menu, select **Customers**. Choose the customer from the list.
2. In the customer menu, select **Users and licenses**. Choose the user from the list.

   ![Choose the user](media/delete-user-account-1.PNG "Go to Users and licenses, then select the user you wish to delete.")

3. At the bottom of the screen, select **Delete user account**.

   ![Delete the user account](media/delete-user-account-2.PNG "Click Delete user account at the bottom.")

### Restore a deleted user by using the web UI method

If a user was deleted in the last 30 days, you can restore them by taking the following steps:

1. From the **Dashboard** menu, select **Customers**. Then choose the customer from the list.
2. Select **Users and licenses.**
3. Select the **Deleted users ( )** tab. (It reads **(1)** or greater when the tab is active.)

   ![Select the Deleted users tab](media/restore-deleted-user-1.PNG "The Deleted users tab holds deleted users for 30 days.")

4. Select one or more of the deleted user's check boxes, and then select **Restore**.

   ![Select Restore](media/restore-deleted-user-2.PNG "The Restore option lights up only if one or more users is selected.")

All selected user accounts reappear in the **Users and licenses** page.

## PowerShell method

### Create a user by using PowerShell

To create a new user account by using PowerShell, use the following commands:

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>'

$password = '<password>'

$passwordSecure = $password | ConvertTo-SecureString -AsPlainText -Force

New-PCCustomerUser -TenantId $customer.id -UsageLocation '<country code>' -UserPrincipalName '<upn>' -FirstName '<first name>' -LastName '<last name>' -DisplayName '<display name>' -ForceChangePassword $true -password $passwordSecure
```

### Update a user by using PowerShell

To update a customer user by using PowerShell, input the following commands:

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>'

$user = Get-PCCustomerUser -TenantId $customer.id -UserId '<user id>'

Set-PCCustomerUser -TenantId $customer.id -User $user -UserPrincipalName '<new UPN>'
```

### Delete a user by using PowerShell

To delete a user account by using PowerShell, input the following commands:

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>'

$user = Get-PCCustomerUser -TenantId $customer.id -UserId '<user id>'

Remove-PCCustomerUser -TenantId $customer.id -user $user
```

### Restore a deleted user by using PowerShell

To restore a deleted user by using PowerShell, input the following commands:

```powershell
$customer = Get-PCCustomer -TenantId '<customer identifier>'

$user = Get-PCCustomerUser -TenantId $customer.id -Deleted | ? id -EQ '<user identifier>'

Restore-PCCustomerUser -TenantId $customer.id -user $user
```

## C# method 

### Create a user by using C#

To create a new user by using C#, take the following steps:

1. Create a new **CustomerUser** object with the relevant user information.
2. Use the **IAggregatePartner.Customers** collection and call the **ById()** method.
3. Call the **Users** property, followed by the **Create** method.

```csharp
// string selectedCustomerId;
// IAggregatePartner partnerOperations;
// var SelectedCustomer;

var userToCreate = new CustomerUser()
            {
                PasswordProfile = new PasswordProfile() { ForceChangePassword = true, Password = "Password!1" },
                DisplayName = "TestDisplayName",
                FirstName = "TestFirstName",
                LastName = "TestLastName",
                UsageLocation = "US",
                UserPrincipalName = Guid.NewGuid().ToString("N") + "@" + selectedCustomer.CompanyProfile.Domain.ToString()
            };;

User createdUser = partnerOperations.Customers.ById(selectedCustomerId).Users.Create(userToCreate);
```

### Update a user by using C#

To update the details for a specified customer user by using C#, take the following steps: 

1. Retrieve the specified customer ID and user to update.
2. Create an updated version of the user in a new **CustomerUser** object.
3. Use the **IAggregatePartner.Customers** collection and call the **ById()** method.
4. Call the **Users** property and the **ById()** method, followed by the **Patch()** method.

```csharp
//string selectedCustomerId;
//customerUser specifiedUser;
//IAggregatePartner partnerOperations;

//updated information
var userToUpdate = new CustomerUser()
            {
                PasswordProfile = new PasswordProfile() { ForceChangePassword = true, Password = "testPw@!122B" },
                DisplayName = "Roger Federer",
                FirstName = "Roger",
                LastName = "Federer",
                UsageLocation = "US",
                UserPrincipalName = Guid.NewGuid().ToString("N") + "@" + selectedCustomer.CompanyProfile.Domain.ToString()
            };

// update customer user information
User updatedCustomerUserInfo = partnerOperations.Customers.ById(selectedCustomerId).Users.ById(specifiedUser.Id).Patch(userToUpdate);
```

### Delete a user by using C#

To delete a user by using C#, take the following steps: 

1. Use the **IAggregatePartner.Customers.ById** method with the customer ID to identify the customer. 

2. Call the **Users.ById** method to identify the user. 

3. Call the **Delete** method to delete the user and set the user state to inactive. 

```csharp
// string selectedCustomerId;
// IAggregatePartner partnerOperations;
// string customerUserIdToDelete;

partnerOperations.Customers.ById(selectedCustomerId).Users.ById(customerUserIdToDelete).Delete();
```

### Restore a deleted user by using C#

To restore a deleted user by using C#, take the following steps: 

1. Create a new instance of the **CustomerUser** class, and then set the **User.State** to **UserState.Active**. Remaining fields in the user resource are automatically  restored from the deleted, inactive user resource. 

2. Use the **IAggregatePartner.Customers.ById** method with the customer ID to identify the customer, and the **Users.ById** method to identify the user. 

3. Call the **Patch** method and pass the **CustomerUser** instance to send the request to restore the user.

```csharp
// string selectedCustomerId;
// IAggregatePartner partnerOperations;
// string selectedCustomerUserId;

var updatedCustomerUser = new CustomerUser()
{
    State = UserState.Active
};

// Restore customer user information.
var restoredCustomerUserInfo = partnerOperations.Customers.ById(selectedCustomerId).Users.ById(selectedCustomerUserId).Patch(updatedCustomerUser);
```

## REST API method

### Create a user by using the REST API method

**Request Syntax**

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|Allows the reseller to filter the results for a customer that belongs to the reseller.|
|**user-id**|**guid**|Belongs to a single user account. (*Optional*)|

**Request example**

```json
POST https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/users HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: b1317092-f087-471e-a637-f66523b2b94c
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
{
      "usageLocation": "country/region code",
      "userPrincipalName": "userid@domain.onmicrosoft.com",
      "firstName": "First",
      "lastName": "Last",
      "displayName": "User name",
      "passwordProfile":{
                 password: "abCD123*",
                 forceChangePassword: true
      },
      "attributes": {
        "objectType": "CustomerUser"
      }
}
```

**Response**

If this method is successful, it returns a user account, including the GUID.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 31942
Content-Type: application/json
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
MS-RequestId: b1317092-f087-471e-a637-f66523b2b94c
Date: June 24 2016 22:00:25 PST

{
  "usageLocation": "country/region code",
  "id": "4b10bf41-ab11-40e3-8c53-cd67849b50de",
  "userPrincipalName": "userid@domain.onmicrosoft.com",
  "firstName": "First",
  "lastName": "Last",
  "displayName": "User name",
  "passwordProfile": {
    "forceChangePassword": true,
    "password": "abCD123*"
  },
  "lastDirectorySyncTime": null,
  "userDomainType": "none",
  "state": "active",
  "softDeletionTime": null,
  "attributes": {
    "objectType": "CustomerUser"
  }
}
```

### Update a user by using the REST API method

**Request syntax**

|Method|Request URI|
|---|---|
|**PATCH**|*{baseURL}*/v1/customers/{customer-tenant-id}/users HTTP/1.1|

**URI parameters**

These are the *required* query parameters for identifying the correct customer:

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|Identifies the customer.|
|**user-id**|**guid**|Belongs to a single user account.|

**Request example**

```json
PATCH https://api.partnercenter.microsoft.com/v1/customers/<customer-tenant-id>/users/<user-id> HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: b1317092-f087-471e-a637-f66523b2b94c
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
{
      "usageLocation": "new country/region code",

      "attributes": {
        "objectType": "CustomerUser"
      }
}
```

**Response**

If this method is successful, it returns a user account with the updated information.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 31942
Content-Type: application/json
MS-CorrelationId: 8a53b025-d5be-4d98-ab20-229d1813de76
MS-RequestId: b1317092-f087-471e-a637-f66523b2b94c
Date: June 24 2016 22:00:25 PST
{
  "usageLocation": "new country/region code",
  "id": "4b10bf41-ab11-40e3-8c53-cd67849b50de",
  "userPrincipalName": "emailidchange@abcdefgh1234.ccsctp.net",
  "firstName": "FirstNameChange",
  "lastName": "LastNameChange",
  "displayName": "DisplayNameChange",
  "userDomainType": "none",
  "state": "active",
  "links": {
    "self": {
      "uri": "/customers/eebd1b55-5360-4438-a11d-5c06918c3014/users/4b10bf41-ab11-40e3-8c53-cd67849b50de",
      "method": "GET",
      "headers": [

      ]
    }
  },
  "attributes": {
    "objectType": "CustomerUser"
  }
}
```

### Delete a user by using the REST API method

**Request syntax**

|Method|Request URI|
|---|---|
|**DELETE**|*{baseURL}*/v1/customers/{customer-tenant-id}/users/{user-id} HTTP/1.1|

**URI parameters**

Use the following parameters to identify the customer and user:

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|Allows the reseller to filter the results for a given customer.|
|**user-id**|**guid**|Belongs to a single user account.|

**Request example**

```http
DELETE https://api.partnercenter.microsoft.com/v1/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/users/a45f1416-3300-4f65-9e8d-f123b397a4ea HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: f113b126-ec13-4baa-ab4d-67c245244971
MS-CorrelationId: 709c0b80-016c-4662-b29f-697fdf03e87a
X-Locale: en-US
Host: api.partnercenter.microsoft.com
Content-Length: 0
```

**Response**

If the method is successful, it returns **204 No Content**.

**Response example**

```http
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 709c0b80-016c-4662-b29f-697fdf03e87a
MS-RequestId: f113b126-ec13-4baa-ab4d-67c245244971
MS-CV: 90KUJA7HKEaG8wHu.0
MS-ServerId: 101112616
Date: Tue, 24 Jan 2017 23:27:18 GMT
```

### Restore a deleted user by using the REST API method

**Request syntax**

|Method|Request URI|
|---|---|
|**PATCH**|*{baseURL}*/v1/customers/{customer-tenant-id}/users/{user-id} HTTP/1.1|


**URI parameters**

Following are the  query parameters that are required to specify the customer ID and user ID:

|Name|Type|Description|
|---|---|---|
|**customer-tenant-id**|**guid**|Allows the reseller to filter the results to a given customer.|
|**user-id**|**guid**|Belongs to a single user account.|

**Request body**

These are the properties in the request body.

|Name|Type|Description|
|---|---|---|
|State|string|The user state. To restore a deleted user, this must contain "active"|
|Attributes|object|Contains "ObjectType":"CustomerUser". *(Optional)*|

**Request example**

```json
PATCH https://api.partnercenter.microsoft.com/v1/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/users/a45f1416-3300-4f65-9e8d-f123b397a4ea HTTP/1.1
Authorization: Bearer <token>
Accept: application/json
MS-RequestId: 6e668bc0-5bd7-44d6-b6fa-529d41ce9659
MS-CorrelationId: 32be760f-8282-4e01-a37b-829c8a700e8a
X-Locale: en-US
Content-Type: application/json
Host: api.partnercenter.microsoft.com
Content-Length: 269
Expect: 100-continue

{
    "State": "active",
    "Attributes": {
        "ObjectType": "CustomerUser"
    }
}
```

**Response**

If this method is successful, it returns the restored user information in the response body.

**Response example**

```json
HTTP/1.1 200 OK
Content-Length: 465
Content-Type: application/json; charset=utf-8
MS-CorrelationId: 32be760f-8282-4e01-a37b-829c8a700e8a
MS-RequestId: 6e668bc0-5bd7-44d6-b6fa-529d41ce9659
MS-CV: ZTeBriO7mEaiM13+.0
MS-ServerId: 101112616
Date: Fri, 20 Jan 2017 22:24:55 GMT

 {
    "usageLocation": "US",
    "id": "a45f1416-3300-4f65-9e8d-f123b397a4ea",
    "userPrincipalName": "e83763f7f2204ac384cfcd49f79f2749@dtdemocspcustomer005.onmicrosoft.com",
    "firstName": "Ferdinand",
    "lastName": "Filibuster",
    "displayName": "Ferdinand",
    "userDomainType": "none",
    "state": "active",
    "links": {
        "self": {
            "uri": "/customers/4d3cf487-70f4-4e1e-9ff1-b2bfce8d9f04/users/a45f1416-3300-4f65-9e8d-f123b397a4ea",
            "method": "GET",
            "headers": []
        }
    },
    "attributes": {
        "objectType": "CustomerUser"
    }
}
```

## Next steps

- Learn about [APIs for Azure CSP integration](../available-apis-overview.md).
- See the list of [Azure CSP integration scenarios](../integration-scenarios-list.md).