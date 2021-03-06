---
title: Séries Dav4 et Dasv4 – Machines virtuelles Microsoft Azure
description: Spécifications pour les machines virtuelles des séries Dav4 et Dasv4.
services: virtual-machines
author: migerdes
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: c7a2fea94e0dc1ff868eff26399877cab66e6f66
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115353"
---
# <a name="dav4-and-dasv4-series"></a>Séries Dav4 et Dasv4

Les séries Dav4 et Dasv4 sont de nouvelles tailles qui utilisent le processeur EPYC<sup>TM</sup> 7452 2,35 GHz d’AMD dans une configuration multithread, avec un cache L3 allant jusqu’à 256 Mo, dont 8 Mo sont dédiés aux 8 cœurs, ce qui offre aux clients davantage d’options pour exécuter leurs charges de travail à usage général. Elles ont les mêmes configurations de mémoire et de disque que les séries D et Dsv3.

## <a name="dav4-series"></a>Série Dav4

ACU : 230-260

Premium Storage : Non pris en charge

Mise en cache du Stockage Premium : Non pris en charge

Migration dynamique : Prise en charge

Mises à jour avec préservation de la mémoire : Prise en charge

Les tailles de la série Dav4 sont basées sur le processeur AMD EPYC<sup>TM</sup> 7452 2,35 GHz, qui peut atteindre une fréquence maximale renforcée de 3,35 GHz. Elles offrent une combinaison de processeur virtuel, de mémoire et de stockage temporaire adaptée à la plupart des charges de travail de production. Le stockage sur disque de données est facturé séparément des machines virtuelles. Pour utiliser un SSD Premium, optez pour les tailles Dasv4. Les tarifs et les compteurs de facturation de ces tailles sont identiques à ceux de la série Dav4.

| Taille | Processeurs virtuels | Mémoire : Gio | Stockage temporaire (SSD) en Gio | Disques de données max. | Débit de stockage temporaire max. : IOPS / MBps en lecture / MBps en écriture | Nombre max. de cartes réseau / Bande passante réseau attendue (Mbits/s) |
|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2a_v4 |  2  | 8  | 50  | 4  | 3000 / 46 / 23   | 2 / 1 000 |
| Standard_D4a_v4 |  4  | 16 | 100 | 8  | 6000 / 93 / 46   | 2 / 2 000 |
| Standard_D8a_v4 |  8  | 32 | 200 | 16 | 12000 / 187 / 93 | 4/4000 |
| Standard_D16a_v4|  16 | 64 | 400 |32  | 24000 / 375 / 187 |8 / 8000 |
| Standard_D32a_v4|  32 | 128| 800 | 32 | 48000 / 750 / 375 |8 / 16 000 |
| Standard_D48a_v4| 48 | 192| 1200 | 32 | 96000 / 1000 / 500 | 8 / 24000 |
| Standard_D64a_v4| 64 | 256 | 1 600 | 32 | 96000 / 1000 / 500 | 8 / 30000 |
| Standard_D96a_v4| 96 | 384 | 2 400 | 32 | 96000 / 1000 / 500 | 8 / 30000 |

## <a name="dasv4-series"></a>Série Dasv4

ACU :  230-260

Premium Storage : Prise en charge

Mise en cache du Stockage Premium : Prise en charge

Migration dynamique : Prise en charge

Mises à jour avec préservation de la mémoire : Prise en charge

Les tailles de la série Dasv4 sont basées sur le processeur AMD EPYC<sup>TM</sup> 7452 2,35 GHz, qui peut atteindre une fréquence maximale renforcée de 3,35 GHz et utiliser un SSD Premium. Elles offrent une combinaison de processeur virtuel, de mémoire et de stockage temporaire adaptée à la plupart des charges de travail de production.

| Taille | Processeurs virtuels | Mémoire : Gio | Stockage temporaire (SSD) en Gio | Disques de données max. | Débit de stockage temporaire et mis en cache max. : IOPS / MBps (taille du cache en Gio) | Débit du disque non mis en cache max. : IOPS / MBps | Nombre max. de cartes réseau / Bande passante réseau attendue (Mbits/s) |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2as_v4|2|8|16|4|4000 / 32 (50)|3200 / 48|2 / 1 000 |
| Standard_D4as_v4|4|16|32|8|8000 / 64 (100)|6400 / 96|2 / 2 000 |
| Standard_D8as_v4|8|32|64|16|16000 / 128 (200)|12800 / 192|4/4000 |
| Standard_D16as_v4|16|64|128|32|32 000 / 255 (400)|25600 / 384|8 / 8000 |
| Standard_D32as_v4|32|128|256|32|64 000 / 510 (800)|51200 / 768|8 / 16 000 |
| Standard_D48as_v4|48|192|384|32|96000 / 1020 (1200)|76800 / 1148|8 / 24000 |
| Standard_D64as_v4|64|256|512|32|128000 / 1020 (1600)|80000 / 1200|8 / 30000 | 
| Standard_D96as_v4|96|384|768|32|192000 / 1020 (2400)|80000 / 1200|8 / 30000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Autres tailles

- [Usage général](sizes-general.md)
- [Mémoire optimisée](sizes-memory.md)
- [Optimisé pour le stockage](sizes-storage.md)
- [Optimisé pour le GPU](sizes-gpu.md)
- [Calcul haute performance](sizes-hpc.md)
- [Générations précédentes](sizes-previous-gen.md)

## <a name="next-steps"></a>Étapes suivantes

Lisez-en davantage sur les [Unités de calcul Azure (ACU)](acu.md) pour découvrir comment comparer les performances de calcul entre les références Azure.
