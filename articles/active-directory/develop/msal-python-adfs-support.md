---
title: Prise en charge d’Azure AD FS (MSAL Python)
titleSuffix: Microsoft identity platform
description: Découvrir la prise en charge des services de fédération Active Directory (AD FS) dans la bibliothèque d’authentification Microsoft pour Python
services: active-directory
author: abhidnya13
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/23/2019
ms.author: abpati
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 01d4cb626aabc83117e864b75b49eec63a6c0af0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76699544"
---
# <a name="active-directory-federation-services-support-in-msal-for-python"></a>Prise en charge des services de fédération Active Directory (AD FS) dans MSAL pour Python

Les services de fédération Active Directory (AD FS) de Windows Server vous permettent d’ajouter l’authentification et l’autorisation OpenID Connect et OAuth 2.0 à vos applications en utilisant la bibliothèque d’authentification Microsoft (MSAL) pour Python. En utilisant MSAL pour la bibliothèque Python, votre application peut authentifier les utilisateurs directement auprès d’AD FS. Pour plus d’informations sur les scénarios, consultez [Scénarios AD FS pour développeurs](/windows-server/identity/ad-fs/ad-fs-development).

Il existe généralement deux façons d’authentifier auprès d’AD FS :

- La bibliothèque MSAL Python communique avec Azure Active Directory, lui-même fédéré à d’autres fournisseurs d’identités. La fédération s’effectue par le biais d’AD FS. MSAL pour Python se connecte à Azure AD, qui connecte les utilisateurs managés dans Azure AD (utilisateurs managés) ou les utilisateurs managés par un autre fournisseur d’identité comme AD FS (utilisateurs fédérés). MSAL Python ne sait pas qu’un utilisateur est fédéré. Elle communique juste avec Azure AD. L’[autorité](msal-client-application-configuration.md#authority) que vous utilisez dans ce cas est l’autorité habituelle (nom d’hôte d’autorité + locataire, commun ou organisations).
- MSAL Python communique directement avec une autorité AD FS. Cette opération est seulement prise en charge par AD FS 2019 et ultérieur.

## <a name="connect-to-active-directory-federated-with-ad-fs"></a>Se connecter à Active Directory fédéré avec AD FS

### <a name="acquire-a-token-interactively-for-a-federated-user"></a>Obtenir un jeton de manière interactive pour un utilisateur fédéré

Ce qui suit s’applique si vous vous connectez directement aux services de fédération Active Directory (AD FS) ou via Active Directory.

Lorsque vous appelez `acquire_token_by_authorization_code` ou `acquire_token_by_device_flow`, l’expérience utilisateur est généralement la suivante :

1. L’utilisateur entre son ID de compte.
2. Azure AD affiche brièvement le message « Vous êtes redirigé vers la page de votre organisation » et l'utilisateur est redirigé vers la page de connexion du fournisseur d’identité. La page de connexion est en règle générale personnalisée avec le logo de l’organisation.

Les versions AD FS prises en charge dans ce scénario fédéré sont :
- Services de fédération Active Directory (AD FS) v2
- Services de fédération Active Directory (AD FS) v3 (Windows Server 2012 R2)
- Services de fédération Active Directory (AD FS) v4 (AD FS 2016)

### <a name="acquire-a-token-via-username-and-password"></a>Obtenir un jeton via un nom d'utilisateur et un mot de passe

Ce qui suit s’applique si vous vous connectez directement aux services de fédération Active Directory (AD FS) ou via Active Directory.

Lorsque vous obtenez un jeton avec `acquire_token_by_username_password`, MSAL Python obtient le fournisseur d’identité à contacter en fonction du nom d’utilisateur. MSAL Python obtient un [jeton SAML 1.1](reference-saml-tokens.md) auprès du fournisseur d’identité, qu’elle fournit ensuite à Azure AD qui retourne le jeton web JSON (JWT).

## <a name="connecting-directly-to-ad-fs"></a>Connexion directe à AD FS

Lorsque vous vous connectez directement à AD FS, l’autorité que vous voulez utiliser pour créer votre application ressemble à `https://somesite.contoso.com/adfs/`.

MSAL Python prend en charge ADFS 2019.

Elle ne prend pas en charge une connexion directe à ADFS 2016 ou ADFS v2. Si vous devez prendre en charge des scénarios qui demandent une connexion directe à AD FS 2016, utilisez la dernière version d’ADAL Python. Une fois que vous avez mis à niveau votre système local vers ADFS 2019, vous pouvez utiliser MSAL Python.

## <a name="next-steps"></a>Étapes suivantes

- Pour l'authentification fédérée, consultez [Configurer le comportement de la connexion Azure Active Directory pour une application à l'aide d'une stratégie de découverte du domaine d'accueil](../manage-apps/configure-authentication-for-federated-users-portal.md).