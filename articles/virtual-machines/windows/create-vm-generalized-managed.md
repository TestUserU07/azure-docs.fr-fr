---
title: Créer une machine virtuelle à partir d’une image managée dans Azure
description: Créez une machine virtuelle Windows à partir d’une image managée généralisée à l’aide d’Azure PowerShell ou du portail.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: b0c6689b66037067a4c5174738945b7c6fabd5b5
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086315"
---
# <a name="create-a-vm-from-a-managed-image"></a>Créer une machine virtuelle à partir d’une image gérée

Vous pouvez créer plusieurs machines virtuelles à partir d’une image de machine virtuelle managée Azure à l’aide de PowerShell ou du portail Azure. Une image de machine virtuelle managée contient les informations nécessaires pour créer une machine virtuelle, y compris le disque du système d’exploitation et les disques de données. Les disques durs virtuels qui composent l’image, dont les disques du système d’exploitation et les disques de données, sont stockés en tant que disques managés. 

Avant de créer une machine virtuelle, vous devez [créer une image de machine virtuelle managée](capture-image-resource.md) à utiliser comme image source et accorder un accès en lecture sur l’image aux utilisateurs qui doivent avoir accès à l’image. 


## <a name="use-the-portal"></a>Utiliser le portail

1. Accédez au [Portail Azure](https://portal.azure.com) pour rechercher une image managée. Recherchez et sélectionnez **Images**.
3. Sélectionnez l’image à utiliser dans la liste. La page **Vue d’ensemble** de l’image s’ouvre.
4. Sélectionnez **Créer une machine virtuelle** dans le menu.
5. Saisissez les informations de la machine virtuelle. Le nom d’utilisateur et le mot de passe que vous avez entrés vous serviront pour vous connecter à la machine virtuelle. Quand vous avez terminé, sélectionnez **OK**. Vous pouvez créer la machine virtuelle dans un groupe de ressources existant ou sélectionner **Créer nouveau** pour créer un groupe de ressources pour stocker la machine virtuelle.
6. Choisissez la taille de la machine virtuelle. Pour voir plus de tailles, sélectionnez **Afficher tout** ou modifiez le filtre **Type de disque pris en charge**. 
7. Sous **Paramètres**, procédez aux modifications nécessaires et cliquez sur **OK**. 
8. Dans la page Résumé, vous pouvez voir le nom de votre image répertorié comme **Image privée**. Cliquez sur **OK** pour démarrer le déploiement de la machine virtuelle.


## <a name="use-powershell"></a>Utiliser PowerShell

Vous pouvez utiliser PowerShell pour créer une machine virtuelle à partir d’une image à l’aide du paramètre simplifié défini pour la cmdlet [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). L’image doit se trouver dans le même groupe de ressources que celui dans lequel vous allez créer la machine virtuelle.

 

Le paramètre simplifié défini pour [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) nécessite uniquement un nom, un groupe de ressources et un nom d’image pour créer une machine virtuelle à partir d’une image. New-AzVm utilise la valeur du paramètre **-Name** comme nom de toutes les ressources qu’il crée automatiquement. Dans cet exemple, nous fournissons des noms plus détaillés pour chaque ressource, mais laissons l’applet de commande les créer automatiquement. Vous pouvez également créer des ressources en avance, comme le réseau virtuel, et passer le nom de ressource dans l’applet de commande. New-AzVm utilisera les ressources existantes s’il peut les trouver par leur nom.

L’exemple suivant crée une machine virtuelle nommée *myVMfromImage* dans le groupe de ressources *myResourceGroup*, à partir de l’image nommée *myImage*. 


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Étapes suivantes
[Créer et gérer des machines virtuelles Windows avec le module Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

