---
title: Set up Microsoft Entra ID and storage resources for VaaS
titleSuffix: Azure Stack Hub
description: Learn how to set up Microsoft Entra ID and storage resources for Azure Stack Hub validation as a service.
author: sethmanheim
ms.topic: tutorial
ms.date: 12/16/2020
ms.author: sethm
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018


ROBOTS: NOINDEX

# Intent: As an Azure Stack Hub user, I want to set up Microsoft Entra ID and storage resources for Azure Stack Hub validation as a service.
# Keyword: azure stack hub vaaldation Microsoft Entra storage resources

---


# Tutorial: Set up resources for Validation as a Service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Validation as a service (VaaS) is an Azure service that's used to validate and support Azure Stack Hub solutions in the market. Follow this article before using the service to validate your solution.

In this tutorial, you learn how to:

> [!div class="checklist"]
> * Get ready to use VaaS by setting up your Microsoft Entra ID.
> * Create a storage account.

<a name='configure-an-azure-ad-tenant'></a>

## Configure a Microsoft Entra tenant

A Microsoft Entra tenant is used to register an organization and authenticate users with VaaS. The partner will use the role-based access control (RBAC) features of the tenant to manage who in the partner organization can use VaaS. For more information, see [What is Microsoft Entra ID?](/azure/active-directory/fundamentals/active-directory-whatis).

### Create a tenant

Create a tenant that your organization will use to access VaaS services. Use a descriptive name (for example, `ContosoVaaS@onmicrosoft.com`).

1. Create a Microsoft Entra tenant in the [Azure portal](https://portal.azure.com), or use an existing tenant. <!-- For instructions on creating new Azure AD tenants, see [Get started with Azure AD](/azure/active-directory/get-started-azure-ad). -->

2. Add members of your organization to the tenant. These users will be responsible for using the service to view or schedule tests. Once you finish registration, you'll define users' access levels.

3. (Optional) If you want to isolate VaaS resources and actions among different groups within an organization, you can create multiple Microsoft Entra tenant directories.

### Register your tenant

This process authorizes your tenant with the **Azure Stack Hub Validation Service** Microsoft Entra application.

1. Send the following information about the tenant to Microsoft at [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com).

    | Data | Description |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Organization Name | The official organization name. |
    | Microsoft Entra tenant Directory Name | The Microsoft Entra tenant Directory name being registered. |
    | Microsoft Entra tenant Directory ID | The Microsoft Entra tenant Directory GUID associated with the directory. For information on how to find your Microsoft Entra tenant Directory ID, see [Get tenant ID](/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-values-for-signing-in). |

2. Wait for confirmation from the Azure Stack Hub Validation team to check that your tenant can use the Azure Stack Hub Validation portal.

### Consent to the VaaS app

There are two applications that require admin consents to function properly, one for accessing the VaaS portal and another for starting up the VaaS agents on DVMs. As the Microsoft Entra administrator, give the VaaS Microsoft Entra applications the required permissions on behalf of your tenant.

For portal application consent:

1. Use the global admin credentials for the tenant to sign into the [Azure Stack Hub Validation portal](https://azurestackvalidation.com/).

2. Select **My Account**.

3. Accept the terms to continue when prompted to grant VaaS the listed Microsoft Entra permissions.

For agent application consent:

1. Complete the **{your-tenant-id}** in the following application admin consent URL: `https://login.microsoftonline.com/{your-tenant-id}/adminconsent?client_id=1cb1f8f6-a492-4155-9aaf-59a444e8cd3c`

2. Access the link from the browser.

3. Accept the terms to continue when prompted to grant VaaS the listed Microsoft Entra permissions.

## Assign User Roles

Authorize the users in your tenant to run actions in VaaS by assigning one of the following roles:

| Role Name | Description |
|---------------------|------------------------------------------|
| Owner | Has full access to all resources. |
| Reader | Can view all resources but can't create or manage. |
| Test Contributor | Can create and manage test resources. |

To assign roles for the VaaS service app:

1. Sign in to the [Azure portal](https://portal.azure.com).
2. Select **All Services** > **Microsoft Entra ID** under the **Identity** section.
3. Select **Enterprise Applications** > **AzureStack Validation Management Api [wsfed enabled] [Migrated]** app.
4. Select **Users and groups**. The **AzureStack Validation Management Api [wsfed enabled] [Migrated] - Users and group** blade lists the users with permission to use the app.
5. Select **+ Add user** to add a user from your tenant and assign a role.

> [!NOTE]
> As a best practice, you should define more than one Owner to avoid the necessity of having to create a new tenant if there is an issue with a single owner.

## Create an Azure Storage account

During test execution, VaaS outputs diagnostic logs to an Azure Storage account. In addition to test logs, the storage account may also be used to the upload the OEM extension packages for the Package Validation workflow.

The Azure Storage account is hosted in the Azure public cloud, not on your Azure Stack Hub environment.

1. In the Azure portal, select **All services** > **Storage** > **Storage accounts**. On the **Storage Accounts** blade, select **Add**.

2. Select the subscription in which to create the storage account.

3. Under **Resource group**, select **Create new**. Enter a name for your new resource group.

4. Review the [naming conventions](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#storage) for Azure Storage accounts. Enter a name for your storage account.

5. Select the **US East** region for your storage account.

    To ensure that networking charges aren't incurred for storing logs, the Azure Storage account can be configured to use only the **US East** region. Data replication and the hot storage tier feature aren't necessary for this data. Enabling either feature will dramatically increase your costs.

6. Leave the settings to the default values except for **Account kind**:

    - The **Deployment model** field is set to **Resource Manager** by default.
    - The **Performance** field is set to **Standard** by default.
    - Select **Account kind** field as **Blob storage**.
    - The **Replication field** is set to **Locally-redundant storage (LRS)** by default.
    - The **Access tier** is set to **Hot** by default.

7. Select **Review + Create** to review your storage account settings and create the account.

## Next steps

If your environment doesn't allow inbound connections, follow the tutorial on deploying the local agent to run a test on your hardware.

> [!div class="nextstepaction"]
> [Deploy the local agent](azure-stack-vaas-local-agent.md)
