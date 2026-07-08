---
layout: default
title: 'Lab 12: Deployments using Azure Bicep templates'
nav_order: 6.12
has_children: false
parent: 'Hands-on Labs Issues'
---

# Software list for Hands-On Labs
{: .no_toc }


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

<br/>

---

<br/>

# Lab 12: Deployments using Azure Bicep templates


This situation was detected on 2026-07-07.


If no organization appears when accessing Azure DevOps, try the following URLs:
- `https://dev.azure.com/ADOCourseOrg01/`
- `https://dev.azure.com/ADOCourseOrg03/`
- `https://dev.azure.com/ADOCourseOrg04/`




2. When checking the selected region, select **westus2**.
 - **Check Selected Region:** westus2



## Bicep: /infra/simple-windows-vm.bicep

```Bicep
@description('Username for the Virtual Machine.')
param adminUsername string = 'Student'

@description('Password for the Virtual Machine.')
@minLength(12)
@secure()
param adminPassword string = 'P@s${uniqueString(newGuid())}!'

@description('Unique DNS Name for the Public IP used to access the Virtual Machine.')
param dnsLabelPrefix string = toLower('${vmName}-${uniqueString(resourceGroup().id, vmName)}')

@description('Name for the Public IP used to access the Virtual Machine.')
param publicIpName string = 'myPublicIP'


// HANDS-ON LAB STEPS: Change the default param value to Static
@description('Allocation method for the Public IP used to access the Virtual Machine.')
@allowed([
  'Dynamic'
  'Static'
])
param publicIPAllocationMethod string = 'Static'

// HANDS-ON LAB STEPS: Change the default param value to Standard
@description('SKU for the Public IP used to access the Virtual Machine.')
@allowed([
  'Basic'
  'Standard'
])
param publicIpSku string = 'Standard'

@description('The Windows version for the VM. This will pick a fully patched image of this given Windows version.')
@allowed([
  '2016-datacenter-gensecond'
  '2016-datacenter-server-core-g2'
  '2016-datacenter-server-core-smalldisk-g2'
  '2016-datacenter-smalldisk-g2'
  '2016-datacenter-with-containers-g2'
  '2016-datacenter-zhcn-g2'
  '2019-datacenter-core-g2'
  '2019-datacenter-core-smalldisk-g2'
  '2019-datacenter-core-with-containers-g2'
  '2019-datacenter-core-with-containers-smalldisk-g2'
  '2019-datacenter-gensecond'
  '2019-datacenter-smalldisk-g2'
  '2019-datacenter-with-containers-g2'
  '2019-datacenter-with-containers-smalldisk-g2'
  '2019-datacenter-zhcn-g2'
  '2022-datacenter-azure-edition'
  '2022-datacenter-azure-edition-core'
  '2022-datacenter-azure-edition-core-smalldisk'
  '2022-datacenter-azure-edition-smalldisk'
  '2022-datacenter-core-g2'
  '2022-datacenter-core-smalldisk-g2'
  '2022-datacenter-g2'
  '2022-datacenter-smalldisk-g2'
])
param OSVersion string = '2022-datacenter-azure-edition'


// HANDS-ON LAB STEPS: Change the default param value to Standard_D2s_v5
@description('Size of the virtual machine.')
param vmSize string = 'Standard_D2s_v5'

@description('Location for all resources.')
param location string = resourceGroup().location

@description('Name of the virtual machine.')
param vmName string = 'simple-vm'

@description('Security Type of the Virtual Machine.')
@allowed([
  'Standard'
  'TrustedLaunch'
])
param securityType string = 'TrustedLaunch'

var storageAccountName = 'bootdiags${uniqueString(resourceGroup().id)}'
var nicName = 'myVMNic'
var addressPrefix = '10.0.0.0/16'
var subnetName = 'Subnet'
var subnetPrefix = '10.0.0.0/24'
var virtualNetworkName = 'MyVNET'
var networkSecurityGroupName = 'default-NSG'
var securityProfileJson = {
  uefiSettings: {
    secureBootEnabled: true
    vTpmEnabled: true
  }
  securityType: securityType
}
var extensionName = 'GuestAttestation'
var extensionPublisher = 'Microsoft.Azure.Security.WindowsAttestation'
var extensionVersion = '1.0'
var maaTenantName = 'GuestAttestation'
var maaEndpoint = substring('emptyString', 0, 0)

// HANDS-ON LAB STEPS: linked tempate '/infra/storage.bicep'
module storageModule './storage.bicep' = {
  name: 'linkedTemplate'
  params: {
    location: location
    storageAccountName: storageAccountName
  }
}



// HANDS-ON LAB STEPS: Remove or comment the storage resource
/*
resource storageAccount 'Microsoft.Storage/storageAccounts@2022-05-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'Storage'
}
*/

resource publicIp 'Microsoft.Network/publicIPAddresses@2022-05-01' = {
  name: publicIpName
  location: location
  sku: {
    name: publicIpSku
  }
  properties: {
    publicIPAllocationMethod: publicIPAllocationMethod
    dnsSettings: {
      domainNameLabel: dnsLabelPrefix
    }
  }
}

resource networkSecurityGroup 'Microsoft.Network/networkSecurityGroups@2022-05-01' = {
  name: networkSecurityGroupName
  location: location
  properties: {
    securityRules: [
      {
        name: 'default-allow-3389'
        properties: {
          priority: 1000
          access: 'Allow'
          direction: 'Inbound'
          destinationPortRange: '3389'
          protocol: 'Tcp'
          sourcePortRange: '*'
          sourceAddressPrefix: '*'
          destinationAddressPrefix: '*'
        }
      }
    ]
  }
}

resource virtualNetwork 'Microsoft.Network/virtualNetworks@2022-05-01' = {
  name: virtualNetworkName
  location: location
  properties: {
    addressSpace: {
      addressPrefixes: [
        addressPrefix
      ]
    }
    subnets: [
      {
        name: subnetName
        properties: {
          addressPrefix: subnetPrefix
          networkSecurityGroup: {
            id: networkSecurityGroup.id
          }
        }
      }
    ]
  }
}

resource nic 'Microsoft.Network/networkInterfaces@2022-05-01' = {
  name: nicName
  location: location
  properties: {
    ipConfigurations: [
      {
        name: 'ipconfig1'
        properties: {
          privateIPAllocationMethod: 'Dynamic'
          publicIPAddress: {
            id: publicIp.id
          }
          subnet: {
            id: resourceId('Microsoft.Network/virtualNetworks/subnets', virtualNetworkName, subnetName)
          }
        }
      }
    ]
  }
  dependsOn: [

    virtualNetwork
  ]
}


// HANDS-ON LAB STEPS: replace the diagnosticsProfile storageUri to 'storageModule.outputs.storageURI'
resource vm 'Microsoft.Compute/virtualMachines@2022-03-01' = {
  name: vmName
  location: location
  properties: {
    hardwareProfile: {
      vmSize: vmSize
    }
    osProfile: {
      computerName: vmName
      adminUsername: adminUsername
      adminPassword: adminPassword
    }
    storageProfile: {
      imageReference: {
        publisher: 'MicrosoftWindowsServer'
        offer: 'WindowsServer'
        sku: OSVersion
        version: 'latest'
      }
      osDisk: {
        createOption: 'FromImage'
        managedDisk: {
          storageAccountType: 'StandardSSD_LRS'
        }
      }
      dataDisks: [
        {
          diskSizeGB: 1023
          lun: 0
          createOption: 'Empty'
        }
      ]
    }
    networkProfile: {
      networkInterfaces: [
        {
          id: nic.id
        }
      ]
    }
    diagnosticsProfile: {
      bootDiagnostics: {
        enabled: true
        storageUri: storageModule.outputs.storageURI
      }
    }
    securityProfile: ((securityType == 'TrustedLaunch') ? securityProfileJson : null)
  }
}

resource vmExtension 'Microsoft.Compute/virtualMachines/extensions@2022-03-01' = if ((securityType == 'TrustedLaunch') && ((securityProfileJson.uefiSettings.secureBootEnabled == true) && (securityProfileJson.uefiSettings.vTpmEnabled == true))) {
  parent: vm
  name: extensionName
  location: location
  properties: {
    publisher: extensionPublisher
    type: extensionName
    typeHandlerVersion: extensionVersion
    autoUpgradeMinorVersion: true
    enableAutomaticUpgrade: true
    settings: {
      AttestationConfig: {
        MaaSettings: {
          maaEndpoint: maaEndpoint
          maaTenantName: maaTenantName
        }
      }
    }
  }
}

output hostname string = publicIp.properties.dnsSettings.fqdn

```


## Bicep: /infra/storage.bicep

```Bicep
@description('Location for all resources.')
param location string = resourceGroup().location

@description('Name for the storage account.')
param storageAccountName string

resource storageAccount 'Microsoft.Storage/storageAccounts@2022-05-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'Storage'
}

output storageURI string = storageAccount.properties.primaryEndpoints.blob
```



## Pipeline  Azure Repos Git (YAML)

1. Select the correct repository.
2. Template: Existing Azure Pipelines YAML File
	- Path: '/.ado/eshoponweb-cd-windows-cm.yml'


```yaml
variables:
- name: resource-group
  value: 'AZ400-RG1'
- name: location
  value: 'westus2'
- name: templateFile
  value: 'infra/simple-windows-vm.bicep'
- name: azureserviceconnection
  value: 'azure subs'
stages:
- stage: Deploy
  displayName: Deploy the Bicep template
  jobs:
  - job: Deploy
    pool:
      vmImage: ubuntu-latest
    steps:
    - task: 6d15af64-176c-496d-b583-fd2ae21d4df4@1
      inputs:
        repository: self
    - task: AzureCLI@2
      inputs:
        azureSubscription: $(azureserviceconnection)
        scriptType: 'bash'
        scriptLocation: 'inlineScript'
        inlineScript: |
          az deployment group create -f $(templateFile) -g $(resource-group) -p location=$(location)
```


## Run the pipeline

If you get the following message ``ERROR: {"code": "InvalidResourceLocation", "message": "The resource 'myPublicIP' already exists in location 'westus2' in resource group 'AZ400-RG1'. A resource with the same name cannot be created in location 'northcentralus'. Please select a new resource name."}`` 

This means that in the pipeline, instead of looking like this:

```yaml
inlineScript: |
          az deployment group create -f $(templateFile) -g $(resource-group) -p location=$(location)
```

You defined it like this:
```yaml
inlineScript: |
  az deployment group create -f $(templateFile) -g $(resource-group) -p location=$(location)

  az deployment group create -f $(templateFile) -g $(resource-group)
```

What's happening is that the pipeline is executing the same deployment twice.

1. `az deployment group create -f $(templateFile) -g $(resource-group) -p location=$(location)`
2. `az deployment group create -f $(templateFile) -g $(resource-group)`

The first execution uses the location defined in the Bicep parameters, and the second uses the pre-defended location of the resource group. In other words, if Resource Group AZ400-RG1 is in northcentralus, the second execution will create the resources in northcentralus.