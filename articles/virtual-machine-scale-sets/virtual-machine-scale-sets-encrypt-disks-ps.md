---
title: 使用 Azure PowerShell 加密 Azure 规模集磁盘 | Microsoft Docs
description: 了解如何使用 Azure PowerShell 来加密 Windows 虚拟机规模集中的 VM 实例和附加的磁盘
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: cynthn
ms.openlocfilehash: f4bf1f35a39042fa38e5d7c7e52c4f8593ffd824
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683519"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell-preview"></a>通过 Azure PowerShell（预览版）对虚拟机规模集中的操作系统和附加的数据磁盘进行加密

虚拟机规模集支持 Azure 磁盘加密 (ADE)，可让用户使用行业标准的加密技术来保护静态数据。 可为 Windows 和 Linux 虚拟机规模集启用加密。 有关详细信息，请参阅[适用于 Windows 和 Linux 的 Azure 磁盘加密](../security/azure-security-disk-encryption.md)。

> [!NOTE]
>  目前已公开发布针对虚拟机规模集的 Azure 磁盘加密，可在所有 Azure 公共区域使用。

下述项支持 Azure 磁盘加密：
- 使用托管磁盘创建的规模集，本机（或未托管）磁盘规模集不受支持。
- Windows 规模集中的 OS 和数据卷。 可对 Windows 规模集的 OS 和数据卷禁用加密功能。
- Linux 规模集中的数据卷。 Linux 规模集的当前预览版中不支持 OS 磁盘加密。

不可在当前预览版中升级规模集 VM 和重置其映像。 建议仅在测试环境中对虚拟机规模集预览版进行 Azure 磁盘加密。 请勿在预览版的生产环境中启用磁盘加密功能，因为你可能需要在加密的规模集中升级 OS 映像。

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](/powershell/azure/install-azurerm-ps)。 如果在本地运行 PowerShell，则还需运行 `Login-AzureRmAccount` 以创建与 Azure 的连接。

## <a name="register-for-disk-encryption-preview"></a>注册磁盘加密预览版

要对虚拟机规模集预览版使用 Azure 磁盘加密功能，需要使用 [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) 自注册订阅。 只需在首次使用磁盘加密预览版功能时执行以下步骤：

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Compute -FeatureName "UnifiedDiskEncryption"
```

传播注册请求最多可能需要 10 分钟。 可以使用 [Get AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature) 检查注册状态。 `RegistrationState` 报告“已注册”时，请使用 [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) 重新注册 Mirosoft.Compute 提供程序：

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.Compute" -FeatureName "UnifiedDiskEncryption"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>创建支持磁盘加密的 Azure Key Vault

Azure 密钥保管库可以存储能够在应用程序和服务中安全实现的密钥、机密或密码。 使用软件保护将加密密钥存储在 Azure 密钥保管库中，或者，可在已通过 FIPS 140-2 级别 2 标准认证的硬件安全模块 (HSM) 中导入或生成密钥。 这些加密密钥用于加密和解密附加到 VM 的虚拟磁盘。 可以控制这些加密密钥，以及审核对它们的使用。

使用 [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) 创建 Key Vault。 若要将 Key Vault 用于磁盘加密，请设置 *EnabledForDiskEncryption* 参数。 以下示例还会定义资源组名称、Key Vault 名称和位置的变量。 请提供自己的唯一 Key Vault 名称：

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzureRmResourceGroup -Name $rgName -Location $location
New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>使用现有的密钥保管库

仅当你要将现有的 Key Vault 用于磁盘加密时，才需要执行此步骤。 如果在上一部分中创建了 Key Vault，请跳过此步骤。

可以使用 [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/AzureRM.KeyVault/Set-AzureRmKeyVaultAccessPolicy)，在规模集所在的同一订阅和区域中启用现有的 Key Vault 进行磁盘加密。 按如下所示，在 *$vaultName* 变量中定义现有 Key Vault 的名称：

```azurepowershell-interactive
$vaultName="myexistingkeyvault"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```

## <a name="create-a-scale-set"></a>创建规模集

首先，使用 [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) 设置 VM 实例的管理员用户名和密码：

```azurepowershell-interactive
$cred = Get-Credential
```

现在，使用 [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) 创建虚拟机规模集。 若要将流量分配到单独的 VM 实例，则还要创建负载均衡器。 负载均衡器包含的规则可在 TCP 端口 80 上分配流量，并允许 TCP 端口 3389 上的远程桌面流量，以及 TCP 端口 5985 上的 PowerShell 远程流量：

```azurepowershell-interactive
$vmssName="myScaleSet"

New-AzureRmVmss `
    -ResourceGroupName $rgName `
    -VMScaleSetName $vmssName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -PublicIpAddressName "myPublicIPAddress" `
    -LoadBalancerName "myLoadBalancer" `
    -UpgradePolicy "Automatic" `
    -Credential $cred
```

## <a name="enable-encryption"></a>启用加密功能

若要在规模集中加密 VM 实例，请先使用 [Get-AzureRmKeyVault](/powershell/module/AzureRM.KeyVault/Get-AzureRmKeyVault) 获取有关 Key Vault URI 和资源 ID 的某些信息。 然后，可以使用 [Set-AzureRmVmssDiskEncryptionExtension](/powershell/module/AzureRM.Compute/Set-AzureRmVmssDiskEncryptionExtension) 结合这些变量来启动加密过程：

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzureRmKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId –VolumeType "All"
```

出现提示时，请键入 *y*，以便继续对规模集 VM 实例执行磁盘加密过程。

## <a name="check-encryption-progress"></a>查看加密进度

若要检查磁盘加密状态，请使用 [Get-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Get-AzureRmVmssDiskEncryption)：

```azurepowershell-interactive
Get-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

加密 VM 实例后，*EncryptionSummary* 代码会报告 *ProvisioningState/succeeded*，如以下示例输出所示：

```powershell
ResourceGroupName            : myResourceGroup
VmScaleSetName               : myScaleSet
EncryptionSettings           :
  KeyVaultURL                : https://myuniquekeyvault.vault.azure.net/
  KeyEncryptionKeyURL        :
  KeyVaultResourceId         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myuniquekeyvault
  KekVaultResourceId         :
  KeyEncryptionAlgorithm     :
  VolumeType                 : All
  EncryptionOperation        : EnableEncryption
EncryptionSummary[0]         :
  Code                       : ProvisioningState/succeeded
  Count                      : 2
EncryptionEnabled            : True
EncryptionExtensionInstalled : True
```

## <a name="disable-encryption"></a>禁用加密功能

如果不再想要使用加密的 VM 实例磁盘，可以使用 [Disable-AzureRmVmssDiskEncryption](/powershell/module/AzureRM.Compute/Disable-AzureRmVmssDiskEncryption) 禁用加密，如下所示：

```azurepowershell-interactive
Disable-AzureRmVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>后续步骤

在本文中，我们已使用 Azure PowerShell 加密了虚拟机规模集。 也可以使用 [Azure CLI](virtual-machine-scale-sets-encrypt-disks-cli.md)，或适用于 [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) 或 [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox) 的模板。
