---
title: Utiliser le Pare-feu Azure pour protéger Windows Virtual Desktop
description: Découvrez comment utiliser le Pare-feu Azure pour protéger les déploiements de Windows Virtual Desktop
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: victorh
ms.openlocfilehash: ab0f4856ff924a2c00f1ee2eec614b7a31ac381f
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610715"
---
# <a name="use-azure-firewall-to-protect-window-virtual-desktop-deployments"></a>Utiliser le Pare-feu Azure pour protéger les déploiements de Windows Virtual Desktop

Windows Virtual Desktop (WVD) est un service de virtualisation de bureau et d’application qui s’exécute sur Azure. Quand un utilisateur final se connecte à un environnement Windows Virtual Desktop, sa session est exécutée par un pool d’hôtes. Un pool d’hôtes est une collection de machines virtuelles Azure qui s’inscrivent auprès de Windows Virtual Desktop en tant qu’hôtes de session. Ces machines virtuelles s’exécutent dans votre réseau virtuel et sont soumises aux contrôles de sécurité de ce dernier. Elles ont besoin d’un accès Internet sortant au service WVD pour fonctionner correctement et peuvent également nécessiter un accès Internet sortant pour les utilisateurs finaux. Le Pare-feu Azure peut vous aider à verrouiller votre environnement et à filtrer le trafic sortant.

[ ![Architecture de Windows Virtual Desktop](media/protect-windows-virtual-desktop/windows-virtual-desktop-architecture-diagram.png) ](media/protect-windows-virtual-desktop/windows-virtual-desktop-architecture-diagram.png#lightbox)

Suivez les instructions de cet article pour renforcer la protection de votre pool d’hôtes à l’aide du Pare-feu Azure.

## <a name="prerequisites"></a>Prérequis


 - Un pool d’hôtes et un environnement WVD déployés.

   Pour plus d’informations, consultez [Didacticiel : Créer un pool d’hôtes en utilisant la Place de marché Azure](../virtual-desktop/create-host-pools-azure-marketplace.md) et [Créer un pool d’hôtes avec un modèle Azure Resource Manager](../virtual-desktop/virtual-desktop-fall-2019/create-host-pools-arm-template.md).

Pour plus d’informations sur les environnements WVD, consultez [Environnement Windows Virtual Desktop](../virtual-desktop/environment-setup.md).

## <a name="host-pool-outbound-access-to-windows-virtual-desktop"></a>Accès sortant du pool d’hôtes à Windows Virtual Desktop

Les machines virtuelles Azure que vous créez pour Windows Virtual Desktop doivent avoir accès à plusieurs noms de domaine complets (FQDN) pour fonctionner correctement. Le Pare-feu Azure fournit une étiquette FQDN Windows Virtual Desktop pour simplifier cette configuration. Effectuez les étapes suivantes pour autoriser le trafic sortant de la plateforme WVD :

- Déployez le Pare-feu Azure et configurez la route définie par l’utilisateur (UDR) du sous-réseau de pool d’hôtes WVD afin que le trafic transite par le Pare-feu Azure. La route par défaut pointe maintenant vers le pare-feu.
- Créez une collection de règles d’application et ajoutez une règle pour activer l’étiquette FQDN *WindowsVirtualDesktop*. La plage d’adresses IP source est le réseau virtuel du pool d’hôtes, le protocole est **https** et la destination est **WindowsVirtualDesktop**.

- L’ensemble des comptes de stockage et Service Bus requis pour votre pool d’hôtes WVD étant propre au déploiement, il n’est pas encore capturé dans l’étiquette FQDN WindowsVirtualDesktop. Vous pouvez résoudre cette situation de l’une des manières suivantes :

   - Autorisez l’accès https à partir de votre sous-réseau de pool d’hôtes vers *xt.blob.core.windows.net, *eh.servicebus.windows.net et *xt.table.core.windows.net. Ces noms de domaine complets génériques permettent l’accès requis, mais sont moins restrictifs.
   - Utilisez la requête Log Analytics suivante pour lister les noms de domaine complets requis exacts, puis autorisez-les explicitement dans les règles d’application de votre pare-feu :
   ```
   AzureDiagnostics
   | where Category == "AzureFirewallApplicationRule"
   | search "Deny"
   | search "gsm*eh.servicebus.windows.net" or "gsm*xt.blob.core.windows.net" or "gsm*xt.table.core.windows.net"
   | parse msg_s with Protocol " request from " SourceIP ":" SourcePort:int " to " FQDN ":" *
   | project TimeGenerated,Protocol,FQDN
   ```

- Créez une collection de règles de réseau, puis ajoutez les règles suivantes :

   - Autoriser DNS : autorisez le trafic à partir de votre adresse IP privée ADDS vers * pour les ports TCP et UDP 53.
   - Autoriser KMS : autorisez le trafic à partir de vos machines virtuelles WVD vers le port TCP 1688 du service d’activation Windows. Pour plus d’informations sur les adresses IP de destination, consultez [L’activation de Windows échoue dans un scénario de tunneling forcé](../virtual-machines/troubleshooting/custom-routes-enable-kms-activation.md#solution).

> [!NOTE]
> Certains déploiements n’ont peut-être pas besoin de règles DNS ; par exemple, les contrôleurs de domaine Azure Active Directory transfèrent les requêtes DNS à Azure DNS à l’adresse 168.63.129.16.

## <a name="host-pool-outbound-access-to-the-internet"></a>Accès sortant du pool d’hôtes à Internet

Selon les besoins de votre organisation, vous souhaiterez peut-être activer l’accès Internet sortant sécurisé pour vos utilisateurs finaux. Dans les cas où la liste des destinations autorisées est bien définie (par exemple [accès Office 365](https://docs.microsoft.com/Office365/Enterprise/office-365-ip-web-service)), vous pouvez utiliser des règles d’application et de réseau du Pare-feu Azure pour configurer l’accès requis. Le trafic des utilisateurs finaux est ainsi directement routé vers Internet, ce qui optimise les performances.

Si vous souhaitez filtrer le trafic Internet utilisateur sortant à l’aide d’une passerelle web sécurisée locale existante, vous pouvez configurer des navigateurs web ou d’autres applications qui s’exécutent sur le pool d’hôtes WVD avec une configuration de proxy explicite. Pour obtenir un exemple, consultez [Procédure d’utilisation des options de ligne de commande MicrosoftEdge pour configurer les paramètres de proxy](https://docs.microsoft.com/deployedge/edge-learnmore-cmdline-options-proxy-settings). Ces paramètres de proxy n’influencent que l’accès Internet de l’utilisateur final, ce qui permet le trafic sortant de la plateforme WVD directement via le Pare-feu Azure.

## <a name="additional-considerations"></a>Considérations supplémentaires

Vous devrez peut-être configurer des règles de pare-feu supplémentaires, en fonction de vos exigences :

- Accès au serveur NTP

   Par défaut, les machines virtuelles exécutant Windows se connectent à time.windows.com via le port UDP 123 pour la synchronisation de l’heure. Créez une règle de réseau pour autoriser cet accès ou pour un serveur de temps que vous utilisez dans votre environnement.


## <a name="next-steps"></a>Étapes suivantes

- Apprenez-en davantage sur Windows Virtual Desktop : [Qu’est-ce que Windows Virtual Desktop ?](../virtual-desktop/overview.md)