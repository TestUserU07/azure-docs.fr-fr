---
title: Se connecter à des réseaux virtuels Azure avec un environnement de service d’intégration (ISE)
description: Créez un environnement de service d’intégration (ISE) pouvant accéder aux réseaux virtuels Azure à partir d’Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 6683c1b78b0e7ecba162026708c83843e2c08180
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478890"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Connexion à des réseaux virtuels Azure à partir d’Azure Logic Apps à l'aide d'un environnement de service d’intégration (ISE)

Pour les scénarios où vos applications logiques et vos comptes d’intégration ont besoin d’accéder à un [réseau virtuel Azure](../virtual-network/virtual-networks-overview.md), créez un [*environnement de service d’intégration* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Un ISE est un environnement isolé qui utilise un stockage dédié et d’autres ressources conservées séparément du service Logic Apps multilocataire « mondial ». Cette séparation réduit également l’impact que d’autres locataires Azure peuvent avoir sur les performances de vos applications. Un environnement ISE vous fournit également vos propres adresses IP statiques. Ces adresses IP sont séparées des adresses IP statiques qui sont partagées par les applications logiques dans le service multilocataire public.

Quand vous créez un environnement ISE, Azure *l’injecte* dans votre réseau virtuel Azure, qui déploie ensuite le service Logic Apps dans votre réseau virtuel. Quand vous créez une application logique ou un compte d’intégration, sélectionnez votre environnement ISE comme emplacement. Votre application logique ou votre compte d’intégration peut ensuite accéder directement à des ressources, comme des machines virtuelles, des serveurs, des systèmes et des services, dans votre réseau virtuel.

![Sélection d’un environnement de service d’intégration](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

> [!IMPORTANT]
> Pour que les applications logiques et les comptes d’intégration fonctionnent ensemble dans un environnement ISE, il faut que tous deux utilisent le *même ISE* que leur emplacement.

Un ISE dispose de limites accrues quant à la durée d’exécution, la conservation du stockage, le débit, le délai d’attente des requêtes et réponses HTTP, la taille des messages et les requêtes de connecteur personnalisé. Pour plus d’informations, consultez [Limites et configuration pour Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md). Pour en savoir plus sur les ISE, consultez [Accéder aux ressources Réseau virtuel Microsoft Azure à partir d’Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

Cet article explique comment effectuer ces tâches à l'aide du portail Azure :

* Activez l’accès pour votre ISE.
* Créez votre ISE.
* Ajoutez de la capacité supplémentaire à votre ISE.

Vous pouvez également créer un environnement ISE en utilisant l’[exemple de modèle de démarrage rapide Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-integration-service-environment) ou à l’aide de l’API REST Logic Apps, notamment en configurant des clés gérées par le client :

* [Créer un environnement de service d'intégration (ISE) à l'aide de l'API REST Logic Apps](../logic-apps/create-integration-service-environment-rest-api.md)
* [Configurer des clés gérées par le client pour chiffrer les données au repos des environnements ISE](../logic-apps/customer-managed-keys-integration-service-environment.md)

## <a name="prerequisites"></a>Prérequis

* Un abonnement Azure. Si vous n’avez pas d’abonnement Azure, [inscrivez-vous pour bénéficier d’un compte Azure gratuit](https://azure.microsoft.com/free/).

  > [!IMPORTANT]
  > Les applications logiques, les déclencheurs et actions intégrés et les connecteurs qui s’exécutent dans votre ISE utilisent un autre plan de tarification que celui basé sur la consommation. Pour plus d’informations sur la tarification et la facturation des environnements de service d’intégration, consultez le [modèle de tarif pour Logic Apps](../logic-apps/logic-apps-pricing.md#fixed-pricing). Pour connaître la tarification, consultez [Tarification Logic Apps](../logic-apps/logic-apps-pricing.md).

* Un [réseau virtuel Azure](../virtual-network/virtual-networks-overview.md). Si vous n’avez pas de réseau virtuel, découvrez comment [créer un réseau virtuel Azure](../virtual-network/quick-create-portal.md).

  * Votre réseau virtuel doit comporter quatre sous-réseaux *vides* pour la création et le déploiement de ressources dans votre ISE. Chaque sous-réseau prend en charge un composant Logic Apps différent de celui utilisé dans votre environnement de service d’intégration. Vous pouvez créer ces sous-réseaux à l’avance, ou attendre de créer votre ISE où vous pouvez créer vos sous-réseaux simultanément. En savoir plus sur [exigences des sous-réseaux](#create-subnet).

  * Les noms des sous-réseaux doivent commencer par un caractère alphabétique ou un trait de soulignement, et les caractères suivants sont interdits : `<`, `>`, `%`, `&`, `\\`, `?`, `/`. 
  
  * Si vous souhaitez déployer l’environnement ISE par le biais d’un modèle Azure Resource Manager, assurez-vous d’abord de déléguer un sous-réseau vide à Microsoft.Logic/integrationServiceEnvironment. Vous n’avez pas besoin de procéder à cette délégation quand vous effectuez le déploiement par le biais du portail Azure.

  * Assurez-vous que votre réseau virtuel [permet d’accéder à votre ISE](#enable-access) afin que votre environnement ISE puisse fonctionner correctement et rester accessible.

  * Si vous utilisez [ExpressRoute](../expressroute/expressroute-introduction.md), qui fournit une connexion privée aux services de cloud computing Microsoft qui est facilitée par le fournisseur de connectivité, vous devez [créer une table de route](../virtual-network/manage-route-table.md) comportant l’itinéraire suivant, et lier cette table à chaque sous-réseau utilisé par votre environnement de service d’intégration :

    **Nom** : <*nom d’itinéraire*><br>
    **Préfixe de l’adresse** : 0.0.0.0/0<br>
    **Tronçon suivant** : Internet

* Si vous souhaitez utiliser des serveurs DNS personnalisés pour votre réseau virtuel Azure, [configurez ces serveurs en suivant ces étapes](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) avant de déployer votre ISE sur votre réseau virtuel. Pour plus d'informations sur la gestion des paramètres d'un serveur DNS, consultez [Créer, modifier ou supprimer un réseau virtuel](../virtual-network/manage-virtual-network.md#change-dns-servers).

  > [!NOTE]
  > Si vous changez votre serveur DNS ou modifiez ses paramètres, vous devez redémarrer votre ISE afin qu’il prenne en compte ces modifications. Pour plus d’informations, consultez [Réinitialiser votre ISE](../logic-apps/ise-manage-integration-service-environment.md#restart-ISE).

<a name="enable-access"></a>

## <a name="enable-access-for-ise"></a>Activer l’accès pour ISE

Lorsque vous utilisez un ISE avec un réseau virtuel Azure, il arrive souvent qu'un ou plusieurs ports soient bloqués. Les connecteurs que vous utilisez pour créer les connexions entre votre ISE et les systèmes de destination peuvent également avoir leurs propres exigences en matière de port. Par exemple, si vous communiquez avec un système FTP en utilisant le connecteur FTP, le port que vous utilisez sur votre système FTP doit être disponible, comme le port 21 par exemple, pour l'envoi de commandes.

Pour vous assurer que votre environnement de service d’intégration est accessible et que ses applications logiques peuvent communiquer entre les sous-réseaux de votre réseau virtuel, [ouvrez les ports décrits dans ce tableau pour chaque sous-réseau](#network-ports-for-ise). Si certains des ports requis ne sont pas disponibles, votre ISE ne fonctionnera pas correctement.

* Si vous avez plusieurs instances d’ISE devant accéder à d’autres points de terminaison qui ont des restrictions d’adresse IP, déployez un [pare-feu Azure](../firewall/overview.md) ou une [appliance virtuelle réseau](../virtual-network/virtual-networks-overview.md#filter-network-traffic) dans votre réseau virtuel, et routez le trafic sortant via ce pare-feu ou cette appliance virtuelle réseau. Vous pouvez ensuite [configurer une adresse IP unique, sortante, publique, statique et prédictible](connect-virtual-network-vnet-set-up-single-ip-address.md) que toutes les instances de l’environnement ISE dans votre réseau virtuel peuvent utiliser pour communiquer avec les systèmes de destination. De cette façon, vous n’avez pas besoin de configurer des ouvertures de pare-feu supplémentaires sur ces systèmes de destination pour chaque environnement ISE.

   > [!NOTE]
   > Vous pouvez utiliser cette approche pour un environnement ISE unique lorsque votre scénario nécessite de limiter le nombre d’adresses IP qui ont besoin d’un accès. Déterminez si les coûts supplémentaires pour le pare-feu ou l’appliance de réseau virtuel ont un sens pour votre scénario. Découvrez plus en détail la [tarification du Pare-feu Azure](https://azure.microsoft.com/pricing/details/azure-firewall/).

* Si vous avez créé un réseau virtuel Azure et des sous-réseaux sans contraintes, vous n'avez pas besoin de configurer des [groupes de sécurité réseau](../virtual-network/security-overview.md#network-security-groups) dans votre réseau virtuel pour contrôler le trafic sur les sous-réseaux.

* Sur un réseau virtuel existant, vous pouvez *facultativement* configurer des groupes de sécurité réseau en [filtrant le trafic réseau sur les sous-réseaux](../virtual-network/tutorial-filter-network-traffic.md). Si vous souhaitez accéder à cet itinéraire ou si vous utilisez déjà des groupes de sécurité réseau (NSG), veillez à [ouvrir les ports figurant dans ce tableau](#network-ports-for-ise) sur le réseau virtuel où vous disposez de NSG ou sur lequel vous souhaitez en configurer.

  > [!NOTE]
  > Si vous utilisez des [règles de sécurité de groupe de sécurité réseau](../virtual-network/security-overview.md#security-rules), vous devez utiliser *les deux* protocoles TCP et UDP. Les règles de sécurité de groupe de sécurité réseau décrivent les ports que vous devez ouvrir pour les adresses IP qui doivent accéder à ces ports. Assurez-vous que tous les pare-feu, routeurs ou autres éléments existant entre ces points de terminaison gardent également ces ports accessibles à ces adresses IP.

<a name="network-ports-for-ise"></a>

### <a name="network-ports-used-by-your-ise"></a>Ports réseau utilisés par votre ISE

Ce tableau décrit les ports du réseau virtuel Azure que votre ISE utilise et l’endroit où ces ports sont utilisés. Pour vous faciliter la tâche lors de la création de règles de sécurité, les [balises de service](../virtual-network/service-tags-overview.md) du tableau représentent les groupes de préfixes d'adresse IP d'un service Azure spécifique.

> [!IMPORTANT]
> Les ports source étant éphémères, affectez-leur la valeur `*` pour toutes les règles. Lorsque cela est mentionné, les environnements de service d’intégration (ISE) interne et externe font référence au [point de terminaison sélectionné lors de la création de l’ISE](connect-virtual-network-vnet-isolated-environment.md#create-environment). Pour plus d’informations, consultez l’article [Accès au point de terminaison](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). 

| Objectif | Sens | Ports de destination | Balise du service source | Identification de destination | Notes |
|---------|-----------|-------------------|--------------------|-------------------------|-------|
| Communication interne aux sous-réseaux au sein de votre réseau virtuel | Trafic entrant et sortant | * | Espace d’adressage pour le réseau virtuel contenant les sous-réseaux de votre environnement de service d’intégration | Espace d’adressage pour le réseau virtuel contenant les sous-réseaux de votre environnement de service d’intégration | Nécessaire pour que le trafic circule *entre* les sous-réseaux de votre réseau virtuel. <p><p>**Important !** Pour que le trafic circule entre les *composants* de chaque sous-réseau, veillez à ouvrir tous les ports de chaque sous-réseau. |
| Communication vers votre application logique | Trafic entrant | 443 | ISE interne : <br>VirtualNetwork <p><p>ISE externe : <br>Internet <br>(voir colonne **Remarques**) | VirtualNetwork | Plutôt que d'utiliser la balise de service **Internet**, vous pouvez spécifier l'adresse IP source de l'ordinateur ou du service qui appelle un déclencheur de requête ou webhook quelconque dans votre application logique. <p><p>**Important !** Fermer ou bloquer ce port empêche les appels HTTP vers Logic Apps avec les déclencheurs de requête. |
| Historique des exécutions d’une application logique | Trafic entrant | 443 | ISE interne : <br>VirtualNetwork <p><p>ISE externe : <br>Internet <br>(voir colonne **Remarques**) | VirtualNetwork | Plutôt que d'utiliser la balise de service **Internet**, vous pouvez spécifier l'adresse IP source de l'ordinateur ou du service à partir duquel vous souhaitez afficher l'historique des exécutions de votre application logique. <p><p>**Important !** Bien que la fermeture ou le blocage de ce port ne vous empêche pas d’afficher l’historique des exécutions, vous ne pouvez pas afficher les entrées et sorties pour chaque étape dans cet historique des exécutions. |
| Concepteur Logic Apps - Propriétés dynamiques | Trafic entrant | 454 | LogicAppsManagement | VirtualNetwork | Les demandes proviennent des adresses IP de point de terminaison d’accès [entrantes](../logic-apps/logic-apps-limits-and-config.md#inbound) de Logic Apps pour cette région. |
| Déploiement du connecteur | Trafic entrant | 454 | AzureConnectors | VirtualNetwork | Nécessaire pour le déploiement et la mise à jour des connecteurs. La fermeture ou le blocage de ce port entraîne l’échec des déploiements de l’ISE et empêche les correctifs ou mises à jour du connecteur. |
| Vérification de l’intégrité du réseau | Trafic entrant | 454 | LogicApps | VirtualNetwork | Les demandes proviennent des adresses IP de point de terminaison d’accès [entrantes](../logic-apps/logic-apps-limits-and-config.md#inbound) et [sortantes](../logic-apps/logic-apps-limits-and-config.md#outbound) de Logic Apps pour cette région. |
| Dépendance de gestion App Service | Trafic entrant | 454, 455 | AppServiceManagement | VirtualNetwork | |
| Communication à partir d’Azure Traffic Manager | Trafic entrant | ISE interne : 454 <p><p>ISE externe : 443 | AzureTrafficManager | VirtualNetwork | |
| Gestion des API - Point de terminaison de gestion | Trafic entrant | 3443 | APIManagement | VirtualNetwork | |
| Déploiement de la stratégie de connecteur | Trafic entrant | 3443 | APIManagement | VirtualNetwork | Nécessaire pour le déploiement et la mise à jour des connecteurs. La fermeture ou le blocage de ce port entraîne l’échec des déploiements de l’ISE et empêche les correctifs ou mises à jour du connecteur. |
| Communication depuis votre application logique | Règle de trafic sortant | 80, 443 | VirtualNetwork | Varie selon la destination | Points de terminaison du service externe avec lesquels votre application logique doit communiquer. |
| Azure Active Directory | Règle de trafic sortant | 80, 443 | VirtualNetwork | AzureActiveDirectory | |
| Gestion des connexions | Règle de trafic sortant | 443 | VirtualNetwork  | AppService | |
| Publier des journaux de diagnostic et métriques | Règle de trafic sortant | 443 | VirtualNetwork  | AzureMonitor | |
| Dépendance du Stockage Azure | Règle de trafic sortant | 80, 443, 445 | VirtualNetwork | Stockage | |
| Dépendance Azure SQL | Règle de trafic sortant | 1433 | VirtualNetwork | SQL | |
| Azure Resource Health | Règle de trafic sortant | 1886 | VirtualNetwork | AzureMonitor | Nécessaire pour publier l’état d’intégrité sur Resource Health |
| Dépendance du journal pour la stratégie Event Hub et l’agent de surveillance | Règle de trafic sortant | 5672 | VirtualNetwork | Event Hub | |
| Accès aux instances du Cache Azure pour Redis entre instances de rôle | Trafic entrant <br>Règle de trafic sortant | 6379 - 6383 | VirtualNetwork | VirtualNetwork | En outre, pour qu’ISE fonctionne avec le cache Azure pour Redis, vous devez ouvrir les [ports entrants et sortants décrits dans la FAQ sur le cache Azure pour Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Créer votre environnement de service d’intégration

1. Dans la zone de recherche principale du [portail Azure](https://portal.azure.com), entrez `integration service environments` comme filtre, puis sélectionnez **Environnements de service d’intégration**.

   ![Rechercher et sélectionner « Environnements de service d’intégration »](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Dans le volet **Environnements de service d’intégration**, sélectionnez **Ajouter**.

   ![Rechercher et sélectionner « Environnements de service d’intégration »](./media/connect-virtual-network-vnet-isolated-environment/add-integration-service-environment.png)

1. Spécifiez ces informations pour votre environnement, puis sélectionnez **Vérifier + créer**, par exemple :

   ![Spécifier les informations pour l’environnement](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propriété | Obligatoire | Valeur | Description |
   |----------|----------|-------|-------------|
   | **Abonnement** | Oui | <*Azure-subscription-name*> | Abonnement Azure à utiliser pour votre environnement |
   | **Groupe de ressources** | Oui | <*nom-groupe-de-ressources-Azure*> | Groupe de ressources Azure nouveau ou existant dans lequel vous voulez créer votre environnement |
   | **Nom de l’environnement de service d’intégration** | Oui | <*nom-environnement*> | Votre nom ISE, qui peut contenir uniquement des lettres, des chiffres, des traits d’union (`-`), des traits de soulignement (`_`) et des points (`.`). |
   | **Lieu** | Oui | <*région-centre de données-Azure*> | Région du centre de données Azure où déployer votre environnement |
   | **Référence (SKU)** | Oui | **Premium** ou **Développeur (aucun contrat SLA)** | Référence SKU d’ISE à créer et à utiliser. Pour connaître les différences entre ces références SKU, consultez [Références SKU d’ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level). <p><p>**Important !** Cette option est disponible uniquement lors de la création de votre ISE et ne peut pas être modifiée ultérieurement. |
   | **Capacité supplémentaire** | Premium : <br>Oui <p><p>Développeur : <br>Non applicable | Premium : <br>0 à 10 <p><p>Développeur : <br>Non applicable | Le nombre d’unités de traitement supplémentaires à utiliser pour cette ressource ISE. Pour ajouter de la capacité après création, consultez [Ajouter de la capacité à l’ISE](../logic-apps/ise-manage-integration-service-environment.md#add-capacity). |
   | **Point de terminaison de l'accès** | Oui | **Interne** ou **externe** | Type de points de terminaison d’accès à utiliser pour votre environnement ISE. Ces points de terminaison déterminent si les déclencheurs de demande ou de webhook sur les applications logiques dans votre ISE peuvent recevoir des appels en dehors de votre réseau virtuel. <p><p>Votre sélection affecte également la façon dont vous pouvez afficher les entrées et les sorties, ainsi qu’y accéder dans l’historique des exécutions de votre application logique. Pour plus d’informations, consultez [Accès aux points de terminaison de l’environnement ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access). <p><p>**Important !** Cette option est disponible uniquement lors de la création de votre ISE et ne peut pas être modifiée ultérieurement. |
   | **Réseau virtuel** | Oui | <*Azure-virtual-network-name*> | Réseau virtuel Azure où vous voulez injecter votre environnement, pour que les applications logiques de cet environnement puissent accéder à votre réseau virtuel. Si vous n’avez pas de réseau, [créez d’abord un réseau virtuel Azure](../virtual-network/quick-create-portal.md). <p><p>**Important !** Vous pouvez effectuer cette injection *seulement*  quand vous créez votre ISE. |
   | **Sous-réseaux** | Oui | <*subnet-resource-list*> | Un environnement ISE nécessite quatre sous-réseaux *vides* pour la création et le déploiement des ressources dans votre environnement. Pour créer chaque sous-réseau, [suivez les étapes décrites dans ce tableau](#create-subnet). |
   |||||

   <a name="create-subnet"></a>

   **Créer un sous-réseau**

   Pour créer et déployer des ressources dans votre environnement, votre ISE a besoin de quatre sous-réseaux *vides* qui ne sont délégués à aucun service. Chaque sous-réseau prend en charge un composant Logic Apps différent de celui utilisé dans votre environnement de service d’intégration. Vous *ne pouvez pas changer* ces adresses de sous-réseaux après avoir créé votre environnement. Chaque sous-réseau doit répondre aux exigences suivantes :

   * A un nom qui commence par un caractère alphabétique ou un trait de soulignement (aucun chiffre) et n’utilise pas ces caractères : `<`, `>`, `%`, `&`, `\\`, `?`, `/`.

   * Il utilise le format [CIDR (Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) et un espace d’adressage de Classe B.

   * Utilise au moins un `/27` dans l’espace d’adressage, car chaque sous-réseau requiert *au moins* 32 adresses. Par exemple :

     * `10.0.0.0/28` a seulement 16 adresses et est trop petit, car 2<sup>(32-28)</sup> représente 2<sup>4</sup>, ou 16.

     * `10.0.0.0/27` a 32 adresses car 2<sup>(32-27)</sup> est égal à 2<sup>5</sup> soit 32.

     * `10.0.0.0/24` a 256 adresses car 2<sup>(32-24)</sup> est égal à 2<sup>8</sup> soit 256. Cependant, un plus grand nombre d’adresses n’apporte aucun avantage supplémentaire.

     Pour en savoir plus sur le calcul des adresses, consultez [Blocs CIDR IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Si vous utilisez [ExpressRoute](../expressroute/expressroute-introduction.md), vous devez [créer une table de route](../virtual-network/manage-route-table.md) comportant l'itinéraire suivant, et lier cette table à chaque sous-réseau utilisé par votre ISE :

     **Nom** : <*nom d’itinéraire*><br>
     **Préfixe de l’adresse** : 0.0.0.0/0<br>
     **Tronçon suivant** : Internet

   1. Dans la liste **Sous-réseaux**, sélectionnez **Gérer la configuration du sous-réseau**.

      ![Gérer la configuration du sous-réseau](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet-configuration.png)

   1. Dans le volet **Sous-réseaux**, sélectionnez **Sous-réseau**.

      ![Ajouter quatre sous-réseaux vides](./media/connect-virtual-network-vnet-isolated-environment/add-empty-subnets.png)

   1. Dans le volet **Ajouter un sous-réseau**, spécifiez ces informations.

      * **Name** : Nom de votre sous-réseau
      * **Plage d’adresses (bloc CIDR)** : Plage de votre sous-réseau dans votre réseau virtuel, au format CIDR

      ![Détails de l’ajout d’un sous-réseau](./media/connect-virtual-network-vnet-isolated-environment/provide-subnet-details.png)

   1. Quand vous avez terminé, sélectionnez **OK**.

   1. Répétez ces étapes pour trois autres sous-réseaux.

      > [!NOTE]
      > Si les sous-réseaux que vous tentez de créer ne sont pas valides, le portail Azure affiche un message mais ne bloque pas votre progression.

   Pour plus d’informations sur la création de sous-réseaux, consultez [Ajouter un sous-réseau à un réseau virtuel](../virtual-network/virtual-network-manage-subnet.md).

1. Une fois qu’Azure a validé les informations de votre environnement de service d’intégration, sélectionnez **Créer**, par exemple :

   ![Après la validation, sélectionner « Créer »](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure commence le déploiement de votre environnement, qui prend généralement moins de deux heures. Parfois, le déploiement peut prendre jusqu’à quatre heures. Pour vérifier l’état du déploiement, dans votre barre d’outils Azure, sélectionnez l’icône Notifications qui ouvre le volet Notifications.

   ![Vérifier l’état du déploiement](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Un fois le déploiement terminé, Azure montre cette notification :

   ![Déploiement réussi](./media/connect-virtual-network-vnet-isolated-environment/deployment-success-message.png)

   Sinon, suivez les instructions du portail Azure pour la résolution des problèmes de déploiement.

   > [!NOTE]
   > Si le déploiement échoue ou si vous supprimez votre ISE, Azure peut prendre jusqu’à une heure avant de libérer vos sous-réseaux. Ce délai signifie que vous devrez peut-être attendre avant de réutiliser ces sous-réseaux dans un autre ISE.
   >
   > Si vous supprimez votre réseau virtuel, Azure a généralement besoin de jusqu'à deux heures pour libérer vos sous-réseaux, mais cette opération peut prendre plus longtemps. 
   > Lors de la suppression de réseaux virtuels, assurez-vous qu’aucune ressource n’est restée connectée. 
   > Consultez [Supprimer un réseau virtuel](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Pour voir votre environnement, si Azure n’y accède automatiquement une fois le déploiement terminé, sélectionnez **Accéder à la ressource**.

1. Pour vérifier l’intégrité du réseau de votre ISE, consultez [Gérer votre environnement de service d’intégration](../logic-apps/ise-manage-integration-service-environment.md#check-network-health).

1. Pour commencer à créer des applications logiques et d'autres artefacts dans votre ISE, consultez [Ajouter des ressources à des environnements de service d'intégration](../logic-apps/add-artifacts-integration-service-environment-ise.md).

   > [!IMPORTANT]
   > Les connecteurs ISE gérés qui deviennent disponibles une fois que vous avez créé votre ISE n’apparaissent pas automatiquement dans le sélecteur de connecteur du concepteur d’applications logiques. Avant de pouvoir utiliser ces connecteurs ISE, vous devez manuellement [ajouter ces connecteurs dans votre ISE](../logic-apps/add-artifacts-integration-service-environment-ise.md#add-ise-connectors-environment) afin qu’ils apparaissent dans le concepteur d’applications logiques.

## <a name="next-steps"></a>Étapes suivantes

* [Ajouter des ressources à des environnements de service d'intégration](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Gérer les environnements de service d’intégration](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)
* En apprendre davantage sur le [Réseau virtuel Azure](../virtual-network/virtual-networks-overview.md)
* Découvrir l’[Intégration d’un réseau virtuel pour les services Azure](../virtual-network/virtual-network-for-azure-services.md)
