---
title: Obtenir l’intention avec un appel REST dans Node.js
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/20/2020
ms.author: diberry
ms.openlocfilehash: 3bef3cbb321465893b3a05242f0d72d77d091d6b
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733310"
---
## <a name="prerequisites"></a>Prérequis

* Langage de programmation [Node.js](https://nodejs.org/)
* [Visual Studio Code](https://code.visualstudio.com/)
* Un ID d’application LUIS : utilisez l’ID d’application IoT publique `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. La requête utilisateur utilisée dans le code du guide de démarrage rapide est propre à cette application.

## <a name="create-luis-runtime-key-for-predictions"></a>Créer une clé Runtime LUIS pour les prédictions

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Cliquez sur [Créer**Language Understanding**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne)
1. Entrez tous les paramètres obligatoires pour la clé **Runtime** :

    |Paramètre|Valeur|
    |--|--|
    |Nom|Nom de votre choix (2-64 caractères)|
    |Abonnement|Sélectionner l’abonnement approprié|
    |Emplacement|Sélectionnez n’importe quel emplacement disponible et proche|
    |Niveau de tarification|`F0` - le niveau tarifaire minimal|
    |Groupe de ressources|Sélectionner un groupe de ressources disponible|

1. Cliquez sur **Créer** et attendez que la ressource soit créée. Après sa création, accédez à la page de ressources.
1. Collectez la valeur `endpoint` configurée et une valeur `key`.

## <a name="get-intent-programmatically"></a>Reconnaître une intention par programmation

Utilisez Node.js pour interroger le [point de terminaison de prédiction](https://aka.ms/luis-apim-v3-prediction) afin d’obtenir le résultat de la prédiction.

1. Copiez l’extrait de code suivant dans le fichier nommé `predict.js` :

    ```javascript
    var request = require('request');
    var requestpromise = require('request-promise');
    var querystring = require('querystring');

    // Analyze text
    //
    getPrediction = async () => {

        // YOUR-KEY - Language Understanding runtime key
        var endpointKey = "YOUR-KEY";

        // YOUR-ENDPOINT Language Understanding endpoint URL, an example is your-resource-name.api.cognitive.microsoft.com
        var endpoint = "YOUR-ENDPOINT";

        // Set the LUIS_APP_ID environment variable
        // to df67dcdb-c37d-46af-88e1-8b97951ca1c2, which is the ID
        // of a public sample application.
        var appId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";

        var utterance = "turn on all lights";

        // Create query string
        var queryParams = {
            "show-all-intents": true,
            "verbose":  true,
            "query": utterance,
            "subscription-key": endpointKey
        }

        // append query string to endpoint URL
        var URI = `https://${endpoint}/luis/prediction/v3.0/apps/${appId}/slots/production/predict?${querystring.stringify(queryParams)}`

        // HTTP Request
        const response = await requestpromise(URI);

        // HTTP Response
        console.log(response);

    }

    // Pass an utterance to the sample LUIS app
    getPrediction().then(()=>console.log("done")).catch((err)=>console.log(err));
    ```

1. Remplacez les valeurs `YOUR-KEY` et `YOUR-ENDPOINT` par votre clé **Runtime** et votre point de terminaison de prédiction.

    |Information|Objectif|
    |--|--|
    |`YOUR-KEY`|Votre clé **Runtime** de prédiction (32 caractères).|
    |`YOUR-ENDPOINT`| L’URL de votre point de terminaison de prédiction. Par exemple : `replace-with-your-resource-name.api.cognitive.microsoft.com`.|

1. Installez les dépendances `request`, `request-promise`et `querystring` avec cette commande :

    ```console
    npm install request request-promise querystring
    ```

1. Exécutez votre application avec la commande suivante :

    ```console
    node predict.js
    ```

 1. Examinez la réponse de prédiction, qui est retournée au format JSON :

    ```console
    {"query":"turn on all lights","prediction":{"topIntent":"HomeAutomation.TurnOn","intents":{"HomeAutomation.TurnOn":{"score":0.5375382},"None":{"score":0.08687421},"HomeAutomation.TurnOff":{"score":0.0207554}},"entities":{"HomeAutomation.Operation":["on"],"$instance":{"HomeAutomation.Operation":[{"type":"HomeAutomation.Operation","text":"on","startIndex":5,"length":2,"score":0.724984169,"modelTypeId":-1,"modelType":"Unknown","recognitionSources":["model"]}]}}}}
    ```

    La réponse JSON mise en forme pour des raisons de lisibilité :

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

## <a name="clean-up-resources"></a>Nettoyer les ressources

Lorsque vous aurez fini de suivre ce guide de démarrage rapide, supprimez le fichier du système de fichiers.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Ajouter des énoncés et effectuer l’apprentissage de](../get-started-get-model-rest-apis.md)