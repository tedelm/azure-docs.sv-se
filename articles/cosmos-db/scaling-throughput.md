---
title: Skala data flöde i Azure Cosmos DB
description: I den här artikeln beskrivs hur Azure Cosmos DB skalar data flödet i olika regioner där Azure Cosmos-kontot har tillhandahållits.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: 440f23afcd08326261be30432ad1f0ecb16f55fd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "74873513"
---
# <a name="globally-scale-provisioned-throughput"></a>Skala etablerat dataflöde globalt 

I Azure Cosmos DB representeras det etablerade data flödet som begär ande enheter per sekund (RU/s eller plural form ru: er). Ru: er mäter kostnaden för både Läs-och skriv åtgärder mot din Cosmos-behållare som visas i följande bild:

![Enheter för programbegäran](./media/scaling-throughput/request-unit-charge-of-read-and-write-operations.png)

Du kan etablera ru: er på en Cosmos-behållare eller en Cosmos-databas. Ru: er som har allokerats på en behållare är exklusivt tillgängliga för de åtgärder som utförs på behållaren. Ru: er som har allokerats på en databas delas av alla behållare i databasen (utom för behållare med exklusivt tilldelade ru: er).

För elastisk skalning av allokerat data flöde kan du när som helst öka eller minska de etablerade RU/s. Mer information finns i [How-to-](set-throughput.md) Restore data flöde och för elastisk skalning av Cosmos-behållare och databaser. För globalt skalnings data flöde kan du när som helst lägga till eller ta bort regioner från ditt Cosmos-konto. Mer information finns i [Lägg till/ta bort regioner från ditt databas konto](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Att associera flera regioner med ett Cosmos-konto är viktigt i många scenarion – för att uppnå låg latens och [hög tillgänglighet](high-availability.md) runtom i världen.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>Hur det etablerade data flödet distribueras mellan regioner

Om du etablerar *r* -ru: er på en Cosmos-behållare (eller databas) säkerställer Cosmos DB att *R* -ru: er är tillgängliga i *varje* region som är kopplad till ditt Cosmos-konto. Varje gången du lägger till en ny region i ditt konto, Cosmos DB automatiskt *"R"* -ru: er i den nyligen tillagda regionen. De åtgärder som utförs mot din Cosmos-behållare garanterar att du kan hämta *R* -ru: er i varje region. Du kan inte selektivt tilldela ru: er till en speciell region. Ru: er som har tillhandahållits på en Cosmos-behållare (eller databas) är etablerade i alla regioner som är kopplade till ditt Cosmos-konto.

Förutsatt att en Cosmos-behållare har kon figurer ATS med *"R"* -ru: er och det finns *' N '* regioner kopplade till Cosmos-kontot, så:

- Om Cosmos-kontot har kon figurer ATS med en enda Skriv region, är det totala ru: er tillgängligt globalt på behållaren = *R* x *N*.

- Om Cosmos-kontot har kon figurer ATS med flera Skriv regioner, är den totala ru: er tillgänglig globalt på behållaren = *R* x (*N*+ 1). Ytterligare *R* -ru: er tillhandahålls automatiskt för att bearbeta uppdaterings konflikter och trafikentropi trafik i regionerna.

Ditt val av [konsekvens modell](consistency-levels.md) påverkar också data flödet. Du kan få ungefär dubbelt dubbelt läsnings data flöde för mer avslappnad konsekvens nivåer (t. ex. *session*, *konsekvent prefix* och *eventuell* konsekvens) jämfört med starkare konsekvens nivåer (t. ex. *avgränsad föråldrad* eller *stark* konsekvens).

## <a name="next-steps"></a>Nästa steg

Härnäst kan du lära dig hur du konfigurerar data flödet på en behållare eller databas:

* [Hämta och ange data flöde för behållare och databaser](set-throughput.md) 

