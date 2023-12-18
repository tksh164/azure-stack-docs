---
title: Deploy the local agent
titleSuffix: Azure Stack Hub
description: Learn how to deploy the local agent for Azure Stack Hub validation as a service.
author: sethmanheim
ms.topic: quickstart
ms.date: 12/16/2020
ms.author: sethm
ms.reviewer: johnhas
ms.lastreviewed: 11/11/2019


ROBOTS: NOINDEX

# Intent: As an Azure Stack Hub user, I want to learn how to deploy the local agent for Azure Stack Hub validation as a service.
# Keyword: azure stack hub deploy local agent

ms.custom: mode-api
---


# Deploy the local agent

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Learn how to use the validation as a service (VaaS) local agent to run validation tests. The local agent must be deployed before running validation tests.

> [!Note]  
> Make sure that the machine on which the local agent is running doesn't lose outbound access to the internet. This machine should be accessible only to users who are authorized to use VaaS on behalf of your tenant.

To deploy the local agent:

1. Download and install the local agent.
2. Perform sanity checks before starting the tests.
3. Run the local agent.

## Download and start the local agent

Download the agent to a machine that meets the prerequisites in your datacenter and has access to all the Azure Stack Hub endpoints. This machine shouldn't be part of the Azure Stack Hub system or hosted in the Azure Stack Hub cloud.

### Machine prerequisites

Check that your machine meets the following criteria:

- Access to all Azure Stack Hub endpoints.
- .NET 4.6 and PowerShell 5.0 installed.
- At least 8-GB RAM.
- Minimum 8 core processors.
- Minimum 200-GB disk space.
- Stable network connectivity to the internet.

### Download and install the local agent

1. Open Windows PowerShell in an elevated prompt on the machine you'll use to run the tests.
2. Run the following command to download and install the local agent dependencies and copy the public image repository (PIR) images (OS VHD) to your Azure Stack Hub environment.

    ```powershell
    # Review and update the following five parameters
    $RootFolder = "c:\VaaS"
    $CloudAdmindUserName = "<Cloud admin user name>"
    $CloudAdminPassword = "<Cloud admin password>"
    $AadServiceAdminUserName = "<AAD service admin user name>"
    $AadServiceAdminPassword = "<AAD service admin password>"

    if (-not(Test-Path($RootFolder))) {
        mkdir $RootFolder
    }
    Set-Location $RootFolder
    Invoke-WebRequest -Uri "https://storage.azurestackvalidation.com/packages/Microsoft.VaaSOnPrem.TaskEngineHost.latest.nupkg" -outfile "$rootFolder\OnPremAgent.zip"
    Expand-Archive -Path "$rootFolder\OnPremAgent.zip" -DestinationPath "$rootFolder\VaaSOnPremAgent" -Force
    Set-Location "$rootFolder\VaaSOnPremAgent\lib\net46"

    $cloudAdminCredential = New-Object System.Management.Automation.PSCredential($cloudAdmindUserName, (ConvertTo-SecureString $cloudAdminPassword -AsPlainText -Force))
    $getStampInfoUri = "https://ASAppGateway:4443/ServiceTypeId/4dde37cc-6ee0-4d75-9444-7061e156507f/CloudDefinition/GetStampInformation" 
    $stampInfo = Invoke-RestMethod -Method Get -Uri $getStampInfoUri -Credential $cloudAdminCredential -ErrorAction Stop
    $serviceAdminCreds = New-Object System.Management.Automation.PSCredential $aadServiceAdminUserName, (ConvertTo-SecureString $aadServiceAdminPassword -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $stampInfo.AADTenantID `
                            -ServiceAdminCreds $serviceAdminCreds `
                            -ArmEndpoint $stampInfo.AdminExternalEndpoints.AdminResourceManager `
                            -Region $stampInfo.RegionName
    ```

> [!Note]  
> `Install-VaaSPrerequisites` cmdlet downloads large VM image files. If you're experiencing slow network speed, you can download files to your local file server and manually add VM images to your test environemnt. For more information, see [Handle slow network connectivity](azure-stack-vaas-troubleshoot.md#handle-slow-network-connectivity).

**Parameters**

| Parameter | Description |
| --- | --- |
| `AadServiceAdminUser` | The global admin user for your Microsoft Entra tenant. For example: vaasadmin@contoso.onmicrosoft.com. |
| `AadServiceAdminPassword` | The password for the global admin user. |
| `CloudAdminUserName` | The cloud admin user who can access and run permitted commands within the privileged endpoint. For example: AzusreStack\CloudAdmin. For more information, see [Workflow common parameters for VaaS](azure-stack-vaas-parameters.md). |
| `CloudAdminPassword` | The password for cloud admin account.|

![Download prerequisites for local agent](media/installing-prereqs.png)

## Perform sanity checks before starting the tests

The tests run remote operations. The machine that runs the tests must have access to the Azure Stack Hub endpoints, otherwise the tests won't work. If you're using the VaaS local agent, use the machine where the agent will run. You can check that your machine has access to the Azure Stack Hub endpoints by running the following checks:

1. Check that the Base URI can be reached. Open a CMD prompt or bash shell and run the following command, replacing `<EXTERNALFQDN>` with the external fully qualified domain name (FQDN) of your environment:

    ```bash
    nslookup adminmanagement.<EXTERNALFQDN>
    ```

2. Open a browser and go to `https://adminportal.<EXTERNALFQDN>` in order to check that the MAS Portal can be reached.

3. Sign in using the Microsoft Entra service administrator name and password values provided when creating the test pass.

4. Check the system's health by running the **Test-AzureStack** PowerShell cmdlet as described in [Run a validation test for Azure Stack Hub](../operator/azure-stack-diagnostic-test.md). Fix any warnings and errors before launching any tests.

## Run the local agent

1. Open Windows PowerShell in an elevated prompt.

2. Run the following command:

    ```powershell
   # Review and update the following five parameters
    $RootFolder = "c:\VAAS"
    $CloudAdmindUserName = "<Cloud admin user name>"
    $CloudAdminPassword = "<Cloud admin password>"
    $VaaSUserId = "<VaaS user ID>"
    $VaaSTenantId = "<VaaS tenant ID>"

    Set-Location "$rootFolder\VaaSOnPremAgent\lib\net46"
    .\Microsoft.VaaSOnPrem.TaskEngineHost.exe -u $VaaSUserId -t $VaaSTenantId -x $CloudAdmindUserName -y $CloudAdminPassword
    ```

      **Parameters**  

    | Parameter | Description |
    | --- | --- |
    | `CloudAdminUserName` | The cloud admin user who can access and run permitted commands within the privileged endpoint. For example: AzusreStack\CloudAdmin. See [Workflow common parameters for VaaS](azure-stack-vaas-parameters.md) for more information. |
    | `CloudAdminPassword` | The password for cloud admin account.|
    | `VaaSUserId` | User ID used to sign in to the Azure Stack Hub Validation portal. For example: UserName\@Contoso.com). |
    | `VaaSTenantId` | Azure AD tenant ID for the Azure account registered with validation as a service. |

    > [!Note]  
    > When you run the agent, the current working directory must be the location of the task engine host executable, **Microsoft.VaaSOnPrem.TaskEngineHost.exe.**

If you don't see any errors reported, then the local agent has succeeded. The following example text appears on the console window.

`Heartbeat was sent successfully.`

![Started agent](media/started-agent.png)

An agent is uniquely identified by its name. By default, it uses the FQDN name of the machine from where it was started. You must minimize the window to avoid any accidental selects on the window as changing the focus pauses all other actions.

## Next steps

- [Troubleshoot Validation as a Service](azure-stack-vaas-troubleshoot.md)
- [Validation as a Service key concepts](azure-stack-vaas-key-concepts.md)
- [Quickstart: Use the Azure Stack Hub Validation portal to schedule your first test](azure-stack-vaas-schedule-test-pass.md)
