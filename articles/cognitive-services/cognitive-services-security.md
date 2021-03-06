---
title: Sécurité
titleSuffix: Azure Cognitive Services
description: Découvrez les différentes considérations relatives à la sécurité dans le cadre de l’utilisation de Cognitive Services.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: dapine
ms.openlocfilehash: c86d806c408c2e8226e632a0b15e1e8729c987f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131535"
---
# <a name="azure-cognitive-services-security"></a>Sécurité Azure Cognitive Services

La sécurité doit être considérée comme une priorité absolue lors du développement de toute application. Avec l’arrivée d’applications prenant en charge l’intelligence artificielle, la sécurité devient encore plus importante. Cet article présente divers aspects de la sécurité Azure Cognitive Services, tels que l’utilisation du protocole TLS, l’authentification et la configuration sécurisée des données sensibles.

## <a name="transport-layer-security-tls"></a>TLS (Transport Layer Security)

Tous les points de terminaison de Cognitive Services exposés via HTTP appliquent le protocole TLS 1.2. Quand un protocole de sécurité est appliqué, les consommateurs qui essaient d’appeler un point de terminaison Cognitive Services doivent respecter les consignes suivantes :

* Le système d’exploitation client doit prendre en charge TLS 1.2
* Le langage (et la plateforme) utilisé pour effectuer l’appel HTTP doit spécifier TLS 1.2 dans le cadre de la requête
  * Selon le langage et la plateforme, la spécification de TLS s’effectue implicitement ou explicitement

Pour les utilisateurs .NET, prenez en compte les <a href="https://docs.microsoft.com/dotnet/framework/network-programming/tls" target="_blank">bonnes pratiques liées au protocole TLS<span class="docon docon-navigate-external x-hidden-focus"></span></a>.

## <a name="authentication"></a>Authentification

En ce qui concerne l’authentification, plusieurs fausses idées sont largement répandues. Les gens confondent souvent authentification et autorisation. L’identité est également une composante majeure de la sécurité. Une identité est une collection d’informations sur un <a href="https://en.wikipedia.org/wiki/Principal_(computer_security)" target="_blank">principal<span class="docon docon-navigate-external x-hidden-focus"></span></a>. Les fournisseurs d’identité (IdP) fournissent des identités aux services d’authentification. L’authentification est l’action consistant à vérifier l’identité d’un utilisateur. L’autorisation est la spécification de droits d’accès et de privilèges sur des ressources pour une identité donnée. Plusieurs des offres de Cognitive Services incluent le contrôle d’accès en fonction du rôle (RBAC). La fonctionnalité RBAC peut être utilisée pour simplifier une partie du cérémonial impliqué dans la gestion manuelle des principaux. Pour en savoir plus, reportez-vous au [contrôle d’accès en fonction du rôle pour les ressources Azure](../role-based-access-control/overview.md).

Pour plus d’informations sur l’authentification avec les clés d’abonnement, les jetons d’accès et Azure Active Directory (AAD), consultez <a href="https://docs.microsoft.com/azure/cognitive-services/authentication" target="_blank">Authentifier des requêtes auprès d’Azure Cognitive Services<span class="docon docon-navigate-external x-hidden-focus"></span></a>.

## <a name="environment-variables-and-application-configuration"></a>Variables d’environnement et configuration d’application

Les variables d’environnement sont des paires nom-valeur, stockées dans un environnement spécifique. Une alternative plus sécurisée à l’utilisation des valeurs codées en dur pour les données sensibles consiste à utiliser des variables d’environnement. Les valeurs codées en dur ne sont pas sécurisées et doivent être évitées.

> [!CAUTION]
> N’utilisez **pas** des valeurs codées en dur pour les données sensibles ; cela constitue une faille de sécurité majeure.

> [!NOTE]
> Lorsque les variables d’environnement sont stockées en texte brut, elles sont isolées dans un environnement. Si un environnement est compromis, il en est de même pour les variables avec l’environnement.

### <a name="set-environment-variable"></a>Définir une variable d’environnement

Pour définir des variables d’environnement, utilisez l’une des commandes suivantes, où `ENVIRONMENT_VARIABLE_KEY` est la clé nommée et `value` la valeur stockée dans la variable d’environnement.

# <a name="command-line"></a>[Ligne de commande](#tab/command-line)

Créez et assignez une variable d’environnement persistante, la valeur étant donnée.

```CMD
:: Assigns the env var to the value
setx ENVIRONMENT_VARIABLE_KEY="value"
```

Dans une nouvelle instance de l’**invite de commandes**, lisez la variable d’environnement.

```CMD
:: Prints the env var value
echo %ENVIRONMENT_VARIABLE_KEY%
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Créez et assignez une variable d’environnement persistante, en fonction de la valeur donnée.

```powershell
# Assigns the env var to the value
[System.Environment]::SetEnvironmentVariable('ENVIRONMENT_VARIABLE_KEY', 'value', 'User')
```

Dans une nouvelle instance de **Windows PowerShell**, lisez la variable d’environnement.

```powershell
# Prints the env var value
[System.Environment]::GetEnvironmentVariable('ENVIRONMENT_VARIABLE_KEY')
```

# <a name="bash"></a>[Bash](#tab/bash)

Créez et assignez une variable d’environnement persistante, en fonction de la valeur donnée.

```Bash
# Assigns the env var to the value
echo export ENVIRONMENT_VARIABLE_KEY="value" >> /etc/environment && source /etc/environment
```

Dans une nouvelle instance de **Bash**, lisez la variable d’environnement.

```Bash
# Prints the env var value
echo "${ENVIRONMENT_VARIABLE_KEY}"

# Or use printenv:
# printenv ENVIRONMENT_VARIABLE_KEY
```

---

> [!TIP]
> Après avoir défini une variable d’environnement, redémarrez votre environnement de développement intégré (IDE) pour vous assurer que les variables d’environnement nouvellement ajoutées sont disponibles.

### <a name="get-environment-variable"></a>Obtenir une variable d’environnement

Pour obtenir une variable d’environnement, elle doit être lue en mémoire. Selon le langage que vous utilisez, prenez en compte les extraits de code suivants. Ces extraits de code montrent comment obtenir une variable d’environnement en fonction de `ENVIRONMENT_VARIABLE_KEY` et l’affecter à une variable nommée `value`.

# <a name="c"></a>[C#](#tab/csharp)

Pour plus d’informations, consultez <a href="https://docs.microsoft.com/dotnet/api/system.environment.getenvironmentvariable" target="_blank">`Environment.GetEnvironmentVariable` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

```csharp
using static System.Environment;

class Program
{
    static void Main()
    {
        // Get the named env var, and assign it to the value variable
        var value =
            GetEnvironmentVariable(
                "ENVIRONMENT_VARIABLE_KEY");
    }
}
```

# <a name="c"></a>[C++](#tab/cpp)

Pour plus d’informations, consultez <a href="https://docs.microsoft.com/cpp/c-runtime-library/reference/getenv-wgetenv" target="_blank">`getenv` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

```cpp
#include <stdlib.h>

int main()
{
    // Get the named env var, and assign it to the value variable
    auto value =
        getenv("ENVIRONMENT_VARIABLE_KEY");
}
```

# <a name="java"></a>[Java](#tab/java)

Pour plus d’informations, consultez <a href="https://docs.oracle.com/javase/7/docs/api/java/lang/System.html#getenv(java.lang.String)" target="_blank">`System.getenv` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

```java
import java.lang.*;

public class Program {
   public static void main(String[] args) throws Exception {
    // Get the named env var, and assign it to the value variable
    String value =
        System.getenv(
            "ENVIRONMENT_VARIABLE_KEY")
   }
}
```

# <a name="nodejs"></a>[Node.JS](#tab/node-js)

Pour plus d’informations, consultez <a href="https://nodejs.org/api/process.html#process_process_env" target="_blank">`process.env` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

```javascript
// Get the named env var, and assign it to the value variable
const value =
    process.env.ENVIRONMENT_VARIABLE_KEY;
```

# <a name="python"></a>[Python](#tab/python)

Pour plus d’informations, consultez <a href="https://docs.python.org/2/library/os.html#os.environ" target="_blank">`os.environ` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

```python
import os

# Get the named env var, and assign it to the value variable
value = os.environ['ENVIRONMENT_VARIABLE_KEY']
```

# <a name="objective-c"></a>[Objective-C](#tab/objective-c)

Pour plus d’informations, consultez <a href="https://developer.apple.com/documentation/foundation/nsprocessinfo/1417911-environment?language=objc" target="_blank">`environment` <span class="docon docon-navigate-external x-hidden-focus"></span></a>.

```objectivec
// Get the named env var, and assign it to the value variable
NSString* value =
    [[[NSProcessInfo processInfo]environment]objectForKey:@"ENVIRONMENT_VARIABLE_KEY"];
```

---

## <a name="next-steps"></a>Étapes suivantes

* Explorer les offres [Cognitive Services](welcome.md)
* En savoir plus sur les [réseaux virtuels Cognitive Services](cognitive-services-virtual-networks.md)
