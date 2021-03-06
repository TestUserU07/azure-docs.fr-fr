---
title: Définir un style de carte avec le kit Android SDK Azure Maps | Microsoft Azure Maps
description: Dans cet article, vous découvrirez les fonctionnalités Microsoft Azure Maps liées aux styles pour le kit Android SDK.
author: philmea
ms.author: philmea
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: a4d761abf54682ed0263922d0a118debc9eccf0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334367"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Définir le style de carte à l’aide d'Android SDK Azure Maps

Cet article vous présente deux façons de définir des styles de carte à l’aide d’Android SDK Azure Maps. Azure Maps offre le choix entre six styles de carte. Pour plus d’informations sur les styles de carte pris en charge, consultez [Styles de carte pris en charge dans Azure Maps](./supported-map-styles.md).


## <a name="prerequisites"></a>Conditions préalables requises

Pour suivre la procédure décrite dans cet article, vous devez installer [Android SDK Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) afin de charger une carte.


## <a name="set-map-style-in-the-layout"></a>Définir le style de carte dans la disposition

Vous pouvez définir un style de carte dans le fichier de disposition de votre classe d’activité. Modifiez l'élément **res > layout > activity_main.xml** de sorte qu'il se présente comme suit :

```XML
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/mapcontrol"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

L'attribut `mapcontrol_style` ci-dessus définit le style de carte sur **grayscale_dark**. 

<center>

![style-grayscale_dark](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>Définir le style de carte dans la classe d’activité

Le style de carte peut être défini dans la classe d’activité. Copiez l’extrait de code suivant dans la méthode **onCreate()** de votre classe `MainActivity.java`. Ce code définit le style de carte sur **satellite_road_labels**.

```Java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![style-satellite-road-labels](./media/set-android-map-styles/satellite-road-labels.png)</center>