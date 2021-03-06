---
title: Långsiktig kvarhållning av säkerhetskopior
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Läs om hur Azure SQL Database-och SQL-hanterad instans stöder lagring av fullständiga säkerhets kopior av databaser i upp till 10 år via principen för långsiktig kvarhållning.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 05/18/2019
ms.openlocfilehash: 0e562b92db16456956ff2fe1cbec0f1addde87ef
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84048289"
---
# <a name="long-term-retention---azure-sql-database--sql-managed-instance"></a>Långsiktig kvarhållning – Azure SQL Database & SQL-hanterad instans
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Många program har regler, efterlevnad eller andra affärs behov som kräver att du bevarar databas säkerhets kopior över 7-35 dagar från Azure SQL Database och [Automatisk säkerhets kopiering](automated-backups-overview.md)av Azure SQL-hanterade instanser. Med hjälp av funktionen för långsiktig kvarhållning (brv) kan du lagra angivna SQL Database-och SQL-hanterade instanser med fullständiga säkerhets kopieringar i Azure Blob Storage med Read-Access Geo-redundant lagring i upp till 10 år. Du kan sedan återställa en säkerhets kopia som en ny databas. Mer information om Azure Storage redundans finns [Azure Storage redundans](../../storage/common/storage-redundancy.md). 

Lång tids kvarhållning kan aktive ras för Azure SQL Database och är i begränsad offentlig för hands version för Azure SQL-hanterad instans. Den här artikeln innehåller en konceptuell översikt över långsiktig kvarhållning. Information om hur du konfigurerar långsiktig kvarhållning finns i [konfigurera Azure SQL Database LTR](long-term-backup-retention-configure.md) och [Konfigurera Azure SQL Managed instance LTR](../managed-instance/long-term-backup-retention-configure.md). 

> [!NOTE]
> Du kan använda SQL Agent-jobb för att schemalägga [säkerhets kopiering av skrivskyddade databaser](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) som ett alternativ till vä-hö över 35 dagar.


## <a name="how-long-term-retention-works"></a>Så här fungerar långsiktig kvarhållning
     
Långsiktig kvarhållning av säkerhets kopior (brv) utnyttjar de fullständiga säkerhets kopiorna av databasen som [skapas automatiskt](automated-backups-overview.md) för att aktivera punkt-tids återställning (PITR). Om en LTR-princip har kon figurer ATS kopieras dessa säkerhets kopior till olika blobbar för långsiktig lagring. Kopieringen är ett bakgrunds jobb som inte påverkar databasens arbets belastning. Principen för vä-hö för varje SQL-databas kan också ange hur ofta säkerhets kopieringarna ska skapas.

Om du vill aktivera vä-hö kan du definiera en princip med en kombination av fyra parametrar: veckovis kvarhållning av säkerhets kopior (W), månatlig kvarhållning av säkerhets kopior (M), årlig kvarhållning av säkerhets kopior (Y) och vecka på år (WeekOfYear). Om du anger a kommer en säkerhets kopia varje vecka att kopieras till långsiktig lagring. Om du anger M kommer den första säkerhets kopian av varje månad att kopieras till långsiktig lagring. Om du anger Y kopieras en säkerhets kopia under den vecka som anges av WeekOfYear till långsiktig lagring. Om den angivna WeekOfYear infaller efter att principen har kon figurer ATS skapas den första säkerhets kopian för LTR under följande år. Varje säkerhets kopia sparas i långtids lagringen enligt de princip parametrar som konfigureras när säkerhets kopian för LTR skapas.

> [!NOTE]
> Alla ändringar i LTR-principen gäller endast för framtida säkerhets kopieringar. Om till exempel veckovis kvarhållning av säkerhets kopior (W), månatlig kvarhållning av säkerhets kopior (M) eller årlig kvarhållning av säkerhets kopior (Y) ändras, kommer den nya inställningen för kvarhållning endast att gälla för nya säkerhets kopior. Kvarhållning av befintliga säkerhets kopior kommer inte att ändras. Om din avsikt är att ta bort gamla LTR-säkerhetskopieringar innan deras kvarhållningsperiod går ut, måste du [ta bort säkerhets kopiorna manuellt](https://docs.microsoft.com/azure/sql-database/sql-database-long-term-backup-retention-configure#delete-ltr-backups).
> 

Exempel på LTR-principen:

-  W = 0, M = 0, Y = 5, WeekOfYear = 3

   Den tredje fullständiga säkerhets kopian av varje år sparas i fem år.
   
- W = 0, M = 3, Y = 0

   Den första fullständiga säkerhets kopian av varje månad sparas i tre månader.

- W = 12, M = 0, Y = 0

   Varje veckovis fullständig säkerhets kopiering sparas i 12 veckor.

- W = 6, M = 12, Y = 10, WeekOfYear = 16

   Varje veckovis fullständig säkerhets kopiering sparas i sex veckor. Förutom första fullständiga säkerhets kopiering av varje månad, som sparas i 12 månader. Förutom den fullständiga säkerhets kopieringen som gjorts den sextonde veckan på året, som sparas i 10 år. 

Följande tabell visar takt och förfallo datum för de långsiktiga säkerhets kopiorna för följande princip:

W = 12 veckor (84 dagar), M = 12 månader (365 dagar), Y = 10 år (3650 dagar), WeekOfYear = 15 (vecka efter 15 april)

   ![vä-exempel](./media/long-term-retention-overview/ltr-example.png)


Om du ändrar principen ovan och anger W = 0 (inga vecko Visa säkerhets kopior), kommer takt för säkerhets kopior att ändras så som visas i tabellen ovan med de markerade datumen. Det lagrings utrymme som krävs för att behålla säkerhets kopiorna minskar därför. 

> [!IMPORTANT]
> Tiden för enskilda säkerhets kopieringar i LTR styrs av Azure. Du kan inte manuellt skapa en LTR-säkerhetskopiering eller kontrol lera tiden för att skapa säkerhets kopior. När du har konfigurerat en LTR-princip kan det ta upp till sju dagar innan den första säkerhets kopian i LTR visas i listan över tillgängliga säkerhets kopior.  


## <a name="geo-replication-and-long-term-backup-retention"></a>Kvarhållning av geo-replikering och långsiktig säkerhets kopiering

Om du använder aktiv geo-replikering eller failover-grupper som verksamhets kontinuitets lösning bör du förbereda för eventuell redundans och konfigurera samma LTR-princip på den sekundära databasen eller instansen. Din LTR Storage-kostnad ökar inte när säkerhets kopiering inte genereras från sekundärerna. Säkerhets kopiorna skapas endast när den sekundära blir primär kommer säkerhets kopiorna att skapas. Den säkerställer icke-avbruten generering av säkerhets kopian av LTR När redundansväxlingen utlöses och den primära flyttas till den sekundära regionen. 

> [!NOTE]
> När den ursprungliga primära databasen återställs från ett avbrott som orsakade redundansväxlingen, blir det en ny sekundär. Därför återupptas inte skapandet av säkerhets kopieringen och den befintliga LTR-principen börjar inte gälla förrän den blir primär igen. 

## <a name="managed-instance-support"></a>Stöd för hanterade instanser

Att använda långsiktig kvarhållning av säkerhets kopior med en hanterad Azure SQL-instans har följande begränsningar:

- **Begränsad offentlig för hands version** – den här för hands versionen är endast tillgänglig för EA-och CSP-prenumerationer och har begränsad tillgänglighet.  
- [**Endast PowerShell**](../managed-instance/long-term-backup-retention-configure.md) – det finns för närvarande inget stöd för Azure Portal. VÄ-hö måste vara aktive rad med PowerShell. 

Om du vill begära registrering skapar du ett [support ärende för Azure](https://azure.microsoft.com/support/create-ticket/). Välj tekniskt problem för tjänsten Välj SQL Database Hanterad instans och för problem typen väljer du **säkerhets kopiering, återställning och affärs kontinuitet/långsiktig kvarhållning av säkerhets kopior**. I din begäran kan du ange det tillstånd som du vill ska registreras i begränsad offentlig för hands version av LTR för hanterad instans.

## <a name="configure-long-term-backup-retention"></a>Konfigurera långsiktig kvarhållning av säkerhetskopior

Du kan konfigurera långsiktig kvarhållning av säkerhets kopior med hjälp av Azure Portal och PowerShell för en Azure SQL Database, och PowerShell för en Azure SQL-hanterad instans. Om du vill återställa en databas från den lokala lagrings platsen kan du välja en speciell säkerhets kopia baserat på dess tidsstämpel. Databasen kan återställas till en befintlig server eller hanterad instans under samma prenumeration som den ursprungliga databasen.

Information om hur du konfigurerar långsiktig kvarhållning eller återställer en databas från en säkerhets kopia för en Azure SQL Database med hjälp av Azure Portal eller PowerShell finns i [hantera Azure SQL Database långsiktig kvarhållning av säkerhets kopior](long-term-backup-retention-configure.md)

Information om hur du konfigurerar långsiktig kvarhållning eller återställer en databas från en säkerhets kopia för en Azure SQL-hanterad instans med hjälp av PowerShell finns i [Hantera Azure SQL Managed instance kvarhållning av säkerhets kopior](../managed-instance/long-term-backup-retention-configure.md). 

Om du vill återställa en databas från den lokala lagrings platsen kan du välja en speciell säkerhets kopia baserat på dess tidsstämpel. Databasen kan återställas till en befintlig server under samma prenumeration som den ursprungliga databasen. Information om hur du återställer databasen från en LTR-säkerhetskopiering med hjälp av Azure Portal eller PowerShell finns i [hantera Azure SQL Database långsiktig kvarhållning av säkerhets kopior](long-term-backup-retention-configure.md). I din begäran kan du ange det tillstånd som du vill ska registreras i den begränsade offentliga för hands versionen av LTR för hanterad instans.

## <a name="next-steps"></a>Nästa steg

Eftersom säkerhets kopiering av databasen skyddar data från oavsiktlig skada eller borttagning, är de en viktig del av all affärs kontinuitet och katastrof återställnings strategi. Mer information om andra SQL Database lösningar för företags kontinuitet finns i [Översikt över affärs kontinuitet](business-continuity-high-availability-disaster-recover-hadr-overview.md).
