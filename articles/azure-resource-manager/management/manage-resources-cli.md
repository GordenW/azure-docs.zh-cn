---
title: 管理资源 - Azure CLI
description: 使用 Azure CLI 和 Azure 资源管理器管理资源。 演示如何部署和删除资源。
author: mumian
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: 8ecaf7a7df80cbb62bcb27373c47e26c70108d1e
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2020
ms.locfileid: "87827004"
---
# <a name="manage-azure-resources-by-using-azure-cli"></a>使用 Azure CLI 管理 Azure 资源

了解如何将 Azure CLI 与 [Azure 资源管理器](overview.md)配合使用来管理 Azure 资源。 若要管理资源组，请参阅[使用 Azure CLI 管理 Azure 资源组](manage-resource-groups-cli.md)。

有关资源管理的其他文章：

- [使用 Azure 门户管理 Azure 资源](manage-resources-portal.md)
- [使用 Azure PowerShell 管理 Azure 资源](manage-resources-powershell.md)

## <a name="deploy-resources-to-an-existing-resource-group"></a>将资源部署到现有的资源组

可以使用 Azure CLI 直接部署 Azure 资源，也可以通过部署资源管理器模板来创建 Azure 资源。

### <a name="deploy-a-resource"></a>部署资源

以下脚本创建一个存储帐户。

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az storage account create --resource-group $resourceGroupName --name $storageAccountName --location $location --sku Standard_LRS --kind StorageV2 &&
az storage account show --resource-group $resourceGroupName --name $storageAccountName 
```

### <a name="deploy-a-template"></a>部署模板

以下脚本通过部署快速入门模板来创建存储帐户。 有关详细信息，请参阅[快速入门：使用 Visual Studio Code 创建 Azure 资源管理器模板](../templates/quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell)。

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the location (i.e. centralus):" &&
read location &&
az group deployment create --resource-group $resourceGroupName --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
```

有关详细信息，请参阅[使用资源管理器模板和 Azure CLI 部署资源](../templates/deploy-cli.md)。

## <a name="deploy-a-resource-group-and-resources"></a>部署资源组和资源

可以创建一个资源组，然后将资源部署到该组。 有关详细信息，请参阅[创建资源组并部署资源](../templates/deploy-to-subscription.md#resource-groups)。

## <a name="deploy-resources-to-multiple-subscriptions-or-resource-groups"></a>将资源部署到多个订阅或资源组

通常情况下，将模板中的所有资源部署到单个资源组。 不过，在某些情况下，你可能希望将一组资源部署在一起但将其放置在不同的资源组或订阅中。 有关详细信息，请参阅[将 Azure 资源部署到多个订阅或资源组](../templates/cross-scope-deployment.md)。

## <a name="delete-resources"></a>删除资源

以下脚本演示如何删除存储帐户。

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az storage account delete --resource-group $resourceGroupName --name $storageAccountName 
```

若要详细了解 Azure 资源管理器如何控制资源的删除，请参阅 [Azure 资源管理器资源组的删除](delete-resource-group.md)。

## <a name="move-resources"></a>移动资源

以下脚本演示如何将存储帐户从一个资源组移到另一个资源组。

```azurecli-interactive
echo "Enter the source Resource Group name:" &&
read srcResourceGroupName &&
echo "Enter the destination Resource Group name:" &&
read destResourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
storageAccount=$(az resource show --resource-group $srcResourceGroupName --name $storageAccountName --resource-type Microsoft.Storage/storageAccounts --query id --output tsv) &&
az resource move --destination-group $destResourceGroupName --ids $storageAccount
```

有关详细信息，请参阅[将资源移到新资源组或订阅](move-resource-group-and-subscription.md)。

## <a name="lock-resources"></a>锁定资源

锁定可以防止组织中的其他用户意外删除或修改关键资源，例如 Azure 订阅、资源组或资源。 

以下脚本锁定一个存储帐户，因此无法删除该帐户。

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az lock create --name LockSite --lock-type CanNotDelete --resource-group $resourceGroupName --resource-name $storageAccountName --resource-type Microsoft.Storage/storageAccounts 
```

以下脚本获取存储帐户的所有锁：

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
az lock list --resource-group $resourceGroupName --resource-name $storageAccountName --resource-type Microsoft.Storage/storageAccounts --parent ""
```

以下脚本删除存储帐户的锁：

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the storage account name:" &&
read storageAccountName &&
lockId=$(az lock show --name LockSite --resource-group $resourceGroupName --resource-type Microsoft.Storage/storageAccounts --resource-name $storageAccountName --output tsv --query id)&&
az lock delete --ids $lockId
```

更多信息请参阅 [使用 Azure Resource Manager 锁定资源](lock-resources.md)。

## <a name="tag-resources"></a>标记资源

标记有助于按逻辑方式组织资源组和资源。 有关信息，请参阅[使用标记组织 Azure 资源](tag-resources.md#azure-cli)。

## <a name="manage-access-to-resources"></a>管理对资源的访问

Azure [RBAC) 的 azure 基于角色的访问控制 (](../../role-based-access-control/overview.md)是管理 azure 中资源访问权限的方式。 有关详细信息，请参阅[使用 RBAC 和 Azure CLI 管理访问权限](../../role-based-access-control/role-assignments-cli.md)。

## <a name="next-steps"></a>后续步骤

- 若要了解 Azure 资源管理器，请参阅 [Azure 资源管理器概述](overview.md)。
- 若要了解资源管理器模板语法，请参阅[了解 Azure 资源管理器模板的结构和语法](../templates/template-syntax.md)。
- 若要了解如何开发模板，请参阅[分步教程](../index.yml)。
- 若要查看 Azure 资源管理器模板架构，请参阅[模板参考](/azure/templates/)。
