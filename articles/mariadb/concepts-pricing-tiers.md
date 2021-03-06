---
title: Prisnivåer – Azure Database for MariaDB
description: Lär dig mer om de olika pris nivåerna för Azure Database for MariaDB inklusive Compute-generationer, lagrings typer, lagrings storlek, virtuella kärnor, minne och kvarhållning av säkerhets kopior.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: f00d93a639bacd1d0862fed7b6b003302bb2920e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82097667"
---
# <a name="azure-database-for-mariadb-pricing-tiers"></a>Azure Database for MariaDB pris nivåer

Du kan skapa en Azure Database for MariaDB-server på en av tre olika pris nivåer: Basic, Generell användning och Minnesoptimerade. Pris nivåerna åtskiljs av mängden data bearbetning i virtuella kärnor som kan tillhandahållas, minne per vCore och lagrings teknik som används för att lagra data. Alla resurser har allokerats på MariaDB server nivå. En server kan ha en eller flera databaser.

|    | **Basic** | **Generell användning** | **Minnesoptimerade** |
|:---|:----------|:--------------------|:---------------------|
| Beräknings generation | Gen 5 |Gen 5 | Gen 5 |
| Virtuella kärnor | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| Minne per vCore | 2 GB | 5 GB | 10 GB |
| Lagrings storlek | 5 GB till 1 TB | 5 GB till 4 TB | 5 GB till 4 TB |
| Kvarhållningsperiod för databas säkerhets kopior | 7 till 35 dagar | 7 till 35 dagar | 7 till 35 dagar |

Om du vill välja en pris nivå använder du följande tabell som utgångs punkt.

| Prisnivå | Målbelastningar |
|:-------------|:-----------------|
| Basic | Arbets belastningar som kräver ljus beräkning och I/O-prestanda. Exempel på detta är servrar som används för utveckling eller testning eller småskaliga program som inte används ofta. |
| Generell användning | De flesta företags arbets belastningar som kräver balanserade beräkning och minne med skalbart I/O-genomflöde. Exempel på det är servrar som är värdar för webb-och mobilappar och andra företags program.|
| Minnesoptimerad | Databas arbets belastningar med höga prestanda som kräver minnes prestanda för snabbare bearbetning av transaktioner och högre samtidighet. Exempel på detta är servrar för bearbetning av real tids data och högpresterande transaktionella eller analytiska appar.|

När du har skapat en server kan du ändra antalet virtuella kärnor och pris nivån (förutom till och från Basic) på några sekunder. Du kan också självständigt justera mängden lagrings utrymme och säkerhets kopierings perioden upp eller ned utan avbrott i programmet. Du kan inte ändra lagrings typen för säkerhets kopia när en server har skapats. Mer information finns i avsnittet [Scale Resources](#scale-resources) .

## <a name="compute-generations-and-vcores"></a>Beräkna generationer och virtuella kärnor

Beräknings resurser tillhandahålls som virtuella kärnor, som representerar den underliggande maskin varans logiska processor. Logiska generation 5-processorer baseras på Intel E5-2673 v4 (Broadwell) 2,3-GHz-processorer.

## <a name="storage"></a>Storage

Lagrings utrymmet du tillhandahåller är mängden lagrings kapacitet som är tillgänglig för din Azure Database for MariaDB-Server. Lagrings utrymmet används för databasfilerna, temporära filer, transaktions loggar och MariaDB-Server loggarna. Den totala mängden lagrings utrymme som du tillhandahåller definierar också den I/O-kapacitet som är tillgänglig för servern.

|    | **Basic** | **Generell användning** | **Minnesoptimerade** |
|:---|:----------|:--------------------|:---------------------|
| Lagringstyp | Basic Storage | Generell användning lagring | Generell användning lagring |
| Lagrings storlek | 5 GB till 1 TB | 5 GB till 4 TB | 5 GB till 4 TB |
| Öknings storlek för lagring | 1 GB | 1 GB | 1 GB |
| IOPS | Variabel |3 IOPS/GB<br/>Min 100 IOPS<br/>Max 6000 IOPS | 3 IOPS/GB<br/>Min 100 IOPS<br/>Max 6000 IOPS |

Du kan lägga till ytterligare lagrings kapacitet under och efter att servern har skapats och göra det möjligt för systemet att växa lagring automatiskt baserat på lagrings förbrukningen för din arbets belastning.

>[!NOTE]
> Lagringen kan bara skalas upp, inte nedåt.

Basic-nivån ger ingen IOPS-garanti. På den Generell användning och minnesoptimerade pris nivån, skalar IOPS med den tillhandahållna lagrings storleken i ett 3:1-förhållande.

Du kan övervaka i/O-förbrukningen i Azure Portal eller genom att använda Azure CLI-kommandon. De relevanta måtten för övervakning är [lagrings gränser, lagrings utrymme, lagring och i/o procent](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Nått lagrings gränsen

Servrar med mindre än lika med 100 GB allokerat lagrings utrymme markeras som skrivskyddade om det lediga lagrings utrymmet är mindre än 5% av den allokerade lagrings storleken. Servrar med mer än 100 GB allokerat lagringsutrymme markeras bara som skrivskyddade när det lediga lagringsutrymmet är mindre än 5 GB.

Om du till exempel har allokerat 110 GB lagrings utrymme och den faktiska användningen går över 105 GB, är servern markerad som skrivskyddad. Alternativt, om du har allokerat 5 GB lagring, markeras servern som skrivskyddad när den kostnads fria lagrings platsen når mindre än 256 MB.

När tjänsten försöker göra så att servern blir skrivskyddad blockeras alla nya skrivtransaktionsbegäranden och befintliga aktiva transaktioner fortsätter att köras. När servern är i skrivskyddat läge misslyckas alla efterföljande skrivåtgärder och transaktioner. Läsfrågor fortsätter att fungera utan avbrott. När du har ökat lagringen är servern redo att acceptera skrivtransaktioner igen.

Vi rekommenderar att du aktiverar automatisk storleks ökning för lagring eller ställer in en avisering som meddelar dig när Server lagringen närmar sig tröskelvärdet, så att du kan undvika att komma in i skrivskyddat läge. Mer information finns i dokumentationen om [hur du konfigurerar en avisering](howto-alert-metric.md).

### <a name="storage-auto-grow"></a>Utöka lagring automatiskt

Med automatisk storleks ökning i arkivet förhindrar du att servern kör lagrings utrymme och blir skrivskyddad. Om lagrings funktionen är aktive rad utökas lagringen automatiskt utan att arbets belastningen påverkas. För servrar med mindre än lika med 100 GB allokerat lagrings utrymme ökas den etablerade lagrings storleken med 5 GB när det lediga lagrings utrymmet är lägre än 10% av det allokerade lagrings utrymmet. För servrar som har mer än 100 GB allokerat lagrings utrymme ökas den allokerade lagrings storleken med 5% när det lediga lagrings utrymmet är mindre än 10 GB av den allokerade lagrings storleken. De maximala lagrings gränserna som anges ovan gäller.

Om du till exempel har allokerat 1000 GB lagrings utrymme och den faktiska användningen går över 990 GB, ökar server lagrings storleken till 1050 GB. Alternativt, om du har etablerad 10 GB lagrings utrymme, ökar lagrings storleken till 15 GB om mindre än 1 GB lagrings utrymme är kostnads fritt.

Kom ihåg att lagringen bara kan skalas upp, inte nedåt.

## <a name="backup"></a>Backup

Tjänsten tar automatiskt säkerhets kopior av servern. Du kan välja en kvarhållningsperiod från mellan 7 och 35 dagar. Generell användning-och Minnesoptimerade servrar kan välja att ha Geo-redundant lagring för säkerhets kopiering. Läs mer om säkerhets kopieringar i [artikeln begrepp](concepts-backup.md).

## <a name="scale-resources"></a>Skala resurser

När du har skapat din server kan du oberoende ändra virtuella kärnor, pris nivån (förutom till och från Basic), mängden lagring och kvarhållningsperioden för säkerhets kopior. Du kan inte ändra lagrings typen för säkerhets kopia när en server har skapats. Antalet virtuella kärnor kan skalas upp eller ned. Kvarhållningsperioden för säkerhets kopior kan skalas upp eller ned från 7 till 35 dagar. Det går bara att öka lagrings storleken. Skalning av resurserna kan göras via portalen eller Azure CLI. 

<!--For an example of scaling by using Azure CLI, see [Monitor and scale an Azure Database for MariaDB server by using Azure CLI](scripts/sample-scale-server.md).-->

När du ändrar antalet virtuella kärnor, eller pris nivån, skapas en kopia av den ursprungliga servern med den nya beräknings allokeringen. När den nya servern är igång växlar anslutningarna över till den nya servern. Under tiden då systemet växlar över till den nya servern kan inga nya anslutningar upprättas, och transaktioner som inte allokerats återställs. Hur lång tid detta tar varierar, men i de flesta fall tar det mindre än en minut.

Skalning av lagring och ändring av kvarhållning av säkerhets kopior är true online-åtgärder. Det finns ingen nedtid och ditt program påverkas inte. Som IOPS-skala med storleken på den allokerade lagringen kan du öka tillgängligheten för IOPS för servern genom att skala upp lagringen.

## <a name="pricing"></a>Prissättning

Den senaste pris informationen finns på [sidan med pris](https://azure.microsoft.com/pricing/details/mariadb/)information för tjänsten. Om du vill se kostnaden för den konfiguration du vill ha, visar [Azure Portal](https://portal.azure.com/#create/Microsoft.MariaDBServer) månads kostnaden på fliken **pris nivå** baserat på de alternativ du väljer. Om du inte har någon Azure-prenumeration kan du använda pris Kalkylatorn för Azure för att få ett uppskattat pris. På webbplatsen för [Azures pris kalkylator](https://azure.microsoft.com/pricing/calculator/) väljer du **Lägg till objekt**, expanderar kategorin **databaser** och väljer **Azure Database for MariaDB** för att anpassa alternativen.

## <a name="next-steps"></a>Nästa steg
- Läs mer om [tjänst begränsningarna](concepts-limits.md).
- Lär dig hur du [skapar en MariaDB-server i Azure Portal](quickstart-create-mariadb-server-database-using-azure-portal.md).

<!--
- Learn how to [monitor and scale an Azure Database for MariaDB server by using Azure CLI](scripts/sample-scale-server.md).-->
