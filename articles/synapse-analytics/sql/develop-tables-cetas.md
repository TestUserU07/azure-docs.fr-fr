---
title: CETAS dans Synapse SQL
description: Utilisation de CETAS avec Synapse SQL
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 71bc20680467d270436e28190bb49db5b9313ca0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420043"
---
# <a name="cetas-with-synapse-sql"></a>CETAS avec Synapse SQL

Dans le pool SQL ou dans SQL à la demande (préversion), vous pouvez utiliser CREATE EXTERNAL TABLE AS SELECT (CETAS) pour effectuer les tâches suivantes :  

- Créer une table externe
- Exporter, en parallèle, les résultats d’une instruction Transact-SQL SELECT vers :

  - Hadoop
  - Azure Storage Blob
  - Azure Data Lake Storage Gen2

## <a name="cetas-in-sql-pool"></a>CETAS dans un pool SQL

Pour plus d’informations sur le pool SQL, ainsi que sur l’utilisation et la syntaxe de CETAS, consultez l’article [CREATE EXTERNAL TABLE AS SELECT](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). En outre, pour obtenir des conseils sur CTAS avec un pool SQL, consultez l’article [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="cetas-in-sql-on-demand"></a>CETAS dans SQL à la demande

Lors de l’utilisation de la ressource SQL à la demande, CETAS est utilisé pour créer une table externe et exporter les résultats de la requête vers Azure Storage Blob ou Azure Data Lake Storage Gen2.

## <a name="syntax"></a>Syntaxe

```syntaxsql
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name
    WITH (
        LOCATION = 'path_to_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
)
    AS <select_statement>  
[;]

<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>
```

## <a name="arguments"></a>Arguments

*[ [ *database_name* . [ *schema_name* ] . ] | *schema_name* . ] *table_name**

Nom (composé d’une à trois parties) de la table à créer. Pour une table externe, SQL à la demande stocke uniquement les métadonnées de la table. Aucune donnée n’est déplacée ni stockée dans SQL à la demande.

LOCATION = *'path_to_folder'*

Spécifie l’emplacement dans lequel écrire les résultats de l’instruction SELECT exécutée sur la source de données externes. Le dossier racine est l’emplacement de données qui est spécifié dans la source de données externe. L’emplacement doit pointer vers un dossier et se terminer par un /. Exemple : aggregated_data/

DATA_SOURCE = *external_data_source_name*

Spécifie le nom de l’objet de source de données externe qui contient l’emplacement où les données externes vont être stockées. Pour créer une source de données externe, utilisez [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](develop-tables-external-tables.md#create-external-data-source).

FILE_FORMAT = *external_file_format_name*

Spécifie le nom de l’objet de format de fichier externe qui contient le format du fichier de données externe. Pour créer un format de fichier externe, utilisez [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](develop-tables-external-tables.md#create-external-file-format). Seuls les formats de fichier externes avec FORMAT='PARQUET' sont pris en charge.

WITH *<common_table_expression>*

Spécifie un jeu de résultats nommé temporaire, désigné par le terme d'expression de table commune (CTE, Common Table Expression). Pour plus d’informations, consultez [WITH common_table_expression (Transact-SQL)](/sql/t-sql/queries/with-common-table-expression-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

SELECT <select_criteria>

Remplit la nouvelle table avec les résultats d’une instruction SELECT. *select_criteria* correspond au corps de l’instruction SELECT qui détermine les données qui sont copiées dans la nouvelle table. Pour plus d’informations sur les instructions SELECT, consultez [SELECT (Transact-SQL)](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

## <a name="permissions"></a>Autorisations

Pour que CETAS fonctionne, vous devez disposer d’autorisations permettant d’afficher le contenu des dossiers et permettant d’écrire dans le dossier LOCATION.

## <a name="examples"></a>Exemples

Ces exemples utilisent CETAS pour enregistrer la population totale agrégée par année et par état dans le dossier aggregated_data qui se trouve dans la source de données population_ds.

Cet exemple s’appuie sur les informations d’identification, la source de données et le format de fichier externe créés précédemment. Reportez-vous au document [tables externes](develop-tables-external-tables.md). Pour enregistrer les résultats d’une requête dans un autre dossier d’une même source de données, modifiez l’argument LOCATION. Pour enregistrer les résultats dans un autre compte de stockage, créez une autre source de données pour l’argument DATA_SOURCE.

> [!NOTE]
> Les exemples qui suivent utilisent un compte Azure Open Data Storage public. Ce compte est en lecture seule. Pour exécuter ces requêtes, vous devez fournir la source de données pour laquelle vous disposez d’autorisations en écriture.

```sql
-- use CETAS to export select statement with OPENROWSET result to  storage
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM
    OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=*/*.parquet',
    FORMAT='PARQUET') AS [r]
GROUP BY decennialTime, stateName
GO

-- you can query created external table
SELECT * FROM population_by_year_state
```

L’exemple ci-dessous utilise une table externe comme source pour CETAS. Il s’appuie sur les informations d’identification, la source de données, le format de fichier externe et la table externe créés précédemment. Reportez-vous au document [tables externes](develop-tables-external-tables.md).

```sql
-- use CETAS with select from external table
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM census_external_table
GROUP BY decennialTime, stateName
GO

-- you can query created external table
SELECT * FROM population_by_year_state
```

## <a name="supported-data-types"></a>Types de données pris en charge

CETAS peut être utilisé pour stocker les jeux de résultats avec les types de données SQL suivants :

- binary
- varbinary
- char
- varchar
- Date
- time
- datetime2
- Décimal
- numeric
- float
- real
- bigint
- int
- SMALLINT
- TINYINT
- bit

Les applications métier ne peuvent pas être utilisés avec CETAS.

Les types de données suivants ne peuvent pas être utilisés dans la partie SELECT de CETAS :

- NCHAR
- NVARCHAR
- DATETIME
- smalldatetime
- datetimeoffset
- money
- SMALLMONEY
- UNIQUEIDENTIFIER

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez essayer d’interroger des [tables Spark](develop-storage-files-spark-tables.md).
