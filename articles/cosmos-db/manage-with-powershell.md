---
title: Skapa och hantera Azure Cosmos DB med PowerShell
description: Använd Azure PowerShell för att hantera Azure Cosmos-konton, databaser, behållare och data flöde.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/13/2020
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: 0ae3ff54e1060255913d8155b297c5d412ce345f
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83656299"
---
# <a name="manage-azure-cosmos-db-sql-api-resources-using-powershell"></a>Hantera Azure Cosmos DB SQL API-resurser med hjälp av PowerShell

I följande guide beskrivs hur du använder PowerShell för att skripta och automatisera hanteringen av Azure Cosmos DB-resurser, som konto, databas, container och dataflöde.

> [!NOTE]
> I exemplen i den här artikeln används [AZ. CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) hanterings-cmdletar. Se referens sidan för [AZ. CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) -API för de senaste ändringarna.

För plattforms oberoende hantering av Azure Cosmos DB kan du använda `Az` `Az.CosmosDB` cmdletarna och med [plattforms oberoende PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell), samt [Azure CLI](manage-with-cli.md), [REST API][rp-rest-api]eller [Azure Portal](create-sql-api-dotnet.md#create-account).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Komma igång

Följ anvisningarna i [så här installerar och konfigurerar du Azure PowerShell][powershell-install-configure] för att installera och logga in på ditt Azure-konto i PowerShell.

## <a name="azure-cosmos-accounts"></a>Azure Cosmos-konton

Följande avsnitt visar hur du hanterar Azure Cosmos-kontot, inklusive:

* [Skapa ett Azure Cosmos-konto](#create-account)
* [Uppdatera ett Azure Cosmos-konto](#update-account)
* [Lista alla Azure Cosmos-konton i en prenumeration](#list-accounts)
* [Skaffa ett Azure Cosmos-konto](#get-account)
* [Ta bort ett Azure Cosmos-konto](#delete-account)
* [Uppdatera taggar för ett Azure Cosmos-konto](#update-tags)
* [Lista nycklar för ett Azure Cosmos-konto](#list-keys)
* [Återskapa nycklar för ett Azure Cosmos-konto](#regenerate-keys)
* [Visa en lista med anslutnings strängar för ett Azure Cosmos-konto](#list-connection-strings)
* [Ändra prioritet för redundans för ett Azure Cosmos-konto](#modify-failover-priority)
* [Utlös en manuell redundansväxling för ett Azure Cosmos-konto](#trigger-manual-failover)

### <a name="create-an-azure-cosmos-account"></a><a id="create-account"></a>Skapa ett Azure Cosmos-konto

Det här kommandot skapar ett Azure Cosmos DB Database-konto med [flera regioner][distribute-data-globally], [Automatisk redundans](how-to-manage-database-account.md#automatic-failover) och avbindad-föråldrad [konsekvens princip](consistency-levels.md).

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$locations = @("West US 2", "East US 2")
$accountName = "mycosmosaccount"
$apiKind = "Sql"
$consistencyLevel = "BoundedStaleness"
$maxStalenessInterval = 300
$maxStalenessPrefix = 100000

New-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Location $locations `
    -Name $accountName `
    -ApiKind $apiKind `
    -EnableAutomaticFailover:$true `
    -DefaultConsistencyLevel $consistencyLevel `
    -MaxStalenessIntervalInSeconds $maxStalenessInterval `
    -MaxStalenessPrefix $maxStalenessPrefix
```

* `$resourceGroupName`Den Azure-resurs grupp som Cosmos-kontot ska distribueras till. Det måste redan finnas.
* `$locations`Regionerna för databas kontot, från och med Skriv regionen och ordnas efter redundans.
* `$accountName`Namnet på Azure Cosmos-kontot. Måste vara unik, gemener, innehålla enbart alfanumeriska tecken och-tecken, och mellan 3 och 31 tecken.
* `$apiKind`Den typ av Cosmos-konto som ska skapas. Mer information finns i [API: er i Cosmos DB](introduction.md#develop-applications-on-cosmos-db-using-popular-open-source-software-oss-apis).
* `$consistencyPolicy`, `$maxStalenessInterval` , och `$maxStalenessPrefix` standard konsekvens nivån och inställningarna för Azure Cosmos-kontot. Mer information finns i [konsekvens nivåer i Azure Cosmos DB](consistency-levels.md).

Azure Cosmos-konton kan konfigureras med IP-brandvägg, Virtual Network tjänst slut punkter och privata slut punkter. Information om hur du konfigurerar IP-brandväggen för Azure Cosmos DB finns i [Konfigurera IP-](how-to-configure-firewall.md)brandvägg. Information om hur du aktiverar tjänstens slut punkter för Azure Cosmos DB finns i [Konfigurera åtkomst från virtuella nätverk](how-to-configure-vnet-service-endpoint.md). Information om hur du aktiverar privata slut punkter för Azure Cosmos DB finns i [Konfigurera åtkomst från privata slut punkter](how-to-configure-private-endpoints.md).

### <a name="list-all-azure-cosmos-accounts-in-a-resource-group"></a><a id="list-accounts"></a>Lista alla Azure Cosmos-konton i en resurs grupp

Det här kommandot visar alla Azure Cosmos-konton i en resurs grupp.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"

Get-AzCosmosDBAccount -ResourceGroupName $resourceGroupName
```

### <a name="get-the-properties-of-an-azure-cosmos-account"></a><a id="get-account"></a>Hämta egenskaperna för ett Azure Cosmos-konto

Med det här kommandot kan du hämta egenskaperna för ett befintligt Azure Cosmos-konto.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBAccount -ResourceGroupName $resourceGroupName -Name $accountName
```

### <a name="update-an-azure-cosmos-account"></a><a id="update-account"></a>Uppdatera ett Azure Cosmos-konto

Med det här kommandot kan du uppdatera Azure Cosmos DB databas konto egenskaper. De egenskaper som kan uppdateras är följande:

* Lägga till eller ta bort regioner
* Ändra standard konsekvens princip
* Ändra filter för IP-intervall
* Ändra Virtual Network konfigurationer
* Aktivera flera huvud

> [!NOTE]
> Du kan inte lägga till eller ta bort regioner samtidigt ( `locations` ) och ändra andra egenskaper för ett Azure Cosmos-konto. Att ändra regioner måste utföras som en separat åtgärd från andra ändringar i kontot.
> [!NOTE]
> Med det här kommandot kan du lägga till och ta bort regioner, men du kan inte ändra prioriteter för redundans eller utlösa en manuell redundansväxling. Se [ändra prioritet för redundans](#modify-failover-priority) och [utlösa manuell redundans](#trigger-manual-failover).

```azurepowershell-interactive
# Create account with two regions
$resourceGroupName = "myResourceGroup"
$locations = @("West US 2", "East US 2")
$accountName = "mycosmosaccount"
$apiKind = "Sql"
$consistencyLevel = "Session"
$enableAutomaticFailover = $true

# Create the Cosmos DB account
New-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Location $locations `
    -Name $accountName `
    -ApiKind $apiKind `
    -EnableAutomaticFailover:$enableAutomaticFailover `
    -DefaultConsistencyLevel $consistencyLevel

# Add a region to the account
$locations2 = @("West US 2", "East US 2", "South Central US")
$locationObjects2 = @()
$i = 0
ForEach ($location in $locations2) {
    $locationObjects2 += @{ locationName = "$location"; failoverPriority = $i++ }
}

Update-AzCosmosDBAccountRegion `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -LocationObject $locationObjects2

Write-Host "Update-AzCosmosDBAccountRegion returns before the region update is complete."
Write-Host "Check account in Azure portal or using Get-AzCosmosDBAccount for region status."
Write-Host "When region was added, press any key to continue."
$HOST.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown") | OUT-NULL
$HOST.UI.RawUI.Flushinputbuffer()

# Remove a region from the account
$locations3 = @("West US 2", "South Central US")
$locationObjects3 = @()
$i = 0
ForEach ($location in $locations3) {
    $locationObjects3 += @{ locationName = "$location"; failoverPriority = $i++ }
}

Update-AzCosmosDBAccountRegion `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -LocationObject $locationObjects3

Write-Host "Update-AzCosmosDBAccountRegion returns before the region update is complete."
Write-Host "Check account in Azure portal or using Get-AzCosmosDBAccount for region status."
```
### <a name="enable-multiple-write-regions-for-an-azure-cosmos-account"></a><a id="multi-master"></a>Aktivera flera Skriv regioner för ett Azure Cosmos-konto

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$enableAutomaticFailover = $false
$enableMultiMaster = $true

# First disable automatic failover - cannot have both automatic
# failover and multi-master on an account
Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -EnableAutomaticFailover:$enableAutomaticFailover

# Now enable multi-master
Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -EnableMultipleWriteLocations:$enableMultiMaster
```

### <a name="delete-an-azure-cosmos-account"></a><a id="delete-account"></a>Ta bort ett Azure Cosmos-konto

Det här kommandot tar bort ett befintligt Azure Cosmos-konto.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Remove-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -PassThru:$true
```

### <a name="update-tags-of-an-azure-cosmos-account"></a><a id="update-tags"></a>Uppdatera taggar för ett Azure Cosmos-konto

Det här kommandot anger [Azures resurs Taggar][azure-resource-tags] för ett Azure Cosmos-konto. Taggar kan ställas in både vid skapande av konto `New-AzCosmosDBAccount` och vid uppdatering av kontot med hjälp av `Update-AzCosmosDBAccount` .

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$tags = @{dept = "Finance"; environment = "Production";}

Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -Tag $tags
```

### <a name="list-account-keys"></a><a id="list-keys"></a>Lista konto nycklar

När du skapar ett Azure Cosmos-konto genererar tjänsten två huvud åtkomst nycklar som kan användas för autentisering när Azure Cosmos-kontot nås. Skrivskyddade nycklar för att autentisera skrivskyddade åtgärder skapas också.
Genom att tillhandahålla två åtkomst nycklar kan Azure Cosmos DB skapa och rotera en nyckel i taget utan avbrott i ditt Azure Cosmos-konto.
Cosmos DB konton har två Skriv-och skriv nycklar (primära och sekundära) och två skrivskyddade nycklar (primära och sekundära).

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -Type "Keys"
```

### <a name="list-connection-strings"></a><a id="list-connection-strings"></a>Lista anslutnings strängar

Följande kommando hämtar anslutnings strängar för att ansluta appar till Cosmos DB-kontot.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -Type "ConnectionStrings"
```

### <a name="regenerate-account-keys"></a><a id="regenerate-keys"></a>Återskapa konto nycklar

Åtkomst nycklar till ett Azure Cosmos-konto bör återskapas regelbundet för att hålla anslutningarna säkra. Ett primärt och sekundärt åtkomst nycklar tilldelas till kontot. Detta gör att klienter kan upprätthålla åtkomst medan en nyckel i taget återskapas.
Det finns fyra typer av nycklar för ett Azure Cosmos-konto (primär, sekundär, PrimaryReadonly och SecondaryReadonly)

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "mycosmosaccount" # Must be all lower case
$keyKind = "primary" # Other key kinds: secondary, primaryReadOnly, secondaryReadOnly

New-AzCosmosDBAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -KeyKind $keyKind
```

### <a name="enable-automatic-failover"></a><a id="enable-automatic-failover"></a>Aktivera automatisk redundans

Följande kommando anger ett Cosmos DB-konto att redundansväxla automatiskt till den sekundära regionen om den primära regionen blir otillgänglig.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$enableAutomaticFailover = $true
$enableMultiMaster = $false

# First disable multi-master - cannot have both automatic
# failover and multi-master on an account
Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -EnableMultipleWriteLocations:$enableMultiMaster

# Now enable automatic failover
Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -EnableAutomaticFailover:$enableAutomaticFailover
```

### <a name="modify-failover-priority"></a><a id="modify-failover-priority"></a>Ändra prioritet för redundans

För konton som har kon figurer ATS med automatisk redundans kan du ändra i vilken ordning Cosmos ska befordra sekundära repliker till primär om den primära blir otillgänglig.

I exemplet nedan antar vi att den aktuella växlings prioriteten är `West US 2 = 0` , `East US 2 = 1` , `South Central US = 2` . Kommandot kommer att ändra detta till `West US 2 = 0` , `South Central US = 1` , `East US 2 = 2` .

> [!CAUTION]
> Om du ändrar platsen för `failoverPriority=0` aktive ras en manuell redundansväxling för ett Azure Cosmos-konto. Eventuella andra prioritets ändringar kommer inte att utlösa redundans.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$locations = @("West US 2", "South Central US", "East US 2") # Regions ordered by UPDATED failover priority

Update-AzCosmosDBAccountFailoverPriority `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -FailoverPolicy $locations
```

### <a name="trigger-manual-failover"></a><a id="trigger-manual-failover"></a>Utlös manuell redundans

För konton som kon figurer ATS med manuell redundans kan du redundansväxla och befordra valfri sekundär replik till primär genom att ändra till `failoverPriority=0` . Den här åtgärden kan användas för att starta en haveri beredskap för att testa haveri beredskaps planeringen.

I exemplet nedan antar du att kontot har en aktuell växlings prioritet för `West US 2 = 0` och `East US 2 = 1` och vänder regionerna.

> [!CAUTION]
> Om du ändrar `locationName` för `failoverPriority=0` aktive ras en manuell redundansväxling för ett Azure Cosmos-konto. Alla andra prioritets ändringar kommer inte att utlösa redundans.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$locations = @("East US 2", "West US 2") # Regions ordered by UPDATED failover priority

Update-AzCosmosDBAccountFailoverPriority `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -FailoverPolicy $locations
```

## <a name="azure-cosmos-db-database"></a>Azure Cosmos DB databas

Följande avsnitt visar hur du hanterar Azure Cosmos DB-databasen, inklusive:

* [Skapa en Azure Cosmos DB databas](#create-db)
* [Skapa en Azure Cosmos DB databas med delat data flöde](#create-db-ru)
* [Hämta data flödet för en Azure Cosmos DB databas](#get-db-ru)
* [Visa en lista över alla Azure Cosmos DB databaser i ett konto](#list-db)
* [Hämta en enda Azure Cosmos DB databas](#get-db)
* [Ta bort en Azure Cosmos DB databas](#delete-db)

### <a name="create-an-azure-cosmos-db-database"></a><a id="create-db"></a>Skapa en Azure Cosmos DB databas

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

New-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

### <a name="create-an-azure-cosmos-db-database-with-shared-throughput"></a><a id="create-db-ru"></a>Skapa en Azure Cosmos DB databas med delat data flöde

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$databaseRUs = 400

New-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName `
    -Throughput $databaseRUs
```

### <a name="get-the-throughput-of-an-azure-cosmos-db-database"></a><a id="get-db-ru"></a>Hämta data flödet för en Azure Cosmos DB databas

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Get-AzCosmosDBSqlDatabaseThroughput `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

### <a name="get-all-azure-cosmos-db-databases-in-an-account"></a><a id="list-db"></a>Hämta alla Azure Cosmos DB databaser i ett konto

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName
```

### <a name="get-a-single-azure-cosmos-db-database"></a><a id="get-db"></a>Hämta en enda Azure Cosmos DB databas

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Get-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

### <a name="delete-an-azure-cosmos-db-database"></a><a id="delete-db"></a>Ta bort en Azure Cosmos DB databas

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Remove-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

## <a name="azure-cosmos-db-container"></a>Azure Cosmos DB behållare

Följande avsnitt visar hur du hanterar Azure Cosmos DB container, inklusive:

* [Skapa en Azure Cosmos DB behållare](#create-container)
* [Skapa en Azure Cosmos DB behållare med en stor partitionsnyckel](#create-container-big-pk)
* [Hämta data flödet för en Azure Cosmos DB container](#get-container-ru)
* [Skapa en Azure Cosmos DB-behållare med anpassad indexering](#create-container-custom-index)
* [Skapa en Azure Cosmos DB-behållare med indexering inaktive rad](#create-container-no-index)
* [Skapa en Azure Cosmos DB container med unik nyckel och TTL](#create-container-unique-key-ttl)
* [Skapa en Azure Cosmos DB behållare med konflikt lösning](#create-container-lww)
* [Visa en lista över alla Azure Cosmos DB behållare i en databas](#list-containers)
* [Hämta en enda Azure Cosmos DB behållare i en databas](#get-container)
* [Ta bort en Azure Cosmos DB-behållare](#delete-container)

### <a name="create-an-azure-cosmos-db-container"></a><a id="create-container"></a>Skapa en Azure Cosmos DB behållare

```azurepowershell-interactive
# Create an Azure Cosmos DB container with default indexes and throughput at 400 RU
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath
```

### <a name="create-an-azure-cosmos-db-container-with-a-large-partition-key-size"></a><a id="create-container-big-pk"></a>Skapa en Azure Cosmos DB behållare med en stor nyckel storlek

```azurepowershell-interactive
# Create an Azure Cosmos DB container with a large partition key value (version = 2)
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -PartitionKeyVersion 2
```

### <a name="get-the-throughput-of-an-azure-cosmos-db-container"></a><a id="get-container-ru"></a>Hämta data flödet för en Azure Cosmos DB container

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"

Get-AzCosmosDBSqlContainerThroughput `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName
```

### <a name="create-an-azure-cosmos-db-container-with-custom-index-policy"></a><a id="create-container-custom-index"></a>Skapa en Azure Cosmos DB behållare med en anpassad index princip

```azurepowershell-interactive
# Create a container with a custom indexing policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"
$indexPathIncluded = "/*"
$indexPathExcluded = "/myExcludedPath/*"

$includedPathIndex = New-AzCosmosDBSqlIncludedPathIndex -DataType String -Kind Range
$includedPath = New-AzCosmosDBSqlIncludedPath -Path $indexPathIncluded -Index $includedPathIndex

$indexingPolicy = New-AzCosmosDBSqlIndexingPolicy `
    -IncludedPath $includedPath `
    -ExcludedPath $indexPathExcluded `
    -IndexingMode Consistent `
    -Automatic $true

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -IndexingPolicy $indexingPolicy
```

### <a name="create-an-azure-cosmos-db-container-with-indexing-turned-off"></a><a id="create-container-no-index"></a>Skapa en Azure Cosmos DB-behållare med indexering inaktive rad

```azurepowershell-interactive
# Create an Azure Cosmos DB container with no indexing
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"

$indexingPolicy = New-AzCosmosDBSqlIndexingPolicy `
    -IndexingMode None

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -IndexingPolicy $indexingPolicy
```

### <a name="create-an-azure-cosmos-db-container-with-unique-key-policy-and-ttl"></a><a id="create-container-unique-key-ttl"></a>Skapa en Azure Cosmos DB container med unik nyckel princip och TTL

```azurepowershell-interactive
# Create a container with a unique key policy and TTL of one day
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"
$uniqueKeyPath = "/myUniqueKeyPath"
$ttlInSeconds = 86400 # Set this to -1 (or don't use it at all) to never expire

$uniqueKey = New-AzCosmosDBSqlUniqueKey `
    -Path $uniqueKeyPath

$uniqueKeyPolicy = New-AzCosmosDBSqlUniqueKeyPolicy `
    -UniqueKey $uniqueKey

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -UniqueKeyPolicy $uniqueKeyPolicy `
    -TtlInSeconds $ttlInSeconds
```

### <a name="create-an-azure-cosmos-db-container-with-conflict-resolution"></a><a id="create-container-lww"></a>Skapa en Azure Cosmos DB behållare med konflikt lösning

För att skriva alla konflikter till ConflictsFeed och hantera separat, pass `-Type "Custom" -Path ""` .

```azurepowershell-interactive
# Create container with last-writer-wins conflict resolution policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"
$conflictResolutionPath = "/myResolutionPath"

$conflictResolutionPolicy = New-AzCosmosDBSqlConflictResolutionPolicy `
    -Type LastWriterWins `
    -Path $conflictResolutionPath

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -ConflictResolutionPolicy $conflictResolutionPolicy
```

Om du vill skapa en konflikt lösnings princip för att använda en lagrad procedur, anropa `New-AzCosmosDBSqlConflictResolutionPolicy` och skicka parametrar `-Type` och `-ConflictResolutionProcedure` .

```azurepowershell-interactive
# Create container with custom conflict resolution policy using a stored procedure
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"
$conflictResolutionSprocName = "mysproc"

$conflictResolutionSproc = "/dbs/$databaseName/colls/$containerName/sprocs/$conflictResolutionSprocName"

$conflictResolutionPolicy = New-AzCosmosDBSqlConflictResolutionPolicy `
    -Type Custom `
    -ConflictResolutionProcedure $conflictResolutionSproc

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -ConflictResolutionPolicy $conflictResolutionPolicy
```


### <a name="list-all-azure-cosmos-db-containers-in-a-database"></a><a id="list-containers"></a>Visa en lista över alla Azure Cosmos DB behållare i en databas

```azurepowershell-interactive
# List all Azure Cosmos DB containers in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Get-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName
```

### <a name="get-a-single-azure-cosmos-db-container-in-a-database"></a><a id="get-container"></a>Hämta en enda Azure Cosmos DB behållare i en databas

```azurepowershell-interactive
# Get a single Azure Cosmos DB container in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"

Get-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName
```

### <a name="delete-an-azure-cosmos-db-container"></a><a id="delete-container"></a>Ta bort en Azure Cosmos DB-behållare

```azurepowershell-interactive
# Delete an Azure Cosmos DB container
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"

Remove-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName
```

## <a name="next-steps"></a>Nästa steg

* [Alla PowerShell-exempel](powershell-samples.md)
* [Hantera Azure Cosmos-konto](how-to-manage-database-account.md)
* [Skapa en Azure Cosmos DB behållare](how-to-create-container.md)
* [Konfigurera Time-to-Live i Azure Cosmos DB](how-to-time-to-live.md)

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/
