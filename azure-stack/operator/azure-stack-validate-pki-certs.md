---
title: Validate Azure Stack Hub PKI certificates
titleSuffix: Azure Stack Hub
description: Learn how to validate PKI certificates for Azure Stack Hub integrated systems using the Azure Stack Hub Readiness Checker tool.
services: azure-stack
documentationcenter: ''
author: sethmanheim
ms.topic: how-to
ms.date:  04/04/2023
ms.author: sethm
ms.reviewer: ppacent
ms.lastreviewed: 06/07/2021

# Intent: As an Azure Stack Hub operator, I want to validate PKI certificates for Azure Stack Hub integrated systems using the Readiness Checker tool.
# Keyword: azure stack hub validate pki certificates readiness checker

---


# Validate Azure Stack Hub PKI certificates

The Azure Stack Hub Readiness Checker tool described in this article is available [from the PowerShell Gallery](https://aka.ms/AzsReadinessChecker). Use the tool to validate that [generated public key infrastructure (PKI) certificates](azure-stack-get-pki-certs.md) are suitable for pre-deployment. Validate certificates by leaving  enough time to test and reissue certificates if necessary.

The Readiness Checker tool performs the following certificate validations:

- **Parse PFX**  
    Checks for valid PFX file, correct password, and whether the public information is protected by the password.
- **Expiry Date**  
    Checks for minimum validity of seven days.
- **Signature algorithm**  
    Checks that the signature algorithm isn't SHA1.
- **Private Key**  
    Checks that the private key is present and is exported with the local machine attribute.
- **Cert chain**  
    Checks certificate chain is intact including a check for self-signed certificates.
- **DNS names**  
    Checks the SAN contains relevant DNS names for each endpoint or if a supporting wildcard is present.
- **Key usage**  
    Checks if the key usage contains a digital signature and key encipherment and checks if enhanced key usage contains server authentication and client authentication.
- **Key size**  
    Checks if the key size is 2048 or larger.
- **Chain order**  
    Checks the order of the other certificates validating that the order is correct.
- **Other certificates**  
    Ensure no other certificates have been packaged in PFX other than the relevant leaf certificate and its chain.

> [!IMPORTANT]  
> The PKI certificate is a PFX file and password should be treated as sensitive information.

## Prerequisites

Your system should meet the following prerequisites before validating PKI certificates for an Azure Stack Hub deployment:

- Microsoft Azure Stack Hub Readiness Checker.
- SSL Certificate(s) exported following the [preparation instructions](azure-stack-prepare-pki-certs.md).
- DeploymentData.json.
- Windows 10 or Windows Server 2016.

## Perform core services certificate validation

Use these steps to validate the Azure Stack Hub PKI certificates for deployment and secret rotation:

1. Install **AzsReadinessChecker** from a PowerShell prompt (5.1 or above) by running the following cmdlet:

    ```powershell  
    Install-Module Microsoft.AzureStack.ReadinessChecker -Force -AllowPrerelease
    ```

2. Create the certificate directory structure. In the example below, you can change `<C:\Certificates\Deployment>` to a new directory path of your choice.

    ```powershell  
    New-Item C:\Certificates\Deployment -ItemType Directory
    
    $directories = 'ACSBlob', 'ACSQueue', 'ACSTable', 'Admin Extension Host', 'Admin Portal', 'ARM Admin', 'ARM Public', 'KeyVault', 'KeyVaultInternal', 'Public Extension Host', 'Public Portal'
    
    $destination = 'C:\Certificates\Deployment'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```

    > [!NOTE]  
    > AD FS and Graph are required if you're using AD FS as your identity system. For example:
    >
    > ```powershell  
    > $directories = 'ACSBlob', 'ACSQueue', 'ACSTable', 'ADFS', 'Admin Extension Host', 'Admin Portal', 'ARM Admin', 'ARM Public', 'Graph', 'KeyVault', 'KeyVaultInternal', 'Public Extension Host', 'Public Portal'
    > ```

     - Place your certificate(s) in the appropriate directories created in the previous step. For example:  
        - `C:\Certificates\Deployment\ACSBlob\CustomerCertificate.pfx`
        - `C:\Certificates\Deployment\Admin Portal\CustomerCertificate.pfx`
        - `C:\Certificates\Deployment\ARM Admin\CustomerCertificate.pfx`

3. In the PowerShell window, change the values of `RegionName`, `FQDN` and `IdentitySystem` appropriate to the Azure Stack Hub environment and run the following cmdlet:

    ```powershell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 
    Invoke-AzsHubDeploymentCertificateValidation -CertificatePath C:\Certificates\Deployment -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD  
    ```

4. Check the output and ensure that all certificates pass all tests. For example:

    ```shell
    Invoke-AzsHubDeploymentCertificateValidation v1.2005.1286.272 started.
    Testing: KeyVaultInternal\KeyVaultInternal.pfx
    Thumbprint: E86699****************************4617D6
        PFX Encryption: OK
        Expiry Date: OK
        Signature Algorithm: OK
        DNS Names: OK
        Key Usage: OK
        Key Length: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: ARM Public\ARMPublic.pfx
    Thumbprint: 8DC4D9****************************69DBAA
        PFX Encryption: OK
        Expiry Date: OK
        Signature Algorithm: OK
        DNS Names: OK
        Key Usage: OK
        Key Length: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: Admin Portal\AdminPortal.pfx
    Thumbprint: 6F9055****************************4AC0EA
        PFX Encryption: OK
        Expiry Date: OK
        Signature Algorithm: OK
        DNS Names: OK
        Key Usage: OK
        Key Length: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        Chain Order: OK
        Other Certificates: OK
    Testing: Public Portal\PublicPortal.pfx


    Log location (contains PII): C:\Users\[*redacted*]\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    Report location (contains PII): C:\Users\[*redacted*]\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
    Invoke-AzsHubDeploymentCertificateValidation Completed
    ```

    To validate certificates for other Azure Stack Hub services, change the value for `-CertificatePath`. For example:

    ```powershell  
    # App Services
    Invoke-AzsHubAppServicesCertificateValidation -CertificatePath C:\Certificates\AppServices -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com

    # DBAdapter
    Invoke-AzsHubDBAdapterCertificateValidation -CertificatePath C:\Certificates\DBAdapter -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com

    # EventHubs
    Invoke-AzsHubEventHubsCertificateValidation -CertificatePath C:\Certificates\EventHubs -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com
    ```

    Each folder should contain a single PFX file for the certificate type. If a certificate type has multi-certificate requirements, nested folders for each individual certificate are expected and name-sensitive. The following code shows an example folder/certificate structure for all certificate types, and the appropriate value for `-CertificatePath`.

    ```shell  
    C:\>tree c:\SecretStore /A /F
        Folder PATH listing
        Volume serial number is 85AE-DF2E
        C:\SECRETSTORE
        \---AzureStack
            +---CertificateRequests
            \---Certificates
                +---AppServices         # Invoke-AzsCertificateValidation `
                |   +---API             # -CertificatePath C:\Certificates\AppServices
                |   |       api.pfx     
                |   |
                |   +---DefaultDomain
                |   |       wappsvc.pfx
                |   |
                |   +---Identity
                |   |       sso.pfx
                |   |
                |   \---Publishing
                |           ftp.pfx
                |
                +---DBAdapter           # Invoke-AzsCertificateValidation `
                |       dbadapter.pfx   # -CertificatePath C:\Certificates\DBAdapter
                |                       
                |
                +---Deployment          # Invoke-AzsCertificateValidation `
                |   +---ACSBlob         # -CertificatePath C:\Certificates\Deployment
                |   |       acsblob.pfx 
                |   |
                |   +---ACSQueue
                |   |       acsqueue.pfx
               ./. ./. ./. ./. ./. ./. ./.    <- Deployment certificate tree trimmed.
                |   \---Public Portal
                |           portal.pfx
                |
                \---EventHubs           # Invoke-AzsCertificateValidation `
                        eventhubs.pfx   # -CertificatePath C:\Certificates\EventHubs
                                        
    ```

### Known issues

**Symptom**: Tests are skipped

**Cause**: AzsReadinessChecker skips certain tests if a dependency isn't met:

- Other certificates are skipped if certificate chain fails.

   ```shell  
   Testing: ACSBlob\singlewildcard.pfx
       Read PFX: OK
       Signature Algorithm: OK
       Private Key: OK
       Cert Chain: OK
       DNS Names: Fail
       Key Usage: OK
       Key Size: OK
       Chain Order: OK
       Other Certificates: Skipped
   Details:
   The certificate records '*.east.azurestack.contoso.com' do not contain a record that is valid for '*.blob.east.azurestack.contoso.com'. Please refer to the documentation for how to create the required certificate file.
   The other certificates check was skipped because cert chain and/or DNS names failed. Follow the guidance to remediate those issues and recheck. 

   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
   Invoke-AzsCertificateValidation Completed
   ```

**Resolution**: Follow the tool's guidance in the details section under each set of tests for each certificate.

**Symptom**: HTTP CRL checking fails despite having an HTTP CDP written to x509 extensions.

**Cause**: Currently, the **AzsReadinessChecker** can't check for HTTP CDP in some languages.

**Resolution**: Run validation with OS language set to EN-US.

## Certificates

| Directory | Certificate |
| ---    | ----        |
| ACSBlob | `wildcard_blob_<region>_<externalFQDN>` |
| ACSQueue  |  `wildcard_queue_<region>_<externalFQDN>` |
| ACSTable  |  `wildcard_table_<region>_<externalFQDN>` |
| Admin Extension Host  |  `wildcard_adminhosting_<region>_<externalFQDN>` |
| Admin Portal  |  `adminportal_<region>_<externalFQDN>` |
| ARM Admin  |  `adminmanagement_<region>_<externalFQDN>` |
| ARM Public  |  `management_<region>_<externalFQDN>` |
| KeyVault  |  `wildcard_vault_<region>_<externalFQDN>` |
| KeyVaultInternal  |  `wildcard_adminvault_<region>_<externalFQDN>` |
| Public Extension Host  |  `wildcard_hosting_<region>_<externalFQDN>` |
| Public Portal  |  `portal_<region>_<externalFQDN>` |

## Next steps

Once your certificates are validated by AzsReadinessChecker, you're ready to use them for Azure Stack Hub deployment or post-deployment secret rotation.

- For deployment, securely transfer your certificates to your deployment engineer so that they can copy them onto the deployment virtual machine host as specified in [Azure Stack Hub PKI requirements - Mandatory certificates](azure-stack-pki-certs.md#mandatory-certificates).
- For secret rotation, see [Rotate secrets in Azure Stack Hub](azure-stack-rotate-secrets.md). Rotation of value-add resource provider certificates is covered in the [Rotate external secrets section](azure-stack-rotate-secrets.md#rotate-external-secrets).
