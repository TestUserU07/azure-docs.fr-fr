---
title: Détecter un problème de connexion au service dans Windows Virtual Desktop – Azure
description: Comment résoudre des problèmes lorsque vous configurez des connexions à un client dans un environnement de locataire Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 05/20/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 7bf05fe039de2ab9e25495f9e2652fde8fac34e1
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83747698"
---
# <a name="windows-virtual-desktop-service-connections"></a>Connexions au service Windows Virtual Desktop

>[!IMPORTANT]
>Ce contenu s’applique à la mise à jour Printemps 2020 avec des objets Azure Resource Manager Windows Virtual Desktop. Si vous utilisez la version Automne 2019 de Windows Virtual Desktop sans objets Azure Resource Manager, consultez [cet article](./virtual-desktop-fall-2019/troubleshoot-service-connection-2019.md).
>
> La mise à jour Printemps 2020 de Windows Virtual Desktop est en préversion publique. Cette préversion est fournie sans contrat de niveau de service et nous déconseillons son utilisation pour les charges de travail de production. Certaines fonctionnalités peuvent être limitées ou non prises en charge. 
> Pour plus d’informations, consultez [Conditions d’Utilisation Supplémentaires relatives aux Évaluations Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Appuyez-vous sur cet article pour résoudre les problèmes liés aux connexions à un client Windows Virtual Desktop.

## <a name="provide-feedback"></a>Fournir des commentaires

Vous pouvez nous fournir des commentaires et discuter du service Windows Virtual Desktop avec l’équipe de produit et d’autres membres actifs de la communauté dans [Windows Virtual Desktop Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop).

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Lorsqu’un utilisateur se connecte, rien ne s’affiche (aucun flux)

Si l’utilisateur peut démarrer les clients Bureau à distance et s’authentifier, il ne voit cependant aucune icône dans le flux de découverte web.

Vérifiez que l’utilisateur signalant les problèmes a été affecté aux groupes d’applications à l’aide de cette ligne de commande :

```PowerShell
Get-AzRoleAssignment -SignInName <userupn>
```

Vérifiez que l’utilisateur se connecte avec les informations d’identification correctes.

Si le client web est utilisé, vérifiez qu’il n’y a aucun problème d’informations d’identification mises en cache.

## <a name="next-steps"></a>Étapes suivantes

- Pour découvrir une vue d’ensemble de la résolution des problèmes Windows Virtual Desktop et des procédures d’escalade, consultez l’article [Vue d’ensemble du dépannage, commentaires et support](troubleshoot-set-up-overview.md).
- Pour résoudre des problèmes lors de la création d’un environnement Windows Virtual Desktop et d’un pool d’hôtes dans un environnement Windows Virtual Desktop, consultez [Création d’un environnement et d’un pool d’hôtes](troubleshoot-set-up-issues.md).
- Pour résoudre les problèmes de configuration d’une machine virtuelle dans Windows Virtual Desktop, consultez [Configuration d’une machine virtuelle hôte de session](troubleshoot-vm-configuration.md).
- Pour résoudre les problèmes d’utilisation de PowerShell avec Windows Virtual Desktop, consultez [Windows Virtual Desktop PowerShell](troubleshoot-powershell.md).
- Suivez le [Didacticiel : Résoudre les problèmes liés aux déploiements de modèles Resource Manager](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
