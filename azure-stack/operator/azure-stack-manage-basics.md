---
title: Azure Stack Hub administration basics 
description: Learn the basics of Azure Stack Hub administration.
author: sethmanheim
ms.topic: article
ms.date: 01/23/2023
ms.author: sethm
ms.lastreviewed: 01/23/2023

# Intent: As an Azure Stack operator, I want to learn the Azure Stack administration basics so I can get my users what they need.
# Keyword: azure stack administration basics

---

# Azure Stack Hub administration basics

If you're new to Azure Stack Hub administration, there are several things you need to know. This article provides an overview of your role as an Azure Stack Hub operator, and what you need to tell your users to help them become productive.

## Understand the builds

### Integrated systems

If you're using an Azure Stack Hub integrated system, update packages distribute updated versions of Azure Stack Hub. You can import these packages and apply them by using the **Updates** tile in the administrator portal.
 
### Development kit

If you're using the Azure Stack Development Kit (ASDK), review [What is Azure Stack Hub?](../asdk/asdk-what-is.md) to learn the purpose and limitations of the ASDK. You can use the ASDK as a *sandbox*, where you can evaluate Azure Stack Hub and develop and test your apps in a non-production environment. For deployment information, see [Azure Stack Development Kit deployment](../asdk/asdk-install.md).

Like Azure, we innovate rapidly. We'll regularly release new builds. If you're running the ASDK and you want to move to the latest build, you must [redeploy Azure Stack Hub](../asdk/asdk-redeploy.md). You can't apply update packages. This process takes time, but the benefit is that you can try out the latest features. The ASDK documentation on our website reflects the latest release build.

## Learn about available services

You'll need an awareness of which services you can make available to your users. Azure Stack Hub supports a subset of Azure services. The list of supported services will continue to evolve.

**Foundational services**

By default, Azure Stack Hub includes the following "foundational services" when you deploy Azure Stack Hub:

- Compute
- Storage
- Networking
- Key Vault

With these foundational services, you can offer Infrastructure-as-a-Service (IaaS) to your users with minimal configuration.

**Additional services**

Currently, we support the following additional Platform-as-a-Service (PaaS) services:

- App Service
- Azure Functions
- SQL and MySQL databases
- Event Hubs
- Kubernetes (in preview)

These services require additional configuration before you can make them available to your users. For more information, see the "Tutorials" and the "How-to guides\Offer services" sections of our Azure Stack Hub operator documentation.

**Service roadmap**

Azure Stack Hub will continue to add support for Azure services. For the projected roadmap, see the [Azure Stack Hub: An extension of Azure](https://azure.microsoft.com/resources/videos/azure-friday-azure-stack-an-extension-of-azure/) whitepaper. You can also monitor the [Azure Stack Hub blog posts](https://techcommunity.microsoft.com/t5/azure-stack-blog/bg-p/AzureStackBlog) for new announcements.

## What account should I use?

There are a few account considerations to be aware of when managing Azure Stack Hub. Especially in deployments using Windows Server Active Directory Federation Services (AD FS) as the identity provider instead of Microsoft Entra ID. The following account considerations apply to both Azure Stack Hub integrated systems and ASDK deployments:

|Account|Microsoft Entra ID|AD FS|
|-----|-----|-----|
|Local Administrator (.\Administrator)|ASDK host administrator.|ASDK host administrator.|
|AzureStack\AzureStackAdmin|ASDK host administrator.<br><br>Can be used to sign in to the Azure Stack Hub administrator portal.<br><br>Access to view and administer Service Fabric rings.|ASDK host administrator.<br><br>No access to the Azure Stack Hub administrator portal.<br><br>Access to view and administer Service Fabric rings.<br><br>No longer owner of the Default Provider Subscription (DPS).|
|AzureStack\CloudAdmin|Can access and run permitted commands within the privileged endpoint.|Can access and run permitted commands within the privileged endpoint.<br><br>Can't sign in to the ASDK host.<br><br>Owner of the Default Provider Subscription (DPS).|
|Microsoft Entra Global Administrator|Used during installation.<br><br>Owner of the Default Provider Subscription (DPS).|Not applicable.|
|

[!INCLUDE [CloudAdmin backup account warning](../includes/warning-cloud-admin-backup-account.md)]

## What tools do I use to manage?
 
You can use the [administrator portal](azure-stack-manage-portals.md) or PowerShell to manage Azure Stack Hub. The easiest way to learn the basic concepts is through the portal. If you want to use PowerShell, there are preparation steps. Before you get started, you might want to get familiar with how PowerShell is used on Azure Stack Hub. For more information, see [Get started with PowerShell on Azure Stack Hub](../user/azure-stack-powershell-overview.md).

Azure Stack Hub uses Azure Resource Manager as its underlying deployment, management, and organization mechanism. If you're going to manage Azure Stack Hub and help support users, you can learn about Resource Manager. See the [Getting Started with Azure Resource Manager](https://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf) whitepaper.

## Your typical responsibilities

Your users want to use services. From their perspective, your main role is to make these services available to them. Decide which services to offer, and make those services available by creating plans, offers, and quotas. For more information, see [Overview of offering services in Azure Stack Hub](service-plan-offer-subscription-overview.md). 

You'll also need to add items to [Azure Stack Hub Marketplace](azure-stack-marketplace.md). The easiest way is to [download marketplace items from Azure to Azure Stack Hub](azure-stack-download-azure-marketplace-item.md).

> [!NOTE]
> If you want to test your plans, offers, and services, you can use the [user portal](azure-stack-manage-portals.md); not the administrator portal.

In addition to providing services, you must do the regular duties of an operator to keep Azure Stack Hub up and running. These duties include the following tasks:

- Add user accounts (for [Microsoft Entra ID](azure-stack-add-new-user-aad.md) deployment or for [AD FS](azure-stack-add-users-adfs.md) deployment).
- [Assign role-based access control (RBAC) roles](azure-stack-manage-permissions.md) (This task isn't restricted to admins.)
- [Monitor infrastructure health](azure-stack-monitor-health.md).
- Manage [network](azure-stack-viewing-public-ip-address-consumption.md) and [storage](azure-stack-manage-storage-accounts.md) resources.
- Replace bad hardware. For example, [replace a failed disk](azure-stack-replace-disk.md).

## Operator tasks

Here is a list of daily, weekly, and monthly tasks for an operator:

# [Daily](#tab/daily)

1. Check alerts.
2. Check backup state.
3. Update Defender Signature (disconnected systems).

# [Weekly](#tab/weekly)

1. Check capacity.

# [Monthly](#tab/monthly)

1. Apply monthly update packages (Microsoft & OEM).
2. Validate backup using ASDK.
3. Manage Azure Stack Hub Marketplace (keep current).
4. Reclaim storage capacity.

# [OnDemand](#tab/ondemand)

1. Secret rotation.
2. Create and update offers, plans, and quotas.
3. Apply hotfix packages.
4. Expand capacity (nodes & IPSpace).
5. Restore storage accounts.
6. Stop system.
7. Diagnostic log collection.

---

## What to tell your users

You'll need to let your users know how to work with services in Azure Stack Hub, how to connect to the environment, and how to subscribe to offers. Besides any custom documentation that you may want to provide your users, you can direct users to [Azure Stack Hub User Documentation](../user/index.yml).

**Understand how to work with services in Azure Stack Hub**

There's information your users must understand before they use services and build apps in Azure Stack Hub. For example, there are specific PowerShell and API version requirements. Also, there are some feature deltas between a service in Azure and the equivalent service in Azure Stack Hub. Make sure that your users review the following articles:

- [Key considerations: Using services or building apps for Azure Stack Hub](../user/azure-stack-considerations.md)
- [Considerations for Virtual Machines in Azure Stack Hub](../user/azure-stack-vm-considerations.md)
- [Storage: differences and considerations](../user/azure-stack-acs-differences.md)

The information in these articles summarizes the differences between a service in Azure and Azure Stack Hub. It supplements the information that's available for an Azure service in the global Azure documentation.

**Connect to Azure Stack Hub as a user**

In an ASDK environment, if a user doesn't use Remote Desktop to connect to the ASDK host, they can configure a virtual private network (VPN) connection to connect to Azure Stack Hub. See [Connect to Azure Stack Hub](../asdk/asdk-connect.md).

Your users will want to know how to [access the user portal](../user/azure-stack-use-portal.md) or how to connect through PowerShell. In an integrated systems environment, the user portal address varies per deployment. You'll need to provide your users with the correct URL.

If using PowerShell, users may have to register resource providers before they can use services. A resource provider manages a service. For example, the networking resource provider manages resources like virtual networks, network interfaces, and load balancers. They must [install](powershell-install-az-module.md) PowerShell, [download](azure-stack-powershell-download.md) additional modules, and [configure](../user/azure-stack-powershell-configure-user.md) PowerShell (which includes resource provider registration).

**Subscribe to an offer**

Before a user can use services, they must [subscribe to an offer](azure-stack-subscribe-plan-provision-vm.md) that you've created as an operator.

## Where to get support

> [!Note]  
> To find support information for earlier releases of Azure Stack Hub, see [Help and Support for earlier releases Azure Stack Hub](azure-stack-servicing-policy.md).

### Integrated systems

For an integrated system, there's a coordinated escalation and resolution process between Microsoft and our original equipment manufacturer (OEM) hardware partners.

If there's a cloud services issue, support is offered through Microsoft Support. To open a support request, select the help and support icon (question mark) in the upper-right corner of the administrator portal. Then select **Help + support** and then **New support request** under the **Support** section.

If there's an issue with deployment, patch and update, hardware (including field replaceable units), or any hardware-branded software, like software running on the hardware lifecycle host, contact your OEM hardware vendor first.

For anything else, contact Microsoft Support.

### Azure Stack Development Kit (ASDK)

For the ASDK, you can ask support-related questions in the [Microsoft forums](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). To get to the forums, select the Help and support icon (question mark) in the upper-right corner of the administrator portal, then select **Help + support**, and then select **MSDN Forums** under the **Support** section. These forums are regularly monitored. Because the ASDK is an evaluation environment, there's no official support offered through Microsoft Support.

## Next steps

[Region management in Azure Stack Hub](azure-stack-region-management.md)
