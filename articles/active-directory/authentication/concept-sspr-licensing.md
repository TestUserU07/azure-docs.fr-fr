---
title: Licence de réinitialisation de mot de passe libre-service - Azure Active Directory
description: Découvrir les différents besoins de licence pour la réinitialisation de mot de passe en libre-service Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45ca11af061e37cf4f804ce2d7ceed72a9448294
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393076"
---
# <a name="licensing-requirements-for-azure-active-directory-self-service-password-reset"></a>Licences requises pour la réinitialisation de mot de passe en libre-service Azure Active Directory

Si vous souhaitez réduire le nombre d’appels au support technique et la perte de productivité quand un utilisateur ne parvient pas à se connecter à son appareil ou à une application, vous pouvez activer la technologie SSPR (réinitialisation de mot de passe en libre-service) pour les comptes d’utilisateurs Azure AD (Azure Active Directory). Les fonctionnalités SSPR incluent le changement, la réinitialisation, le déverrouillage et la réécriture de mot de passe dans un annuaire local. Les fonctionnalités SSPR de base sont disponibles gratuitement dans Office 365 et pour tous les utilisateurs Azure AD.

Cet article détaille les différentes façons d’obtenir et d’utiliser la licence de réinitialisation de mot de passe en libre-service. Pour des détails spécifiques sur les prix et la facturation, consultez la [page de tarification d’Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Comparer les éditions et les fonctionnalités

La technologie SSPR est concédée sous licence par utilisateur. Pour des raisons de conformité, les organisations doivent affecter la licence appropriée à leurs utilisateurs.

Le tableau suivant présente les différents scénarios SSPR liés au changement, à la réinitialisation ou à la réécriture locale de mot de passe ainsi que les références SKU qui fournissent la fonctionnalité correspondante.

| Fonctionnalité | Azure AD Gratuit | Office 365 Business Premium | Microsoft 365 Business | Azure AD Premium P1 ou P2 |
| --- |:---:|:---:|:---:|:---:|
| **Changement de mot de passe utilisateur cloud uniquement**<br />Quand un utilisateur d’Azure AD connaît son mot de passe et souhaite le remplacer par un nouveau. | ● | ● | ● | ● |
| **Réinitialisation de mot de passe utilisateur cloud uniquement**<br />Quand un utilisateur d’Azure AD a oublié son mot de passe et doit le réinitialiser. | | ● | ● | ● |
| **Changement ou réinitialisation de mot de passe utilisateur hybride avec réécriture locale**<br />Quand un utilisateur d’Azure AD synchronisé à partir d’un annuaire local via Azure AD Connect souhaite changer ou réinitialiser son mot de passe mais également réécrire le nouveau mot de passe localement. | | | ● | ● |

> [!WARNING]
> Les plans de licence Office 365 autonomes ne prennent pas en charge SSPR avec réécriture locale. Ces plans de licence Office 365 nécessitent Azure AD Premium P1, Premium P2 ou Microsoft 365 Business pour que cette fonctionnalité marche.

Pour plus d’informations sur les licences, notamment les coûts, consultez les pages suivantes :

* [Tarification d’Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [Fonctionnalités Azure Active Directory](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Entreprise](https://www.microsoft.com/microsoft-365/enterprise)
* [Microsoft 365 Business](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description)

## <a name="enable-group-or-user-based-licensing"></a>Activer les licences utilisateur ou groupe

Azure AD prend en charge les licences basées sur les groupes. Les administrateurs peuvent attribuer des licences en bloc à un groupe d’utilisateurs, plutôt que de les attribuer une à la fois. Pour plus d’informations, consultez [Affecter, vérifier et résoudre les problèmes de licences](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Selon le lieu où vous vous trouvez, certains services Microsoft ne sont pas disponibles. Avant de pouvoir attribuer une licence à un utilisateur, l’administrateur doit spécifier la propriété **Emplacement d’utilisation** sur l’utilisateur. Vous pouvez effectuer l’attribution de licences dans la section **Utilisateur** > **Profil** > **Paramètres** du portail Azure. *Quand vous attribuez une licence à un groupe, tous les utilisateurs sans emplacement d’utilisation spécifié héritent de l’emplacement de l’annuaire.*

## <a name="next-steps"></a>Étapes suivantes

Pour bien démarrer avec SSPR, suivez le tutoriel suivant :

> [!div class="nextstepaction"]
> [Tutoriel : Activer SSPR (réinitialisation de mot de passe en libre-service)](tutorial-enable-sspr.md)
