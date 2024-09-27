---
title: What's in Azure Stack HCI, 2306.1 Supplemental Package and preview channel (preview)
description: Preview features and OS versions available via preview channel and 2306.1 supplemental package features.
author: alkohli
ms.author: alkohli
ms.topic: overview
ms.service: azure-stack
ms.subservice: azure-stack-hci
ms.date: 11/30/2023
---

# What's in preview for Azure Stack HCI, 2306.1 release (preview)

> Applies to: Azure Stack HCI preview channel and Supplemental Package

This article describes the new features or enhancements that are currently available in the preview for Azure Stack HCI. This article includes:

- [What's in the preview channel](#azure-stack-hci-preview-channel).
- [What's in the Azure Stack HCI, Supplemental Package](#azure-stack-hci-23061-supplemental-package-preview).

## Azure Stack HCI preview channel

The Azure Stack HCI preview channel features preview versions of Azure Stack HCI OS release. For more information, see [Join the preview channel](./preview-channel.md).

## Azure Stack HCI, 2306.1 Supplemental Package (preview)

Azure Stack HCI, 2306.1 Supplemental Package is now in preview. You can deploy this package on servers running the English version of the Azure Stack HCI, version 22H2 OS. For more information on Azure Stack HCI, version 22H2, see [What's new in 22H2](../whats-new-in-hci-22h2.md).

[!INCLUDE [hci-deployment-tool-sp](../../includes/hci-deployment-tool-sp-2306.md)]


To learn more about the new deployment methods, see [Deployment overview](../deploy/deployment-tool-introduction.md).


### What's new

This release includes the Azure Stack HCI, version 22H2 operating system refreshed to include the latest cumulative update corresponding to July 2023.

- **ISO refresh** - in this release, the ISO for the installation of Azure Stack HCI, version 22H2 operating system is refreshed to include the latest cumulative update corresponding to July 2023.

- **Bug fix** - this release contains a bug fix for an issue that prevented the health service from starting when Azure Monitor Stack HCI insights was enabled.

For more information on the July release, see [Azure Stack HCI, version 22H2 OS build 20349.1850](../release-information.md#azure-stack-hci-version-22h2-os-build-20349).

> [!NOTE]
> The Supplemental Package supports only the English version of the Azure Stack HCI OS. Make sure to download the English version and use the refreshed ISO. For more information on the July release, see [Azure Stack HCI, version 22H2 OS build 20349.1850](../release-information.md#azure-stack-hci-version-22h2-os-build-20349).

## Known issues

To review a list of the known issues for this release, see [View known issues in Azure Stack HCI, 2306.1 Supplemental Package release (preview)](../hci-known-issues-2306-1.md).

## Next steps

- [Join the preview channel](./preview-channel.md) and [install a preview version of Azure Stack HCI](./install-preview-version.md).

- For new Azure Stack HCI deployments via supplemental package:
    - Read the [Deployment overview](../deploy/deployment-tool-introduction.md).
    - Learn how to [Deploy interactively](../deploy/deployment-tool-new-file.md) using the Azure Stack HCI, Supplemental Package.