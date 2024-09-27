---
title: Use API version profiles with Ruby in Azure Stack Hub 
description: Learn how to use API version profiles with Ruby in Azure Stack Hub.
author: sethmanheim

ms.topic: article
ms.date: 11/4/2021
ms.author: sethm
ms.reviewer: weshi1
ms.lastreviewed: 3/24/2021

# Intent: As an Azure Stack user, I want to use API version profiles with Ruby in Azure Stack so I can benefit from the use of profiles.
# Keyword: azure stack api profiles ruby

---


# Use API version profiles with Ruby in Azure Stack Hub

## Ruby and API version profiles

The Ruby SDK for the Azure Stack Hub Resource Manager provides tools to help you build and manage your infrastructure. Resource providers in the SDK include Compute, Virtual Networks, and Storage, with the Ruby language. API profiles in the Ruby SDK enable hybrid cloud development by helping you switch between global Azure resources and resources on Azure Stack Hub.

An API profile is a combination of resource providers and service versions. You can use an API profile to combine different resource types.

- To use the latest versions of all the services, use the **Latest** profile of the Azure SDK rollup gem.
- Profiles are named by date in format such as `V2020_09_01_Hybrid` or `V2019_03_01_Hybrid`.
- To use the latest **api-version** of a service, use the **Latest** profile of the specific gem. For example, to use the latest **api-version** of compute service alone, use the **Latest** profile of the **Compute** gem.
- To use a specific **api-version** for a service, use the specific API versions defined inside the gem.

## Install the Azure Ruby SDK

- Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
- Install [Ruby](https://www.ruby-lang.org/en/documentation/installation/).
  - When installing, choose **Add Ruby to PATH variable**.
  - When prompted during Ruby installation, install the development kit.
  - Next, install the bundler using the following command: 

       ```ruby
       Gem install bundler
       ```

- If not available, create a subscription and save the subscription ID to be used later. Instructions to create a subscription are in the [Create subscriptions to offers in Azure Stack Hub](../operator/azure-stack-subscribe-plan-provision-vm.md) article.
- Create a service principal and save its ID and secret. Instructions to create a service principal for Azure Stack Hub are in the [Use an app identity to access resources](../operator/give-app-access-to-resources.md) article.
- Make sure your service principal has the contributor/owner role assigned on your subscription. Instructions on how to assign a role to a service principal are in the [Use an app identity to access resources](../operator/give-app-access-to-resources.md) article.

## Install the RubyGem packages

You can install the Azure RubyGem packages directly.

```ruby  
gem install azure_mgmt_compute
gem install azure_mgmt_storage
gem install azure_mgmt_resources
gem install azure_mgmt_network
```

Or, use them in your Gemfile.

```ruby
gem 'azure_mgmt_storage'
gem 'azure_mgmt_compute'
gem 'azure_mgmt_resources'
gem 'azure_mgmt_network'
```

The Azure Resource Manager Ruby SDK is in preview and will likely have breaking interface changes in upcoming releases. An increased number in the minor version may indicate breaking changes.

## Use the azure_sdk gem

The **azure_sdk** gem is a rollup of all the supported gems in the Ruby SDK.

You can install the azure_sdk rollup gem with the following command:  

```ruby  
gem install 'azure_sdk'
```

## Profiles

For profiles containing dates, to use a different SDK profile or version, substitute the date in `V<date>_Hybrid`. For example, for the 2008 version, the profile is `2019_03_01`, and the string becomes `V2019_03_01_Hybrid`. Note that sometimes the SDK team changes the name of the packages, so simply replacing the date of a string with a different date might not work. See the following table for association of profiles and Azure Stack versions.

You may also use `latest` instead of the date.

| Azure Stack version | Profile |
|---------------------|---------|
|2311|2020_09_01|
|2301|2020_09_01|
|2206|2020_09_01|
|2108|2020_09_01|
|2102|2020_09_01|
|2008|2019_03_01|

For more information about Azure Stack Hub and API profiles, see the [Summary of API profiles](azure-stack-version-profiles.md).

See [Ruby SDK profiles](https://github.com/Azure/azure-sdk-for-ruby/tree/master/azure_sdk/lib).

## Subscription

If you do not already have a subscription, create a subscription and save the subscription ID to be used later. For information about how to create a subscription, see this [document](../operator/azure-stack-subscribe-plan-provision-vm.md).

## Service Principal

A service principal and its associated environment information should be created and saved somewhere. Service principal with `owner` role is recommended, but depending on the sample, a `contributor` role may suffice. Refer to the table below for the required values.

| Value | Environment variables | Description |
| --- | --- | --- |
| Tenant ID | `AZURE_TENANT_ID` | Your Azure Stack Hub [tenant ID](../operator/azure-stack-identity-overview.md). |
| Client ID | `AZURE_CLIENT_ID` | The service principal app ID saved when the service principal was created in the previous section of this article.  |
| Subscription ID | `AZURE_SUBSCRIPTION_ID` | You use the [subscription ID](../operator/service-plan-offer-subscription-overview.md#subscriptions) to access offers in Azure Stack Hub. |
| Client Secret | `AZURE_CLIENT_SECRET` | The service principal app secret saved when the service principal was created. |
| Resource Manager Endpoint | `ARM_ENDPOINT` | See [The Azure Stack Hub Resource Manager endpoint](#azure-stack-resource-manager-endpoint).  |

## Tenant ID

To find the directory or tenant ID for your Azure Stack Hub, follow the instructions [in this article](./authenticate-azure-stack-hub.md#get-the-tenant-id).

## Register resource providers

Register required resource providers by following this [document](/azure/azure-resource-manager/management/resource-providers-and-types). These resource providers will be required depending on the samples you want to run. For example, if you want to run a VM sample, the `Microsoft.Compute` resource provider registration is required.

## Azure Stack resource manager endpoint

Azure Resource Manager (ARM) is a management framework that enables administrators to deploy, manage, and monitor Azure resources. Azure Resource Manager can handle these tasks as a group, rather than individually, in a single operation. You can get the metadata info from the Resource Manager endpoint. The endpoint returns a JSON file with the info required to run your code.

- The **ResourceManagerEndpointUrl** in the Azure Stack Development Kit (ASDK) is: `https://management.local.azurestack.external/`.
- The **ResourceManagerEndpointUrl** in integrated systems is: `https://management.region.<fqdn>/`, where `<fqdn>` is your fully qualified domain name.
- To retrieve the metadata required: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`.
For available API versions, see [Azure rest API specifications](https://github.com/Azure/azure-rest-api-specs/tree/main/profile). E.g., in `2020-09-01` profile version, you can change the `api-version` to `2019-10-01` for resource provider `microsoft.resources`.

Sample JSON:
```json
{
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",
   "graphEndpoint": "https://graph.windows.net/",
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": 
      {
         "loginEndpoint": "https://login.windows.net/",
         "audiences": ["https://management.yourtenant.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## Set environment variables

### Microsoft Windows

To set the environment variables, use the following format in a Windows command prompt:

```console
set AZURE_TENANT_ID=<YOUR_TENANT_ID>
```

### macOS, Linux, and Unix-based systems

In Unix-based systems, use the following command:

```bash
export AZURE_TENANT_ID=<YOUR_TENANT_ID>
```

For more info on Azure Stack Hub and API profiles, see the [Summary of API profiles](azure-stack-version-profiles.md#summary-of-api-profiles).

## Azure Ruby SDK API profile usage

Use the following code to instantiate a profile client. This parameter is only required for Azure Stack Hub or other private clouds. Global Azure already has these settings by default.

```ruby  
active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])

provider = MsRestAzure::ApplicationTokenProvider.new(
  ENV['AZURE_TENANT_ID'],
  ENV['AZURE_CLIENT_ID'],
  ENV['AZURE_CLIENT_SECRET'],
  active_directory_settings
)
credentials = MsRest::TokenCredentials.new(provider)
options = {
  credentials: credentials,
  subscription_id: subscription_id,
  active_directory_settings: active_directory_settings,
  base_url: ENV['ARM_ENDPOINT']
}

# Target profile built for Azure Stack Hub
client = Azure::Resources::Profiles::V2019_03_01_Hybrid::Mgmt::Client.new(options)
```

The profile client can be used to access individual resource providers, such as Compute, Storage, and Network:

```ruby  
# To access the operations associated with Compute
profile_client.compute.virtual_machines.get 'RESOURCE_GROUP_NAME', 'VIRTUAL_MACHINE_NAME'

# Option 1: To access the models associated with Compute
purchase_plan_obj = profile_client.compute.model_classes.purchase_plan.new

# Option 2: To access the models associated with Compute
# Notice Namespace: Azure::Profiles::<Profile Name>::<Service Name>::Mgmt::Models::<Model Name>
purchase_plan_obj = Azure::Profiles::V2019_03_01_Hybrid::Compute::Mgmt::Models::PurchasePlan.new
```

## Define Azure Stack Hub environment setting functions

To authenticate the service principal to the Azure Stack Hub environment, define the endpoints using `get_active_directory_settings()`. This method uses the **ARM_Endpoint** environment variable that you set previously:

```ruby  
# Get Authentication endpoints using Arm Metadata Endpoints
def get_active_directory_settings(armEndpoint)
  settings = MsRestAzure::ActiveDirectoryServiceSettings.new
  response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
  status_code = response.code
  response_content = response.body
  unless status_code == "200"
    error_model = JSON.load(response_content)
    fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Hub Metadata Endpoints", response, error_model)
  end
  result = JSON.load(response_content)
  settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
  settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
  settings
end
```

## Samples

Use the following samples on GitHub as references for creating solutions with Ruby and Azure Stack Hub API profiles:

- [Manage Azure resources and resource groups with Ruby](https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Resources-And-Groups).
- [Manage virtual machines using Ruby](https://github.com/Azure-Samples/Hybrid-Compute-Ruby-Manage-VM)
- [Deploy an SSH Enabled VM with a Template in Ruby](https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Template-Deployment).

### Sample resource manager and groups

To run the sample, ensure that you've installed Ruby. If you're using Visual Studio Code, download the Ruby SDK extension as well.

> [!NOTE]  
> The repository for the sample is [Hybrid-Resource-Manager-Ruby-Resources-And-Groups](https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Resources-And-Groups).

1. Clone the repository:

   ```console
   git clone https://github.com/Azure-Samples/Hybrid-Resource-Manager-Ruby-Resources-And-Groups.git
   ```

1. Install the dependencies using bundle:

   ```console
   cd Hybrid-Resource-Manager-Ruby-Resources-And-Groups
   bundle install
   ```

1. Create an Azure service principal using PowerShell and retrieve the values needed.

   For instructions on creating a service principal, see [Use Azure PowerShell to create a service principal with a certificate](../operator/give-app-access-to-resources.md).

   Values needed are:

   - Tenant ID
   - Client ID
   - Client secret
   - Subscription ID
   - Resource Manager endpoint

   Set the following environment variables using the information retrieved from the service principal you created:

   - `export AZURE_TENANT_ID={your tenant ID}`
   - `export AZURE_CLIENT_ID={your client ID}`
   - `export AZURE_CLIENT_SECRET={your client secret}`
   - `export AZURE_SUBSCRIPTION_ID={your subscription ID}`
   - `export ARM_ENDPOINT={your Azure Stack Hub Resource Manager URL}`

   > [!NOTE]  
   > On Windows, use `set` instead of `export`.

1. Ensure the location variable is set to your Azure Stack Hub location; for example, `LOCAL="local"`.

1. To target the correct active directory endpoints, add the following line of code if you're using Azure Stack Hub or other private clouds:

   ```ruby  
   active_directory_settings = get_active_directory_settings(ENV['ARM_ENDPOINT'])
   ```

1. In the `options` variable, add the Active Directory settings and the base URL to work with Azure Stack Hub:

   ```ruby  
   options = {
   credentials: credentials,
   subscription_id: subscription_id,
   active_directory_settings: active_directory_settings,
   base_url: ENV['ARM_ENDPOINT']
   }
   ```

1. Create a profile client that targets the Azure Stack Hub profile:

   ```ruby  
   client = Azure::Resources::Profiles::V2019_03_01_Hybrid::Mgmt::Client.new(options)
   ```

1. To authenticate the service principal with Azure Stack Hub, the endpoints should be defined using **get_active_directory_settings()**. This method uses the **ARM_Endpoint** environment variable that you set previously:

   ```ruby  
   def get_active_directory_settings(armEndpoint)
     settings = MsRestAzure::ActiveDirectoryServiceSettings.new
     response = Net::HTTP.get_response(URI("#{armEndpoint}/metadata/endpoints?api-version=1.0"))
     status_code = response.code
     response_content = response.body
     unless status_code == "200"
       error_model = JSON.load(response_content)
       fail MsRestAzure::AzureOperationError.new("Getting Azure Stack Hub Metadata Endpoints", response, error_model)
     end
     result = JSON.load(response_content)
     settings.authentication_endpoint = result['authentication']['loginEndpoint'] unless result['authentication']['loginEndpoint'].nil?
     settings.token_audience = result['authentication']['audiences'][0] unless result['authentication']['audiences'][0].nil?
     settings
   end
   ```

1. Run the sample.

   ```ruby
   bundle exec ruby example.rb
   ```


## Next steps

- [Install PowerShell for Azure Stack Hub](../operator/powershell-install-az-module.md)
- [Configure the Azure Stack Hub user's PowerShell environment](azure-stack-powershell-configure-user.md)  
