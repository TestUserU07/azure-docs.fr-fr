---
title: Résolution des erreurs internes 500xxx d’Azure IoT Hub
description: Comprendre comment corriger les erreurs internes 500xxx
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 7f3f5177e084693c45bed1088a4e1d091be100ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233181"
---
# <a name="500xxx-internal-errors"></a>500xxx Erreurs internes

Cet article décrit les causes et solutions des **erreurs internes 500xxx**.

## <a name="symptoms"></a>Symptômes

Votre demande adressée à IoT Hub échoue en générant une erreur commençant par 500 et/ou une sorte d’« erreur de serveur ». Voici quelques possibilités :

* **500001 ServerError** : IoT Hub a rencontré un problème côté serveur.

* **500008 GenericTimeout** : IoT Hub n’est pas parvenu à effectuer la demande de connexion avant l’expiration du délai de connexion.

* **ServiceUnavailable (aucun code d’erreur)**  : IoT Hub a rencontré une erreur interne.

* **InternalServerError (aucun code d’erreur)**  : IoT Hub a rencontré une erreur interne.

## <a name="cause"></a>Cause

Une réponse d’erreur 500xxx peut avoir plusieurs causes. Dans tous les cas, le problème est très probablement temporaire. Bien que l’équipe IoT Hub s’efforce continuellement de respecter les termes du [Contrat de niveau de service (SLA)](https://azure.microsoft.com/support/legal/sla/iot-hub/), de petits sous-ensembles de nœuds IoT Hub rencontrent parfois des erreurs temporaires. Lorsque votre appareil tente de se connecter à un nœud présentant un problème, vous recevez cette erreur.

## <a name="solution"></a>Solution

Pour atténuer les erreurs 500xxx, effectuez une nouvelle tentative à partir de l’appareil. Pour [gérer automatiquement les nouvelles tentatives](./iot-hub-reliability-features-in-sdks.md#connection-and-retry), vérifiez que vous utilisez la dernière version des [Kits de développement logiciel (SDK) Azure IoT](./iot-hub-devguide-sdks.md). Pour connaître les meilleures pratiques en matière de gestion des erreurs temporaires et de nouvelles tentatives, consultez l’article [Gestion des erreurs temporaires](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults).  Si le problème persiste, voir le service [Resource Health](./iot-hub-monitor-resource-health.md#use-azure-resource-health) et la page des [états d’Azure](https://status.azure.com/) pour déterminer si IoT Hub rencontre un problème connu. Vous pouvez également utiliser la [fonctionnalité de basculement manuel](./tutorial-manual-failover.md). S’il n’existe aucun problème connu et que le problème persiste, [contactez le support](https://azure.microsoft.com/support/options/) pour un examen approfondi.
