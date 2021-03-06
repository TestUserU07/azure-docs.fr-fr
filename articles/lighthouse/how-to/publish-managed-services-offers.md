---
title: Publier une offre de services gérés sur la place de marché Azure
description: Découvrez comment publier une offre de service géré qui intègre des clients à la gestion des ressources déléguées Azure.
ms.date: 05/04/2020
ms.topic: conceptual
ms.openlocfilehash: 04da33d0232371f4175a935bf1e35925376babbc
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82788737"
---
# <a name="publish-a-managed-service-offer-to-azure-marketplace"></a>Publier une offre de services gérés sur la place de marché Azure

Dans cet article, vous allez découvrir comment publier une offre de service managé publique ou privée sur la [Place de marché Azure](https://azuremarketplace.microsoft.com) à l’aide du programme [Place de marché commerciale](../../marketplace/partner-center-portal/commercial-marketplace-overview.md) dans l’Espace partenaires. Les clients qui achètent l’offre peuvent alors intégrer des abonnements et des groupes de ressources pour la [gestion des ressources déléguées Azure](../concepts/azure-delegated-resource-management.md).

## <a name="publishing-requirements"></a>Exigences de publication

Pour créer et publier des offres, vous devez disposer d’un [compte valide dans l’Espace partenaires](../../marketplace/partner-center-portal/create-account.md). Si vous n’avez pas encore de compte, le [processus d’inscription](https://aka.ms/joinmarketplace) vous guide lors des étapes de création de compte dans l’Espace partenaires et d’inscription au programme de la Place de marché commerciale.

Conformément aux [exigences de certification des offres de services managés](https://docs.microsoft.com/legal/marketplace/certification-policies#7004-business-requirements), vous devez disposer d’un [niveau de compétence de plateforme cloud Silver ou Gold](https://docs.microsoft.com/partner-center/learn-about-competencies) ou être [Fournisseur de services managés Azure Expert](https://partner.microsoft.com/membership/azure-expert-msp) pour publier une offre de services managés.

Votre ID Microsoft Partner Network (MPN) est [automatiquement associé](../../billing/billing-partner-admin-link-started.md) aux offres que vous publiez pour suivre votre impact sur les engagements client.

> [!NOTE]
> Si vous ne souhaitez pas publier d’offre sur la Place de marché Azure, vous pouvez intégrer des clients manuellement à l’aide de modèles Azure Resource Manager. Pour plus d’informations, voir [Intégrer un client dans la gestion des ressources déléguées Azure](onboard-customer.md).

## <a name="create-your-offer"></a>Créer votre offre

Pour obtenir des instructions détaillées sur la création de votre offre, y compris l’ensemble des informations et ressources que vous devrez fournir, consultez [Créer une offre de service managé](../../marketplace/partner-center-portal/create-new-managed-service-offer.md).

Pour en savoir plus sur le processus général de publication, consultez [Guide de l’éditeur Place de marché Microsoft Azure et AppSource](../../marketplace/marketplace-publishers-guide.md). Vous devez également examiner les [stratégies de certification de la Place de marché commerciale](https://docs.microsoft.com/legal/marketplace/certification-policies), en particulier la section [Managed Services](https://docs.microsoft.com/legal/marketplace/certification-policies#700-managed-services).

Une fois qu’un client a ajouté votre offre, il est en mesure de déléguer un ou plusieurs abonnements ou groupes de ressources, qui seront ensuite [intégrés pour la gestion des ressources déléguées Azure](#the-customer-onboarding-process).

> [!IMPORTANT]
> Chaque plan d’une offre de service managé comprend une section **Détails du manifeste** dans laquelle vous définissez les entités Azure Active Directory (Azure AD) de votre locataire qui ont accès aux groupes de ressources ou aux abonnements délégués pour les clients qui achètent ce plan. Il est important de savoir que tout groupe (ou utilisateur ou principal de service) que vous incluez aura les mêmes autorisations pour chaque client achetant le plan. Pour affecter différents groupes à chaque client, vous devez publier un [plan privé](../../marketplace/private-offers.md) distinct exclusif pour chaque client.

## <a name="publish-your-offer"></a>Publier votre offre

Lorsque vous avez terminé chacune des sections, l’étape suivante consiste à publier l’offre sur la Place de marché Azure. Sélectionnez le bouton **Publier** pour démarrer le processus de mise en ligne de votre offre. Pour plus d’informations sur ce processus, consultez cette [page](../../marketplace/partner-center-portal/create-new-managed-service-offer.md#publish). 

Vous pouvez [publier une version mise à jour de votre offre](../..//marketplace/partner-center-portal/update-existing-offer.md) à tout moment. Par exemple, vous pouvez souhaiter ajouter une nouvelle définition de rôle à une offre publiée précédemment. Dans ce cas, les clients qui ont déjà ajouté l’offre voient s’afficher une icône sur la page [**Fournisseurs de services**](view-manage-service-providers.md) du Portail Azure, ce qui leur permet de savoir qu’une mise à jour est disponible. Chaque client pourra [passer en revue les modifications](view-manage-service-providers.md#update-service-provider-offers) et décider s’il souhaite effectuer la mise à jour vers la nouvelle version. 

## <a name="the-customer-onboarding-process"></a>Processus d’intégration des clients

Une fois qu’un client a ajouté votre offre, il est en mesure de [déléguer un ou plusieurs abonnements ou groupes de ressources spécifiques](view-manage-service-providers.md#delegate-resources), qui seront ensuite intégrés pour la gestion des ressources déléguées Azure. Si un client a accepté une offre mais n’a pas encore délégué de ressources, il voit une note s’afficher en haut de la section **Offres de fournisseur** de la page [**Fournisseurs de services**](view-manage-service-providers.md) du portail Azure.

> [!IMPORTANT]
> La délégation doit être effectuée par un compte non invité dans le locataire client qui a le [rôle intégré Propriétaire](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) pour l’abonnement en cours d’intégration (ou qui contient les groupes de ressources en cours d’intégration). Pour voir tous les utilisateurs qui peuvent déléguer l’abonnement, un utilisateur du locataire du client peut sélectionner l’abonnement dans le portail Azure, ouvrir **Contrôle d’accès (IAM)** et [afficher tous les utilisateurs ayant le rôle Propriétaire](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription).

Une fois que le client aura délégué un abonnement (ou un ou plusieurs groupes de ressources d’un abonnement), le fournisseur de ressources **Microsoft.ManagedServices** sera inscrit pour cet abonnement, et les utilisateurs de votre locataire pourront accéder aux ressources déléguées conformément aux autorisations de votre offre.

## <a name="next-steps"></a>Étapes suivantes

- Apprenez-en davantage sur la [Place de marché commerciale](../../marketplace/partner-center-portal/commercial-marketplace-overview.md).
- Découvrez les [Expériences de gestion inter-locataire](../concepts/cross-tenant-management-experience.md).
- [Affichez et gérez les clients](view-manage-customers.md) en accédant à **Mes clients** sur le portail Azure.
