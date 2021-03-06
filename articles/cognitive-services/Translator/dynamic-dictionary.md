---
title: Dictionnaire dynamique – Translator
titleSuffix: Azure Cognitive Services
description: Cet article explique comment utiliser la fonctionnalité de dictionnaire dynamique d’Azure Cognitive Services Translator.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: 3ef881efc417a6c3816a92d9af7d77bba74355aa
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592540"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Comment utiliser un dictionnaire dynamique

Si vous connaissez déjà la traduction que vous souhaitez appliquer à un mot ou à une phrase, vous pouvez la fournir en tant que balisage dans la demande. Le dictionnaire dynamique est sûr seulement pour des noms composés, comme des noms propres et des noms de produits.

**Syntaxe :**

<mstrans:dictionary translation=”traduction d’expression”>phrase</mstrans:dictionary>

**Configuration requise :**

* Les langues `From` et `To` doivent inclure l’anglais et une autre langue prise en charge. 
* Vous devez inclure le paramètre `From` dans votre demande de traduction d’API au lieu d’utiliser la fonctionnalité de détection automatique. 

**Exemple : en-de :**

Entrée source : `The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.`

Sortie cible : `Das Wort "wordomatic" ist ein Wörterbucheintrag.`

Cette fonctionnalité opère de la même façon avec ou sans mode HTML.

Utilisez la fonctionnalité avec modération. Une meilleure façon de personnaliser la traduction est d’utiliser Custom Translator. Custom Translator utilise totalement le contexte et les probabilités statistiques. Si vous avez ou pouvez créer des données d’apprentissage qui montrent votre mot ou phrase en contexte, vous obtenez de bien meilleurs résultats. Vous trouverez plus d’informations sur Custom Translator à l’adresse [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).