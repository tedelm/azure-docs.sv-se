---
title: Begränsningar – Azure Database for MariaDB
description: I den här artikeln beskrivs begränsningar i Azure Database for MariaDB, till exempel antalet anslutnings-och lagrings motor alternativ.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 4/1/2020
ms.openlocfilehash: d4450689f6865c19436e437e09a3aa9f286c6e21
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83653132"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>Begränsningar i Azure Database for MariaDB
I följande avsnitt beskrivs kapacitet, stöd för lagrings motor, stöd för stöd för data manipulation och funktionella gränser i databas tjänsten.

## <a name="server-parameters"></a>Serverparametrar

De lägsta och högsta värdena för flera populära Server parametrar bestäms av pris nivån och virtuella kärnor. Se de följande tabellerna som gränser.

### <a name="max_connections"></a>max_connections

|**Pris nivå**|**vCore (s)**|**Standardvärde**|**Minsta värde**|**Max värde**|
|---|---|---|---|---|
|Basic|1|50|10|50|
|Basic|2|100|10|100|
|Generell användning|2|300|10|600|
|Generell användning|4|625|10|1250|
|Generell användning|8|1250|10|2500|
|Generell användning|16|2500|10|5000|
|Generell användning|32|5000|10|10000|
|Generell användning|64|10000|10|20000|
|Minnesoptimerad|2|600|10|800|
|Minnesoptimerad|4|1250|10|2500|
|Minnesoptimerad|8|2500|10|5000|
|Minnesoptimerad|16|5000|10|10000|
|Minnesoptimerad|32|10000|10|20000|

När anslutningar överskrider gränsen kan följande fel meddelande visas:
> FEL 1040 (08004): för många anslutningar

> [!IMPORTANT]
> För bästa möjliga upplevelse rekommenderar vi att du använder en anslutningspool som ProxySQL för att effektivt hantera anslutningar.

Att skapa nya klient anslutningar till MariaDB tar tid och när de har upprättats, tar dessa anslutningar med databas resurser, även när de är inaktiva. De flesta program begär många anslutningar för kort period, vilket utvärderar den här situationen. Resultatet är färre resurser som är tillgängliga för den faktiska arbets belastningen, vilket leder till försämrade prestanda. En anslutningspool som minskar inaktiva anslutningar och återanvänder befintliga anslutningar hjälper till att undvika detta. Mer information om hur du konfigurerar ProxySQL finns i vårt [blogg inlägg](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/load-balance-read-replicas-using-proxysql-in-azure-database-for/ba-p/880042).

### <a name="query_cache_size"></a>query_cache_size

Frågesyntaxen är inaktive rad som standard. Konfigurera-parametern om du vill aktivera Query Cache `query_cache_type` . 

Läs mer om den här parametern i [MariaDB-dokumentationen](https://mariadb.com/kb/en/server-system-variables/#query_cache_size) .

|**Pris nivå**|**vCore (s)**|**Standardvärde**|**Minsta värde**|**Max värde**|
|---|---|---|---|---|
|Basic|1|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Basic|2|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Generell användning|2|0|0|16777216|
|Generell användning|4|0|0|33554432|
|Generell användning|8|0|0|67108864|
|Generell användning|16|0|0|134217728|
|Generell användning|32|0|0|134217728|
|Generell användning|64|0|0|134217728|
|Minnesoptimerad|2|0|0|33554432|
|Minnesoptimerad|4|0|0|67108864|
|Minnesoptimerad|8|0|0|134217728|
|Minnesoptimerad|16|0|0|134217728|
|Minnesoptimerad|32|0|0|134217728|

### <a name="sort_buffer_size"></a>sort_buffer_size

Läs mer om den här parametern i [MariaDB-dokumentationen](https://mariadb.com/kb/en/server-system-variables/#sort_buffer_size) .

|**Pris nivå**|**vCore (s)**|**Standardvärde**|**Minsta värde**|**Max värde**|
|---|---|---|---|---|
|Basic|1|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Basic|2|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Generell användning|2|524288|32768|4194304|
|Generell användning|4|524288|32768|8388608|
|Generell användning|8|524288|32768|16777216|
|Generell användning|16|524288|32768|33554432|
|Generell användning|32|524288|32768|33554432|
|Generell användning|64|524288|32768|33554432|
|Minnesoptimerad|2|524288|32768|8388608|
|Minnesoptimerad|4|524288|32768|16777216|
|Minnesoptimerad|8|524288|32768|33554432|
|Minnesoptimerad|16|524288|32768|33554432|
|Minnesoptimerad|32|524288|32768|33554432|

### <a name="join_buffer_size"></a>join_buffer_size

Läs mer om den här parametern i [MariaDB-dokumentationen](https://mariadb.com/kb/en/server-system-variables/#join_buffer_size) .

|**Pris nivå**|**vCore (s)**|**Standardvärde**|**Minsta värde**|**Max värde**|
|---|---|---|---|---|
|Basic|1|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Basic|2|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Generell användning|2|262144|128|268435455|
|Generell användning|4|262144|128|536870912|
|Generell användning|8|262144|128|1073741824|
|Generell användning|16|262144|128|2147483648|
|Generell användning|32|262144|128|4294967295|
|Generell användning|64|262144|128|4294967295|
|Minnesoptimerad|2|262144|128|536870912|
|Minnesoptimerad|4|262144|128|1073741824|
|Minnesoptimerad|8|262144|128|2147483648|
|Minnesoptimerad|16|262144|128|4294967295|
|Minnesoptimerad|32|262144|128|4294967295|

### <a name="max_heap_table_size"></a>max_heap_table_size

Läs mer om den här parametern i [MariaDB-dokumentationen](https://mariadb.com/kb/en/server-system-variables/#max_heap_table_size) .

|**Pris nivå**|**vCore (s)**|**Standardvärde**|**Minsta värde**|**Max värde**|
|---|---|---|---|---|
|Basic|1|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Basic|2|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Generell användning|2|16777216|16384|268435455|
|Generell användning|4|16777216|16384|536870912|
|Generell användning|8|16777216|16384|1073741824|
|Generell användning|16|16777216|16384|2147483648|
|Generell användning|32|16777216|16384|4294967295|
|Generell användning|64|16777216|16384|4294967295|
|Minnesoptimerad|2|16777216|16384|536870912|
|Minnesoptimerad|4|16777216|16384|1073741824|
|Minnesoptimerad|8|16777216|16384|2147483648|
|Minnesoptimerad|16|16777216|16384|4294967295|
|Minnesoptimerad|32|16777216|16384|4294967295|

### <a name="tmp_table_size"></a>tmp_table_size

Läs mer om den här parametern i [MariaDB-dokumentationen](https://mariadb.com/kb/en/server-system-variables/#tmp_table_size) .

|**Pris nivå**|**vCore (s)**|**Standardvärde**|**Minsta värde**|**Max värde**|
|---|---|---|---|---|
|Basic|1|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Basic|2|Kan inte konfigureras på Basic-nivå|Ej tillämpligt|Ej tillämpligt|
|Generell användning|2|16777216|1024|67108864|
|Generell användning|4|16777216|1024|134217728|
|Generell användning|8|16777216|1024|268435456|
|Generell användning|16|16777216|1024|536870912|
|Generell användning|32|16777216|1024|1073741824|
|Generell användning|64|16777216|1024|1073741824|
|Minnesoptimerad|2|16777216|1024|134217728|
|Minnesoptimerad|4|16777216|1024|268435456|
|Minnesoptimerad|8|16777216|1024|536870912|
|Minnesoptimerad|16|16777216|1024|1073741824|
|Minnesoptimerad|32|16777216|1024|1073741824|

### <a name="time_zone"></a>time_zone

Du kan fylla i tids zons tabellerna genom att anropa den `mysql.az_load_timezone` lagrade proceduren från ett verktyg som MySQL kommando rad eller MySQL Workbench. Mer information om hur du anropar den lagrade proceduren finns i [Azure Portal](howto-server-parameters.md#working-with-the-time-zone-parameter) -eller [Azure CLI](howto-configure-server-parameters-cli.md#working-with-the-time-zone-parameter) -artiklarna och ställer in globala eller sessionsbaserade tids zoner.

### <a name="innodb_file_per_table"></a>innodb_file_per_table

MariaDB lagrar InnoDB-tabellen i olika tabell utrymmen baserat på den konfiguration du angav när tabellen skapades. [Systemets tabell utrymme](https://mariadb.com/kb/en/innodb-system-tablespaces/) är lagrings utrymmet för data ord listan InnoDB. Ett tabell namn för en [fil per tabell](https://mariadb.com/kb/en/innodb-file-per-table-tablespaces/) innehåller data och index för en enskild InnoDB-tabell och lagras i fil systemet i en egen datafil. Detta beteende styrs av `innodb_file_per_table` Server parametern. Inställningen `innodb_file_per_table` `OFF` gör att InnoDB skapar tabeller i System register utrymmet. Annars skapar InnoDB tabeller i tabell utrymmen per tabell.

Azure Database for MariaDB stöder högst **1 TB**i en enskild datafil. Om databas storleken är större än 1 TB bör du skapa tabellen i [innodb_file_per_table](https://mariadb.com/kb/en/innodb-system-variables/#innodb_file_per_table) tabell utrymme. Om du har en enskild tabell storlek som är större än 1 TB bör du använda partitionstabellen.

## <a name="storage-engine-support"></a>Stöd för lagrings motor

### <a name="supported"></a>Stöds
- [InnoDB](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [MINNESOPTIMERADE](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>Stöd saknas
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [BLACKHOLE](https://mariadb.com/kb/en/library/blackhole/)
- [ARKIVATTRIBUTET](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Behörighets stöd

### <a name="unsupported"></a>Stöd saknas
- DBA-roll: många Server parametrar och inställningar kan oavsiktligt försämra serverns prestanda eller negera syre egenskaper i DBMS. För att upprätthålla tjänste integriteten och service avtalet på en produkt nivå exponerar inte den här tjänsten DBA-rollen. Standard användar kontot, som skapas när en ny databas instans skapas, gör att användaren kan utföra de flesta DDL-och DML-instruktioner i den hanterade databas instansen.
- SUPER Privilege: liknande [superbehörighet](https://mariadb.com/kb/en/library/grant/#global-privileges) är också begränsad.
- Avrundning: kräver Super-behörighet för att skapa och är begränsad. Om du importerar data med hjälp av en säkerhets kopia tar du bort `CREATE DEFINER` kommandona manuellt eller genom att använda `--skip-definer` kommandot när du utför en mysqldump.

## <a name="data-manipulation-statement-support"></a>Stöd för data manipulations sats

### <a name="supported"></a>Stöds
- `LOAD DATA INFILE`stöds, men `[LOCAL]` parametern måste anges och dirigeras till en UNC-sökväg (Azure Storage monteras via SMB).

### <a name="unsupported"></a>Stöd saknas
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Funktionella begränsningar

### <a name="scale-operations"></a>Skalnings åtgärder
- Dynamisk skalning till och från de grundläggande pris nivåerna stöds inte för närvarande.
- Det finns inte stöd för att minska lagrings storleken för servern.

### <a name="server-version-upgrades"></a>Uppgraderingar av Server version
- Automatisk migrering mellan huvud versioner av databas motorn stöds inte för närvarande.

### <a name="point-in-time-restore"></a>Återställning till tidpunkt
- När du använder funktionen PITR skapas den nya servern med samma konfigurationer som den server den baseras på.
- Det finns inte stöd för att återställa en borttagen Server.

### <a name="subscription-management"></a>Prenumerationshantering
- Det finns för närvarande inte stöd för att flytta tidigare skapade servrar över prenumerationen och resurs gruppen.

### <a name="vnet-service-endpoints"></a>VNet-tjänstslutpunkter
- Stöd för VNet-tjänstens slut punkter är bara för Generell användning och minnesoptimerade servrar.

### <a name="storage-size"></a>Lagrings storlek
- Se [pris nivåer](concepts-pricing-tiers.md) för lagrings storleks gränser per pris nivå.

## <a name="current-known-issues"></a>Aktuella kända problem
- MariaDB Server-instans visar fel Server version när anslutningen har upprättats. Använd kommandot för att få rätt version av Server instans motorn `select version();` .

## <a name="next-steps"></a>Nästa steg
- [Vad som är tillgängligt i varje tjänst nivå](concepts-pricing-tiers.md)
- [MariaDB databas versioner som stöds](concepts-supported-versions.md)
