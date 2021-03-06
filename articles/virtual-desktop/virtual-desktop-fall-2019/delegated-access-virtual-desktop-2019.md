---
title: Accès délégué dans Windows Virtual Desktop - Azure
description: Découvrez comment déléguer des fonctionnalités d’administration lors d'un déploiement de Windows Virtual Desktop, avec exemples.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 899e42e3b592ce1bc92d7d47578e10355b1c6de7
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82613962"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Accès délégué dans Windows Virtual Desktop

>[!IMPORTANT]
>Ce contenu s’applique à la version Automne 2019 qui ne prend pas en charge les objets Azure Resource Manager Windows Virtual Desktop. Si vous essayez de gérer les objets Azure Resource Manager Windows Virtual Desktop introduits dans la mise à jour Printemps 2020, consultez [cet article](../delegated-access-virtual-desktop.md).

Windows Virtual Desktop dispose d’un modèle d’accès délégué qui vous permet de définir les droits d’accès d’un utilisateur particulier en lui attribuant un rôle. Une attribution de rôle se compose de trois éléments : un principal de sécurité, une définition de rôle et une étendue. Le modèle d’accès délégué de Windows Virtual Desktop est basé sur le modèle RBAC d’Azure. Pour en savoir plus sur les attributions de rôle spécifique et leurs composants, consultez [la vue d’ensemble du contrôle d'accès en fonction du rôle Azure](../../role-based-access-control/built-in-roles.md).

L’accès délégué de Windows Virtual Desktop prend en charge les valeurs suivantes pour chaque élément de l’attribution de rôle :

* Principal de sécurité
    * Utilisateurs
    * Principaux de service
* Définition de rôle
    * Rôles intégrés
* Étendue
    * Groupes de locataires
    * Locataires
    * Pools hôtes
    * Groupes d’applications

## <a name="built-in-roles"></a>Rôles intégrés

L’accès délégué dans Windows Virtual Desktop a plusieurs définitions de rôles intégrés que vous pouvez affecter aux utilisateurs et principaux de service.

* Un propriétaire RDS peut tout gérer, y compris l’accès aux ressources.
* Un contributeur RDS (Services Bureau à distance) peut tout gérer, mais il n’a pas accès aux ressources.
* Un lecteur RDS (Services Bureau à distance) peut tout voir, mais il ne peut apporter aucune modification.
* Un opérateur RDS peut consulter les activités de diagnostic.

## <a name="powershell-cmdlets-for-role-assignments"></a>Cmdlets Azure PowerShell pour les attributions de rôles

Vous pouvez exécuter les cmdlets suivantes pour créer, afficher et supprimer des attributions de rôles :

* **Get-RdsRoleAssignment** pour afficher une liste des attributions de rôles.
* **Get-RdsRoleAssignment** pour créer une nouvelle attribution de rôle.
* **Remove-RdsRoleAssignment** pour supprimer les attributions de rôles.

### <a name="accepted-parameters"></a>Paramètres acceptés

Vous pouvez modifier les trois cmdlets de base avec les paramètres suivants :

* **AadTenantId** : spécifie l’ID du locataire Azure Active Directory dont le principal du service est membre.
* **AppGroupName** : nom du groupe d’application du Bureau à distance.
* **Diagnostics** : indique la portée des diagnostics. (Doit être associé aux paramètres **Infrastructure** ou **Locataire**)
* **HostPoolName** : nom du pool d’hôte du Bureau à distance.
* **Infrastructure** : indique la portée de l’infrastructure.
* **RoleDefinitionName** : nom du rôle de contrôle d’accès en fonction du rôle des Services Bureau à distance attribué à l’utilisateur, au groupe ou à l’application. (Par exemple, propriétaire des Services Bureau à distance, lecteur des Services Bureau à distance et ainsi de suite.)
* **ServerPrincipleName** : nom de l’application Azure Active Directory.
* **SignInName** : adresse e-mail de l’utilisateur ou nom d’utilisateur principal.
* **TenantName** : nom du locataire du Bureau à distance.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir une liste plus complète des cmdlets PowerShell que chaque rôle peut utiliser, consultez les [Informations de référence PowerShell](/powershell/windows-virtual-desktop/overview).

Pour obtenir des instructions et savoir comment configurer un environnement Bureau virtuel Windows, consultez [Environnement dans Windows Virtual Desktop](environment-setup-2019.md).
