---
title: Microsoft Teams sur Windows Virtual Desktop - Azure
description: Comment utiliser Microsoft Teams sur Windows Virtual Desktop
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a860b005457c6e02187423a3ffbbc63fe7c758b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82187526"
---
# <a name="use-microsoft-teams-on-windows-virtual-desktop"></a>Utiliser Microsoft Teams sur Windows Virtual Desktop

> S’applique à : Windows 10 et Windows 10 IoT Entreprise

Les environnements virtualisés présentent des défis uniques en termes d'applications de collaboration telles que Microsoft Teams, notamment une latence accrue, une utilisation intensive du processeur hôte et des performances audio et vidéo globalement médiocres. Pour en savoir plus sur l’utilisation de Microsoft teams dans les environnements VDI, consultez [Teams pour Virtual Desktop Infrastructure](https://docs.microsoft.com/microsoftteams/teams-for-vdi).

## <a name="prerequisites"></a>Prérequis

Avant d'utiliser Microsoft Teams sur Windows Virtual Desktop, vous devez effectuer les opérations suivantes :

- Installez le [client Windows Desktop](connect-windows-7-and-10.md) sur un appareil Windows 10 répondant à la [configuration matérielle](https://docs.microsoft.com/microsoftteams/hardware-requirements-for-the-teams-app) requise pour Microsoft Teams.
- Connectez-vous à une machine virtuelle Windows 10 multisession ou Windows 10 Entreprise.
- [Préparez votre réseau](https://docs.microsoft.com/microsoftteams/prepare-network) pour Microsoft Teams.

## <a name="use-unoptimized-microsoft-teams"></a>Utiliser une version non optimisée de Microsoft Teams

Vous pouvez utiliser Microsoft Teams dans vos environnements Windows Virtual Desktop pour tirer parti des fonctionnalités de conversation et de collaboration de Microsoft Teams. Windows Virtual Desktop ne prend pas en charge Teams sur les optimisations audio/vidéo (AV) de l’infrastructure VDI. Les appels et les réunions ne sont pas pris en charge. Si les stratégies de votre organisation le permettent, vous pouvez toujours effectuer des appels audio et joindre des réunions avec du son, mais la qualité audio non optimisée dans les appels varie en fonction de la configuration de votre hôte et peut ne pas être fiable. Pour plus d’informations, consultez [Considérations relatives aux performances de Teams sur l’infrastructure VDI](https://docs.microsoft.com/microsoftteams/teams-for-vdi#teams-on-vdi-performance-considerations).

### <a name="prepare-your-image-for-teams"></a>Préparer votre image pour Teams

Pour permettre l’installation par machine de Teams, définissez la clé de registre suivante sur l’hôte :

```shell
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Teams\IsWVDEnvironment]
  Type: REG_DWORD
  Value: 1
```

### <a name="install-microsoft-teams"></a>Installer Microsoft Teams

Vous pouvez déployer l’application de bureau Teams à l’aide d’une installation par machine. Pour installer Microsoft Teams dans votre environnement Windows Virtual Desktop :

1. Téléchargez le [package MSI Teams](https://docs.microsoft.com/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm) correspondant à votre environnement. Nous vous recommandons d’utiliser le programme d’installation 64 bits sur un système d’exploitation 64 bits.
2. Exécutez cette commande pour installer le MSI sur la machine virtuelle hôte.

      ```shell
      msiexec /i <msi_name> /l*v < install_logfile_name> ALLUSER=1
      ```

      Cela permet d’installer Teams dans Program Files ou Program Files (x86). Ensuite, lorsque vous vous connectez et démarrez Teams, l'application vous demande vos informations d’identification.

      > [!NOTE]
      > Les utilisateurs et les administrateurs ne peuvent pas désactiver le lancement automatique de Teams lors de la connexion.

      Pour désinstaller le MSI de la machine virtuelle hôte, exécutez la commande suivante :

      ```shell
      msiexec /passive /x <msi_name> /l*v <uninstall_logfile_name>
      ```

      > [!NOTE]
      > Si vous installez Teams avec le paramètre MSI ALLUSER=1, les mises à jour automatiques sont désactivées. Nous vous recommandons de mettre à jour Teams au moins une fois par mois. Pour en savoir plus sur le déploiement de l’application de bureau Teams, consultez [Déployer l’application de bureau Teams sur la machine virtuelle](https://docs.microsoft.com/microsoftteams/teams-for-vdi#deploy-the-teams-desktop-app-to-the-vm).

## <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Personnaliser les propriétés de RDP pour un pool d’hôtes
La personnalisation des propriétés du protocole RDP (Remote Desktop Protocol) d’un pool d’hôtes, comme l’utilisation de plusieurs écrans et l’activation de la redirection audio et du microphone, vous permet d’offrir une expérience optimale pour vos utilisateurs en fonction de leurs besoins. Vous pouvez personnaliser les propriétés de RDP dans Windows Virtual Desktop avec le paramètre **-CustomRdpProperty** dans l’applet de commande **Set-RdsHostPool**.
Pour obtenir la liste complète des propriétés prises en charge et leur valeur par défaut, consultez [Paramètres de fichier RDP pris en charge](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context).
