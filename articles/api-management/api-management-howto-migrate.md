---
title: Migrer une instance Gestion des API Azure d'une région vers une autre
description: Apprenez à migrer une instance Gestion des API d'une région vers une autre.
services: api-management
documentationcenter: ''
author: miaojiang
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/26/2019
ms.author: apimpm
ms.openlocfilehash: 3294a7b2112e9527041ef343f4452aedb7a2a272
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70073461"
---
# <a name="how-to-migrate-azure-api-management-across-regions"></a>Migrer une instance Gestion des API Azure d'une région vers une autre
Pour migrer des instances Gestion des API d'une région Azure vers une autre, vous pouvez utiliser la fonctionnalité [de sauvegarde et de restauration](api-management-howto-disaster-recovery-backup-restore.md). Vous devez choisir le même niveau tarifaire Gestion des API dans les régions source et cible. 

> [!NOTE]
> La sauvegarde et la restauration ne fonctionneront pas lors de la migration entre différents types de cloud. Pour cela, vous devez exporter la ressource [en tant que modèle](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-portal#export-resource-groups-to-templates). Adaptez ensuite le modèle exporté à la région Azure cible et recréez la ressource. 

## <a name="option-1-use-a-different-api-management-instance-name"></a>Option 1 : Utiliser un nom d'instance Gestion des API différent

1. Dans la région cible, créez une nouvelle instance Gestion des API avec le même niveau tarifaire que l'instance source. La nouvelle instance doit porter un nom différent. 
1. Sauvegardez l'instance Gestion des API existante sur un compte de stockage.
1. Restaurez la sauvegarde créée à l'étape 2 sur la nouvelle instance Gestion des API créée dans la nouvelle région à l'étape 1.
1. Si vous disposez d'un domaine personnalisé pointant vers l'instance Gestion des API de la région source, mettez à jour l'enregistrement CNAME du domaine personnalisé pour qu'il pointe vers la nouvelle instance Gestion des API. 


## <a name="option-2-use-the-same-api-management-instance-name"></a>Option n°2 : Utiliser le même nom d'instance Gestion des API

> [!NOTE]
> Cette option entraînera un temps d'arrêt pendant la migration.

1. Sauvegardez l'instance Gestion des API de la région source sur un compte de stockage.
1. Supprimez l'instance Gestion des API de la région source. 
1. Créez une nouvelle instance Gestion des API dans la région cible en lui attribuant le même nom que celle de la région source.
1. Restaurez la sauvegarde créée à l'étape 1 sur la nouvelle instance Gestion des API de la région cible.  


## <a name="next-steps"></a><a name="next-steps"> </a>Étapes suivantes
* Pour plus d'informations sur la fonctionnalité de sauvegarde et de restauration, consultez [Implémenter la récupération d'urgence](api-management-howto-disaster-recovery-backup-restore.md).
* Pour plus d'informations sur les ressources de migration Azure, consultez [Conseils sur la migration entre régions Azure](https://github.com/Azure/Azure-Migration-Guidance).