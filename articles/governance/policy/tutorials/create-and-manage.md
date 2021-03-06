---
title: 'Tutoriel : Générer des stratégies pour appliquer la conformité'
description: Dans ce tutoriel, vous utilisez des stratégies pour appliquer des normes, maîtriser les coûts, garantir la sécurité et imposer des principes de conception à l’échelle de l’entreprise.
ms.date: 03/24/2020
ms.topic: tutorial
ms.openlocfilehash: dcebbbfcc2f86ace7ea4400a2fdb6f1392f4efe6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82190824"
---
# <a name="tutorial-create-and-manage-policies-to-enforce-compliance"></a>Tutoriel : Créer et gérer des stratégies pour appliquer la conformité

Il est important de comprendre comment créer et gérer des stratégies dans Azure pour rester conforme aux normes de votre entreprise et aux contrats de niveau de service. Dans ce tutoriel, vous allez découvrir comment utiliser Azure Policy pour effectuer certaines des tâches les plus courantes liées à la création, à l’affectation et à la gestion des stratégies dans votre organisation, notamment :

> [!div class="checklist"]
> - Affecter une stratégie pour appliquer une condition aux ressources que vous créez dans le futur
> - Créer et affecter une définition d’initiative pour effectuer le suivi de la conformité de plusieurs ressources
> - Résoudre une ressource non conforme ou refusée
> - Implémenter une nouvelle stratégie dans l’ensemble de l’entreprise

Si vous souhaitez affecter une stratégie pour identifier l’état actuel de la conformité de vos ressources existantes, consultez les articles de démarrage rapide qui passent en revue la marche à suivre.

## <a name="prerequisites"></a>Prérequis

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="assign-a-policy"></a>Attribution d’une stratégie

La première étape de l’application de la conformité avec une stratégie Azure consiste à affecter une définition de stratégie. Une définition de stratégie définit dans quelle condition une stratégie est appliquée et l’action à effectuer. Dans cet exemple, affectez la définition de stratégie intégrée appelée _Hériter d’une étiquette du groupe de ressources si elle est manquante_ pour ajouter l’étiquette spécifiée avec sa valeur du groupe de ressources parent à des ressources nouvelles ou mises à jour auxquelles il manque l’étiquette.

1. Accédez au portail Azure pour attribuer des stratégies. Recherchez et sélectionnez **Stratégie**.

   :::image type="content" source="../media/create-and-manage/search-policy.png" alt-text="Rechercher Stratégie dans la barre de recherche" border="false":::

1. Sélectionnez **Affectations** du côté gauche de la page Azure Policy. Une affectation est une stratégie qui a été affectée pour être appliquée dans une étendue spécifique.

   :::image type="content" source="../media/create-and-manage/select-assignments.png" alt-text="Sélectionner des affectations à partir de la page de vue d’ensemble de la stratégie" border="false":::

1. Sélectionnez **Assigner une stratégie** en haut de la page**Stratégie - Affectations**.

   :::image type="content" source="../media/create-and-manage/select-assign-policy.png" alt-text="Affecter une définition de stratégie à partir de la page Affectations" border="false":::

1. Dans la page **Assigner une stratégie**, sous l’onglet **De base**, sélectionnez l’**étendue** en sélectionnant les points de suspension, puis en sélectionnant un groupe d’administration ou un abonnement. Sélectionnez éventuellement un groupe de ressources. Une étendue détermine les ressources ou le regroupement de ressources sur lequel la stratégie est appliquée.
   Cliquez ensuite sur **Sélectionner** au bas de la page **Étendue**.

   Cet exemple utilise l’abonnement **Contoso**. Votre abonnement sera différent.

1. Vous pouvez exclure des ressources en fonction de **l’étendue**. Les **exclusions** commencent à un niveau inférieur à celui de **l’étendue**. Les **exclusions** étant facultatives, laissez ce champ vide pour l’instant.

1. Sélectionnez les points de suspension de **Définition de stratégie** pour ouvrir la liste des définitions disponibles. Vous pouvez filtrer le **Type** de définition de stratégie sur _BuiltIn_ pour les afficher tous et lire leurs descriptions.

1. Sélectionnez **Hériter d’une étiquette du groupe de ressources si elle est manquante**. Si vous ne trouvez pas l’option immédiatement, tapez **hériter d’une étiquette** dans la zone de recherche, puis appuyez sur Entrée ou cliquez en dehors de la zone de recherche.
   Cliquez sur **Sélectionner** au bas de la page **Définitions disponibles** une fois que vous avez trouvé et sélectionné la définition de stratégie.

   :::image type="content" source="../media/create-and-manage/select-available-definition.png" alt-text="Utiliser le filtre de recherche pour localiser une stratégie":::

1. Le **Nom de l’attribution** est automatiquement rempli avec le nom de stratégie que vous avez sélectionné, mais vous pouvez le modifier. Pour cet exemple, laissez _Hériter d’une étiquette du groupe de ressources si elle est manquante_. Vous pouvez également ajouter une **Description** (facultatif). La description fournit des détails sur cette affectation de stratégie.

1. Laissez **Application de la stratégie** définie sur _Activée_. Le paramètre _Désactivée_ permet de tester le résultat de la stratégie sans en déclencher l’effet. Pour plus d’informations, consultez [Mode de mise en conformité](../concepts/assignment-structure.md#enforcement-mode).

1. **Affectée par** est automatiquement renseigné en fonction de l’utilisateur connecté. Ce champ étant facultatif, vous pouvez entrer des valeurs personnalisées.

1. Sélectionnez l’onglet **Paramètres** en haut de l’Assistant.

1. Pour **Nom de l’étiquette**, entrez _Environnement_.

1. Sélectionnez l’onglet **Correction** en haut de l’Assistant.

1. Laissez la case **Créer une tâche de correction** décochée. Cette case vous permet de créer une tâche pour modifier les ressources existantes en plus des ressources nouvelles ou mises à jour. Pour plus d’informations, consultez [Corriger les ressources](../how-to/remediate-resources.md).

1. La case **Créer une identité managée** est automatiquement cochée car cette définition de stratégie utilise l’effet [modifier](../concepts/effects.md#modify). L’option **Autorisations** a automatiquement la valeur _Contributeur_ en fonction de la définition de stratégie. Pour plus d’informations, consultez [Identités managées](../../../active-directory/managed-identities-azure-resources/overview.md) et [Fonctionnement de la sécurité par correction](../how-to/remediate-resources.md#how-remediation-security-works).

1. Sélectionnez l’onglet **Vérifier + créer** en haut de l’Assistant.

1. Passez en revue vos sélections, puis sélectionnez **Créer** au bas de la page.

## <a name="implement-a-new-custom-policy"></a>Implémenter une nouvelle stratégie personnalisée

Maintenant que vous avez affecté une définition de stratégie intégrée, vous pouvez continuer avec Azure Policy. Créez une stratégie personnalisée pour réduire les coûts en veillant à ce que les machines virtuelles créées dans votre environnement ne puissent pas être dans la série G. De cette manière, à chaque fois qu’un utilisateur de votre organisation tente de créer une machine virtuelle dans la série G, la requête est refusée.

1. Sélectionnez **Définitions** sous **Création** dans la partie gauche de la page Azure Policy.

   :::image type="content" source="../media/create-and-manage/definition-under-authoring.png" alt-text="Page Définition sous Création de groupe" border="false":::

1. Sélectionnez **+ Définition de stratégie** en haut de la page. Ce bouton ouvre la page **Définition de stratégie**.

1. Entrez les informations suivantes :

   - Le groupe d’administration ou l’abonnement dans lequel la définition de stratégie est enregistrée. Sélectionnez-le à l’aide du bouton de sélection **Emplacement de définition**.

     > [!NOTE]
     > Si vous envisagez d’appliquer cette définition de stratégie à plusieurs abonnements, l’emplacement doit correspondre à un groupe d’administration qui contient les abonnements auxquels vous affectez la stratégie. Il en va de même pour une définition d’initiative.

   - Le nom de la définition de stratégie : _*_nécessite des références (SKU) de machines virtuelles inférieures à la série G_
   - La description de l’objectif de la définition de stratégie : _cette définition de stratégie impose que toutes les machines virtuelles créées dans cette étendue possèdent des références SKU inférieures à la série G afin de réduire le coût._
   - Faites votre choix parmi les options existantes (par exemple _Calculer_), ou créez une catégorie pour cette définition de stratégie.
   - Copiez le code JSON suivant, puis mettez-le à jour selon vos besoins avec :
      - Les paramètres de la stratégie.
      - Les règles/conditions de la stratégie, dans ce cas : la taille de la référence (SKU) de la machine virtuelle est égale à la série G
      - L’effet de la stratégie, dans ce cas – **Refuser**.

   Voici à quoi le code JSON doit ressembler. Collez le code révisé dans le portail Azure.

   ```json
   {
       "policyRule": {
           "if": {
               "allOf": [{
                       "field": "type",
                       "equals": "Microsoft.Compute/virtualMachines"
                   },
                   {
                       "field": "Microsoft.Compute/virtualMachines/sku.name",
                       "like": "Standard_G*"
                   }
               ]
           },
           "then": {
               "effect": "deny"
           }
       }
   }
   ```

   La propriété _champ_ dans la règle de stratégie doit être une valeur prise en charge. La liste complète des valeurs est disponible dans les [champs de structure de la définition de stratégie](../concepts/definition-structure.md#fields). Exemple d’alias : `"Microsoft.Compute/VirtualMachines/Size"`.

   Pour voir d’autres exemples Azure Policy, consultez [Exemples Azure Policy](../samples/index.md).

1. Sélectionnez **Enregistrer**.

## <a name="create-a-policy-definition-with-rest-api"></a>Créer une définition de stratégie avec l’API REST

Vous pouvez créer une stratégie avec l’API REST pour les définitions Azure Policy. L’API REST vous permet de créer et de supprimer des définitions de stratégies, ainsi que d’obtenir des informations sur les définitions existantes. Pour créer une définition de stratégie, utilisez l’exemple suivant :

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Incluez un texte de demande semblable à l’exemple suivant :

```json
{
    "properties": {
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

## <a name="create-a-policy-definition-with-powershell"></a>Créer une définition de stratégie avec PowerShell

Avant de passer à l’exemple PowerShell, assurez-vous d’avoir installé la dernière version du module Azure PowerShell Az.

Vous pouvez créer une définition de stratégie en utilisant l’applet de commande `New-AzPolicyDefinition`.

Pour créer une définition de stratégie à partir d’un fichier, transmettez le chemin d’accès au fichier. Pour un fichier externe, utilisez l’exemple suivant :

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -DisplayName 'Deny cool access tiering for storage' `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Pour un fichier local, utilisez l’exemple suivant :

```azurepowershell-interactive
$definition = New-AzPolicyDefinition `
    -Name 'denyCoolTiering' `
    -Description 'Deny cool access tiering for storage' `
    -Policy 'c:\policies\coolAccessTier.json'
```

Pour créer une définition de stratégie avec une règle en ligne, utilisez l’exemple suivant :

```azurepowershell-interactive
$definition = New-AzPolicyDefinition -Name 'denyCoolTiering' -Description 'Deny cool access tiering for storage' -Policy '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

Le résultat est stocké dans un objet `$definition` utilisé lors de l’attribution de la stratégie. L’exemple suivant crée une définition de stratégie qui inclut des paramètres :

```azurepowershell-interactive
$policy = '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of locations that can be specified when deploying storage accounts.",
            "strongType": "location",
            "displayName": "Allowed locations"
        }
    }
}'

$definition = New-AzPolicyDefinition -Name 'storageLocations' -Description 'Policy to specify locations for storage accounts.' -Policy $policy -Parameter $parameters
```

### <a name="view-policy-definitions-with-powershell"></a>Voir les définitions de stratégie avec PowerShell

Pour afficher toutes les définitions de stratégie dans votre abonnement, utilisez la commande suivante :

```azurepowershell-interactive
Get-AzPolicyDefinition
```

Elle renvoie toutes les définitions de stratégie disponibles, y compris les stratégies intégrées. Chaque stratégie est renvoyée au format suivant :

```output
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

## <a name="create-a-policy-definition-with-azure-cli"></a>Créer une définition de stratégie avec Azure CLI

Vous pouvez créer une définition de stratégie à l’aide d’Azure CLI avec la commande `az policy definition`. Pour créer une définition de stratégie avec une règle en ligne, utilisez l’exemple suivant :

```azurecli-interactive
az policy definition create --name 'denyCoolTiering' --description 'Deny cool access tiering for storage' --rules '{
    "if": {
        "allOf": [{
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "field": "kind",
                "equals": "BlobStorage"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/accessTier",
                "equals": "cool"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}'
```

### <a name="view-policy-definitions-with-azure-cli"></a>Voir les définitions de stratégie avec Azure CLI

Pour afficher toutes les définitions de stratégie dans votre abonnement, utilisez la commande suivante :

```azurecli-interactive
az policy definition list
```

Elle renvoie toutes les définitions de stratégie disponibles, y compris les stratégies intégrées. Chaque stratégie est renvoyée au format suivant :

```json
{
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",
    "displayName": "Allowed locations",
    "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
    "policyRule": {
        "if": {
            "not": {
                "field": "location",
                "in": "[parameters('listOfAllowedLocations')]"
            }
        },
        "then": {
            "effect": "Deny"
        }
    },
    "policyType": "BuiltIn"
}
```

## <a name="create-and-assign-an-initiative-definition"></a>Créer et attribuer une définition d’initiative

Avec une définition d’initiative, vous pouvez regrouper plusieurs définitions de stratégie pour atteindre un objectif global. Une initiative vérifie si les ressources figurant dans l’étendue de l’attribution sont conformes aux stratégies incluses. Pour plus d’informations sur les définitions d’initiative, consultez [Vue d’ensemble d’Azure Policy](../overview.md).

### <a name="create-an-initiative-definition"></a>Créer une définition d’initiative

1. Sélectionnez **Définitions** sous **Création** dans la partie gauche de la page Azure Policy.

   :::image type="content" source="../media/create-and-manage/definition-under-authoring.png" alt-text="Sélectionner une définition dans la page Définitions" border="false":::

1. Sélectionnez **+ Définition d’initiative** en haut de la page pour ouvrir la page **Définition d’initiative**.

   :::image type="content" source="../media/create-and-manage/initiative-definition.png" alt-text="Passer en revue la page Définition d’initiative" border="false":::

1. Utilisez le bouton de sélection **Emplacement de définition** pour sélectionner un groupe d'administration ou un abonnement afin de stocker la définition. Si la page précédente se limite à un seul groupe d’administration ou à un seul abonnement, **Emplacement de la définition** est automatiquement renseigné. Après sélection, l’option **Définitions disponibles** est renseignée.

1. Entrez le **nom** et la **description** de l’initiative.

   Cet exemple vérifie que les ressources sont conformes aux définitions de stratégie relatives à la sécurisation. Nommez l’initiative **Garantir la sécurité** et définissez la description comme suit : **Cette initiative a été créée pour gérer toutes les définitions de stratégie associées à la sécurisation des ressources**.

1. Pour **Catégorie**, choisissez une des options existantes ou créez une catégorie.

1. Parcourez la liste des **définitions disponibles** (partie droite de la page **Définition d’initiative**) et sélectionnez les définitions de stratégie à ajouter à cette initiative. Pour l’initiative **Garantir la sécurité**, ajoutez les définitions de stratégie prédéfinies suivantes en sélectionnant **+** en regard des informations correspondantes ou en sélectionnant une ligne de définition de stratégie, puis l’option **+ Ajouter** dans la page de détails :

   - Emplacements autorisés
   - Superviser les agents Endpoint Protection manquants dans Azure Security Center
   - Les règles de groupe de sécurité réseau pour les machines virtuelles accessibles sur Internet doivent être renforcées
   - La sauvegarde Azure doit être activée pour les machines virtuelles
   - Le chiffrement de disque doit être appliqué sur les machines virtuelles

   Une fois que la définition de stratégie est sélectionnée dans la liste, elle est ajoutée sous **Catégorie**.

   :::image type="content" source="../media/create-and-manage/initiative-definition-2.png" alt-text="Passer en revue les paramètres de la définition d’initiative" border="false":::

1. Si une définition de stratégie ajoutée à l’initiative comporte des paramètres, ils apparaissent sous le nom de la stratégie dans la zone située sous la zone **Catégorie**. La _valeur_ peut être définie sur « Définir une valeur » (codée en dur pour toutes les affectations de cette initiative) ou « Utiliser le paramètre d'initiative » (définie au cours de chaque affectation d’initiative). Si « Définir une valeur » est sélectionné, la liste déroulante à droite de _Valeurs_ permet d’entrer ou de sélectionner les valeurs. Si l’option Utiliser le paramètre d’initiative est sélectionnée, une nouvelle section **Paramètres d’initiative** s’affiche, ce qui vous permet de définir le paramètre défini durant l’affectation d’initiative. Les valeurs autorisées sur ce paramètre d’initiative peuvent restreindre davantage ce qui peut être défini au cours de l’affectation d’initiative.

   :::image type="content" source="../media/create-and-manage/initiative-definition-3.png" alt-text="Modifier les paramètres de la définition d’initiative à partir des valeurs autorisées" border="false":::

   > [!NOTE]
   > Dans le cas de certains paramètres `strongType`, la liste de valeurs ne peut pas être déterminée automatiquement. Dans ce cas, un bouton de sélection s’affiche à droite de la ligne de paramètre. En le sélectionnant, vous ouvrez la page Étendue du paramètre (&lt;nom du paramètre&gt;). Sur cette page, sélectionnez l’abonnement à utiliser pour fournir les options de valeur. L’étendue de ce paramètre est utilisée uniquement lors de la création de la définition d’initiative et n’a aucun impact sur l’évaluation de la stratégie ou l’étendue de l’initiative lors de l’affectation.

   Définissez le paramètre « Emplacements autorisés » sur « USA Est 2 » et laissez les autres paramètres définis sur la valeur par défaut « AuditifNotExists ».

1. Sélectionnez **Enregistrer**.

#### <a name="create-a-policy-initiative-definition-with-azure-cli"></a>Créer une définition d’initiative de stratégie avec Azure CLI

Vous pouvez créer une définition d’initiative de stratégie à l’aide d’Azure CLI avec la commande `az policy set-definition`. Pour créer une définition d’initiative de stratégie avec une définition de stratégie existante, utilisez l’exemple suivant :

```azurecli-interactive
az policy set-definition create -n readOnlyStorage --definitions '[
        {
            "policyDefinitionId": "/subscriptions/mySubId/providers/Microsoft.Authorization/policyDefinitions/storagePolicy",
            "parameters": { "storageSku": { "value": "[parameters(\"requiredSku\")]" } }
        }
    ]' \
    --params '{ "requiredSku": { "type": "String" } }'
```

#### <a name="create-a-policy-initiative-definition-with-azure-powershell"></a>Créer une définition d’initiative de stratégie avec Azure PowerShell

Vous pouvez créer une définition d’initiative de stratégie à l’aide d’Azure PowerShell avec la cmdlet `New-AzPolicySetDefinition`. Pour créer une définition d’initiative de stratégie avec une définition de stratégie existante, utilisez le fichier de définition d’initiative de stratégie en tant que `VMPolicySet.json` :

```json
[
    {
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
        "parameters": {
            "tagName": {
                "value": "Business Unit"
            },
            "tagValue": {
                "value": "Finance"
            }
        }
    },
    {
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/464dbb85-3d5f-4a1d-bb09-95a9b5dd19cf"
    }
]
```

```azurepowershell-interactive
New-AzPolicySetDefinition -Name 'VMPolicySetDefinition' -Metadata '{"category":"Virtual Machine"}' -PolicyDefinition C:\VMPolicySet.json
```

### <a name="assign-an-initiative-definition"></a>Attribuer une définition d’initiative

1. Sélectionnez **Définitions** sous **Création** dans la partie gauche de la page Azure Policy.

1. Recherchez la définition d’initiative **Garantir la sécurité** que vous avez précédemment créée et sélectionnez-la. Sélectionnez **Affecter** en haut de la page pour ouvrir la page **Garantir la sécurité : affecter l’initiative**.

   :::image type="content" source="../media/create-and-manage/assign-definition.png" alt-text="Affecter une définition à partir de la page Définition d’initiative" border="false":::

   Vous pouvez également cliquer avec le bouton droit sur la ligne sélectionnée ou sélectionner les points de suspension situé en fin de ligne pour faire apparaître un menu contextuel. Sélectionnez ensuite **Affecter**.

   :::image type="content" source="../media/create-and-manage/select-right-click.png" alt-text="Autres options pour une initiative" border="false":::

1. Renseignez la page **Garantir la sécurité : affecter l’initiative** en entrant les exemples d’informations suivants. Vous pouvez utiliser vos propres informations.

   - Étendue : l’abonnement ou le groupe d’administration dans lequel vous avez enregistré l’initiative devient la valeur par défaut.
     Vous pouvez changer l’étendue pour affecter l’initiative à un abonnement ou un groupe de ressources, à l’emplacement d’enregistrement.
   - Exclusions : configurez des ressources dans l’étendue afin d’empêcher que l’affectation d’initiative leur soit appliquée.
   - Définition d’initiative et Nom de l’affectation : Garantir la sécurité (prérenseigné avec le nom de l’initiative assignée).
   - Description : cette affectation d’initiative est adaptée afin d’appliquer ce groupe de définitions de stratégie.
   - Application de la stratégie : Conservez la valeur par défaut _Activée_.
   - Affectée par : renseigné automatiquement en fonction de l’utilisateur connecté. Ce champ étant facultatif, vous pouvez entrer des valeurs personnalisées.

1. Sélectionnez l’onglet **Paramètres** en haut de l’Assistant. Si vous avez configuré un paramètre d’initiative dans les étapes précédentes, définissez une valeur ici.

1. Sélectionnez l’onglet **Correction** en haut de l’Assistant. Laissez la case **Créer une identité managée** non cochée. Cette case _doit_ être cochée quand la stratégie ou l’initiative affectée inclut une stratégie avec les effets [deployIfNotExists](../concepts/effects.md#deployifnotexists) ou [modifier](../concepts/effects.md#modify). Comme ce n’est pas le cas pour la stratégie utilisée dans ce tutoriel, ne cochez pas la case. Pour plus d’informations, consultez [Identités managées](../../../active-directory/managed-identities-azure-resources/overview.md) et [Fonctionnement de la sécurité par correction](../how-to/remediate-resources.md#how-remediation-security-works).

1. Sélectionnez l’onglet **Vérifier + créer** en haut de l’Assistant.

1. Passez en revue vos sélections, puis sélectionnez **Créer** au bas de la page.

## <a name="check-initial-compliance"></a>Vérifier la conformité initiale

1. Sélectionnez **Conformité** dans la partie gauche de la page Azure Policy.

1. Localisez l’initiative **Garantir la sécurité**. Elle est probablement toujours à l’_État de conformité_**Non démarrée**.
   Sélectionnez l’initiative pour obtenir tous les détails sur la progression de l’affectation.

   :::image type="content" source="../media/create-and-manage/compliance-status-not-started.png" alt-text="Page de conformité de l’initiative - évaluations non démarrées" border="false":::

1. Une fois l’affectation de l’initiative effectuée, la page de conformité est mise à jour avec l’_État de conformité_**Conforme**.

   :::image type="content" source="../media/create-and-manage/compliance-status-compliant.png" alt-text="Page de conformité de l’initiative - ressources conformes" border="false":::

1. Pour ouvrir la page de détails sur la conformité de la stratégie, sélectionnez une stratégie dans la page de conformité de l’initiative. Cette page fournit des détails au niveau des ressources pour la conformité.

## <a name="exempt-a-non-compliant-or-denied-resource-using-exclusion"></a>Exempter une ressource non conforme ou refusée en utilisant l’exclusion

Après avoir attribué une initiative de stratégie pour exiger un emplacement spécifique, toutes les ressources créées dans un emplacement différent sont refusées. Dans cette section, vous allez résoudre pas à pas une situation dans laquelle la demande de création d’une ressource a été refusée en créant une exclusion sur un seul groupe de ressources. L’exclusion empêche l’application de la stratégie (ou de l’initiative) à ce groupe de ressources. Dans l’exemple suivant, tous les emplacements sont autorisés dans le groupe de ressources exclu. Une exclusion peut s’appliquer à un abonnement, à un groupe de ressources ou à des ressources individuelles.

Les déploiements empêchés par une stratégie ou une initiative affectée peuvent être vus dans le groupe de ressources ciblé par le déploiement : Sélectionnez **Déploiements** dans la partie gauche de la page, puis sélectionnez le **nom du déploiement** qui a échoué. La ressource refusée est listée avec l’état _Interdit_. Pour déterminer la stratégie ou l’initiative et l’affectation qui a refusé la ressource, sélectionnez **Échec. Cliquez ici pour plus d’informations ->** dans la page Vue d’ensemble du déploiement. Une fenêtre s’ouvre dans la partie droite de la page pour présenter les informations d’erreur. Sous **Détails de l’erreur** figurent les GUID des objets de stratégie associés.

:::image type="content" source="../media/create-and-manage/rg-deployment-denied.png" alt-text="Déploiement refusé par l’affectation de stratégie" border="false":::

Dans la page Azure Policy : Sélectionnez **Conformité** du côté gauche de la page et sélectionnez l’initiative de stratégie **Garantir la sécurité**. Dans cette page figure une augmentation du nombre de **refus** de ressources bloquées. Sous l’onglet **Événements** se trouvent des détails sur les personnes qui ont essayé de créer ou de déployer la ressource refusée par la définition de stratégie.

:::image type="content" source="../media/create-and-manage/compliance-overview.png" alt-text="Vue d’ensemble de la conformité d’une stratégie affectée" border="false":::

Dans cet exemple, Trent Baker, l’un des spécialistes de la virtualisation chez Contoso, effectuait des tâches requises. Nous avons besoin d’accorder à Trent un espace pour une exception. Créez un groupe de ressources, **LocationsExcluded**, puis octroyez-lui une exception à cette affectation de stratégie.

### <a name="update-assignment-with-exclusion"></a>Mettre à jour une affectation avec une exclusion

1. Sélectionnez **Affectations** sous **Création** dans la partie gauche de la page Azure Policy.

1. Parcourez toutes les affectations de stratégie et ouvrez _Garantir la sécurité_.

1. Définissez l’**exclusion** en sélectionnant les points de suspension, puis en sélectionnant le groupe de ressources à exclure, _LocationsExcluded_ dans cet exemple. Sélectionnez **Ajouter à l’étendue sélectionnée**, puis **Enregistrer**.

   :::image type="content" source="../media/create-and-manage/request-exclusion.png" alt-text="Ajouter un groupe de ressources exclues à l’affectation de stratégie" border="false":::

   > [!NOTE]
   > En fonction de la définition de stratégie et de son effet, l’exclusion peut également être octroyée à des ressources spécifiques au sein du groupe de ressources dans l’étendue de l’affectation. Comme un effet **Refuser** a été utilisé dans ce tutoriel, il ne serait pas judicieux de définir l’exclusion sur une ressource spécifique qui existe déjà.

1. Sélectionnez **Réviser + enregistrer**, puis **Enregistrer**.

Dans cette section, vous avez résolu la demande refusée en créant une exclusion sur un seul groupe de ressources.

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous avez fini d’utiliser les ressources de ce tutoriel, effectuez les étapes suivantes pour supprimer les affectations ou définitions de stratégie créées ci-dessus :

1. Sélectionnez **Définitions** (ou **Affectations** si vous essayez de supprimer une affectation) sous **Création** dans la partie gauche de la page Azure Policy.

1. Recherchez la nouvelle définition d’initiative ou de stratégie (ou affectation) à supprimer.

1. Cliquez avec le bouton droit sur la liste ou cliquez sur le bouton de sélection en fin de définition (ou d’affectation), puis sélectionnez **Supprimer la définition** (ou **Supprimer l’affectation**).

## <a name="review"></a>Révision

Dans ce didacticiel, vous avez effectué avec succès les tâches suivantes :

> [!div class="checklist"]
> - Attribué une stratégie pour appliquer une condition aux ressources que vous créez dans le futur
> - Créé et attribué une définition d’initiative pour effectuer le suivi de la conformité de plusieurs ressources
> - Résolu une ressource non conforme ou refusée
> - Implémenté une nouvelle stratégie dans l’ensemble de l’entreprise

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les structures des définitions de stratégie, consultez cet article :

> [!div class="nextstepaction"]
> [Structure de définition Azure Policy](../concepts/definition-structure.md)