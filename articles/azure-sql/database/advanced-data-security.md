---
title: Avancerad datasäkerhet
description: Lär dig mer om funktioner för att identifiera och klassificera känsliga data, hantera dina databas sårbarheter och identifiera avvikande aktiviteter som kan tyda på ett hot mot din Azure SQL Database, Azure SQL-hanterad instans eller Azure-Synapse.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.custom: sqldbrb=2
ms.topic: conceptual
ms.author: memildin
author: memildin
manager: rkarlin
ms.reviewer: vanto
ms.date: 04/23/2020
ms.openlocfilehash: ed7d4b10219f4d4a3c437331bd1daf870495949d
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84047855"
---
# <a name="advanced-data-security"></a>Avancerad datasäkerhet
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]


Avancerad data säkerhet (ADS) är ett enhetligt paket för avancerade SQL-säkerhetsfunktioner. ADS är tillgängligt för Azure SQL Database, Azure SQL-hanterad instans och Azure-Synapse. Det innefattar funktioner för att identifiera och klassificera känsliga data, lyfta fram och åtgärda potentiella säkerhetsrisker i databasen och identifiera avvikande aktiviteter som kan indikera ett hot mot databasen. Det ger en samlad plats för aktivering och hantering av dessa funktioner.

## <a name="overview"></a>Översikt

Avancerad data säkerhet (ADS) innehåller en uppsättning avancerade SQL-säkerhetsfunktioner, inklusive data identifiering & klassificering, sårbarhets bedömning och Avancerat skydd.

- [Data identifierings & klassificering](data-discovery-and-classification-overview.md) innehåller funktioner som är inbyggda i Azure SQL Database, Azure SQL-hanterad instans och Azure-Synapse för att upptäcka, klassificera och märka & rapportering av känsliga data i dina databaser. Det kan användas för att ge insyn i tillståndet på din databasklassificering och för att spåra åtkomst till känsliga data i och utanför databasen.
- [Vulnerability Assessment](sql-vulnerability-assessment.md) är en tjänst som är enkel att konfigurera och som kan identifiera, spåra och hjälpa dig att åtgärda eventuella säkerhetsrisker i databasen. Den ger inblick i dina säkerhetstillstånd och inkluderar lämpliga åtgärder för att lösa säkerhetsproblem och förbättra databasens skydd.
- [Advanced Threat Protection](threat-detection-overview.md) identifierar avvikande aktiviteter som indikerar ovanliga och potentiellt skadliga försök att komma åt eller utnyttja din databas. Den övervakar kontinuerligt databasen för misstänkta aktiviteter och ger omedelbara säkerhetsaviseringar om potentiella säkerhetsproblem, SQL-inmatningsattacker samt avvikande åtkomstmönster i databasen. Advanced Threat Protection-aviseringar ger detaljerad information om misstänkt aktivitet och rekommenderar åtgärder för att undersöka och minska risken.

Aktivera SQL-annonser en gång för att aktivera alla dessa inkluderade funktioner. Med ett klick kan du aktivera annonser för alla databaser på [servern](logical-servers.md) i Azure (som är värdar SQL Database eller Azure Synapse Analytics) eller i instans i Azure SQL-hanterad instans. Aktivering eller hantering av ADS-inställningar kräver tillhöra rollen [SQL Security Manager](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) , rollen administratör för SQL-databas eller rollen SQL Server-administratör.

Prissättningen för ANNONSERas med Azure Security Center standard nivå, där varje skyddad Server eller hanterad instans räknas som en nod. Nyligen skyddade resurser är berättigade till en kostnads fri utvärderings version av Security Center standard nivån. Mer information finns på sidan med [Azure Security Center priser](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Komma igång med annonser

Följande steg hjälper dig att komma igång med annonser.

## <a name="1-enable-ads"></a>1. Aktivera annonser

Aktivera annonser genom att navigera till **Avancerad data säkerhet** under **säkerhets** rubriken för servern eller den hanterade instansen.

> [!NOTE]
> Ett lagrings konto skapas och konfigureras automatiskt för att lagra dina genomsöknings resultat för **sårbarhets bedömning** . Om du redan har aktiverat annonser för en annan server i samma resurs grupp och region används det befintliga lagrings kontot.

![Aktivera annonser](./media/advanced-data-security/enable_ads.png)

> [!NOTE]
> Kostnaden för annonser justeras med Azure Security Center standard pris nivå per nod, där en nod är hela servern eller en hanterad instans. Du betalar därför bara en gång för att skydda alla databaser på servern eller den hanterade instansen med annonser. Du kan prova att börja med att börja använda ADS med en kostnads fri utvärderings version.

## <a name="2-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>2. börja klassificera data, spåra sårbarheter och undersöka hot aviseringar

Klicka på **klassificerings kortet data identifierings &** för att se rekommenderade känsliga kolumner för att klassificera och klassificera data med beständiga känslighets etiketter. Klicka på kortet för **sårbarhets bedömning** för att visa och hantera sårbarhets genomsökningar och rapporter och för att spåra din säkerhets datasekretesstandarder. Om säkerhets aviseringar har mottagits klickar du på kortet **Avancerat skydd** för att visa information om aviseringarna och visa en konsol IDE rad rapport om alla aviseringar i din Azure-prenumeration via sidan Azure Security Center säkerhets aviseringar.

## <a name="3-manage-ads-settings"></a>3. hantera ADS-inställningar

Om du vill visa och hantera ADS-inställningar går du till **Avancerad data säkerhet** under **säkerhets** rubriken för servern eller den hanterade instansen. På den här sidan kan du aktivera eller inaktivera annonser och ändra inställningarna för sårbarhets bedömning och Avancerat skydd för hela servern eller den hanterade instansen.

![Serverinställningar
](./media/advanced-data-security/server_settings.png)

## <a name="4-manage-ads-settings-for-a-sql-database"></a>4. hantera ADS-inställningar för en SQL-databas

Om du vill åsidosätta ADS-inställningarna för en viss databas markerar du kryss rutan **aktivera avancerad data säkerhet på databas nivå** . Använd bara det här alternativet om du har ett särskilt krav för att ta emot separata varningar om Avancerat skydd eller sårbarhets bedömning för den enskilda databasen, i stället för de aviseringar och resultat som tagits emot för alla databaser på servern eller den hanterade instansen.

När kryss rutan är markerad kan du konfigurera de relevanta inställningarna för den här databasen.

![Inställningar för databas-och Avancerat skydd](./media/advanced-data-security/database_threat_detection_settings.png)

Avancerade data säkerhets inställningar för servern eller den hanterade instansen kan också nås från fönstret ADS Database. Klicka på **Inställningar** i rutan huvud annonser och klicka sedan på **Visa avancerade data säkerhets Server inställningar**.

![Databas inställningar](./media/advanced-data-security/database_settings.png)

## <a name="next-steps"></a>Nästa steg

- Läs mer om [data identifiering & klassificering](data-discovery-and-classification-overview.md)
- Läs mer om [sårbarhets bedömning](sql-vulnerability-assessment.md)
- Läs mer om [Avancerat skydd](threat-detection-configure.md)
- Läs mer om [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
