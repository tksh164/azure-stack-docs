---
title: Add-ons and extensions available for AKS hybrid
description: Learn about what add-ons and integrations are supported in AKS hybrid
author: sethmanheim
ms.topic: how-to
ms.date: 02/15/2023
ms.author: sethm 
ms.lastreviewed: 02/15/2023
ms.reviewer: baziwane
---

# Add-ons, extensions, and other integrations with AKS hybrid

Azure Kubernetes Service (AKS) hybrid provides augmented functionality for your Kubernetes cluster, mainly through extensions which are supported by Microsoft. There are also many integrations offered by open-source projects and third-party organizations. It is important to note that these integrations, which are not supported by AKS hybrid, are not included in the [AKS hybrid support policy](support-policies.md).

## Add-ons

Add-ons are a fully supported way to provide extra capabilities for your AKS hybrid cluster. The add-ons installation, configuration, and lifecycle are managed by AKS hybrid. For instructions on how to install each add-on, see the available add-ons list.

The following rules are used by AKS hybrid for applying updates to installed add-ons:

- Any breaking or behavior changes to the add-on will be announced well before, usually 60 days in advance of a release.
- Updates to add-ons will be communicated through the release notes accompanying each new release of AKS hybrid.

### Available add-ons

| Add-on                           | Description                                                  |
|--------------------------------------|------------------------------------------------------------------|
| [Install-AksHciAdAuth](reference/ps/install-akshciadauth.md)      | Installs Active Directory authentication.                        |
| [Install-AksHciCsiNfs](reference/ps/install-akshcicsinfs.md)      | Installs the CSI NFS plug-in to a cluster.                       |
| [Install-AksHciCsiSmb](reference/ps/install-akshcicsismb.md)      | Installs the CSI SMB plug-in to a cluster.                       |
| [Install-AksHciGmsaWebhook](reference/ps/install-akshcigmsawebhook.md) | Installs gMSA webhook add-on to the cluster.                     |
| [Install-AksHciMonitoring](reference/ps/install-akshcimonitoring.md)  | Installs Prometheus for monitoring in the AKS hybrid deployment. |

## Azure Arc extensions

Cluster extensions build on top of certain Helm charts and provide an Azure Resource Manager-driven experience for installation and lifecycle
management of different Azure capabilities on top of your Kubernetes cluster. You can [deploy these extensions to your clusters](/azure/azure-arc/kubernetes/extensions) to improve cluster management.

### Available extensions

For more information about the specific cluster extensions for AKS hybrid, see [Currently available extensions](/azure/azure-arc/kubernetes/extensions-release).

## Difference between extensions and add-ons

Both extensions and add-ons are supported ways to add functionality to your AKS hybrid cluster. When you install an add-on, the functionality is added as part of the AKS hybrid deployment. When you install an extension, the functionality is added as part of a separate resource provider in the Azure API.

## Open source and third-party integrations

You can install many open source and third-party integrations on your AKS cluster, but these open-source and third-party integrations are not covered by the [AKS hybrid support policy](support-policies.md).

The following table shows a few examples of open-source and third-party integrations:

| Name                | Description                                                                                           | More details                                                                                                                                                               |
|-------------------------|-----------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Helm](https://helm.sh/)         | An open-source packaging tool that helps you install and manage the lifecycle of Kubernetes applications. | [Quickstart: Develop on Azure Kubernetes Service (AKS) with Helm](/azure/aks/quickstart-helm)                                                                                                     |
| [Istio](https://istio.io/)        | An open-source service mesh.                                                                              | [Istio installation guides](https://istio.io/latest/docs/setup/install/)                                                                                                                                        |
| [Linkerd](https://linkerd.io/)      | An open-source service mesh.                                                                              | [Linkerd getting started](https://linkerd.io/getting-started/)                                                                                                                                             |

## Next steps

- [AKS hybrid overview](aks-hybrid-options-overview.md)
