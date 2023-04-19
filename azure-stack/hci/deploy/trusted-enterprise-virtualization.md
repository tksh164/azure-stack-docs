---
title: Deploy trusted enterprise virtualization on Azure Stack HCI
description: This topic provides guidance on how to plan, configure, and deploy a highly secure infrastructure that uses trusted enterprise virtualization on the Azure Stack HCI operating system.
author: hsuzuki
ms.author: hsuzuki
ms.topic: how-to
ms.date: 04/17/2023
---

# Deploy trusted enterprise virtualization on Azure Stack HCI

[!INCLUDE [applies-to](../../includes/hci-applies-to-22h2-21h2.md)]

This topic provides guidance on how to plan, configure, and deploy a highly secure infrastructure that uses trusted enterprise virtualization on the Azure Stack HCI operating system. Leverage your Azure Stack HCI investment to run secure workloads on hardware that uses [virtualization-based security (VBS)](/windows-hardware/design/device-experiences/oem-vbs) and hybrid cloud services through Windows Admin Center and the Azure portal.

## Overview
VBS is a key component of the [security investments in Azure Stack HCI](/windows-server/get-started-19/whats-new-19#security) to protect hosts and virtual machines (VMs) from security threats. For example, the [Security Technical Implementation Guide (STIG)](https://ncp.nist.gov/checklist/914), which is published as a tool to improve the security of Department of Defense (DoD) information systems, lists VBS and [Hypervisor-Protected Code Integrity (HVCI)](/windows-hardware/drivers/bringup/device-guard-and-credential-guard) as general security requirements. It is imperative to use host hardware that is enabled for VBS and HVCI to protect workloads on VMs, because a compromised host cannot guarantee VM protection.

VBS uses hardware virtualization features to create and isolate a secure region of memory from the operating system. You can use [Virtual Secure Mode (VSM)](/virtualization/hyper-v-on-windows/tlfs/vsm) in Windows to host a number of security solutions to greatly increase protection from operating system vulnerabilities and malicious exploits.

VBS uses the Windows hypervisor to create and manage security boundaries in operating system software, enforce restrictions to protect vital system resources, and protect security assets, such as authenticated user credentials. With VBS, even if malware gains access to the operating system kernel, you can greatly limit and contain possible exploits, because the hypervisor prevents malware from executing code or accessing platform secrets.

The hypervisor, the most privileged level of system software, sets and enforces page permissions across all system memory. While in VSM, pages can only execute after passing code integrity checks. Even if a vulnerability, such as a buffer overflow that could allow malware to attempt to modify memory occurs, code pages cannot be modified, and modified memory cannot be executed. VBS and HVCI significantly strengthen code integrity policy enforcement. All kernel mode drivers and binaries are checked before they can start, and unsigned drivers or system files are prevented from loading into system memory.

## Deploy trusted enterprise virtualization
This section describes at a high level how to acquire hardware to deploy a highly secure infrastructure that uses trusted enterprise virtualization on Azure Stack HCI and Windows Admin Center for management.

### Step 1: Acquire hardware for trusted enterprise virtualization on Azure Stack HCI
First, you'll need to procure hardware. The easiest way to do that is to locate your preferred Microsoft hardware partner in the [Azure Stack HCI Catalog](https://aka.ms/AzureStackHCICatalog) and purchase an integrated system with the Azure Stack HCI operating system preinstalled. In the catalog, you can filter to see vendor hardware that is optimized for this type of workload.

Otherwise, you'll need to deploy the Azure Stack HCI operating system on your own hardware. For details on Azure Stack HCI deployment options and installing Windows Admin Center, see [Deploy the Azure Stack HCI operating system](./operating-system.md).

Next, use Windows Admin Center to [create an Azure Stack HCI cluster](./create-cluster.md).

All partner hardware for Azure Stack HCI is certified with the Hardware Assurance Additional Qualification. The qualification process tests for all required [VBS](/windows-hardware/design/device-experiences/oem-vbs) functionality. However, VBS and HVCI are not automatically enabled in Azure Stack HCI. For more information about the the Hardware Assurance Additional Qualification, see "Hardware Assurance" under **Systems** in the [Windows Server Catalog](https://www.windowsservercatalog.com/content.aspx?ctf=AQinfo-systems.htm#:~:text=Hardware%20Assurance%20Windows%20Server%20systems%20that%20are%20awarded,of%20Windows%20Server%2C%20starting%20with%20Windows%20Server%202016).

   >[!WARNING]
   > HVCI may be incompatible with hardware devices not listed in the Azure Stack HCI Catalog. We strongly recommend using Azure Stack HCI validated hardware from our partners for trusted enterprise virtualization infrastructure.

### Step 2: Enable HVCI
Enable HVCI on your server hardware and VMs. For details, see [Enable virtualization-based protection of code integrity](/windows/security/threat-protection/device-guard/enable-virtualization-based-protection-of-code-integrity).

### Step 3: Set up Azure Security Center in Windows Admin Center
In Windows Admin Center, set up [Azure Security Center](/azure/security-center/security-center-introduction) to add threat protection and quickly assess the security posture of your workloads.

To learn more, see [Protect Windows Admin Center resources with Security Center](/azure/security-center/windows-admin-center-integration).

To get started with Security Center:
- You need a subscription to Microsoft Azure. If you don’t have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/free).
- Security Center's free pricing tier is enabled on all your current Azure subscriptions once you either visit the Azure Security Center dashboard in the Azure portal, or enable it programmatically via API.
To take advantage of advanced security management and threat detection capabilities, you must enable Azure Defender. You can use Azure Defender free for 30 days. For more information, see [Security Center pricing](https://azure.microsoft.com/pricing/details/security-center).
- If you're ready to enable Azure Defender, see [Quickstart: Setting up Azure Security Center](/azure/security-center/security-center-get-started) to walk through the steps.

You can also use Windows Admin Center to set up additional Azure hybrid services, such as Backup, File Sync, Site Recovery, Point-to-Site VPN, and Update Management.

## Next steps
For more information related to trusted enterprise virtualization, see:
- [Hyper-V on Windows Server](/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)