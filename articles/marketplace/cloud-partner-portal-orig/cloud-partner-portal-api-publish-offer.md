---
title: Publier une offre | Place de marché Microsoft Azure
description: API permettant de publier l’offre spécifiée.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 960d5facb53f20719045c5fdbe2179f549aca3f2
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255938"
---
# <a name="publish-an-offer"></a>Publication d’une offre

> [!NOTE]
> Les API du portail Cloud Partner sont intégrées à l’Espace partenaires et continueront à fonctionner après la migration de vos offres vers l’Espace partenaires. L’intégration apporte de légères modifications. Passez en revue les changements listés dans la [référence API du portail Cloud Partner](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) pour vous assurer que votre code continue à fonctionner après la migration vers l’Espace partenaires.

Démarre le processus de publication pour l’offre spécifiée. Cet appel est une opération de longue durée.

  `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/publish?api-version=2017-10-31`

## <a name="uri-parameters"></a>Paramètres URI
--------------

|  **Nom**      |    **Description**                               |  **Type de données** |
|  ------------- |  ------------------------------------            |   -----------  |
|  publisherId   | Identificateur du serveur de publication, par exemple `contoso`      |   String       |
|  offerId       | Identificateur de l’offre                                 |   String       |
|  api-version   | Version la plus récente de l’API                        |   Date         |
|  |  |

## <a name="header"></a>En-tête
------

|  **Nom**        |    **Valeur**          |
|  --------        |    ---------          |
|  Content-Type    | `application/json`    |
|  Autorisation   |  `Bearer YOUR_TOKEN`  |
|  |  |


## <a name="body-example"></a>Exemple de corps
------------

### <a name="request"></a>Requête

``` json
  { 
      'metadata': 
          { 
              'notification-emails': 'jdoe@contoso.com'
          } 
  }
```

### <a name="request-body-properties"></a>Propriétés du corps de la requête

|  **Nom**               |   **Description**                                                                                 |
|  ---------------------  | ------------------------------------------------------------------------------------------------- |
|  e-mails de notification    | Liste d’adresses e-mail séparées par des virgules pour informer de la progression de l’opération de publication. |
|  |  |


### <a name="response"></a>response

#### <a name="migrated-offers"></a>Offres migrées

`Location: /api/publishers/contoso/offers/contoso-offer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8?api-version=2017-10-31`

#### <a name="non-migrated-offers"></a>Offres non migrées

`Location: /api/operations/contoso$contoso-offer$2$preview?api-version=2017-10-31`


### <a name="response-header"></a>En-tête de réponse

|  **Nom**             |    **Valeur**                                                                 |
|  -------------------- | ---------------------------------------------------------------------------- |
| Emplacement    | Chemin relatif permettant de récupérer l’état de cette opération     |
|  |  |


### <a name="response-status-codes"></a>Codes d’état de réponse

| **Code** |  **Description**                                                                                                                           |
| ------   |  ----------------------------------------------------------------------------------------------------------------------------------------- |
| 202   | `Accepted` : la requête a été acceptée. La réponse contient un emplacement qui peut être utilisé pour suivre l’opération lancée. |
| 400   | `Bad/Malformed request` : le corps de la réponse d’erreur peut fournir plus d’informations.                                                               |
| 422   | `Un-processable entity` : indique que la validation de l’entité à publier a échoué.                                                        |
| 404   | `Not found` : l’entité spécifiée par le client n’existe pas.                                                                              |
|  |  |
