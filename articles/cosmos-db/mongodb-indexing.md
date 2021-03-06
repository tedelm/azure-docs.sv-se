---
title: Hantera indexering i Azure Cosmos DBs API för MongoDB
description: 'Den här artikeln visar en översikt över Azure Cosmos DB indexerings funktioner med hjälp av MongoDB-API: et.'
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 04/03/2020
author: timsander1
ms.author: tisande
ms.openlocfilehash: fd602f88acf26e821e57e0a844f543aac08dad0d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732712"
---
# <a name="manage-indexing-in-azure-cosmos-dbs-api-for-mongodb"></a>Hantera indexering i Azure Cosmos DBs API för MongoDB

Azure Cosmos DBs API för MongoDB drar nytta av kärn funktionerna för hantering av Azure Cosmos DB. Den här artikeln fokuserar på hur du lägger till index med Azure Cosmos DB s API för MongoDB. Du kan också läsa en [Översikt över indexering i Azure Cosmos DB](index-overview.md) som är relevant för alla API: er.

## <a name="indexing-for-mongodb-server-version-36"></a>Indexering för MongoDB-Server version 3,6

Azure Cosmos DBs API för MongoDB-Server version 3,6 indexerar automatiskt `_id` fältet, som inte går att släppa. Det tillämpar automatiskt `_id` fältets unikhet per Shard-nyckel.

Om du vill indexera ytterligare fält tillämpar du MongoDB index-Management-kommandon. Precis som i MongoDB indexerar Azure Cosmos DBs-API för MongoDB automatiskt `_id` fältet. Den här standard indexerings principen skiljer sig från Azure Cosmos DB SQL API, som som standard indexerar alla fält.

Om du vill tillämpa en sortering på en fråga måste du skapa ett index för fälten som används i sorterings åtgärden.

## <a name="index-types"></a>Indextyper

### <a name="single-field"></a>Enskilt fält

Du kan skapa index för ett enskilt fält. Sorterings ordningen för det enskilda fält indexet spelar ingen roll. Följande kommando skapar ett index i fältet `name`:

`db.coll.createIndex({name:1})`

En fråga använder flera enstaka fält index där det är tillgängligt. Du kan skapa upp till 500 enstaka fält index per behållare.

### <a name="compound-indexes-mongodb-server-version-36"></a>Sammansatta index (MongoDB Server version 3,6)

Azure Cosmos DBs API för MongoDB stöder sammansatta index för konton som använder version 3,6 Wire-protokollet. Du kan inkludera upp till åtta fält i ett sammansatt index. Till skillnad från MongoDB bör du endast skapa ett sammansatt index om frågan behöver sortera effektivt på flera fält samtidigt. För frågor med flera filter som inte behöver sorteras, skapar du flera fält index i stället för ett enda sammansatt index.

Följande kommando skapar ett sammansatt index för fälten `name` och: `age`

`db.coll.createIndex({name:1,age:1})`

Du kan använda sammansatta index för att sortera effektivt på flera fält samtidigt, som du ser i följande exempel:

`db.coll.find().sort({name:1,age:1})`

Du kan också använda föregående sammansatta index för att på ett effektivt sätt sortera efter en fråga med den motsatta sorterings ordningen i alla fält. Här är ett exempel:

`db.coll.find().sort({name:-1,age:-1})`

Sekvensen för Sök vägarna i det sammansatta indexet måste dock exakt matcha frågan. Här är ett exempel på en fråga som kräver ett ytterligare sammansatt index:

`db.coll.find().sort({age:1,name:1})`

### <a name="multikey-indexes"></a>MultiKey-index

Azure Cosmos DB skapar MultiKey-index för index innehåll som lagras i matriser. Om du indexerar ett fält med ett mat ris värde indexeras Azure Cosmos DB automatiskt varje element i matrisen.

### <a name="geospatial-indexes"></a>Geospatiala index

Många geospatiala operatörer kommer att dra fördel av geospatiala index. För närvarande stöder `2dsphere` Azure Cosmos DB s API för MongoDB index. API: t har ännu inte `2d` stöd för index.

Här är ett exempel på hur du `location` skapar ett geospatialt index i fältet:

`db.coll.createIndex({ location : "2dsphere" })`

### <a name="text-indexes"></a>Text index

Azure Cosmos DBs API för MongoDB stöder för närvarande inte text index. För texts öknings frågor på strängar bör du använda [Azure kognitiv sökning](https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb) integration med Azure Cosmos dB.

## <a name="index-properties"></a>Index egenskaper

Följande åtgärder är vanliga för konton som hanterar Wire Protocol version 3,6 och konton som hanterar tidigare versioner. Du kan läsa mer om [index som stöds och indexerade egenskaper](mongodb-feature-support-36.md#indexes-and-index-properties).

### <a name="unique-indexes"></a>Unika index

[Unika index](unique-keys.md) är användbara för att tvinga fram att två eller flera dokument inte innehåller samma värde för indexerade fält.

> [!IMPORTANT]
> Det går bara att skapa unika index när samlingen är tom (innehåller inga dokument).

Följande kommando skapar ett unikt index i fältet `student_id`:

```shell
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1 }, {unique:true} )
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 4
}
```

För shardade-samlingar måste du ange Shard-nyckeln (partition) för att skapa ett unikt index. Med andra ord så utgör alla unika index i en fragmenterad samling alltså sammansatta index där ett av fälten är en partitionsnyckel.

Följande kommandon skapar en shardade-samling ```coll``` (Shard-nyckeln är ```university```) med ett unikt index för fälten `student_id` och: `university`

```shell
globaldb:PRIMARY> db.runCommand({shardCollection: db.coll._fullName, key: { university: "hashed"}});
{
        "_t" : "ShardCollectionResponse",
        "ok" : 1,
        "collectionsharded" : "test.coll"
}
globaldb:PRIMARY> db.coll.createIndex( { "student_id" : 1, "university" : 1 }, {unique:true})
{
        "_t" : "CreateIndexesResponse",
        "ok" : 1,
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 3,
        "numIndexesAfter" : 4
}
```

I föregående exempel returnerar- ```"university":1``` satsen ett fel med följande meddelande:

```"cannot create unique index over {student_id : 1.0} with shard key pattern { university : 1.0 }"```

### <a name="ttl-indexes"></a>TTL-index

Om du vill aktivera dokumentets förfallo datum i en viss samling måste du skapa ett [TTL-index (Time-to-Live)](../cosmos-db/time-to-live.md). Ett TTL-index är ett index i `_ts` fältet med ett `expireAfterSeconds` värde.

Exempel:

```JavaScript
globaldb:PRIMARY> db.coll.createIndex({"_ts":1}, {expireAfterSeconds: 10})
```

Föregående kommando tar bort alla dokument i ```db.coll``` samlingen som inte har ändrats under de senaste 10 sekunderna.

> [!NOTE]
> **_Tss** fältet är särskilt för Azure Cosmos DB och kan inte nås från MongoDB-klienter. Det är en reserverad (system) egenskap som innehåller tidsstämpeln för dokumentets senaste ändring.

## <a name="track-index-progress"></a>Spåra index förlopp

Version 3,6 av Azure Cosmos DB s API för MongoDB stöder `currentOp()` kommandot för att spåra index förloppet på en databas instans. Det här kommandot returnerar ett dokument som innehåller information om pågående åtgärder på en databas instans. Du kan använda `currentOp` kommandot för att spåra alla pågående åtgärder i interna MongoDB. I Azure Cosmos DB s API för MongoDB stöder det här kommandot endast spårning av index åtgärden.

Här följer några exempel som visar hur du använder `currentOp` kommandot för att spåra indexerings förlopp:

* Hämta index förloppet för en samling:

   ```shell
   db.currentOp({"command.createIndexes": <collectionName>, "command.$db": <databaseName>})
   ```

* Hämta index förloppet för alla samlingar i en databas:

  ```shell
  db.currentOp({"command.$db": <databaseName>})
  ```

* Hämta index förloppet för alla databaser och samlingar i ett Azure Cosmos-konto:

  ```shell
  db.currentOp({"command.createIndexes": { $exists : true } })
  ```

### <a name="examples-of-index-progress-output"></a>Exempel på utdata för index förlopp

Informationen om indexets förlopp visar procent andelen förloppet för den aktuella index åtgärden. Här är ett exempel som visar utdatafilens format för olika steg i index förloppet:

- En index åtgärd för en "foo"-samling och "stapel"-databasen som är 60 procent slutförd kommer att ha följande utdata-dokument. I `Inprog[0].progress.total` fältet visas 100 som målets slut för ande procent.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 60 %",
                "progress" : {
                        "done" : 60,
                        "total" : 100
                },
                …………..…..
        }
        ],
        "ok" : 1
   }
   ```

- Om en index åtgärd precis har startats i en "foo"-samling och "stapel"-databasen kan utmatnings dokumentet Visa 0 procents förlopp tills det når en mätbar nivå.

   ```json
   {
        "inprog" : [
        {
                ………………...
                "command" : {
                        "createIndexes" : foo
                        "indexes" :[ ],
                        "$db" : bar
                },
                "msg" : "Index Build (background) Index Build (background): 0 %",
                "progress" : {
                        "done" : 0,
                        "total" : 100
                },
                …………..…..
        }
        ],
       "ok" : 1
   }
   ```

- När den pågående index åtgärden har slutförts, visar utmatnings dokumentet tomma `inprog` åtgärder.

   ```json
   {
      "inprog" : [],
      "ok" : 1
   }
   ```

### <a name="background-index-updates"></a>Uppdateringar av bakgrunds index

Oavsett vilket värde som angetts för egenskapen för **bakgrunds** index görs alltid index uppdateringar i bakgrunden. Eftersom index uppdateringar använder ru: er-enheter () med lägre prioritet än andra databas åtgärder, resulterar index ändringar inte i någon nedtid för skrivningar, uppdateringar eller borttagningar.

När du lägger till ett nytt index använder frågorna direkt indexet. Det innebär att frågor kanske inte returnerar alla matchande resultat och gör det utan att returnera några fel. När omvandling av indexet är slutförd kommer frågeresultatet att bli konsekvent. Du kan [spåra index förloppet](#track-index-progress).

## <a name="migrate-collections-with-indexes"></a>Migrera samlingar med index

För närvarande kan du bara skapa unika index när samlingen inte innehåller några dokument. Populära Migreringsverktyg för MongoDB försöker skapa unika index när du har importerat data. För att kringgå det här problemet kan du skapa motsvarande samlingar och unika index manuellt i stället för att tillåta att migrations verktyget försöker. (Du kan få det här beteendet ```mongorestore``` genom att `--noIndexRestore` använda-flaggan på kommando raden.)

## <a name="indexing-for-mongodb-version-32"></a>Indexering för MongoDB-version 3,2

Tillgängliga indexerings funktioner och standardvärden skiljer sig åt för Azure Cosmos-konton som är kompatibla med version 3,2 av MongoDB-Wire-protokollet. Du kan [kontrol lera kontots version](mongodb-feature-support-36.md#protocol-support). Du kan uppgradera till 3,6-versionen genom att skicka en [support förfrågan](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

Om du använder version 3,2 beskriver det här avsnittet viktiga skillnader med version 3,6.

### <a name="dropping-default-indexes-version-32"></a>Släpper standard index (version 3,2)

Till skillnad från 3,6-versionen av Azure Cosmos DB s API för MongoDB indexerar version 3,2 varje egenskap som standard. Du kan använda följande kommando för att ta bort dessa standard index för en samling (```coll```):

```JavaScript
> db.coll.dropIndexes()
{ "_t" : "DropIndexesResponse", "ok" : 1, "nIndexesWas" : 3 }
```

När du har släppt standard indexen kan du lägga till fler index som du skulle göra i version 3,6.

### <a name="compound-indexes-version-32"></a>Sammansatta index (version 3,2)

Sammansatta index innehåller referenser till flera fält i ett dokument. Om du vill skapa ett sammansatt index uppgraderar du till version 3,6 genom att skicka en [support förfrågan](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Nästa steg

* [Indexering i Azure Cosmos DB](../cosmos-db/index-policy.md)
* [Ta bort data från Azure Cosmos DB automatiskt med hjälp av förfallodatum](../cosmos-db/time-to-live.md)
