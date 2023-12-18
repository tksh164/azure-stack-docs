---
title: Common workflow parameters in VaaS
titleSuffix: Azure Stack Hub
description: Learn about common workflow parameters for Azure Stack Hub validation as a service.
author: sethmanheim
ms.topic: article
ms.date: 12/16/2020
ms.author: sethm
ms.reviewer: johnhas
ms.lastreviewed: 11/11/2019


ROBOTS: NOINDEX

# Intent: As an Azure Stack Hub user, I want to learn about workflow common parameters for Azure Stack Hub validation as a service.
# Keyword: azure stack hub vaas workflow parameters

---


# Workflow common parameters for Azure Stack Hub Validation as a Service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Common parameters include values like environment variables and user credentials required by all tests in validation as a service (VaaS). These values are defined at the workflow level when you create or change a workflow. When scheduling tests, these values are passed as parameters to each test under the workflow.

> [!NOTE]
> Each test defines its own set of parameters. At scheduling time, a test may require you to enter a value independently of the common parameters or may allow you to override the common parameter value.

## Environment parameters

Environment parameters describe the Azure Stack Hub environment under test. These values must be provided by generating and uploading an Azure Stack Hub stamp information file for the specific instance you're testing.

> [!NOTE]
> In official validation workflows, environment parameters can't be modified after workflow creation.

### Generate the stamp information file

1. Sign in to the DVM or any machine that has access to the Azure Stack Hub environment.
2. Run the following commands in an elevated PowerShell window:

    ```powershell  
    $CloudAdminUser = "<cloud admin username>"
    $CloudAdminPassword = ConvertTo-SecureString "<cloud admin password>" -AsPlainText -Force
    $stampInfoCreds = New-Object System.Management.Automation.PSCredential($CloudAdminUser, $CloudAdminPassword)
    $params = Invoke-RestMethod -Method Get -Uri 'https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation' -Credential $stampInfoCreds
    ConvertTo-Json $params > stampinfoproperties.json
    ```

### Locate values in the ECE configuration file

Environment parameter values can also be manually located in the **ECE configuration file** located at `C:\EceStore\403314e1-d945-9558-fad2-42ba21985248\80e0921f-56b5-17d3-29f5-cd41bf862787` on the DVM.

## Test parameters

Common test parameters include sensitive information that can't be stored in configuration files. These parameters must be manually provided.

Parameter    | Description
-------------|-----------------
Tenant Administrator User                            | Microsoft Entra tenant admin that was provisioned by the service administrator in the Microsoft Entra directory. This user performs tenant-level actions like deploying templates to set up resources (VMs, storage accounts, and so on.) and executing workloads. For details on provisioning the tenant account, see [Add a new Azure Stack Hub tenant](../operator/azure-stack-add-new-user-aad.md).
Service Administrator User             | Microsoft Entra admin of the Microsoft Entra directory tenant specified during Azure Stack Hub deployment. Search for `AADTenant` in the ECE configuration file and select the value in the `UniqueName` element.
Cloud Administrator User               | Azure Stack Hub domain admin account (for example, `contoso\cloudadmin`). Search for `User Role="CloudAdmin"` in the ECE configuration file and select the value in the `UserName` element.
Diagnostics Connection String          | A SAS URL to an Azure Storage Account to which diagnostics logs will be copied during test execution. For instructions on generating the SAS URL, see [Generate the diagnostics connection string](#generate-the-diagnostics-connection-string). |

> [!IMPORTANT]
> The **diagnostics connection string** must be valid before proceeding.

### Generate the diagnostics connection string

The diagnostics connection string is required for storing diagnostics logs during test execution. Use the Azure Storage Account created during setup (see [Set up your validation as a service resources](azure-stack-vaas-set-up-resources.md)) to create a shared access signature (SAS) URL to give VaaS access to upload logs to your storage account.

1. [!INCLUDE [azure-stack-vaas-sas-step_navigate](includes/azure-stack-vaas-sas-step_navigate.md)]

1. Select **Blob** from **Allowed Services options**. Deselect any remaining options.

1. Select **Service**, **Container**, and **Object** from **Allowed resource types**.

1. Select **Read**, **Write**, **List**, **Add**, **Create** from **Allowed permissions**. Deselect any remaining options.

1. Set **Start time** to the current time, and **End time** to three months from the current time.

1. [!INCLUDE [azure-stack-vaas-sas-step_generate](includes/azure-stack-vaas-sas-step_generate.md)]

> [!NOTE]  
> The SAS URL expires at the end time specified when the URL was generated. When scheduling tests, ensure that the URL is valid for at least 30 days plus the time required for test execution (three months is suggested).

## Next steps

- Learn about [Validation as a Service key concepts](azure-stack-vaas-key-concepts.md)
