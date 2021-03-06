---
title: Modèle de métadonnées partagées Azure Synapse Analytics
description: Azure Synapse Analytics permet aux différents moteurs de calcul d’espace de travail de partager des bases de données et des tables entre leurs pools Spark (préversion), le moteur SQL à la demande (préversion) et les pools SQL.
services: synapse-analytics
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: 3b26d516080961a482a3ba67f314e98ece4c9f24
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420173"
---
# <a name="azure-synapse-analytics-shared-metadata"></a>Métadonnées partagées Azure Synapse Analytics

Azure Synapse Analytics permet aux différents moteurs de calcul d’espace de travail de partager des bases de données et des tables entre leurs pools Spark (préversion), le moteur SQL à la demande (préversion) et les pools SQL.

[!INCLUDE [preview](../includes/note-preview.md)]



Le partage prend en charge ce que l’on appelle le modèle d’entrepôt de données moderne, et procure aux moteurs SQL d’espace de travail un accès aux bases de données et aux tables créées avec Spark. Il permet également aux moteurs SQL de créer leurs propres objets qui ne sont pas partagés avec les autres moteurs.

## <a name="support-the-modern-data-warehouse"></a>Prendre en charge l’entrepôt de données moderne

Le modèle de métadonnées partagées prend en charge le modèle d’entrepôt de données moderne de la manière suivante :

1. Les données du lac de données sont préparées et structurées efficacement avec Spark en stockant les données préparées dans des tables Parquet (éventuellement partitionnées) contenues dans plusieurs bases de données.

2. Les bases de données créées avec Spark et toutes leurs tables sont visibles dans n’importe quelle instance du pool Spark d’espace de travail Azure Synapse et peuvent être utilisées à partir de n’importe quel travail Spark. Cette fonctionnalité est soumise aux [autorisations](#security-model-at-a-glance), dans la mesure où tous les pools Spark d’un espace de travail partagent le même magasin de métadonnées de catalogue sous-jacent.

3. Les bases de données créées avec Spark et leurs tables Parquet sont visibles dans le moteur SQL à la demande de l’espace de travail. Les [bases de données](database.md) sont créées automatiquement dans les métadonnées SQL à la demande, et les [tables externes et gérées](table.md) créées par un travail Spark sont rendues accessibles en tant que tables externes dans les métadonnées SQL à la demande dans le schéma `dbo` de la base de données correspondante. <!--For more details, see [ADD LINK].-->

4. Si l’espace de travail contient des instances de pools SQL pour lesquels la synchronisation des métadonnées est activée, <!--[ADD LINK]--> ou si vous créez une instance de pool SQL avec la synchronisation des métadonnées activée, les bases de données créées avec Spark et leurs tables Parquet sont mappées automatiquement dans la base de données du pool SQL comme décrit dans [Base de données partagée Azure Synapse Analytics](database.md).

<!--[INSERT PICTURE]-->

<!--__Figure 1 -__ Supporting the Modern Data Warehouse Pattern with shared metadata-->

La synchronisation des objets se produit de façon asynchrone. Il y a un délai de quelques secondes avant que les objets apparaissent dans le contexte SQL. Une fois qu’ils apparaissent, ils peuvent être interrogés, mais ils ne peuvent pas être mis à jour ni modifiés par les moteurs SQL qui y ont accès.

## <a name="which-metadata-objects-are-shared"></a>Quels sont les objets de métadonnées partagés ?

Spark vous permet de créer des bases de données, des tables externes, des tables gérées et des vues. Étant donné que les vues Spark nécessitent un moteur Spark pour traiter l’instruction SQL Spark de définition, et ne peuvent pas être traitées par un moteur SQL, seules les bases de données et les tables externes et gérées qu’elles contiennent qui utilisent le format de stockage Parquet sont partagées avec les moteurs SQL de l’espace de travail. Les vues Spark sont partagées uniquement par les instances de pool Spark.

## <a name="security-model-at-a-glance"></a>Modèle de sécurité en un clin d’œil

Les bases de données et les tables Spark, ainsi que leurs représentations synchronisées dans les moteurs SQL, sont sécurisées au niveau du stockage sous-jacent. Quand la table est interrogée par l’un des moteurs que l’émetteur de la requête a le droit d’utiliser, le principal de sécurité de l’émetteur de la requête est transmis aux fichiers sous-jacents. Les autorisations sont vérifiées au niveau du système de fichiers.

Pour plus d’informations, consultez [Base de données partagée Azure Synapse Analytics](database.md).

## <a name="change-maintenance"></a>Maintenance des modifications

Si un objet de métadonnées est supprimé ou modifié avec Spark, les modifications sont récupérées et propagées vers le moteur SQL à la demande et les pools SQL qui synchronisent les objets. La synchronisation est asynchrone, et les modifications sont reflétées dans les moteurs SQL après un bref délai.

## <a name="next-steps"></a>Étapes suivantes

- [Apprenez-en davantage sur les bases de données de métadonnées partagées Azure Synapse Analytics](database.md)
- [Apprenez-en davantage sur les tables de métadonnées partagées Azure Synapse Analytics](table.md)

