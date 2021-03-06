---
title: Azure PowerShell exempel för API för Azure Cosmos DB-MongoDB
description: Hämta de Azure PowerShell exemplen för att utföra olika vanliga uppgifter i Azure Cosmos DBs API för MongoDB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/13/2020
ms.author: mjbrown
ms.openlocfilehash: c8e0a7a60a3512d19a1dfdfdb07b20e523ce7b92
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649710"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-mongodb-api"></a>Azure PowerShell exempel för API för Azure Cosmos DB MongoDB

Följande tabell innehåller länkar till exempel Azure PowerShell skript för Azure Cosmos DB för MongoDB-API: et.

> [!NOTE]
> För närvarande kan du bara skapa 3,2-versionen (det vill säga konton som använder slut punkten i formatet `*.documents.azure.com` ) för Azure Cosmos DBS API för MongoDB-konton med hjälp av PowerShell-, CLI-och Resource Manager-mallar. Om du vill skapa en 3,6-version av konton använder du i stället Azure Portal.

> [!NOTE]
> I exemplen används [AZ. CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) -hanterings-cmdletar. Sök efter uppdateringar `Az.CosmosDB` regelbundet.

| | |
|---|---|
|[Skapa ett konto, en databas och en samling](scripts/powershell/mongodb/ps-mongodb-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Skapar ett Azure Cosmos-konto, en databas och en samling. |
|[Lista eller hämta databaser eller samlingar](scripts/powershell/mongodb/ps-mongodb-list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Lista eller hämta databas eller samling. |
|[Hämta RU/s](scripts/powershell/mongodb/ps-mongodb-ru-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hämta RU/s för en databas eller samling. |
|[Uppdatera RU/s](scripts/powershell/mongodb/ps-mongodb-ru-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Uppdatera RU/s för en databas eller samling. |
|[Uppdatera ett konto eller Lägg till en region](scripts/powershell/common/ps-account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Lägg till en region i ett Cosmos-konto. Kan också användas för att ändra andra konto egenskaper, men de måste vara åtskilda från ändringar i regioner. |
|[Ändra växlings prioritet eller utlös redundans](scripts/powershell/common/ps-account-failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Ändra den regionala växlings prioriteten för ett Azure Cosmos-konto eller utlösa en manuell redundansväxling. |
|[Konto nycklar eller anslutnings strängar](scripts/powershell/common/ps-account-keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hämta primära och sekundära nycklar, anslutnings strängar eller återskapa en konto nyckel för ett Azure Cosmos-konto. |
|[Skapa ett Cosmos-konto med IP-brandvägg](scripts/powershell/common/ps-account-firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Skapa ett Azure Cosmos-konto med IP-brandvägg aktive rad. |
|||
