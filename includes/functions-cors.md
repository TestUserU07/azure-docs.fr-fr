---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: a5f829fc0e153c3a427fd1c7cc7d5c613cd624f6
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648247"
---
Azure Functions prend en charge le partage des ressources cross-origin (CORS). CORS est configuré [dans le portail](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors) et par le biais d’[Azure CLI](/cli/azure/functionapp/cors). La liste des origines autorisées CORS s’applique au niveau de l’application de fonction. Quand CORS est activé, les réponses incluent l’en-tête `Access-Control-Allow-Origin`. Pour plus d'informations, consultez la page [Partage des ressources cross-origin](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors). 