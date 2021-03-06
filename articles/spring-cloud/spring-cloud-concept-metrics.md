---
title: Présentation des métriques pour Azure Spring Cloud
description: Découvrir comment passer en revue les indicateurs de performance dans Azure Spring Cloud
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: brendm
ms.openlocfilehash: bb23afff2b4b449897d8e420934d038938d20205
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228785"
---
# <a name="understand-metrics-for-azure-spring-cloud"></a>Comprendre les indicateurs de performance pour Azure Spring Cloud

Azure Metrics Explorer est un composant du portail Microsoft Azure qui permet de tracer des graphiques, de corréler visuellement des tendances et d’examiner les pics et les creux des indicateurs de performance. Utilisez l’explorateur de métriques pour examiner l’intégrité et l’utilisation de vos ressources. 

Dans Azure Spring Cloud, il existe deux points de vue pour les indicateurs de performance.
* Graphiques dans la page de présentation de chaque application
* Page des indicateurs de performance courants

 ![Graphiques d’indicateurs de performance](media/metrics/metrics-1.png)

Les graphiques dans la **Vue d’ensemble** de l’application permettent des vérifications d’état rapides pour chaque application. La page des **indicateurs de performance** courants contient tous les indicateurs de performance disponibles à des fins de référence. Vous pouvez créer vos propres graphiques dans la page des indicateurs de performance courants et les épingler au tableau de bord.

## <a name="application-overview-page"></a>Page de vue d’ensemble de l’application
Sélectionnez une application dans **Gestion des applications** pour rechercher des graphiques dans la page de présentation.  

 ![Gestion des indicateurs de performance d’application](media/metrics/metrics-2.png)

La page **Vue d’ensemble de l’application** de chaque application, présente un graphique des métriques qui vous permet d’effectuer une vérification rapide de l’état de votre application.  

 ![Vue d’ensemble des indicateurs de performance d’application](media/metrics/metrics-3.png)

Azure Spring Cloud fournit les cinq graphiques suivants, dont les indicateurs de performance sont mis à jour toutes les minutes :

* **Erreurs de serveur Http** : Nombre d’erreurs pour les requêtes HTTP adressées à votre application
* **Données entrantes** : Octets reçus par votre application
* **Données sortantes** : Octets envoyés par votre application
* **Requêtes** : Requêtes reçues par votre application
* **Temps de réponse moyen** : Temps de réponse Moyen de votre application

Pour le graphique, vous pouvez sélectionner un intervalle de temps d’une heure à sept jours.

## <a name="common-metrics-page"></a>Page des indicateurs de performance courants

Les **indicateurs de performance** dans le volet de navigation gauche sont liés à la page des indicateurs de performance courants.

Sélectionnez tout d’abord les indicateurs de performance à afficher :

![Sélectionner l’affichage des indicateurs de performance](media/metrics/metrics-4.png)

Vous trouverez les détails de toutes les options des indicateurs de performance dans la [section](#user-metrics-options) ci-dessous.

Sélectionnez ensuite le type d’agrégation de chaque indicateur de performance :

![Agrégation d’indicateurs de performance](media/metrics/metrics-5.png)

Le type d'agrégation indique comment agréger des points d’indicateurs de performance dans le graphique en fonction du temps. Il existe un point d’indicateur de performance brut toutes les minutes, et le type de préagrégation dans une minute est prédéfini par un type d’indicateur de performance.
* Somme : Additionne toutes les valeurs comme résultat cible.
* Moyenne : Utilise la valeur moyenne de la période comme résultat cible.
* Max/Min : Utilise la valeur maximale/minimale de la période comme résultat cible.

L’intervalle de temps à afficher peut également être Modifié. L’intervalle de temps peut être choisi entre les 30 dernières minutes et les 30 derniers jours, ou un intervalle de temps personnalisé.

![Modification d’indicateur de performance](media/metrics/metrics-6.png)

Par défaut, la vue regroupe tous les indicateurs de performance de l’application du service Azure Spring Cloud. Les indicateurs de performance d’une application ou d’une instance peuvent être filtrés dans l’affichage.  Cliquez sur **Ajouter un filtre**, définissez la propriété sur **Application**, puis sélectionnez l’application cible que vous voulez superviser dans la zone de texte **Valeurs**. 

Vous pouvez utiliser deux types de filtres (propriétés) :
* Application : filtrer par nom d’application
* Instance : filtrer par instance d’application

![Filtres d’indicateurs de performance](media/metrics/metrics-7.png)

Vous pouvez également utiliser l’option **Appliquer la division**, qui permet de créer plusieurs lignes pour une application :

![Division d’indicateurs de performance](media/metrics/metrics-8.png)

>[!TIP]
> Vous pouvez créer vos propres graphiques dans la page des métriques et les épingler à votre **tableau de bord**. Commencez par nommer votre graphique.  Ensuite, sélectionnez **Épingler au tableau de bord dans le coin supérieur droit**. Vous pouvez maintenant vérifier votre application sur votre **tableau de bord** du portail.

## <a name="user-metrics-options"></a>Options d’indicateur de performance de l’utilisateur

Les tableaux suivants affichent les indicateurs de performance disponibles et leurs détails.

### <a name="error"></a>Error
>[!div class="mx-tdCol2BreakAll"]
>| Nom | Nom de l’indicateur de performance Spring Actuator | Unité | Détails |
>|----|----|----|------------|
>| Erreur globale Tomcat | tomcat.global.error | Count | Nombre d’erreurs de requêtes traitées |

### <a name="performance"></a>Performances
>[!div class="mx-tdCol2BreakAll"]
>| Nom | Nom de l’indicateur de performance Spring Actuator | Unité | Détails |
>|----|----|----|------------|
>|Pourcentage d’utilisation du processeur système | system.cpu.usage | Pourcentage | Utilisation récente de l’UC pour l’ensemble du système. Cette valeur est double dans l’intervalle [0.0,1.0]. La valeur 0.0 indique que toutes les UC étaient inactives pendant la dernière période observée, alors que la valeur 1.0 indique que toutes les UC ont été actives 100 % du temps pendant la dernière période observée.|
>| Pourcentage d’utilisation du processeur d’application | Pourcentage d’utilisation du processeur d’application | Pourcentage | Utilisation récente de l’UC pour le processus Machine virtuelle Java. Cette valeur est double dans l’intervalle [0.0,1.0]. La valeur 0.0 indique qu’aucune des UC n’exécutait de threads du processus JVM pendant la dernière période observée, alors que la valeur 1.0 indique que toutes les UC ont exécuté des threads du processus JVM 100 % du temps pendant la dernière période observée. Les threads de JVM incluent les threads d’application, ainsi que les threads internes JVM.|
>| Mémoire d’application affectée | jvm.memory.committed | Octets | Représente la quantité de mémoire dont la disponibilité est garantie pour une utilisation par la machine virtuelle Java. La machine virtuelle Java peut libérer de la mémoire sur le système et l’allocation peut être inférieure à la quantité initialement. validée sera toujours supérieure ou égale à la quantité utilisée. |
>| Mémoire d’application utilisée | jvm.memory.used | Octets | Représente la quantité de mémoire actuellement utilisée, en octets. |
>| Mémoire d’application maximale | jvm.memory.max | Octets | Représente la quantité maximale de mémoire utilisable pour la gestion de la mémoire. La quantité de mémoire utilisée et validée sera toujours inférieure ou égale à Max si la quantité maximale est définie. Une allocation de mémoire peut échouer si elle tente d’augmenter la mémoire utilisée de telle sorte que used > committed même si used <= max serait toujours vrai (par exemple, lorsque la mémoire virtuelle du système est insuffisante). |
>| Taille maximale des données d’ancienne génération disponibles | jvm.gc.max.data.size | Octets | Pic d’utilisation de la mémoire du pool de mémoire d’ancienne génération depuis le démarrage de la machine virtuelle Java. |
>| Taille des données d’ancienne génération | jvm.gc.live.data.size | Octets | Taille du pool de mémoire d’ancienne génération après un GC complet. |
>| Promouvoir à la taille des données d’ancienne génération | jvm.gc.memory.promoted | Octets | Nombre d’augmentations positives de la taille du pool de mémoire d’ancienne génération avant l’application du GC jusqu’au terme de cette application. |
>| Promouvoir à la taille des données de nouvelle génération | jvm.gc.memory.allocated | Octets | Incrémenté pour une augmentation de la taille du pool de mémoire de nouvelle génération après un GC avant le suivant. |
>| Nombre d’interruptions du GC | jvm.gc.pause (nombre total) | Count | Nombre total de GC après le démarrage de cette JMV, y compris les GC de nouvelle et d’ancienne génération. |
>| Durée totale de pause du GC | jvm.gc.pause (durée totale) | Millisecondes | Durée totale du GC utilisée après le démarrage de cette JMV, y compris les GC de nouvelle et d’ancienne génération. |

### <a name="request"></a>Requête
>[!div class="mx-tdCol2BreakAll"]
>| Nom | Nom de l’indicateur de performance Spring Actuator | Unité | Détails |
>|----|----|----|------------|
>| Nombre total d’octets envoyés par Tomcat | tomcat.global.sent | Octets | Quantité de données envoyées par le serveur web Tomcat |
>| Nombre total d’octets reçus par Tomcat | tomcat.global.received | Octets | Quantité de données reçues par le serveur web Tomcat |
>| Durée totale des requêtes Tomcat | tomcat.global.request (durée totale) | Millisecondes | Durée totale de traitement des requêtes par le serveur web Tomcat |
>| Nombre total de demandes Tomcat | tomcat.global.request (nombre total) | Count | Nombre total de requêtes traitées par le serveur web Tomcat |
>| Durée maximale des demandes Tomcat | tomcat.global.request.max | Millisecondes | Durée maximale de traitement d’une requête par le serveur web Tomcat |

### <a name="session"></a>session
>[!div class="mx-tdCol2BreakAll"]
>| Nom | Nom de l’indicateur de performance Spring Actuator | Unité | Détails |
>|----|----|----|------------|
>| Nombre maximal de sessions actives Tomcat | tomcat.sessions.active.max | Count | Nombre maximal de sessions actives simultanément |
>| Durée maximale des sessions actives Tomcat | tomcat.sessions.alive.max | Millisecondes | Durée maximale (en secondes) pendant laquelle une session ayant expiré a été active |
>| Nombre de sessions Tomcat créées | tomcat.sessions.created | Count | Nombre de sessions créées |
>| Nombre de sessions Tomcat ayant expiré | tomcat.sessions.expired | Count | Nombre de sessions qui ont expiré |
>| Nombre de sessions Tomcat rejetées | tomcat.sessions.rejected | Count | Nombre de sessions qui n’ont pas été créées parce que le nombre maximal de sessions actives a été atteint. |

## <a name="see-also"></a>Voir aussi
* [Bien démarrer avec Azure Metrics Explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started)

* [Analyser les journaux et les indicateurs de performance avec les paramètres de diagnostic](https://docs.microsoft.com/azure/spring-cloud/diagnostic-services)

## <a name="next-steps"></a>Étapes suivantes
* [Tutoriel : Superviser les ressources Spring Cloud avec des alertes et des groupes d’actions](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)

* [Quotas et plans de service pour Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quotas)

