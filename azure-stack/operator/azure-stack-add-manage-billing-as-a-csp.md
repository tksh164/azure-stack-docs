---
title: Manage Azure Stack Hub usage and billing as a Cloud Solution Provider 
description: Learn how to register Azure Stack Hub as a Cloud Solution Provider (CSP) and add customers for billing.
author: sethmanheim
ms.topic: article
ms.date: 05/07/2024
ms.author: sethm
ms.reviewer: rtiberiu
ms.lastreviewed: 10/15/2019

# Intent: As an Azure Stack operator, I want to manage usage and billing for my users.
# Keyword: azure stack cloud service provider csp

---


# Manage usage and billing for Azure Stack Hub as a Cloud Solution Provider

This article describes how to register Azure Stack Hub as a Cloud Solution Provider (CSP) and how to add customers.

As a CSP, you work with diverse customers using your Azure Stack Hub. Each customer has a CSP subscription in Azure. You must direct usage from your Azure Stack Hub to each user subscription.

The following figure shows the required steps to choose your shared services account, and to register the Azure account with the Azure Stack Hub account. Once registered, you can onboard your end customers:

[![Process for enabling usage and management as a Cloud Solution Provider](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.svg "Process for enabling usage and management as a Cloud Solution Provider")](media/azure-stack-add-manage-billing-as-a-csp/process-add-useage-as-a-csp.svg)

## Create a CSP or APSS subscription

### CSP subscription types

Choose the type of shared services account that you use for Azure Stack Hub. The types of subscriptions that can be used for registration of a multitenant Azure Stack Hub are:

- Cloud Solution Provider
- Azure Partner Shared Services subscription

See this tutorial video to help you understand how to manage your multitenant rights:

> [!VIDEO https://www.youtube.com/embed/ZP6jkbLeS34]

#### Azure Partner Shared Services

Azure Partner Shared Services (APSS) subscriptions are the preferred choice for registration when a direct CSP or a CSP distributor operates Azure Stack Hub.

APSS subscriptions are associated with a shared-services tenant. When you register Azure Stack Hub, you provide credentials for an account that's an owner of the subscription. The account you use to register Azure Stack Hub can be different from the admin account that you use for deployment. Furthermore, the two accounts don't need to belong to the same domain; you can deploy using the tenant that you already use. For example, you can use `ContosoCSP.onmicrosoft.com`, then register using a different tenant; for example, `IURContosoCSP.onmicrosoft.com`. You must remember to sign in using `ContosoCSP.onmicrosoft.com` when you perform daily Azure Stack Hub administration. You sign in to Azure using `IURContosoCSP.onmicrosoft.com` when you need to perform registration operations.

For a description of APSS subscriptions and how to create them, see [Add Azure Partner Shared Services](/partner-center/shared-services).

#### CSP subscriptions

CSP subscriptions are the preferred choice for registration when a CSP reseller or an end customer operates Azure Stack Hub.

Services such as [Azure Site Recovery](azure-site-recovery-overview.md#how-is-site-recovery-billed) only report usage data in the subscription associated with the Azure Stack Hub registration. The CSP that operates the multitenant environment typically managed this subscription. As a result, it's the responsibility of the CSP to accurately determine and bill each of their customers who utilize the Azure Site Recovery service.

## Register Azure Stack Hub

Use the APSS subscription created using the information in the preceding section to register Azure Stack Hub with Azure. For more information, see [Register Azure Stack Hub with your Azure subscription](azure-stack-registration.md).

## Add end customer

To configure Azure Stack Hub so that a new tenant's resource usage is reported to their CSP subscription, see [Add tenant for usage and billing to Azure Stack Hub](azure-stack-csp-howto-register-tenants.md).

## Charge the right subscriptions

Azure Stack Hub uses a feature called *registration*. A registration is an object stored in Azure. The registration object documents which Azure subscriptions to use to charge for a given Azure Stack Hub. This section addresses the importance of registration.

Using registration, Azure Stack Hub can:

- Forward [Azure Stack Hub usage data](azure-stack-billing-and-chargeback.md) to Azure Commerce and bill an Azure subscription.
- Report each customer's usage on a different subscription with a multitenant Azure Stack Hub deployment. Multitenancy enables Azure Stack Hub to support different organizations on the same Azure Stack Hub instance.

For each Azure Stack Hub, there is one default subscription and many tenant subscriptions. The default subscription is an Azure subscription that is charged if there's no tenant-specific subscription. It must be the first subscription to be registered. For multitenant usage reporting to work, the subscription must be a CSP or APSS subscription.

Then, the registration is updated with an Azure subscription for each tenant that uses Azure Stack Hub. Tenant subscriptions must be of the CSP type, and must roll up to the partner who owns the default subscription. You cannot register someone else's customers.

When Azure Stack Hub forwards usage info to global Azure, a service in Azure consults the registration and maps each tenant's usage to the appropriate tenant subscription. If a tenant was not registered, that usage goes to the default subscription for the Azure Stack Hub instance from which it originated.

Because tenant subscriptions are CSP subscriptions, their bill is sent to the CSP partner, and usage info is not visible to the end customer.

## Next steps

- To learn more about the CSP program, see [Cloud Solution Provider program](https://partner.microsoft.com/solutions/microsoft-cloud-solutions).
- To learn more about how to retrieve resource usage info from Azure Stack Hub, see [Usage and billing in Azure Stack Hub](azure-stack-billing-and-chargeback.md).
