---
title: Composants et versions Apache Hadoop – Azure HDInsight
description: Découvrez les composants et versions Apache Hadoop dans Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017,seoapr2020
ms.date: 04/09/2020
ms.openlocfilehash: 87c3e2439d1b4bef4a58663e3ea06d8bb7cb9b19
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192533"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>Quels sont les composants et versions Apache Hadoop disponibles avec HDInsight ?

Découvrez les composants et versions de l’environnement [Apache Hadoop](https://hadoop.apache.org/) inclus dans Microsoft Azure HDInsight et le Pack Sécurité Entreprise. Découvrez également comment vérifier les versions des composants Hadoop dans HDInsight.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>Composants Apache Hadoop disponibles avec différentes versions de HDInsight

Azure HDInsight prend en charge plusieurs versions de cluster Hadoop qui peuvent être déployées à tout moment. Depuis le 4 avril 2017, la version de cluster par défaut utilisée par Azure HDInsight est la version 3.6.

Les versions de composants associées aux versions de cluster HDInsight sont répertoriées dans le tableau suivant :

> [!NOTE]  
> La version par défaut associée au service HDInsight peut changer sans préavis. Si vous dépendez d’une version, spécifiez cette dernière lorsque vous créez vos clusters à l’aide du kit SDK .NET avec Azure PowerShell et Azure Classic CLI.

| Composant              | HDInsight 4.0 | HDInsight 3.6 (par défaut)     |
|------------------------|---------------|-----------------------------|
| Apache Hadoop et YARN | 3.1.1         | 2.7.3                       |
| Apache Tez             | 0.9.1         | 0.7.0                       |
| Apache Pig             | 0.16.0        | 0.16.0                      |
| Apache Hive            | 3.1.0         | 1.2.1 (2.1.0 sur ESP Interactive Query) |
| Apache Tez/Hive2       | -             | 0.8.4                       |
| Apache Ranger          | 1.1.0         | 0.7.0                       |
| Apache HBase           | 2.0.2         | 1.1.2                       |
| Apache Sqoop           | 1.4.7         | 1.4.6                       |
| Apache Oozie           | 4.3.1         | 4.2.0                       |
| Apache Zookeeper       | 3.4.6         | 3.4.6                       |
| Apache Storm           | -             | 1.1.0                       |
| Apache Mahout          | -             | 0.9.0+                      |
| Apache Phoenix         | 5             | 4.7.0                       |
| Apache Spark           | 2.3.1, 2.4    | 2.3.0, 2.2.0, 2.1.0         |
| Apache Livy            | 0.5           | 0.4, 0.4, 0.3               |
| Apache Kafka           | 1.1.1, 2.1    | 1.1, 1.0 * (Voir la remarque ci-dessous) |
| Apache Ambari          | 2.7.0         | 2.6.0                       |
| Apache Zeppelin        | 0.8.0         | 0.7.3                       |
| Mono                   | 4.2.1         | 4.2.1                       |

> [!NOTE]
> En raison de considérations sur les performances système, la prise en charge de Kafka version 0.10 a expiré en mars 2019.

## <a name="check-for-current-hadoop-component-version-information"></a>Comment vérifier les informations de version du composant Hadoop actuel

Les versions de composant d’environnement Hadoop associées aux versions de cluster HDInsight peuvent changer avec les mises à jour de HDInsight. Pour vérifier les composants Hadoop ainsi que les versions utilisées pour un cluster, utilisez l’API REST Ambari. La commande **GetComponentInformation** extrait des informations sur les composants du service. Pour plus d’informations, consultez la page [Documentation Apache Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

### <a name="release-notes"></a>Notes de publication

Pour d’autres notes de publication sur les dernières versions de HDInsight, consultez la page [Notes de publication de HDInsight](hdinsight-release-notes.md) .

## <a name="supported-hdinsight-versions"></a>Versions de HDInsight prises en charge

### <a name="support-expiration-and-retirement-for-hdinsight-versions"></a>Expiration du support et mise hors service des versions de HDInsight

L’**expiration du support** signifie que Microsoft ne prend plus en charge la version de HDInsight spécifiée. Celle-ci ne sera plus disponible par le biais du portail Azure pour la création de cluster. Pour autant, ces versions peuvent toujours être créées à l’aide d'Azure CLI ou des différents kits de développement logiciel (SDK).

La **mise hors service** d'une version de HDInsight signifie que les clusters existants continueront à s’exécuter tels quels. Il n’est toutefois pas possible de créer des clusters de cette version par quelque moyen que ce soit (CLI et SDK notamment). D'autres fonctionnalités du plan de contrôle (mise à l’échelle manuelle ou automatique, par exemple) peuvent également ne plus fonctionner après une mise hors service de version. Aucun support n’est disponible pour les versions mises hors service.

Les tableaux suivants listent les versions HDInsight. Les dates d’expiration et de mise hors service sont également fournies, lorsqu’elles sont connues.

### <a name="available-versions"></a>Versions disponibles

Le tableau suivant répertorie les versions HDInsight qui sont disponibles dans le portail Azure ainsi que d’autres méthodes de déploiement telles que PowerShell et le SDK .NET.

| Version de HDInsight | SYSTÈME D’EXPLOITATION DE LA MACHINE VIRTUELLE | Date de publication | Date d’expiration du support | Date de mise hors service | Haute disponibilité |  Disponibilité dans le portail Azure |
| --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |Ubuntu 16.0.4 LTS |24 septembre 2018 | | |Oui |Oui |
| HDInsight 3.6 |Ubuntu 16.0.4 LTS |4 avril 2017 | 31 décembre 2020 |31 décembre 2020 |Oui |Oui |

Le support de Spark 2.1/2.2 et de Kafka 1.0 expirera le 30 juin 2020.

> [!NOTE]  
> Quand une version cesse d’être prise en charge, elle n’est plus proposée dans le portail Microsoft Azure. Toutefois, les versions des clusters restent disponibles avec le paramètre `Version` de la commande Windows PowerShell [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) et le kit SDK .NET, jusqu’à leur date de mise hors service.

### <a name="retired-versions"></a>Versions supprimées

Le tableau suivant liste les versions HDInsight qui **ne sont pas** disponibles dans le portail Azure.

| Version de HDInsight | Version de la plateforme HDP | SYSTÈME D’EXPLOITATION DE LA MACHINE VIRTUELLE | Date de publication | Date d’expiration du support | Date de mise hors service | Haute disponibilité |  Disponibilité sur le portail Azure |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 |HDP 2.5 |Ubuntu 16.0.4 LTS |30 septembre 2016 |5 septembre 2017 |28 juin 2018 |Oui |Non  |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |29 mars 2016 |29 décembre 2016 |9 janvier 2018 |Oui |Non  |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 décembre 2015 |27 juin 2016 |31 juillet 2018 |Oui |Non  |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2 décembre 2015 |27 juin 2016 |31 juillet 2017 |Oui |Non  |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS ou Windows Server 2012 R2 |18 février 2015 |1 mars 2016 |1 avril 2017 |Oui |Non  |
| HDInsight 3.1 |HDP 2,1 |Windows Server 2012 R2 |24 juin 2014 |18 mai 2015 |30 juin 2016 |Oui |Non  |
| HDInsight 3.0 |HDP 2,0 |Windows Server 2012 R2 |11 février 2014 |17 septembre 2014 |30 juin 2015 |Oui |Non  |
| HDInsight 2.1 |HDP 1,3 |Windows Server 2012 R2 |28 octobre 2013 |12 mai 2014 |31 mai 2015 |Oui |Non  |
| HDInsight 1.6 |HDP 1.1 | |28 octobre 2013 |26 avril 2014 |31 mai 2015 |Non  |Non  |

> [!NOTE]  
> Les clusters à haute disponibilité avec deux nœuds principaux sont déployés par défaut pour les clusters HDInsight 2.1 et versions ultérieures. Ils ne sont pas disponibles pour les clusters HDInsight version 1.6.

## <a name="enterprise-security-package-for-hdinsight"></a>Package de sécurité d’entreprise pour HDInsight

Le package de sécurité d’entreprise est un package facultatif que vous pouvez ajouter à votre cluster HDInsight dans le cadre du flux de travail de création du cluster. Le package de sécurité d’entreprise prend en charge les aspects suivants :

- Intégration à Active Directory pour l’authentification.

    Avant, vous pouviez créer des clusters HDInsight avec un utilisateur administrateur local et un utilisateur SSH local. L’utilisateur administrateur local pouvait accéder à tous les fichiers, dossiers, tables et colonnes.  Avec le Pack Sécurité Entreprise, vous activez le contrôle d’accès en fonction du rôle en intégrant HDInsight à votre Active Directory, qui inclut Active Directory en local, Azure Active Directory Domain Services ou Active Directory sur une machine virtuelle IaaS. L’administrateur de domaine sur le cluster peut autoriser les utilisateurs à utiliser leurs propres nom d’utilisateur et mot de passe d’entreprise (domaine).

    Pour plus d'informations, consultez les pages suivantes :

    - [Introduction aux fonctions de sécurité Apache Hadoop sur la base de clusters HDInsight joints à un domaine](./domain-joined/hdinsight-security-overview.md)
    - [Planifier des clusters Apache Hadoop Azure joints à un domaine dans HDInsight](./domain-joined/apache-domain-joined-architecture.md)
    - [Configurer un environnement de bac à sable joint à un domaine](./domain-joined/apache-domain-joined-configure.md)
    - [Configurer les clusters HDInsight joints à un domaine avec Azure Active Directory Domain Services](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Autorisation pour les données

  - Intégration à Apache Ranger pour l’autorisation sur les files d’attente Yarn Hive et Spark SQL.
  - Vous pouvez définir le contrôle d’accès sur les fichiers et dossiers.

    Pour plus d'informations, consultez les pages suivantes :

  - [Configurer les stratégies Apache Hive dans HDInsight joint à un domaine](./domain-joined/apache-domain-joined-run-hive.md)

- Affichage des journaux d’audit pour surveiller les accès et les stratégies configurées.

### <a name="supported-cluster-types"></a>Types de cluster pris en charge

Seuls les types de cluster suivants prennent en charge le package de sécurité d’entreprise :

- Hadoop (HDInsight 3.6 uniquement)
- Spark
- Kafka
- hbase
- Interactive Query

### <a name="support-for-azure-data-lake-storage"></a>Prise en charge d’Azure Data Lake Storage

Le Pack Sécurité Entreprise prend en charge l’utilisation d’Azure Data Lake Storage comme stockage principal et stockage complémentaire.

### <a name="pricing-and-service-level-agreement-sla"></a>Tarification et contrat SLA

Pour plus d’informations sur les tarifs et le contrat SLA du package de sécurité d’entreprise, consultez [Tarification HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Contrat de niveau de service pour les versions de cluster HDInsight

Le contrat SLA est défini en termes de _fenêtre de support_. La fenêtre de support est la période pendant laquelle une version de HDInsight est prise en charge par `Microsoft Customer Service and Support`. Si la version du logiciel est associée à une _date d’expiration du support_ dépassée, cela signifie que le cluster HDInsight n’est plus inclus dans la fenêtre de support. L’expiration du support d’une version donnée de HDInsight (lorsqu’une version X+1 plus récente est disponible) est calculée comme suit :  

- Formule 1 : ajoutez 180 jours à la date de lancement du cluster HDInsight version X.
- Formula 2 : ajoutez 90 jours à la date de lancement du cluster HDInsight version X+1 dans le portail Azure.

La _date de mise hors service_ est la date après laquelle la version du cluster ne peut plus être créée sur HDInsight. Depuis le 31 juillet 2017, vous ne pouvez plus redimensionner un cluster HDInsight après sa date de mise hors service.

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Tailles des machines virtuelles et configuration des nœuds par défaut pour les clusters

Pour plus d’informations sur les références SKU de machine virtuelle à sélectionner pour votre cluster, consultez [Détails de configuration du cluster Azure HDInsight](hdinsight-supported-node-configuration.md).

## <a name="next-steps"></a>Étapes suivantes

- [Création de clusters pour Apache Hadoop, Spark, etc. dans HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Travailler à partir d’un PC Windows dans Apache Hadoop sur HDInsight](hdinsight-hadoop-windows-tools.md)
- [Notes de publication Hortonworks associées aux versions d’Azure HDInsight](./hortonworks-release-notes.md)
