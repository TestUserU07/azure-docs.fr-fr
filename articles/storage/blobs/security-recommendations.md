---
title: Recommandations de sécurité pour Stockage Blob
titleSuffix: Azure Storage
description: Découvrez les recommandations de sécurité pour Stockage Blob. La mise en œuvre de ces conseils vous aidera à répondre à vos obligations de sécurité, comme décrit dans notre modèle de responsabilité partagée.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: tamram
ms.custom: security-recommendations
ms.openlocfilehash: 0b32f7e8fa2ec0d1d28f2fd42147e140d2d03341
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086111"
---
# <a name="security-recommendations-for-blob-storage"></a>Recommandations de sécurité pour Stockage Blob

Cet article contient des recommandations de sécurité pour Stockage Blob. Mettez en œuvre ces recommandations pour répondre à vos obligations de sécurité comme décrit dans notre modèle de responsabilité partagée. Pour plus d’informations sur les mesures prises par Microsoft pour répondre à ses responsabilités de fournisseur de services, lisez [Responsabilités partagées pour le cloud computing](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/225366/1/Shared%20Responsibility%20for%20Cloud%20Computing-2019-10-25.pdf).

Certaines recommandations contenues dans cet article peuvent être supervisées automatiquement par Azure Security Center. Azure Security Center est la première ligne de défense dans la protection de vos ressources Azure. Pour plus d’informations sur Azure Security Center, consultez [Présentation d’Azure Security Center](../../security-center/security-center-intro.md).

Azure Security Center analyse périodiquement l’état de sécurité de vos ressources Azure pour identifier les vulnérabilités de sécurité potentielles. Il fournit ensuite des recommandations sur la façon de les corriger. Pour plus d’informations sur les recommandations Azure Security Center, consultez [Recommandations de sécurité dans Azure Security Center](../../security-center/security-center-recommendations.md).

## <a name="data-protection"></a>Protection de données

| Recommandation | Commentaires | Security Center |
|-|----|--|
| Utiliser le Modèle de déploiement Azure Resource Manager | Créez des comptes de stockage en utilisant le Modèle de déploiement Azure Resource Manager pour les améliorations de sécurité importantes, notamment les fonctionnalités de contrôle d’accès (RBAC) et d’audit de premier plan, le déploiement et la gouvernance basés sur Resource Manager, l’accès aux identités managées, l’accès pour Azure Key Vault pour les secrets, et l’authentification et l’autorisation basées sur Azure AD pour l’accès aux données et aux ressources de Stockage Azure. Si possible, migrez les comptes de stockage existants qui utilisent le Modèle de déploiement classique pour qu’ils utilisent Azure Resource Manager. Pour plus d’informations sur Azure Resource Manager, consultez [Vue d’ensemble d’Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview). | - |
| Activer l’option **Transfert sécurisé requis** sur tous vos comptes de stockage | Quand vous activez l’option **Transfert sécurisé requis**, toutes les demandes effectuées auprès du compte de stockage doivent se faire via des connexions sécurisées. Toutes les demandes effectuées sur HTTP échouent. Pour plus d’informations, consultez [Exiger un transfert sécurisé dans Stockage Azure](../common/storage-require-secure-transfer.md). | [Oui](../../security-center/security-center-sql-service-recommendations.md) |
| Activez Advanced Threat Protection pour tous vos comptes de stockage | La protection avancée contre les menaces pour Stockage Azure offre une couche supplémentaire de veille de sécurité qui détecte les tentatives d’accès ou d’exploitation inhabituelles et potentiellement dangereuses des comptes de stockage. Les alertes de sécurité sont déclenchées dans Azure Security Center quand des anomalies se produisent dans des activités ; elles sont également envoyées par e-mail aux administrateurs d’abonnement, avec les détails des activités suspectes et des recommandations sur la façon d’examiner et de remédier aux menaces. Pour plus d’informations, consultez [Advanced Threat Protection pour Stockage Azure](../common/storage-advanced-threat-protection.md). | [Oui](../../security-center/security-center-sql-service-recommendations.md) |
| Activer la suppression réversible pour les données d’objet blob | La suppression réversible vous permet de récupérer des données d’objet blob après qu’elles ont été supprimées. Pour plus d’informations sur la suppression réversible, consultez [Suppression réversible pour les objets blob de Stockage Azure](storage-blob-soft-delete.md). | - |
| Stocker les données critiques pour l’entreprise dans des objets blob immuables | Configurez des stratégies de conservation légales et basées sur la durée pour stocker des données d’objets blob dans un état WORM (Write Once, Read Many). Les objets blob stockés de façon immuable peuvent être lus, mais ils ne peuvent pas être modifiés ou supprimés pendant la durée de l’intervalle de conservation. Pour plus d’informations, consultez [Stocker des données blob critiques pour l’entreprise avec un stockage immuable](storage-blob-immutable-storage.md). | - |
| Limiter les jetons de signature d’accès partagé (SAS) aux seules connexions HTTPS | Le fait d’exiger HTTPS quand un client utilise un jeton SAS pour accéder à des données d’objet blob permet de réduire le risque d’écoute. Pour plus d’informations, consultez [Accorder un accès limité aux ressources du Stockage Azure à l’aide des signatures d’accès partagé (SAS)](../common/storage-sas-overview.md). | - |

## <a name="identity-and-access-management"></a>Gestion de l’identité et de l’accès

| Recommandation | Commentaires | Security Center |
|-|----|--|
| Utiliser Azure Active Directory (Azure AD) pour autoriser l’accès aux données d’objet blob | Azure AD offre une sécurité et une facilité d’utilisation supérieures sur les clés partagées pour autoriser les demandes auprès du stockage d’objets blob. Pour plus d’informations, voir [Autoriser l’accès aux objets blob et files d’attente Azure à l’aide d’Azure Active Directory](../common/storage-auth-aad.md). | - |
| Gardez à l’esprit le principe du Moindre privilège lors de l’attribution d’autorisations à un principal de sécurité Azure AD via RBAC | Quand vous attribuez un rôle à un utilisateur, un groupe ou une application, accordez à ce principal de sécurité seulement les autorisations nécessaires pour effectuer ses tâches. La limitation de l’accès aux ressources permet d’éviter une mauvaise utilisation accidentelle et malveillante de vos données. | - |
| Utiliser une signature d’accès partagé de délégation d’utilisateur pour accorder aux clients un accès limité aux données d’objet blob | Une SAP de délégation d’utilisateur est sécurisée avec les informations d’identification Azure Active Directory (Azure AD) ainsi que par les autorisations spécifiées pour la SAP. Une signature d’accès partagé de délégation d’utilisateur est analogue à une signature d’accès partagé de service en termes d’étendue et de fonction, mais elle offre des avantages en matière de sécurité par rapport à la signature d’accès partagé de service. Pour plus d’informations, consultez [Accorder un accès limité aux ressources du Stockage Azure à l’aide des signatures d’accès partagé (SAS)](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). | - |
| Sécuriser vos clés d’accès de compte avec Azure Key Vault | Microsoft recommande d’utiliser Azure AD pour autoriser les demandes auprès de Stockage Azure. Cependant, si vous devez utiliser l’autorisation Clé partagée, sécurisez vos clés de compte avec Azure Key Vault. Vous pouvez récupérer les clés du coffre de clés au moment de l’exécution, au lieu de les enregistrer avec votre application. Pour plus d’informations sur Azure Key Vault, consultez [Vue d’ensemble d’Azure Key Vault](../../key-vault/general/overview.md). | - |
| Regénérer régulièrement vos clés de compte | Effectuer une rotation des clés de compte régulièrement réduit le risque d’exposer vos données à des acteurs malveillants. | - |
| Gardez à l’esprit le principe du Moindre privilège lors de l’attribution d’autorisations à une signature d’accès partagé | Lors de la création d’une signature d’accès partagé, spécifiez seulement les autorisations nécessaires au client pour remplir sa fonction. La limitation de l’accès aux ressources permet d’éviter une mauvaise utilisation accidentelle et malveillante de vos données. | - |
| Disposer d’un plan de révocation pour les signatures d’accès partagé que vous émettez pour les clients | Si une signature d’accès partagé est compromise, vous voudrez révoquer cette signature dès que possible. Pour révoquer une signature d’accès partagé de délégation d’utilisateur, révoquez la clé de délégation d’utilisateur pour invalider rapidement toutes les signatures associées à cette clé. Pour révoquer une signature d’accès partagé de service associée à une stratégie d’accès stockée, vous pouvez supprimer la stratégie d’accès stockée, renommer la stratégie ou changer son délai d’expiration pour le fixer à un Moment du passé. Pour plus d’informations, consultez [Accorder un accès limité aux ressources du Stockage Azure à l’aide des signatures d’accès partagé (SAS)](../common/storage-sas-overview.md).  | - |
| Si une signature d’accès partagé de service n’est pas associée à une stratégie d’accès stockée, définissez le délai d’expiration sur une heure ou Moins. | Une signature d’accès partagé de service qui n’est pas associée à une stratégie d’accès stockée ne peut pas être révoquée. Pour cette raison, il est recommandé de limiter le délai d’expiration afin que la signature d’accès partagé soit valide pendant une heure au maximum. | - |
| Limiter l’accès en lecture public anonyme pour les conteneurs et les objets blob | L’accès en lecture anonyme et public à un conteneur et à ses objets blob accorde à tous les clients un accès en lecture seule à ces ressources. Évitez d’activer l’accès en lecture public, sauf si votre scénario l’exige. | - |

## <a name="networking"></a>Mise en réseau

| Recommandation | Commentaires | Security Center |
|-|----|--|
| Activer des règles de pare-feu | Configurez des règles de pare-feu pour limiter l’accès à votre compte de stockage aux demandes provenant d’adresses IP ou de plages d’adresses IP spécifiées, ou d’une liste de sous-réseaux dans un réseau virtuel Azure (VNet). Pour plus d’informations sur la configuration des règles de pare-feu, consultez [Paramètres de proxy et de pare-feu d’Azure File Sync](../files/storage-sync-files-firewall-and-proxy.md). | - |
| Autoriser les services Microsoft approuvés à accéder au compte de stockage | L’activation des règles de pare-feu pour votre compte de stockage bloque les demandes entrantes pour les données par défaut, sauf si les demandes proviennent d’un service qui fonctionne au sein d’un réseau virtuel (VNet) Azure ou à partir d’adresses IP publiques autorisées. Les demandes qui sont bloquées comprennent les demandes émanant d’autres services Azure, du portail Azure, des services de journalisation et de métriques, etc. Vous pouvez autoriser les demandes provenant d’autres services Azure en ajoutant une exception pour autoriser les services Microsoft approuvés à accéder au compte de stockage. Pour plus d’informations sur l’ajout d’une exception pour les services Microsoft approuvés, consultez [Paramètres de proxy et de pare-feu d’Azure File Sync](../files/storage-sync-files-firewall-and-proxy.md).| - |
| Utiliser des points de terminaison privés | Un point de terminaison privé affecte une adresse IP privée de votre réseau virtuel Azure au compte de stockage. Il sécurise tout le trafic entre votre réseau virtuel et le compte de stockage via une liaison privée. Pour plus d’informations sur les points de terminaison privés, consultez [Connexion privée à un compte de stockage à l’aide d’Azure Private Endpoint](../../private-link/create-private-endpoint-storage-portal.md). | - |
| Utiliser des balises de service de réseau virtuel | Une balise de service représente un groupe de préfixes d’adresses IP d’un service Azure donné. Microsoft gère les préfixes d’adresse englobés par la balise de service et met à jour automatiquement la balise de service quand les adresses changent. Pour plus d’informations sur les balises de service prises en charge par Stockage Azure, consultez [Vue d’ensemble des balises de service Azure](../../virtual-network/service-tags-overview.md). Pour un tutoriel qui montre comment utiliser les balises de service pour créer des règles de réseau sortant, consultez [Restreindre l’accès aux ressources PaaS](../../virtual-network/tutorial-restrict-network-access-to-resources.md). | - |
| Limiter l’accès réseau à des réseaux spécifiques | La limitation de l’accès réseau aux réseaux hébergeant des clients nécessitant un accès réduit l’exposition de vos ressources aux attaques réseau. | [Oui](../../security-center/security-center-sql-service-recommendations.md) |

## <a name="loggingmonitoring"></a>Journalisation/Supervision

| Recommandation | Commentaires | Security Center |
|-|----|--|
| Suivre les autorisations des demandes | Activez la journalisation de Stockage Azure pour suivre comment chaque demande effectuée auprès de Stockage Azure était autorisée. Les journaux indiquent si une demande a été faite anonymement, en utilisant un jeton OAuth 2.0, avec une clé partagée ou avec une signature d’accès partagé. Pour plus d’informations, consultez [Journalisation Azure Storage Analytics](../common/storage-analytics-logging.md). | - |

## <a name="next-steps"></a>Étapes suivantes

- [Documentation sur la sécurité Azure](https://docs.microsoft.com//azure/security/)
- [Documentation sur le développement sécurisé](https://docs.microsoft.com/azure/security/develop/).
