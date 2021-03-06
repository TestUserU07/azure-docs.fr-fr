---
title: Résolution des problèmes liés à la solution Start/Stop VMs during off-hours
description: Cet article fournit des informations sur la résolution des problèmes rencontrés avec la solution Start/Stop VMs during off hours (Démarrer/Arrêter des machines virtuelles durant les heures creuses).
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 611e8441fab56114ca010d0b555c9ed156ae9d40
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82855061"
---
# <a name="troubleshoot-the-startstop-vms-during-off-hours-solution"></a>Résoudre les problèmes liés à la solution Start/Stop VMs during off-hours

Cet article fournit des informations sur la résolution des problèmes qui surviennent lorsque vous utilisez la solution Azure Automation Start/Stop VMs during off-hours (Démarrer/Arrêter des machines virtuelles durant les heures creuses).

>[!NOTE]
>Cet article a été mis à jour pour tenir compte de l’utilisation du nouveau module Az d’Azure PowerShell. Vous pouvez toujours utiliser le module AzureRM, qui continue à recevoir des correctifs de bogues jusqu’à au moins décembre 2020. Pour en savoir plus sur le nouveau module Az et la compatibilité avec AzureRM, consultez [Présentation du nouveau module Az d’Azure PowerShell](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Pour obtenir des instructions relatives à l’installation du module Az sur votre runbook Worker hybride, voir [Installer le module Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Pour votre compte Azure Automation, vous pouvez mettre à jour vos modules vers la toute dernière version en suivant les instructions du [Guide de mise à jour des modules Azure PowerShell dans Azure Automation](../automation-update-azure-modules.md).

## <a name="scenario-the-startstop-vms-during-off-hours-solution-fails-to-properly-deploy"></a><a name="deployment-failure"></a>Scénario : Le déploiement de la solution Start/Stop VMs during off-hours ne fonctionne pas correctement

### <a name="issue"></a>Problème

Lorsque vous déployez la [solution Start/Stop VMs during off-hours](../automation-solution-vm-management.md), vous recevez l’une des erreurs suivantes :

```error
Account already exists in another resourcegroup in a subscription. ResourceGroupName: [MyResourceGroup].
```

```error
Resource 'StartStop_VM_Notification' was disallowed by policy. Policy identifiers: '[{\\\"policyAssignment\\\":{\\\"name\\\":\\\"[MyPolicyName]".
```

```error
The subscription is not registered to use namespace 'Microsoft.OperationsManagement'.
```

```error
The subscription is not registered to use namespace 'Microsoft.Insights'.
```

```error
The scope '/subscriptions/000000000000-0000-0000-0000-00000000/resourcegroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView' cannot perform write operation because following scope(s) are locked: '/subscriptions/000000000000-0000-0000-0000-00000000/resourceGroups/<ResourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<WorkspaceName>/views/StartStopVMView'. Please remove the lock and try again
```

```error
A parameter cannot be found that matches parameter name 'TagName'
```

```error
Start-AzureRmVm : Run Login-AzureRmAccount to login
```

### <a name="cause"></a>Cause

Les déploiements peuvent échouer pour l’une des raisons suivantes :

- Il existe déjà un compte Automation du même nom dans la région sélectionnée.
- Une stratégie interdit le déploiement de la solution Start/Stop VMs during off-hours.
- Le type de ressource `Microsoft.OperationsManagement`, `Microsoft.Insights` ou `Microsoft.Automation` n’est pas inscrit.
- Votre espace de travail Log Analytics est verrouillé.
- Vous avez une version obsolète des modules AzureRM ou de la solution Start/Stop VMs during off-hours.

### <a name="resolution"></a>Résolution

Examinez les correctifs suivants pour trouver les solutions possibles à votre problème :

* Les comptes Automation doivent être uniques dans une région Azure, même s’ils se trouvent dans différents groupes de ressources. Vérifiez vos comptes Automation dans la région cible.
* Une stratégie existante empêche le déploiement d’une ressource nécessaire à la solution Start/Stop VMs during off-hours. Accédez à vos affectations de stratégie sur le portail Azure et regardez s’il en existe une qui interdit le déploiement de cette ressource. Pour plus d’informations, consultez [Erreur RequestDisallowedByPolicy](../../azure-resource-manager/templates/error-policy-requestdisallowedbypolicy.md).
* Votre abonnement doit être inscrit aux espaces de noms de ressources Azure suivants pour que vous puissiez déployer la solution Start/Stop VMs :

    * `Microsoft.OperationsManagement`
    * `Microsoft.Insights`
    * `Microsoft.Automation`

   Pour plus d’informations sur les erreurs survenant lors de l’inscription des fournisseurs, consultez [Résoudre les erreurs d’inscription de fournisseurs de ressources](../../azure-resource-manager/templates/error-register-resource-provider.md).
* Si votre espace de travail Log Analytics est verrouillé, accédez-y sur le Portail Azure et supprimez tous les verrous de la ressource.
* Si ces solutions ne résolvent pas votre problème, suivez les instructions sous [Mettre à jour la solution](../automation-solution-vm-management.md#update-the-solution) pour redéployer la solution Start/Stop VMs during off hours.

## <a name="scenario-all-vms-fail-to-start-or-stop"></a><a name="all-vms-fail-to-startstop"></a>Scénario : Échec du démarrage ou de l’arrêt de toutes les machines virtuelles

### <a name="issue"></a>Problème

Vous avez configuré la solution Start/Stop VMs during off-hours, mais elle ne démarre pas ou n’arrête pas toutes les machines virtuelles.

### <a name="cause"></a>Cause

Cette erreur peut être due à l’une des raisons suivantes :

- Une planification n’est pas configurée correctement.
- Le compte d’identification n’est peut-être pas configuré correctement.
- Un runbook peut avoir rencontré des erreurs.
- Les machines virtuelles ont peut-être été exclues.

### <a name="resolution"></a>Résolution

Examinez la liste suivante pour trouver les solutions possibles à votre problème :

* Vérifiez que vous avez correctement configuré une planification pour la solution Start/Stop VMs during off-hours. Pour savoir comment configurer une planification, consultez [Planifications](../automation-schedules.md).

* Vérifiez les [flux de tâches](../automation-runbook-execution.md#job-statuses) afin de détecter d’éventuelles erreurs. Recherchez des tâches dans l’un des runbooks suivants :

  * **AutoStop_CreateAlert_Child**
  * **AutoStop_CreateAlert_Parent**
  * **AutoStop_Disable**
  * **AutoStop_VM_Child**
  * **ScheduledStartStop_Base_Classic**
  * **ScheduledStartStop_Child_Classic**
  * **ScheduledStartStop_Child**
  * **ScheduledStartStop_Parent**
  * **SequencedStartStop_Parent**

* Vérifiez que votre [compte d’identification](../manage-runas-account.md) dispose des autorisations appropriées pour les machines virtuelles que vous tentez de démarrer ou d’arrêter. Pour savoir comment vérifier les autorisations sur une ressource, consultez [Démarrage rapide : Afficher les rôles attribués à un utilisateur à l’aide du portail Azure](../../role-based-access-control/check-access.md). Vous devez fournir l’ID d’application du principal de service utilisé par le compte d’identification. Vous pouvez récupérer cette valeur en accédant à votre compte Automation dans le portail Azure. Sélectionnez **Comptes d’identification** sous **Paramètres du compte**, puis sélectionnez le compte d’identification approprié.

* Les machines virtuelles peuvent ne pas être démarrées ou arrêtées si elles sont explicitement exclues. Les machines virtuelles exclues sont définies dans la variable `External_ExcludeVMNames` du compte Automation sur lequel la solution est déployée. L’exemple suivant montre comment vous pouvez interroger cette valeur avec PowerShell.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

## <a name="scenario-some-of-my-vms-fail-to-start-or-stop"></a><a name="some-vms-fail-to-startstop"></a>Scénario : Le démarrage/l’arrêt de certaines de mes machines virtuelles échouent

### <a name="issue"></a>Problème

Vous avez configuré la solution Start/Stop VMs during off-hours, mais elle ne démarre pas ou n’arrête pas certaines des machines virtuelles configurées.

### <a name="cause"></a>Cause

Cette erreur peut être due à l’une des raisons suivantes :

- Dans le scénario de séquence, une balise est peut-être manquante ou incorrecte.
- La machine virtuelle est peut-être exclue.
- Le compte d’identification ne dispose peut-être pas des autorisations suffisantes sur la machine virtuelle.
- La machine virtuelle peut avoir un problème qui l’a empêchée de démarrer ou de s’arrêter.

### <a name="resolution"></a>Résolution

Examinez la liste suivante pour trouver des solutions possibles à votre problème ou des endroits où en chercher :

* Quand vous utilisez le [scénario de séquence](../automation-solution-vm-management.md) de la solution Start/Stop VMs during off-hours, vous devez vérifier que chaque machine virtuelle que vous voulez démarrer ou arrêter dispose de la balise appropriée. Vérifiez que les machines virtuelles que vous voulez démarrer ont la balise `sequencestart` et que celles que vous voulez arrêter ont la balise `sequencestop`. Les deux balises exigent une valeur entière positive. Vous pouvez utiliser une requête semblable à l’exemple suivant pour rechercher toutes les machines virtuelles avec les balises et leurs valeurs.

  ```powershell-interactive
  Get-AzResource | ? {$_.Tags.Keys -contains "SequenceStart" -or $_.Tags.Keys -contains "SequenceStop"} | ft Name,Tags
  ```

* Les machines virtuelles peuvent ne pas être démarrées ou arrêtées si elles sont explicitement exclues. Les machines virtuelles exclues sont définies dans la variable `External_ExcludeVMNames` du compte Automation sur lequel la solution est déployée. L’exemple suivant montre comment vous pouvez interroger cette valeur avec PowerShell.

  ```powershell-interactive
  Get-AzAutomationVariable -Name External_ExcludeVMNames -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName> | Select-Object Value
  ```

* Pour démarrer et arrêter des machines virtuelles, le compte d’identification du compte Automation doit disposer des autorisations appropriées sur la machine virtuelle. Pour savoir comment vérifier les autorisations sur une ressource, consultez [Démarrage rapide : Afficher les rôles attribués à un utilisateur à l’aide du portail Azure](../../role-based-access-control/check-access.md). Vous devez fournir l’ID d’application du principal de service utilisé par le compte d’identification. Vous pouvez récupérer cette valeur en accédant à votre compte Automation dans le portail Azure. Sélectionnez **Comptes d’identification** sous **Paramètres du compte**, puis sélectionnez le compte d’identification approprié.
* Si la machine virtuelle rencontre un problème de démarrage ou de désallocation, il peut y avoir un problème sur la machine virtuelle elle-même. Une mise à jour qui est appliquée lorsque la machine virtuelle tente de s’arrêter, un service qui se bloque, etc. constituent des exemples. Accédez à la ressource de votre machine virtuelle et vérifiez les **Journaux d’activité** pour voir s’ils indiquent des erreurs. Vous pouvez également essayer de vous connecter à la machine virtuelle pour voir s’il existe des erreurs dans les journaux des événements. Pour en savoir plus sur le dépannage de votre machine virtuelle, voir [Résolution des problèmes liés aux machines virtuelles Azure](../../virtual-machines/troubleshooting/index.yml).
* Vérifiez les [flux de tâches](../automation-runbook-execution.md#job-statuses) afin de détecter d’éventuelles erreurs. Dans le portail, accédez à votre compte Automation, puis sélectionnez **Tâches** sous **Automatisation des processus**.

## <a name="scenario-my-custom-runbook-fails-to-start-or-stop-my-vms"></a><a name="custom-runbook"></a>Scénario : Mon runbook personnalisé ne parvient pas à démarrer ou à arrêter mes machines virtuelles

### <a name="issue"></a>Problème

Vous avez créé un runbook personnalisé ou vous en avez téléchargé un à partir de PowerShell Gallery, et il ne fonctionne pas correctement.

### <a name="cause"></a>Cause

Plusieurs causes peuvent être à l’origine de l’échec. Accédez à votre compte Automation dans le portail Azure, puis sélectionnez **Travaux** sous **Automatisation des processus**. Dans la page **Tâches**, recherchez les tâches de votre runbook pour afficher les éventuels échecs des tâches.

### <a name="resolution"></a>Résolution

Nous vous recommandons :

* Utiliser la [solution Start/Stop VMs during off-hours](../automation-solution-vm-management.md) pour démarrer et arrêter des machines virtuelles dans Azure Automation. Cette solution a été créée par Microsoft. 
* N’oubliez pas que Microsoft ne prend pas en charge les runbooks personnalisés. Vous pouvez trouver une solution pour votre runbook personnalisé dans la [Résolution des problèmes liés aux runbooks](runbooks.md). Vérifiez les [flux de tâches](../automation-runbook-execution.md#job-statuses) afin de détecter d’éventuelles erreurs. 

## <a name="scenario-vms-dont-start-or-stop-in-the-correct-sequence"></a><a name="dont-start-stop-in-sequence"></a>Scénario : Les machines virtuelles ne démarrent pas ou ne s’arrêtent pas dans l’ordre approprié

### <a name="issue"></a>Problème

Les machines virtuelles que vous avez configurées dans la solution ne démarrent pas ou ne s’arrêtent pas dans l’ordre approprié.

### <a name="cause"></a>Cause

Ce problème est dû à un balisage incorrect sur les machines virtuelles.

### <a name="resolution"></a>Résolution

Suivez ces étapes pour vérifier que la solution est correctement configurée.

1. Vérifiez que toutes les machines virtuelles à démarrer ou à arrêter ont une balise `sequencestart` ou `sequencestop`, suivant votre situation. La valeur de ces balises doit être un entier positif. Les machines virtuelles sont traitées dans l’ordre croissant en fonction de cette valeur.
1. Vérifiez que les groupes de ressources des machines virtuelles à démarrer ou à arrêter sont dans les variables `External_Start_ResourceGroupNames` ou `External_Stop_ResourceGroupNames`, suivant votre situation.
1. Testez vos modifications en exécutant le runbook `SequencedStartStop_Parent` avec le paramètre `WHATIF` défini sur True pour les prévisualiser.

Pour plus d’informations sur l’utilisation de la solution permettant de démarrer et d’arrêter des machines virtuelles en séquence, consultez [Démarrer/arrêter des machines virtuelles en séquence](../automation-solution-vm-management.md).

## <a name="scenario-startstop-vms-during-off-hours-job-fails-with-403-forbidden-error"></a><a name="403"></a>Scénario : La tâche Démarrer/arrêter des machines virtuelles pendant les heures creuses échoue avec une erreur 403 forbidden

### <a name="issue"></a>Problème

Vous trouvez des tâches qui ont échoué avec une erreur `403 forbidden` pour les runbooks de la solution Start/Stop VMs during off-hours.

### <a name="cause"></a>Cause

Ce problème peut être dû à un compte d’identification incorrectement configuré ou ayant expiré. Il peut également être dû à des autorisations inappropriées pour les ressources des machines virtuelles accordées par le compte d’identification.

### <a name="resolution"></a>Résolution

Pour vérifier que votre compte d’identification est correctement configuré, accédez à votre compte Automation dans le Portail Azure, puis sélectionnez **Comptes d’identification** sous **Paramètres du compte**. Si un compte d’identification n’est pas correctement configuré ou a expiré, l’état affiche la condition.

Si votre compte d’identification est mal configuré, supprimez-le et recréez-le. Pour plus d’informations, consultez [Gérer les comptes d’identification Azure Automation](../manage-runas-account.md).

Si le certificat a expiré pour votre compte d’identification, suivez les étapes dans [Renouvellement de certificat auto-signé](../manage-runas-account.md#cert-renewal) pour renouveler le certificat.

S’il manque des autorisations, consultez [Démarrage rapide : Afficher les rôles attribués à un utilisateur à l’aide du portail Azure](../../role-based-access-control/check-access.md). Vous devez fournir l’ID d’application du principal de service utilisé par le compte d’identification. Vous pouvez récupérer cette valeur en accédant à votre compte Automation dans le portail Azure. Sélectionnez **Comptes d’identification** sous **Paramètres du compte**, puis sélectionnez le compte d’identification approprié.

## <a name="scenario-my-problem-isnt-listed-here"></a><a name="other"></a>Scénario : Mon problème n’est pas répertorié ici

### <a name="issue"></a>Problème

Lors de l’utilisation de la solution Start/Stop VMs during off-hours, vous rencontrez un problème ou obtenez un résultat inattendu qui n’est pas listé dans cette page.

### <a name="cause"></a>Cause

Souvent, les erreurs peuvent être dues à l’utilisation d’une ancienne version obsolète de la solution.

> [!NOTE]
> La solution Start/Stop VMs during off-hours a été testée avec les modules Azure importés dans votre compte Automation lors du déploiement de la solution. Actuellement, la solution ne fonctionne pas avec des versions plus récentes du module Azure. Cette restriction affecte uniquement le compte Automation que vous utilisez pour exécuter la solution Start/Stop VMs during off-hours. Vous pouvez toujours utiliser des versions plus récentes du module Azure dans vos autres comptes Automation, comme décrit dans le [Guide de mise à jour des modules Azure PowerShell dans Azure Automation](../automation-update-azure-modules.md).

### <a name="resolution"></a>Résolution

Pour résoudre de nombreuses erreurs, pensez à supprimer et à [mettre à jour la solution Start/Stop VMs during off-hours](../automation-solution-vm-management.md#update-the-solution). Vous pouvez aussi vérifier les [flux de travail](../automation-runbook-execution.md#job-statuses) afin de détecter d’éventuelles erreurs. 

## <a name="next-steps"></a>Étapes suivantes

Si votre problème ne figure pas ici ou que vous ne pouvez pas le résoudre, utilisez un des canaux suivants pour obtenir une aide supplémentaire :

* Obtenez des réponses de la part d’experts Azure via les [Forums Azure](https://azure.microsoft.com/support/forums/).
* Connectez-vous à [@AzureSupport](https://twitter.com/azuresupport), le compte Microsoft Azure officiel pour améliorer l’expérience client. Le Support Azure fournit à la communauté Azure des réponses, un support technique et des experts.
* Signaler un incident au support Azure Accédez au [site du support Azure](https://azure.microsoft.com/support/options/), puis sélectionnez **Obtenir de l’aide**.