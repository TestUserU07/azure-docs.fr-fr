---
title: Planifier des exécutions de packages SSIS en utilisant l’agent Azure SQL Database Managed Instance
description: Découvrez comment planifier des exécutions de packages SSIS en utilisant l’agent Azure SQL Database Managed Instance.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: lle
author: lle
ms.date: 04/14/2020
ms.openlocfilehash: f230e4d33686b006b20e856d5e8033847e3f3d67
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628484"
---
# <a name="schedule-ssis-package-executions-by-using-azure-sql-database-managed-instance-agent"></a>Planifier des exécutions de packages SSIS en utilisant l’agent Azure SQL Database Managed Instance

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Cet article décrit comment exécuter un package SQL Server Integration Services (SSIS) en utilisant l’agent Azure SQL Database Managed Instance. Cette fonctionnalité fournit des comportements qui rappellent la planification de packages SSIS à l’aide de SQL Server Agent dans un environnement local.

Avec cette fonctionnalité, vous pouvez exécuter des packages SSIS stockés dans SSISDB dans une instance managée Azure SQL Database ou un système de fichiers comme Azure Files.

## <a name="prerequisites"></a>Prérequis
Pour utiliser cette fonctionnalité, [téléchargez](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) et installez la dernière version de SSMS (SQL Server Management Studio), à savoir la version 18.5.

Vous devez également [provisionner un runtime d’intégration Azure-SSIS](tutorial-create-azure-ssis-runtime-portal.md) dans Azure Data Factory. Il utilise une instance managée Azure SQL Database comme serveur de point de terminaison. 

## <a name="run-an-ssis-package-in-ssisdb"></a>Exécuter un package SSIS dans SSISDB
Dans cette procédure, vous utilisez l’agent Azure SQL Database Managed Instance pour appeler un package SSIS stocké dans SSISDB.

1. Dans la dernière version de SSMS, connectez-vous à une instance managée Azure SQL Database.
1. Créez un travail d’agent et une étape de travail. Sous **SQL Server Agent**, cliquez avec le bouton droit sur le dossier **Travaux**, puis sélectionnez **Nouveau travail**.

   ![Sélections permettant la création d’un travail d’agent](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. Dans la page **Nouvelle étape de travail**, sélectionnez **Package SQL Server Integration Services** comme type.

   ![Sélections permettant la création d’une étape de travail SSIS](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. Sous l’onglet **Package**, sélectionnez **Catalogue SSIS** comme type de source du package.
1. SSISDB se trouvant dans la même instance managée Azure SQL Database, vous n’êtes pas tenu de spécifier l’authentification.
1. Spécifiez un package SSIS à partir de SSISDB.

   ![Onglet Package avec des sélections pour le type de source du package](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb.png)

1. Sous l’onglet **Configuration**, vous pouvez :
  
   - Spécifier des valeurs de paramètre sous **Paramètres**.
   - Remplacer des valeurs sous **Gestionnaires de connexions**.
   - Remplacer la propriété et choisir le niveau de journalisation sous **Avancé**.

   ![Onglet Configuration avec des sélections pour le type de source du package](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb-configuration.png)

1. Sélectionnez **OK** pour enregistrer la configuration du travail d’agent.
1. Démarrez le travail d’agent pour exécuter le package SSIS.


## <a name="run-an-ssis-package-in-the-file-system"></a>Exécuter un package SSIS dans le système de fichiers
Dans cette procédure, vous utilisez l’agent Azure SQL Database Managed Instance pour exécuter un package SSIS stocké dans le système de fichiers.

1. Dans la dernière version de SSMS, connectez-vous à une instance managée Azure SQL Database.
1. Créez un travail d’agent et une étape de travail. Sous **SQL Server Agent**, cliquez avec le bouton droit sur le dossier **Travaux**, puis sélectionnez **Nouveau travail**.

   ![Sélections permettant la création d’un travail d’agent](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. Dans la page **Nouvelle étape de travail**, sélectionnez **Package SQL Server Integration Services** comme type.

   ![Sélections permettant la création d’une étape de travail SSIS](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. Sous l’onglet **Package** :

   1. Pour **Source du package**, sélectionnez **Système de fichiers**.
   
   1. Pour **Type de source de fichier** :   

      - Si votre package est chargé sur Azure Files, sélectionnez **Partage de fichiers Azure**.

        ![Options pour le type de source de fichier](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-file-system.png)
      
        Le chemin d’accès au package est **`\\<storage account name>.file.core.windows.net\<file share name>\<package name>.dtsx`** .
      
        Sous **Informations d’identification d’accès au fichier du package**, entrez le nom du compte de fichier Azure et la clé du compte pour accéder au fichier Azure. Le domaine est défini sur **Azure**.

      - Si votre package est chargé sur un partage réseau, sélectionnez **Partage réseau**.
      
        Le chemin du package correspond au chemin UNC de votre fichier de package avec son extension dtsx.
      
        Entrez le domaine, le nom d’utilisateur et le mot de passe correspondants pour accéder au fichier de package de partage réseau.
   1. Si votre fichier de package est chiffré avec un mot de passe, sélectionnez **Mot de passe de chiffrement** et entrez le mot de passe.
1. Sous l’onglet **Configurations**, entrez le chemin du fichier de configuration s’il vous faut un fichier de configuration pour exécuter le package SSIS.
   Si vous stockez votre configuration dans Azure Files, son chemin d’accès de configuration sera **`\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`** .
1. Sous l’onglet **Options d’exécution**, sélectionnez **Authentification Windows** ou **Runtime 32 bits** pour exécuter le package SSIS.
1. Sous l’onglet **Journalisation**, sélectionnez le chemin de journalisation et les informations d’identification d’accès à la journalisation correspondantes pour stocker les fichiers journaux. 
   Par défaut, le chemin de journalisation est identique au chemin du dossier du package, et les informations d’identification d’accès à la journalisation sont identiques à celles du package.
   Si vous stockez vos journaux dans Azure Files, votre chemin d’accès de journalisation sera **`\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`** .
1. Sous l’onglet **Valeurs définies**, vous pouvez entrer le chemin de la propriété et la valeur pour remplacer les propriétés du package.
 
   Par exemple, pour remplacer la valeur de votre variable utilisateur, entrez son chemin d’accès au format suivant : **`\Package.Variables[User::<variable name>].Value`** .
1. Sélectionnez **OK** pour enregistrer la configuration du travail d’agent.
1. Démarrez le travail d’agent pour exécuter le package SSIS.


## <a name="cancel-ssis-package-execution"></a>Annuler une exécution de package SSIS
Pour annuler l’exécution d’un package à partir d’un travail de l’agent Azure SQL Database Managed Instance, suivez les étapes ci-dessous plutôt que d’arrêter directement le travail de l’agent :

1. Recherchez la valeur **jobId** de votre agent SQL à partir de **msdb.dbo.sysjobs**.
1. Recherchez la valeur **executionId** SSIS correspondante en fonction de l’ID de travail à l’aide de cette requête :
   ```sql
   select * from ssisdb.internal.execution_parameter_values_noncatalog where  parameter_value = 'SQL_Agent_Job_{jobId}' order by execution_id desc
   ```
1. Cliquez avec le bouton droit sur le catalogue SSISDB, puis sélectionnez **Opérations actives**.

   ![« Opérations actives » dans le menu contextuel du catalogue SSISDB](./media/how-to-invoke-ssis-package-managed-instance-agent/catalog-active-operations.png)

1. Arrêtez l’opération correspondante en fonction de la valeur **executionId**.

## <a name="next-steps"></a>Étapes suivantes
Vous pouvez également planifier des packages SSIS à l’aide d’Azure Data Factory. Pour obtenir des instructions pas à pas, consultez [Déclencheur d’événements Azure Data Factory](how-to-create-event-trigger.md). 
