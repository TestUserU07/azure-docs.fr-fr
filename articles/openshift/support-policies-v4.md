---
title: Stratégie de prise en charge des clusters Azure Red Hat OpenShift 4
description: Comprendre les exigences de la stratégie de prise en charge pour Red Hat OpenShift 4.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 7bdcccee3270f9d2b611682a9a59505158a494d2
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204862"
---
# <a name="azure-red-hat-openshift-support-policy"></a>Stratégie de prise en charge d’Azure Red Hat OpenShift

Certaines configurations pour les clusters Azure Red Hat OpenShift 4 peuvent affecter la prise en charge de votre cluster. Azure Red Hat OpenShift 4 permet aux administrateurs de clusters d’apporter des modifications aux composants de cluster internes, mais les modifications ne sont pas toutes prises en charge. La stratégie de prise en charge ci-dessous partage les modifications qui enfreignent la stratégie et invalident la prise en charge de Microsoft et Red Hat.

> [!NOTE]
> Les fonctionnalités marquées Technology Preview dans OpenShift Container Platform ne sont pas prises en charge dans Azure Red Hat OpenShift.

## <a name="cluster-configuration-requirements"></a>Exigences de configuration de cluster

* Tous les opérateurs de cluster OpenShift doivent rester dans un état managé. La liste des opérateurs de cluster peut être retournée en exécutant `oc get clusteroperators`.
* Ne supprimez pas ou ne modifiez pas les services Prometheus et AlertManager du cluster.
* Ne supprimez pas les règles AlertManager du service.
* Ne modifiez pas la version de cluster OpenShift.
* Ne supprimez pas ou ne modifiez pas la journalisation du service Azure Red Hat OpenShift (pods MDSD).
* Ne supprimez pas ou ne modifiez pas le secret d’extraction de cluster « arosvc.azurecr.io ».
* Toutes les machines virtuelles du cluster doivent disposer d’un accès Internet sortant, du moins aux points de terminaison Azure Resource Manager (ARM), et de la journalisation des services (Geneva).
* Le service Azure Red Hat OpenShift accède à votre cluster par le biais du service de liaison privée.  Ne supprimez pas et ne modifiez pas l’accès au service.
* Les nœuds de calcul non RHCOS ne sont pas pris en charge. Par exemple, vous ne pouvez pas utiliser un nœud de calcul RHEL.

## <a name="supported-virtual-machine-sizes"></a>Tailles de machine virtuelle prises en charge

Azure Red Hat OpenShift 4 prend en charge les instances de nœuds de travail sur les tailles de machine virtuelle suivantes :

### <a name="general-purpose"></a>Usage général

|Série|Taille|Processeurs virtuels|Mémoire : Gio|
|-|-|-|-|
|Dasv4|Standard_D4as_v4|4|16|
|Dasv4|Standard_D8as_v4|8|32|
|Dasv4|Standard_D16as_v4|16|64|
|Dasv4|Standard_D32as_v4|32|128|
|Dsv3|Standard_D4s_v3|4|16|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|

### <a name="memory-optimized"></a>Mémoire optimisée

|Série|Taille|Processeurs virtuels|Mémoire : Gio|
|-|-|-|-|
|Esv3|Standard_E4s_v3|4|32|
|Esv3|Standard_E8s_v3|8|64|
|Esv3|Standard_E16s_v3|16|128|
|Esv3|Standard_E32s_v3|32|256|

### <a name="compute-optimized"></a>Optimisé pour le calcul

|Série|Taille|Processeurs virtuels|Mémoire : Gio|
|-|-|-|-|
|Fsv2|Standard_F4s_v2|4|8|
|Fsv2|Standard_F8s_v2|8|16|
|Fsv2|Standard_F16s_v2|16|32|
|Fsv2|Standard_F32s_v2|32|64|
