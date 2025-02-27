---
title: Enable VM extension using Azure PowerShell
description: This article describes how to deploy virtual machine extensions to Azure Arc-enabled servers running in hybrid cloud environments using Azure PowerShell.
ms.date: 08/05/2021
ms.topic: conceptual 
ms.custom: devx-track-azurepowershell
---

# Enable Azure VM extensions using Azure PowerShell

This article shows you how to deploy and uninstall Azure VM extensions, supported by Azure Arc-enabled servers, to a Linux or Windows hybrid machine using Azure PowerShell.

> [!NOTE]
> Azure Arc-enabled servers does not support deploying and managing VM extensions to Azure virtual machines. For Azure VMs, see the following [VM extension overview](../../virtual-machines/extensions/overview.md) article.

## Prerequisites

- A computer with Azure PowerShell. For instructions, see [Install and configure Azure PowerShell](/powershell/azure/).

Before using Azure PowerShell to manage VM extensions on your hybrid server managed by Azure Arc-enabled servers, you need to install the `Az.ConnectedMachine` module. These management operations can be performed from your workstation, you don't need to run them on the Azure Arc-enabled server.

Run the following command on your Azure Arc-enabled server:

`Install-Module -Name Az.ConnectedMachine`.

When the installation completes, the following message is returned:

`The installed extension `Az.ConnectedMachine` is experimental and not covered by customer support. Please use with discretion.`

## Enable extension

To enable a VM extension on your Azure Arc-enabled server, use [New-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/new-azconnectedmachineextension) with the `-Name`, `-ResourceGroupName`, `-MachineName`, `-Location`, `-Publisher`, -`ExtensionType`, and `-Settings` parameters.

The following example enables the Log Analytics VM extension on a Azure Arc-enabled Linux server:

```powershell
$Setting = @{ "workspaceId" = "workspaceId" }
$protectedSetting = @{ "workspaceKey" = "workspaceKey" }
New-AzConnectedMachineExtension -Name OMSLinuxAgent -ResourceGroupName "myResourceGroup" -MachineName "myMachine" -Location "regionName" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -Settings $Setting -ProtectedSetting $protectedSetting -ExtensionType "OmsAgentForLinux"
```

To enable the Log Analytics VM extension on an Azure Arc-enabled Windows server, change the value for the `-ExtensionType` parameter to `"MicrosoftMonitoringAgent"` in the previous example.

The following example enables the Custom Script Extension on an Azure Arc-enabled server:

```powershell
$Setting = @{ "commandToExecute" = "powershell.exe -c Get-Process" }
New-AzConnectedMachineExtension -Name "custom" -ResourceGroupName "myResourceGroup" -MachineName "myMachineName" -Location "regionName" -Publisher "Microsoft.Compute"  -Settings $Setting -ExtensionType CustomScriptExtension
```

The following example enables the Microsoft Antimalware extension on an Azure Arc-enabled Windows server:

```powershell
$Setting = @{ "AntimalwareEnabled" = $true }
New-AzConnectedMachineExtension -Name "IaaSAntimalware" -ResourceGroupName "myResourceGroup" -MachineName "myMachine" -Location "regionName" -Publisher "Microsoft.Azure.Security" -Settings $Setting -ExtensionType "IaaSAntimalware"
```

### Key Vault VM extension

> [!WARNING]
> PowerShell clients often add `\` to `"` in the settings.json which will cause akvvm_service fails with error: `[CertificateManagementConfiguration] Failed to parse the configuration settings with:not an object.`

The following example enables the Key Vault VM extension on an Azure Arc-enabled server:

```powershell
# Build settings
    $settings = @{
      secretsManagementSettings = @{
       observedCertificates = @(
        "observedCert1"
       )
      certificateStoreLocation = "myMachineName" # For Linux use "/var/lib/waagent/Microsoft.Azure.KeyVault.Store/"
      certificateStore = "myCertificateStoreName"
      pollingIntervalInS = "pollingInterval"
      }
    authenticationSettings = @{
     msiEndpoint = "http://localhost:40342/metadata/identity"
     }
    }

    $resourceGroup = "resourceGroupName"
    $machineName = "myMachineName"
    $location = "regionName"

    # Start the deployment
    New-AzConnectedMachineExtension -ResourceGroupName $resourceGRoup -Location $location -MachineName $machineName -Name "KeyVaultForWindows or KeyVaultforLinux" -Publisher "Microsoft.Azure.KeyVault" -ExtensionType "KeyVaultforWindows or KeyVaultforLinux" -Setting (ConvertTo-Json $settings)
```

## List extensions installed

To get a list of the VM extensions on your Azure Arc-enabled server, use [Get-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/get-azconnectedmachineextension) with the `-MachineName` and `-ResourceGroupName` parameters.

Example:

```powershell
Get-AzConnectedMachineExtension -ResourceGroupName myResourceGroup -MachineName myMachineName

Name    Location  PropertiesType        ProvisioningState
----    --------  --------------        -----------------
custom  westus2   CustomScriptExtension Succeeded
```

## Remove an installed extension

To remove an installed VM extension on your Azure Arc-enabled server, use [Remove-AzConnectedMachineExtension](/powershell/module/az.connectedmachine/remove-azconnectedmachineextension) with the `-Name`, `-MachineName` and `-ResourceGroupName` parameters.

For example, to remove the Log Analytics VM extension for Linux, run the following command:

```powershell
Remove-AzConnectedMachineExtension -MachineName myMachineName -ResourceGroupName myResourceGroup -Name OmsAgentforLinux
```

## Next steps

- You can deploy, manage, and remove VM extensions using the [Azure CLI](manage-vm-extensions-cli.md), from the [Azure portal](manage-vm-extensions-portal.md), or [Azure Resource Manager templates](manage-vm-extensions-template.md).

- Troubleshooting information can be found in the [Troubleshoot VM extensions guide](troubleshoot-vm-extensions.md).
