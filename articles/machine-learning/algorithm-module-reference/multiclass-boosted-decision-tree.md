---
title: 'Arbre de décision optimisé à plusieurs classes : Informations de référence sur les modules'
titleSuffix: Azure Machine Learning
description: Découvrez comment utiliser le module de l’arbre de décision optimisé multiclasse dans Azure Machine Learning pour créer un classifieur à l’aide de données étiquetées.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/19/2020
ms.openlocfilehash: cfe35f81526a729092edf522f693ccd18494d1ec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82137822"
---
# <a name="multiclass-boosted-decision-tree"></a>Arbre de décision optimisé à plusieurs classes

Cet article décrit un module dans le concepteur Azure Machine Learning (version préliminaire).

Ce module vous permet de créer un modèle Machine Learning reposant sur l’algorithme d’arbres de décision optimisés.

Un arbre de décision optimisé est une méthode d’apprentissage d’ensemble dans laquelle le second arbre corrige les erreurs du premier arbre, le troisième arbre corrige les erreurs du premier et du deuxième arbres, et ainsi de suite. Les prédictions sont basées sur l’ensemble des arborescences.

## <a name="how-to-configure"></a>Comment configurer 

Ce module crée un modèle de classification non entraîné. Étant donné que la classification est une méthode d’apprentissage supervisée, vous avez besoin d’un *jeu de données étiqueté* incluant une colonne étiquette avec une valeur pour toutes les lignes.

Vous pouvez effectuer l’apprentissage de ce type de modèle à l’aide de l’article [Entraîner le modèle](././train-model.md). 

1.  Ajoutez le module **Arbre de décision optimisé multiclasse** à votre pipeline.

1.  Spécifiez le mode d’apprentissage du modèle en définissant l’option **Créer un mode d’apprentissage**.

    + **Single Parameter** (Paramètre unique) : si vous savez comment vous voulez configurer le modèle, vous pouvez fournir un ensemble spécifique de valeurs comme arguments.
    
    + **Plage de paramètres** : Sélectionnez cette option si vous n’êtes pas sûr des paramètres à choisir et que vous souhaitez exécuter un balayage des paramètres. Sélectionnez la plage de valeurs sur laquelle vous souhaitez effectuer l'itération. Le module [Optimiser les hyperparamètres du modèle](tune-model-hyperparameters.md) effectue alors une itération sur toutes les combinaisons possibles des paramètres que vous avez fournis pour déterminer les hyperparamètres qui produisent les résultats optimaux.  

1. **Maximum number of leaves per tree** (Nombre maximal de nœuds terminaux par arbre) limite le nombre maximal de nœuds terminaux qui peuvent être créés dans un arbre quelconque.
    
        By increasing this value, you potentially increase the size of the tree and achieve higher precision, at the risk of overfitting and longer training time.
  
1. **Minimum number of samples per leaf node** (Nombre minimal d’échantillons par nœud terminal), indique le nombre de cas requis pour la création d’un nœud terminal dans un arbre.  

         By increasing this value, you increase the threshold for creating new rules. For example, with the default value of 1, even a single case can cause a new rule to be created. If you increase the value to 5, the training data would have to contain at least five cases that meet the same conditions.

1. **Learning rate** (taux d’apprentissage) définit la taille de l’étape pendant l’apprentissage. Entrez un nombre compris entre 0 et 1.

         The learning rate determines how fast or slow the learner converges on an optimal solution. If the step size is too large, you might overshoot the optimal solution. If the step size is too small, training takes longer to converge on the best solution.

1. **Number of trees constructed** (Nombre d’arbres construits) indique le nombre total d’arbres de décision à créer dans l’ensemble. En créant plusieurs arbres de décision, vous pouvez obtenir une meilleure couverture, mais cette opération augmente la durée d’apprentissage.

1. **Random number seed** (Valeur de départ aléatoire) définit un entier non négatif à utiliser comme valeur initiale aléatoire. La spécification d’une valeur de départ garantit la reproductibilité entre les exécutions qui présentent les mêmes données et paramètres.  

         The random seed is set by default to 42. Successive runs using different random seeds can have different results.

1. Effectuez l’apprentissage du modèle :

    + Si vous définissez **Create trainer mode** (Créer un mode d’apprentissage) sur **Single Parameter** (Paramètre unique), connectez un jeu de données balisé au module [Entraîner le modèle](train-model.md).  
  
    + Si vous définissez le mode **Créer un formateur** sur **Plage de paramètre**, connectez un jeu de données balisé et formez le modèle à l’aide d’[Optimiser des hyperparamètres de modèles](tune-model-hyperparameters.md).  
  
    > [!NOTE]
    > 
    > Si vous transmettez une plage de paramètres à [Effectuer l'apprentissage du modèle](train-model.md), elle utilise uniquement la valeur par défaut dans la liste de paramètres unique.  
    > 
    > Si vous transmettez un ensemble unique de valeurs de paramètre au module [Optimiser des hyperparamètres de modèles](tune-model-hyperparameters.md), quand il attend une plage de paramètres de configuration pour chaque paramètre, il ignore les valeurs et utilise les valeurs par défaut pour l’apprenant.  
    > 
    > Si vous sélectionnez l’option **Plage de paramètres** et que vous entrez une valeur unique pour un paramètre, cette valeur unique que vous avez spécifiée est utilisée tout au long du balayage, même si d’autres paramètres changent sur une plage de valeurs.

## <a name="next-steps"></a>Étapes suivantes

Consultez [l’ensemble des modules disponibles](module-reference.md) pour Azure Machine Learning. 
