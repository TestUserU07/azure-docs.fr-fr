---
title: Gérer les conflits entre les régions dans Azure Cosmos DB
description: Découvrir comment gérer des conflits dans Azure Cosmos DB en créant la stratégie de type last-writer-wins (dernière version valide) ou une stratégie personnalisée de résolution de conflit
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: mjbrown
ms.openlocfilehash: 6d364f1a9974d6d638bb0f824e88ed3866644c15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79225909"
---
# <a name="manage-conflict-resolution-policies-in-azure-cosmos-db"></a>Gérer les stratégies de résolution des conflits dans Azure Cosmos DB

Avec les écritures multirégions, quand plusieurs clients écrivent dans le même élément, des conflits peuvent se produire. Quand un conflit se produit, vous pouvez le résoudre à l’aide des différentes stratégies de résolution de conflit. Cet article explique comment gérer les stratégies de résolution de conflit.

## <a name="create-a-last-writer-wins-conflict-resolution-policy"></a>Créer une stratégie de résolution de conflit de type last-writer-wins (dernière version valide)

Ces exemples montrent comment configurer un conteneur avec une stratégie de résolution de conflit de type last-writer-wins (dernière version valide). Le chemin par défaut de la stratégie last-writer-wins est le champ d’horodatage ou la propriété `_ts`. Pour l’API SQL, il peut aussi s’agir d’un chemin défini par l’utilisateur avec un type numérique. En cas de conflit, la valeur la plus élevée l’emporte. Si le chemin n’est pas défini ou n’est pas valide, il suit par défaut la propriété `_ts`. Les conflits résolus avec cette stratégie ne s’affichent pas dans le flux de conflit. Cette stratégie peut être utilisée par toutes les API.

### <a name="net-sdk-v2"></a><a id="create-custom-conflict-resolution-policy-lww-dotnet"></a>Kit SDK .NET V2

```csharp
DocumentCollection lwwCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.lwwCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.LastWriterWins,
          ConflictResolutionPath = "/myCustomId",
      },
  });
```

### <a name="net-sdk-v3"></a><a id="create-custom-conflict-resolution-policy-lww-dotnet-v3"></a>Kit SDK .NET V3

```csharp
Container container = await createClient.GetDatabase(this.databaseName)
    .CreateContainerIfNotExistsAsync(new ContainerProperties(this.lwwCollectionName, "/partitionKey")
    {
        ConflictResolutionPolicy = new ConflictResolutionPolicy()
        {
            Mode = ConflictResolutionMode.LastWriterWins,
            ResolutionPath = "/myCustomId",
        }
    });
```

### <a name="java-async-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-java-async"></a>Kit SDK Java Async

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a name="java-sync-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-java-sync"></a>Kit SDK Java Sync

```java
DocumentCollection lwwCollection = new DocumentCollection();
lwwCollection.setId(this.lwwCollectionName);
ConflictResolutionPolicy lwwPolicy = ConflictResolutionPolicy.createLastWriterWinsPolicy("/myCustomId");
lwwCollection.setConflictResolutionPolicy(lwwPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, lwwCollection);
```

### <a name="nodejsjavascripttypescript-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-javascript"></a>Kit SDK Node.js/JavaScript/TypeScript

```javascript
const database = client.database(this.databaseName);
const { container: lwwContainer } = await database.containers.createIfNotExists(
  {
    id: this.lwwContainerName,
    conflictResolutionPolicy: {
      mode: "LastWriterWins",
      conflictResolutionPath: "/myCustomId"
    }
  }
);
```

### <a name="python-sdk"></a><a id="create-custom-conflict-resolution-policy-lww-python"></a>Kit SDK Python

```python
udp_collection = {
    'id': self.udp_collection_name,
    'conflictResolutionPolicy': {
        'mode': 'LastWriterWins',
        'conflictResolutionPath': '/myCustomId'
    }
}
udp_collection = self.try_create_document_collection(
    create_client, database, udp_collection)
```

## <a name="create-a-custom-conflict-resolution-policy-using-a-stored-procedure"></a>Créer une stratégie de résolution de conflit personnalisée à l’aide d’une procédure stockée

Ces exemples montrent comment configurer un conteneur avec une stratégie de résolution de conflits personnalisée et une procédure stockée pour résoudre le conflit. Ces conflits n’apparaissent pas dans le flux de conflit, sauf s’il existe une erreur dans votre procédure stockée. Une fois la stratégie créée avec le conteneur, vous devez créer la procédure stockée. L’exemple de kit de développement logiciel (SDK) .NET ci-dessous montre un exemple. Cette stratégie est prise en charge uniquement par l’API Core (SQL).

### <a name="sample-custom-conflict-resolution-stored-procedure"></a>Exemple de procédure stockée de résolution de conflit personnalisée

Les procédures stockées de résolution de conflit personnalisées doivent être implémentées en utilisant la signature de fonction présentée ci-dessous. Le nom de fonction ne doit pas nécessairement correspondre au nom utilisé pendant l’inscription de la procédure stockée auprès du conteneur, mais l’affectation de noms est simplifiée. Voici une description des paramètres qui doivent être implémentés pour cette procédure stockée.

- **incomingItem** : élément inséré ou mis à jour dans la validation qui génère les conflits. A la valeur null pour les opérations de suppression.
- **existingItem** : élément en cours de validation. Cette valeur est non Null dans une mise à jour et Null pour une insertion ou des suppressions.
- **isTombstone** : valeur booléenne indiquant si l’élément incomingItem est en conflit avec un élément supprimé précédemment. Quand sa valeur est true, existingItem a aussi la valeur null.
- **conflictingItems** : Tableau de la version validée de tous les éléments du conteneur qui sont en conflit avec incomingItem concernant l’ID ou d’autres propriétés de l’index unique.

> [!IMPORTANT]
> Comme n’importe quelle procédure stockée, une procédure de résolution de conflit personnalisée peut accéder à des données avec la même clé de partition et peut effectuer une opération d’insertion, de mise à jour ou de suppression pour résoudre des conflits.

Cet exemple de procédure stockée résout des conflits en sélectionnant la valeur la plus basse dans le chemin `/myCustomId`.

```javascript
function resolver(incomingItem, existingItem, isTombstone, conflictingItems) {
  var collection = getContext().getCollection();

  if (!incomingItem) {
      if (existingItem) {

          collection.deleteDocument(existingItem._self, {}, function (err, responseOptions) {
              if (err) throw err;
          });
      }
  } else if (isTombstone) {
      // delete always wins.
  } else {
      if (existingItem) {
          if (incomingItem.myCustomId > existingItem.myCustomId) {
              return; // existing item wins
          }
      }

      var i;
      for (i = 0; i < conflictingItems.length; i++) {
          if (incomingItem.myCustomId > conflictingItems[i].myCustomId) {
              return; // existing conflict item wins
          }
      }

      // incoming item wins - clear conflicts and replace existing with incoming.
      tryDelete(conflictingItems, incomingItem, existingItem);
  }

  function tryDelete(documents, incoming, existing) {
      if (documents.length > 0) {
          collection.deleteDocument(documents[0]._self, {}, function (err, responseOptions) {
              if (err) throw err;

              documents.shift();
              tryDelete(documents, incoming, existing);
          });
      } else if (existing) {
          collection.replaceDocument(existing._self, incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      } else {
          collection.createDocument(collection.getSelfLink(), incoming,
              function (err, documentCreated) {
                  if (err) throw err;
              });
      }
  }
}
```

### <a name="net-sdk-v2"></a><a id="create-custom-conflict-resolution-policy-stored-proc-dotnet"></a>Kit SDK .NET V2

```csharp
DocumentCollection udpCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.udpCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
          ConflictResolutionProcedure = string.Format("dbs/{0}/colls/{1}/sprocs/{2}", this.databaseName, this.udpCollectionName, "resolver"),
      },
  });

//Create the stored procedure
await clients[0].CreateStoredProcedureAsync(
UriFactory.CreateStoredProcedureUri(this.databaseName, this.udpCollectionName, "resolver"), new StoredProcedure
{
    Id = "resolver",
    Body = File.ReadAllText(@"resolver.js")
});
```

### <a name="net-sdk-v3"></a><a id="create-custom-conflict-resolution-policy-stored-proc-dotnet-v3"></a>Kit SDK .NET V3

```csharp
Container container = await createClient.GetDatabase(this.databaseName)
    .CreateContainerIfNotExistsAsync(new ContainerProperties(this.udpCollectionName, "/partitionKey")
    {
        ConflictResolutionPolicy = new ConflictResolutionPolicy()
        {
            Mode = ConflictResolutionMode.Custom,
            ResolutionProcedure = string.Format("dbs/{0}/colls/{1}/sprocs/{2}", this.databaseName, this.udpCollectionName, "resolver")
        }
    });

await container.Scripts.CreateStoredProcedureAsync(
    new StoredProcedureProperties("resolver", File.ReadAllText(@"resolver.js"))
);
```

### <a name="java-async-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-java-async"></a>Kit SDK Java Async

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy("resolver");
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

Après la création de votre conteneur, vous devez créer la procédure stockée `resolver`.

### <a name="java-sync-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-java-sync"></a>Kit SDK Java Sync

```java
DocumentCollection udpCollection = new DocumentCollection();
udpCollection.setId(this.udpCollectionName);
ConflictResolutionPolicy udpPolicy = ConflictResolutionPolicy.createCustomPolicy(
        String.format("dbs/%s/colls/%s/sprocs/%s", this.databaseName, this.udpCollectionName, "resolver"));
udpCollection.setConflictResolutionPolicy(udpPolicy);
DocumentCollection createdCollection = this.tryCreateDocumentCollection(createClient, database, udpCollection);
```

Après la création de votre conteneur, vous devez créer la procédure stockée `resolver`.

### <a name="nodejsjavascripttypescript-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-javascript"></a>Kit SDK Node.js/JavaScript/TypeScript

```javascript
const database = client.database(this.databaseName);
const { container: udpContainer } = await database.containers.createIfNotExists(
  {
    id: this.udpContainerName,
    conflictResolutionPolicy: {
      mode: "Custom",
      conflictResolutionProcedure: `dbs/${this.databaseName}/colls/${
        this.udpContainerName
      }/sprocs/resolver`
    }
  }
);
```

Après la création de votre conteneur, vous devez créer la procédure stockée `resolver`.

### <a name="python-sdk"></a><a id="create-custom-conflict-resolution-policy-stored-proc-python"></a>Kit SDK Python

```python
udp_collection = {
    'id': self.udp_collection_name,
    'conflictResolutionPolicy': {
        'mode': 'Custom',
        'conflictResolutionProcedure': 'dbs/' + self.database_name + "/colls/" + self.udp_collection_name + '/sprocs/resolver'
    }
}
udp_collection = self.try_create_document_collection(
    create_client, database, udp_collection)
```

Après la création de votre conteneur, vous devez créer la procédure stockée `resolver`.

## <a name="create-a-custom-conflict-resolution-policy"></a>Créer une stratégie de résolution de conflits personnalisée

Ces exemples montrent comment configurer un conteneur avec une stratégie de résolution de conflits personnalisée. Ces conflits apparaissent dans le flux de conflit.

### <a name="net-sdk-v2"></a><a id="create-custom-conflict-resolution-policy-dotnet"></a>Kit SDK .NET V2

```csharp
DocumentCollection manualCollection = await createClient.CreateDocumentCollectionIfNotExistsAsync(
  UriFactory.CreateDatabaseUri(this.databaseName), new DocumentCollection
  {
      Id = this.manualCollectionName,
      ConflictResolutionPolicy = new ConflictResolutionPolicy
      {
          Mode = ConflictResolutionMode.Custom,
      },
  });
```

### <a name="net-sdk-v3"></a><a id="create-custom-conflict-resolution-policy-dotnet-v3"></a>Kit SDK .NET V3

```csharp
Container container = await createClient.GetDatabase(this.databaseName)
    .CreateContainerIfNotExistsAsync(new ContainerProperties(this.manualCollectionName, "/partitionKey")
    {
        ConflictResolutionPolicy = new ConflictResolutionPolicy()
        {
            Mode = ConflictResolutionMode.Custom
        }
    });
```

### <a name="java-async-sdk"></a><a id="create-custom-conflict-resolution-policy-java-async"></a>Kit SDK Java Async

```java
DocumentCollection collection = new DocumentCollection();
collection.setId(id);
ConflictResolutionPolicy policy = ConflictResolutionPolicy.createCustomPolicy();
collection.setConflictResolutionPolicy(policy);
DocumentCollection createdCollection = client.createCollection(databaseUri, collection, null).toBlocking().value();
```

### <a name="java-sync-sdk"></a><a id="create-custom-conflict-resolution-policy-java-sync"></a>Kit SDK Java Sync

```java
DocumentCollection manualCollection = new DocumentCollection();
manualCollection.setId(this.manualCollectionName);
ConflictResolutionPolicy customPolicy = ConflictResolutionPolicy.createCustomPolicy(null);
manualCollection.setConflictResolutionPolicy(customPolicy);
DocumentCollection createdCollection = client.createCollection(database.getSelfLink(), collection, null).getResource();
```

### <a name="nodejsjavascripttypescript-sdk"></a><a id="create-custom-conflict-resolution-policy-javascript"></a>Kit SDK Node.js/JavaScript/TypeScript

```javascript
const database = client.database(this.databaseName);
const {
  container: manualContainer
} = await database.containers.createIfNotExists({
  id: this.manualContainerName,
  conflictResolutionPolicy: {
    mode: "Custom"
  }
});
```

### <a name="python-sdk"></a><a id="create-custom-conflict-resolution-policy-python"></a>Kit SDK Python

```python
database = client.ReadDatabase("dbs/" + self.database_name)
manual_collection = {
    'id': self.manual_collection_name,
    'conflictResolutionPolicy': {
        'mode': 'Custom'
    }
}
manual_collection = client.CreateContainer(database['_self'], collection)
```

## <a name="read-from-conflict-feed"></a>Lire le flux de conflit

Ces exemples montrent comment lire le flux de conflit d’un conteneur. Les conflits figurent dans le flux de conflit uniquement s’ils n’ont pas été résolus automatiquement ou si une stratégie de conflit personnalisée a été utilisée.

### <a name="net-sdk-v2"></a><a id="read-from-conflict-feed-dotnet"></a>Kit SDK .NET V2

```csharp
FeedResponse<Conflict> conflicts = await delClient.ReadConflictFeedAsync(this.collectionUri);
```

### <a name="net-sdk-v3"></a><a id="read-from-conflict-feed-dotnet-v3"></a>Kit SDK .NET V3

```csharp
FeedIterator<ConflictProperties> conflictFeed = container.Conflicts.GetConflictQueryIterator();
while (conflictFeed.HasMoreResults)
{
    FeedResponse<ConflictProperties> conflicts = await conflictFeed.ReadNextAsync();
    foreach (ConflictProperties conflict in conflicts)
    {
        // Read the conflicted content
        MyClass intendedChanges = container.Conflicts.ReadConflictContent<MyClass>(conflict);
        MyClass currentState = await container.Conflicts.ReadCurrentAsync<MyClass>(conflict, new PartitionKey(intendedChanges.MyPartitionKey));

        // Do manual merge among documents
        await container.ReplaceItemAsync<MyClass>(intendedChanges, intendedChanges.Id, new PartitionKey(intendedChanges.MyPartitionKey));

        // Delete the conflict
        await container.Conflicts.DeleteAsync(conflict, new PartitionKey(intendedChanges.MyPartitionKey));
    }
}
```

### <a name="java-async-sdk"></a><a id="read-from-conflict-feed-java-async"></a>Kit SDK Java Async

```java
FeedResponse<Conflict> response = client.readConflicts(this.manualCollectionUri, null)
                    .first().toBlocking().single();
for (Conflict conflict : response.getResults()) {
    /* Do something with conflict */
}
```

### <a name="java-sync-sdk"></a><a id="read-from-conflict-feed-java-sync"></a>Kit SDK Java Sync

```java
Iterator<Conflict> conflictsIterator = client.readConflicts(this.collectionLink, null).getQueryIterator();
while (conflictsIterator.hasNext()) {
    Conflict conflict = conflictsIterator.next();
    /* Do something with conflict */
}
```

### <a name="nodejsjavascripttypescript-sdk"></a><a id="read-from-conflict-feed-javascript"></a>Kit SDK Node.js/JavaScript/TypeScript

```javascript
const container = client
  .database(this.databaseName)
  .container(this.lwwContainerName);

const { result: conflicts } = await container.conflicts.readAll().toArray();
```

### <a name="python"></a><a id="read-from-conflict-feed-python"></a>Python

```python
conflicts_iterator = iter(client.ReadConflicts(self.manual_collection_link))
conflict = next(conflicts_iterator, None)
while conflict:
    # Do something with conflict
    conflict = next(conflicts_iterator, None)
```

## <a name="next-steps"></a>Étapes suivantes

Découvrez les concepts Azure Cosmos DB suivants :

- [Article relatif au principe de la distribution mondiale d’Azure Cosmos DB](global-dist-under-the-hood.md)
- [Configurer la fonction multimaître dans vos applications](how-to-multi-master.md)
- [Configurer les clients pour la multirésidence](how-to-manage-database-account.md#configure-multiple-write-regions)
- [Ajouter ou supprimer des régions de votre compte Azure Cosmos DB](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
- [Guide pratique pour configurer les fonctionnalités multimaîtres dans vos applications](how-to-multi-master.md).
- [Partitionnement et distribution des données](partition-data.md)
- [Indexation dans Azure Cosmos DB](indexing-policies.md)
