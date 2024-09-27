---
title: Local billing meters | Microsoft Docs
description: List of local Azure Stack meters
services: azure-stack
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 10/11/2021
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 12/20/2019

---

# Local Azure Stack billing meters

This article lists local Azure Stack billing meters. Usage is reported for the following resource providers:

## Network

**Meter ID**: F271A8A388C44D93956A063E1D2FA80B  
**Meter name**: Static IP Address Usage  
**Unit**: IP addresses  
**Notes**: Count of IP addresses used. If you call the usage API with a daily granularity, the meter returns IP address multiplied by the number of hours.  
  
**Meter ID**: 9E2739BA86744796B465F64674B822BA  
**Meter name**: Dynamic IP Address Usage  
**Unit**: IP addresses  
**Notes**: Count of IP addresses used. If you call the usage API with a daily granularity, the meter returns IP address multiplied by the number of hours.

## Storage

**Meter ID**: B4438D5D-453B-4EE1-B42A-DC72E377F1E4  
**Meter name**: TableCapacity  
**Unit**: GB\*hours  
**Notes**: Total capacity consumed by tables.  
  
**Meter ID**: B5C15376-6C94-4FDD-B655-1A69D138ACA3  
**Meter name**: PageBlobCapacity  
**Unit**: GB\*hours  
**Notes**: Total capacity consumed by page blobs.  
  
**Meter ID**: B03C6AE7-B080-4BFA-84A3-22C800F315C6  
**Meter name**: QueueCapacity  
**Unit**: GB\*hours  
**Notes**: Total capacity consumed by queue.  
  
**Meter ID**: 09F8879E-87E9-4305-A572-4B7BE209F857  
**Meter name**: BlockBlobCapacity  
**Unit**: GB*hours  
**Notes**: Total capacity consumed by block blobs.  
  
**Meter ID**: B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90  
**Meter name**: TableTransactions  
**Unit**: Request count * 10,000  
**Notes**: Table service requests (* 10,000).  
  
**Meter ID**: 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D  
**Meter name**: TableDataTransIn  
**Unit**: Ingress data in GB  
**Notes**: Table service data ingress in GB.  
  
**Meter ID**: 1B8C1DEC-EE42-414B-AA36-6229CF199370  
**Meter name**: TableDataTransOut  
**Unit**: Egress in GB  
**Notes**: Table service data egress in GB.
  
**Meter ID**: 43DAF82B-4618-444A-B994-40C23F7CD438  
**Meter name**: BlobTransactions  
**Unit**: Requests count * 10,000
**Notes**: Blob service requests (* 10,000).  
  
**Meter ID**: 9764F92C-E44A-498E-8DC1-AAD66587A810  
**Meter name**: BlobDataTransIn  
**Unit**: Ingress data in GB  
**Notes**: Blob service data ingress in GB.  
  
**Meter ID**: 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8  
**Meter name**: BlobDataTransOut  
**Unit**: Egress in GB  
**Notes**: Blob service data egress in GB.  
  
**Meter ID**: EB43DD12-1AA6-4C4B-872C-FAF15A6785EA  
**Meter name**: QueueTransactions  
**Unit**: Requests count * 10,000
**Notes**: Queue service requests (* 10,000).  
  
**Meter ID**: E518E809-E369-4A45-9274-2017B29FFF25  
**Meter name**: QueueDataTransIn  
**Unit**: Ingress data in GB  
**Notes**: Queue service data ingress in GB.  
  
**Meter ID**: DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2  
**Meter name**: QueueDataTransOut  
**Unit**: Egress in GB  
**Notes**: Queue service data egress in GB

## Compute

**Meter ID**: FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5  
**Meter name**: Base VM Size Hours  
**Unit**: Virtual core hours  
**Notes**: Number of virtual cores multiplied by the hours the VM ran.  
  
**Meter ID**: 9CD92D4C-BAFD-4492-B278-BEDC2DE8232A  
**Meter name**: Windows VM Size Hours  
**Unit**: Virtual core hours  
**Notes**: Number of virtual cores multiplied by hours the VM ran.  
  
**Meter ID**: 6DAB500F-A4FD-49C4-956D-229BB9C8C793  
**Meter name**: VM size hours  
**Unit**: VM hours  
**Notes**: Captures both base and Windows VM. Does not adjust for cores.

## Managed Disks

**Meter ID**: 380874f9-300c-48e0-95a0-d2d9a21ade8f  
**Meter name**: S4  
**Unit**: Count of Disks * month  
**Notes**: Standard Managed Disk - 32 GB  

**Meter ID**: 1b77d90f-427b-4435-b4f1-d78adec53222  
**Meter name**: S6  
**Unit**: Count of Disks * month  
**Notes**: Standard Managed Disk - 64 GB  

**Meter ID**: d5f7731b-f639-404a-89d0-e46186e22c8d  
**Meter name**: S10  
**Unit**: Count of Disks * month  
**Notes**: Standard Managed Disk - 128 GB  

**Meter ID**: ff85ef31-da5b-4eac-95dd-a69d6f97b18a  
**Meter name**: S15  
**Unit**: Count of Disks * month  
**Notes**: Standard Managed Disk - 256 GB  

**Meter ID**: 88ea9228-457a-4091-adc9-ad5194f30b6e  
**Meter name**: S20  
**Unit**: Count of Disks * month  
**Notes**: Standard Managed Disk - 512 GB  

**Meter ID**: 5b1db88a-8596-4002-8052-347947c26940  
**Meter name**: S30  
**Unit**: Count of Disks * month
**Notes**: Standard Managed Disk - 1024 GB

**Meter ID**: 7660b45b-b29d-49cb-b816-59f30fbab011  
**Meter name**: P4  
**Unit**: Count of Disks * month  
**Notes**: Premium Managed Disk - 32 GB  

**Meter ID**: 817007fd-a077-477f-bc01-b876f27205fd  
**Meter name**: P6  
**Unit**: Count of Disks * month  
**Notes**: Premium Managed Disk - 64 GB  

**Meter ID**: e554b6bc-96cd-4938-a5b5-0da990278519  
**Meter name**: P10  
**Unit**: Count of Disks * month  
**Notes**: Premium Managed Disk - 128 GB    

**Meter ID**: cdc0f53a-62a9-4472-a06c-e99a23b02907  
**Meter name**: P15  
**Unit**: Count of Disks * month  
**Notes**: Premium Managed Disk - 256 GB  

**Meter ID**: b9cb2d1a-84c2-4275-aa8b-70d2145d59aa  
**Meter name**: P20  
**Unit**: Count of Disks * month  
**Notes**: Premium Managed Disk - 512 GB  

**Meter ID**: 06bde724-9f94-43c0-84c3-d0fc54538369  
**Meter name**: P30  
**Unit**: Count of Disks * month  
**Notes**: Premium Managed Disk - 1024 GB  

**Meter ID**: 7ba084ec-ef9c-4d64-a179-7732c6cb5e28  
**Meter name**: ActualStandardDiskSize  
**Unit**: GB * month  
**Notes**: The actual size on disk of standard managed disk  

**Meter ID**: daef389a-06e5-4684-a7f7-8813d9f792d5  
**Meter name**: ActualPremiumDiskSize  
**Unit**: GB * month  
**Notes**: The actual size on disk of premium managed disk  

**Meter ID**: 108fa95b-be0d-4cd9-96e8-5b0d59505df1  
**Meter name**: ActualStandardSnapshotSize  
**Unit**: GB * month  
**Notes**: The actual size on disk of managed standard snapshot  

**Meter ID**: 578ae51d-4ef9-42f9-85ae-42b52d3d83ac  
**Meter name**: ActualPremiumSnapshotSize  
**Unit**: GB * month  
**Notes**: The actual size on disk of managed premium snapshot  

**Meter ID**: 5d76e09f-4567-452a-94cc-7d1f097761f0  
**Meter name**: S4  
**Unit**: Count of Disks * hours  
**Notes**: Standard Managed Disk - 32 GB (Deprecated)  

**Meter ID**: dc9fc6a9-0782-432a-b8dc-978130457494  
**Meter name**: S6  
**Unit**: Count of Disks * hours  
**Notes**: Standard Managed Disk - 64 GB (Deprecated)  

**Meter ID**: e5572fce-9f58-49d7-840c-b168c0f01fff  
**Meter name**: S10  
**Unit**: Count of Disks * hours  
**Notes**: Standard Managed Disk - 128 GB (Deprecated)  

**Meter ID**: 9a8caedd-1195-4cd5-80b4-a4c22f9302b8  
**Meter name**: S15  
**Unit**: Count of Disks * hours  
**Notes**: Standard Managed Disk - 256 GB (Deprecated)  

**Meter ID**: 5938f8da-0ecd-4c48-8d5a-c7c6c23546be  
**Meter name**: S20  
**Unit**: Count of Disks * hours  
**Notes**: Standard Managed Disk - 512 GB (Deprecated)  

**Meter ID**: 7705a158-bd8b-4b2b-b4c2-0782343b81e6  
**Meter name**: S30  
**Unit**: Count of Disks * hours  
**Notes**: Standard Managed Disk - 1024 GB (Deprecated)  

**Meter ID**: 5c105f5f-cbdf-435c-b49b-3c7174856dcc  
**Meter name**: P4 
**Unit**: Count of Disks * hours  
**Notes**: Premium Managed Disk - 32 GB (Deprecated)  

**Meter ID**: 518b412b-1927-4f25-985f-4aea24e55c4f  
**Meter name**: P6  
**Unit**: Count of Disks * hours  
**Notes**: Premium Managed Disk - 64 GB (Deprecated)  

**Meter ID**: 5cfb1fed-0902-49e3-8217-9add946fd624  
**Meter name**: P10  
**Unit**: Count of Disks * hours  
**Notes**: Premium Managed Disk - 128 GB (Deprecated)  

**Meter ID**: 8de91c94-f740-4d9a-b665-bd5974fa08d4  
**Meter name**: P15  
**Unit**: Count of Disks * hours  
**Notes**: Premium Managed Disk - 256 GB (Deprecated)  

**Meter ID**: c7e7839c-293b-4761-ae4c-848eda91130b  
**Meter name**: P20  
**Unit**: Count of Disks * hours  
**Notes**: Premium Managed Disk - 512 GB (Deprecated)  

**Meter ID**: 9f502103-adf4-4488-b494-456c95d23a9f  
**Meter name**: P30  
**Unit**: Count of Disks * hours  
**Notes**: Premium Managed Disk - 1024 GB (Deprecated)  

**Meter ID**: 8a409390-1913-40ae-917b-08d0f16f3c38  
**Meter name**: ActualStandardDiskSize  
**Unit**: Byte * hours  
**Notes**: The actual size on disk of standard managed disk (Deprecated)  

**Meter ID**: 1273b16f-8458-4c34-8ce2-a515de551ef6  
**Meter name**: ActualPremiumDiskSize  
**Unit**: Byte * hours  
**Notes**: The actual size on disk of premium managed disk (Deprecated)  

**Meter ID**: 89009682-df7f-44fe-aeb1-63fba3ddbf4c  
**Meter name**: ActualStandardSnapshotSize  
**Unit**: Byte * hours  
**Notes**: The actual size on disk of managed standard snapshot (Deprecated)  

**Meter ID**: 95b0c03f-8a82-4524-8961-ccfbf575f536 
**Meter name**: ActualPremiumSnapshotSize  
**Unit**: Byte * hours  
**Notes**: The actual size on disk of managed premium snapshot (Deprecated)  

**Meter ID**: 75d4b707-1027-4403-9986-6ec7c05579c8  
**Meter name**: ActualStandardSnapshotSize  
**Unit**: GB * month  
**Notes**: The actual size on disk of managed standard snapshot (Deprecated)  

**Meter ID**: 5ca1cbb9-6f14-4e76-8be8-1ca91547965e  
**Meter name**: ActualPremiumSnapshotSize  
**Unit**: GB * month  
**Notes**: The actual size on disk of managed premium snapshot (Deprecated)  

### Sql RP
  
**Meter ID**: CBCFEF9A-B91F-4597-A4D3-01FE334BED82    
**Meter name**: DatabaseSizeHourSqlMeter    
**Unit**: MB * hours  
**Notes**: Total DB capacity at creation. If you call the usage API with a daily granularity, the meter returns MB multiplied by the number of hours.

## MySql RP
  
**Meter ID**: E6D8CFCD-7734-495E-B1CC-5AB0B9C24BD3  
**Meter name**: DatabaseSizeHourMySqlMeter  
**Unit**: MB * hours  
**Notes**: Total DB capacity at creation. If you call the usage API with a daily granularity, the meter returns MB multiplied by the number of hours  

## Key Vault
  
**Meter ID**: EBF13B9F-B3EA-46FE-BF54-396E93D48AB4  
**Meter name**: Key Vault transactions  
**Unit**: Request count * 10,000  
**Notes**: Number of REST API requests received by Key Vault data plane    
  
**Meter ID**: 2C354225-B2FE-42E5-AD89-14F0EA302C87  
**Meter name**: Advanced keys transactions  
**Unit**:  10,000 transactions  
**Notes**: RSA 3K/4K, ECC key transactions. (preview)  

## App service
  
**Meter ID**: 190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  
**Meter name**: App Service   
**Unit**: Virtual core hours  
**Notes**: Number of virtual cores used to run app service. Note: Microsoft uses this meter to charge the App Service on Azure Stack. Cloud Solution Providers can use the other App Service meters (below) to calculate usage for their tenants.    
  
**Meter ID**: 67CC4AFC-0691-48E1-A4B8-D744D1FEDBDE      
**Meter name**: Functions Requests  
**Unit**: 10 Requests  
**Notes**: Total number of requested executions (per 10 executions).   Executions are counted each time a function runs in response to an event, or is triggered by a binding.  
  
**Meter ID**: D1D04836-075C-4F27-BF65-0A1130EC60ED  
**Meter name**: Functions - Compute  
**Unit**:  GB  
**Notes**:  Resource consumption measured in gigabyte per seconds (GB/s).   
**Observed resource consumption** is calculated by multiplying average memory size in GB by the time in milliseconds it takes to execute the function. Memory used by a function is measured by rounding up to the nearest 128 MB, up to the maximum memory size of 1,536 MB, with execution time calculated by rounding up to the nearest 1 ms. The minimum execution time and memory for a single function execution is 100 ms and 128 mb, respectively.  
  
**Meter ID**: 957E9F36-2C14-45A1-B6A1-1723EF71A01D  
**Meter name**: Shared App Service Hours  
**Unit**: 1 hour  
**Notes**: Per hour usage of shard App Service Plan. Plans are metered on a per App basis.  
  
**Meter ID**: 539CDEC7-B4F5-49F6-AAC4-1F15CFF0EDA9   
**Meter name**: Free App Service Hours  
**Unit**: 1 hour  
**Notes**: Per hour usage of free App Service Plan. Plans are metered on a per App basis.  
  
**Meter ID**: 88039D51-A206-3A89-E9DE-C5117E2D10A6  
**Meter name**: Small Standard App Service Hours  
**Unit**: 1 hour  
**Notes**: Calculated based on size and number of instances.  
  
**Meter ID**: 83A2A13E-4788-78DD-5D55-2831B68ED825  
**Meter name**: Medium Standard App Service Hours  
**Unit**: 1 hour  
**Notes**: Calculated based on size and number of instances.  
  
**Meter ID**: 1083B9DB-E9BB-24BE-A5E9-D6FDD0DDEFE6  
**Meter name**: Large Standard App Service Hours  
**Unit**: 1 hour  
**Notes**: Calculated based on size and number of instances.  

## Custom Worker Tiers
  
**Meter ID**: *Custom Worker Tiers*  
**Meter name**: Custom Worker Tiers    
**Unit**: Hours  
**Notes**: Deterministic meter ID is created based on SKU and custom worker tier name. This meter ID is unique for each custom worker tier.  
  
**Meter ID**: 264ACB47-AD38-47F8-ADD3-47F01DC4F473  
**Meter name**: SNI SSL  
**Unit**: Per SNI SSL Binding  
**Notes**: App Service supports two types of SSL connections: Server Name Indication (SNI) SSL Connections and IP Address SSL Connections. SNI-based SSL works on modern browsers while IP-based SSL works on all browsers.  
  
**Meter ID**: 60B42D72-DC1C-472C-9895-6C516277EDB4    
**Meter name**: IP SSL  
**Unit**: Per IP Based SSL Binding  
**Notes**: App Service supports two types of SSL connections: Server Name Indication (SNI) SSL Connections and IP Address SSL Connections. SNI-based SSL works on modern browsers while IP-based SSL works on all browsers.  
  
**Meter ID**: 73215A6C-FA54-4284-B9C1-7E8EC871CC5B  
**Meter name**:  Web Process  
**Unit**:   
**Notes**: Calculated per active site per hour.  
  
**Meter ID**: 5887D39B-0253-4E12-83C7-03E1A93DFFD9    
**Meter name**: External Egress Bandwidth  
**Unit**: GB  
**Notes**: Total incoming request response bytes + total outgoing request bytes + total incoming FTP request response bytes + total incoming web deploy request response bytes.  