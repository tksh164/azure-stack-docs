---
title: Make API requests to Azure Stack Hub
description: Learn how to retrieve an authentication from Azure to make API requests to Azure Stack Hub.
author: sethmanheim

ms.topic: how-to
ms.date: 09/30/2021
ms.author: sethm
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2020

# Intent: As an Azure Stack user, I want to make API requests to Azure Stack so I can automate operations.
# Keyword: api requests azure stack
---


<!--  cblackuk and charliejllewellyn. This is a community contribution by cblackuk-->

# Make API requests to Azure Stack Hub

You can use the Azure Stack Hub REST APIs to automate operations, such as adding a virtual machine (VM) to your Azure Stack Hub cloud.

The APIs require your client to authenticate to the Microsoft Azure sign-in endpoint. The endpoint returns a token to use in the header of every request sent to the Azure Stack Hub APIs. Microsoft Azure uses Oauth 2.0.

This article provides examples that use the **cURL** utility to create Azure Stack Hub requests. cURL is a command-line tool with a library for transferring data. These examples describe the process of retrieving a token to access the Azure Stack Hub APIs. Most programming languages provide Oauth 2.0 libraries, which have robust token management and handle tasks such as refreshing the token.

Review the entire process of using the Azure Stack Hub REST APIs with a generic REST client, such as **cURL**, to help you understand the underlying requests and what you can expect in a response payload.

This article does not explore all the options available for retrieving tokens, such as interactive sign-in or creating dedicated app IDs. For information about these topics, see the [Azure REST API reference](/rest/api/).

## Get a token from Azure

Create a request body formatted using the content type `x-www-form-urlencoded` to obtain an access token. POST your request to the Azure REST authentication and login endpoint.

### URI

```bash  
POST https://login.microsoftonline.com/{tenant id}/oauth2/token
```

**Tenant ID** is either:

- Your tenant domain, such as `fabrikam.onmicrosoft.com`
- Your tenant ID, such as `8eaed023-2b34-4da1-9baa-8bc8c9d6a491`
- Default value for tenant-independent keys: `common`

### Post body

```bash  
grant_type=password
&client_id=1950a258-227b-4e31-a9cf-717495945fc2
&resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
&username=admin@fabrikam.onmicrosoft.com
&password=Password123
&scope=openid
```

For each value:

- **grant_type**:  
   The type of authentication scheme you'll use. In this example, the value is `password`.

- **resource**:  
   The resource the token accesses. You can find the resource by querying the Azure Stack Hub management metadata endpoint. Look at the **audiences** section.

- **Azure Stack Hub management endpoint**:

   ```bash
   https://management.{region}.{Azure Stack Hub domain}/metadata/endpoints?api-version=2015-01-01
   ```

  > [!NOTE]  
  > If you are an admin trying to access the tenant API, make sure to use the tenant endpoint; for example, `https://adminmanagement.{region}.{Azure Stack Hub domain}/metadata/endpoints?api-version=2015-01-011`.

  For example, with the Azure Stack Development Kit (ASDK) as an endpoint:

   ```bash
   curl 'https://management.local.azurestack.external/metadata/endpoints?api-version=2015-01-01'
   ```

  Response:

  ```bash
  {
  "galleryEndpoint":"https://adminportal.local.azurestack.external:30015/",
  "graphEndpoint":"https://graph.windows.net/",
  "portalEndpoint":"https://adminportal.local.azurestack.external/",
  "authentication":{
     "loginEndpoint":"https://login.windows.net/",
     "audiences":["https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"]
     }
  }
  ```

### Example 1

  ```bash
  https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155
  ```

- **client_id**

  This value is hardcoded to a default value:

  ```bash
  1950a258-227b-4e31-a9cf-717495945fc2
  ```

  Alternative options are available for specific scenarios:

  | Application | ApplicationID |
  | --------------------------------------- |:-------------------------------------------------------------:|
  | LegacyPowerShell | 0a7bdc5c-7b57-40be-9939-d4c5fc7cd417 |
  | PowerShell | 1950a258-227b-4e31-a9cf-717495945fc2 |
  | WindowsAzureActiveDirectory | 00000002-0000-0000-c000-000000000000 |
  | VisualStudio | 872cd9fa-d31f-45e0-9eab-6e460a02d1f1 |
  | AzureCLI | 04b07795-8ddb-461a-bbee-02f9e1bf7b46 |

- **username**

  For example, the Azure Stack Hub Microsoft Entra account:

  ```bash
  azurestackadmin@fabrikam.onmicrosoft.com
  ```

- **password**

  The Azure Stack Hub Microsoft Entra admin password.

### Example 2

Request:

```bash
curl -X "POST" "https://login.windows.net/fabrikam.onmicrosoft.com/oauth2/token" \
-H "Content-Type: application/x-www-form-urlencoded" \
--data-urlencode "client_id=1950a258-227b-4e31-a9cf-717495945fc2" \
--data-urlencode "grant_type=password" \
--data-urlencode "username=admin@fabrikam.onmicrosoft.com" \
--data-urlencode 'password=Password12345' \
--data-urlencode "resource=https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155"
```

Response:

```bash
{
  "token_type": "Bearer",
  "scope": "user_impersonation",
  "expires_in": "3599",
  "ext_expires_in": "0",
  "expires_on": "1512574780",
  "not_before": "1512570880",
  "resource": "https://contoso.onmicrosoft.com/4de154de-f8a8-4017-af41-df619da68155",
  "access_token": "eyJ0eXAiOi...truncated for readability..."
}
```

## API queries

Once you get the access token, add it as a header to each of your API requests. To add it as a header, create an **authorization** header with the value: `Bearer <access token>`. For example:

Request:

```bash  
curl -H "Authorization: Bearer eyJ0eXAiOi...truncated for readability..." 'https://adminmanagement.local.azurestack.external/subscriptions?api-version=2016-05-01'
```

Response:

```bash  
offerId : /delegatedProviders/default/offers/92F30E5D-F163-4C58-8F02-F31CFE66C21B
id : /subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8
subscriptionId : 800c4168-3eb1-406b-a4ca-919fe7ee42e8
tenantId : 9fea4606-7c07-4518-9f3f-8de9c52ab628
displayName : Default Provider Subscription
state : Enabled
subscriptionPolicies : @{locationPlacementId=AzureStack}
```

### URL structure and query syntax

Generic request URI, consists of: `{URI-scheme} :// {URI-host} / {resource-path} ? {query-string}`

- **URI scheme**:  
The URI indicates the protocol used to send the request. For example, `http` or `https`.
- **URI host**:  
The host specifies the domain name or IP address of the server where the REST service endpoint is hosted, such as `graph.microsoft.com` or `adminmanagement.local.azurestack.external`.
- **Resource path**:  
The path specifies the resource or resource collection, which may include multiple segments used by the service in determining the selection of those resources. For example: `beta/applications/00003f25-7e1f-4278-9488-efc7bac53c4a/owners` can be used to query the list a specific application's owners within the applications collection.
- **Query string**:  
The string provides additional simple parameters, such as the API version or resource selection criteria.

## Azure Stack Hub request URI construct

```bash
{URI-scheme} :// {URI-host} / {subscription id} / {resource group} / {provider} / {resource-path} ? {OPTIONAL: filter-expression} {MANDATORY: api-version}
```

### URI syntax

```bash
https://adminmanagement.local.azurestack.external/{subscription id}/resourcegroups/{resource group}/providers/{provider}/{resource-path}?{api-version}
```

### Query URI example

```bash
https://adminmanagement.local.azurestack.external/subscriptions/800c4168-3eb1-406b-a4ca-919fe7ee42e8/resourcegroups/system.local/providers/microsoft.infrastructureinsights.admin/regionhealths/local/Alerts?$filter=(Properties/State eq 'Active') and (Properties/Severity eq 'Critical')&$orderby=Properties/CreatedTimestamp desc&api-version=2016-05-01"
```

## Next steps

For more information about using the Azure REST endpoints, see the [Azure REST API reference](/rest/api/).
