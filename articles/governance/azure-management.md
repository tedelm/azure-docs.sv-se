---
title: Översikt över Azure Management – Azure-styrning
description: Översikt över hanterings områden för Azure-program och-resurser med länkar till innehåll på hanterings verktyg för Azure.
ms.date: 05/22/2020
ms.topic: overview
ms.openlocfilehash: e293ab743b7c3b49a13cab97200c136a9bd044f5
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83828155"
---
# <a name="overview-of-management-services-in-azure"></a>Översikt över hanterings tjänster i Azure

Styrning i Azure är en aspekt av Azure-hantering. Den här artikeln beskriver olika hanterings områden för att distribuera och underhålla dina resurser i Azure.

Hantering syftar på uppgifter och processer som krävs för att underhålla företagsprogram samt resurser som stöder dessa program. Azure har många tjänster och verktyg som samarbetar för att tillhandahålla fullständig hantering. Dessa tjänster är inte bara för resurser i Azure, utan även i andra moln och lokalt. Att förstå de olika verktygen och hur de fungerar tillsammans är det första steget i utformningen av en komplett hanterings miljö.

Följande diagram visar de olika hanteringsområdena som krävs för att underhålla alla program och resurser. Dessa olika områden kan ses som en livs cykel. Varje zon krävs i kontinuerliga framgångar över livs längd för en resurs. Den här resurs livs cykeln börjar med den första distributionen, genom fortsatta åtgärder och slutligen när den dras tillbaka.

:::image type="content" source="../monitoring/media/management-overview/management-capabilities.png" alt-text="Hanterings ämnes områden i Azure" border="false":::

Ingen enskild Azure-tjänst fyller fullständigt kraven i ett visst hanterings utrymme. I stället realiseras varje tjänst av flera tjänster som arbetar tillsammans. Vissa tjänster, till exempel Application Insights, tillhandahåller riktade övervaknings funktioner för webb program. Andra, som Azure Monitor loggar, lagrar hanterings data för andra tjänster. Med den här funktionen kan du analysera data av olika typer som samlas in av olika tjänster.

Följande avsnitt beskriver i korta ordalag de olika hanteringsområdena och tillhandahåller länkar till detaljerat innehåll om de viktigaste Azure-tjänsterna.

## <a name="monitor"></a>Övervaka

Övervakning handlar om att samla in och analysera data för att granska prestanda, hälsa och tillgänglighet för dina resurser. En effektiv övervaknings strategi hjälper dig att förstå driften av komponenter och öka din drift tid med meddelanden. Läs en översikt över övervakning som täcker de olika tjänsterna som används vid [övervakning av Azure-program och-resurser](../monitoring/monitoring-overview.md).

## <a name="configure"></a>Konfigurera

Konfigurera syftar på den första distributionen och konfigurationen av resurser och pågående underhåll.
Genom att automatisera dessa uppgifter kan du undvika redundans, minimera din tid och ansträngning och öka din precision och effektivitet. [Azure Automation](../automation/automation-intro.md) innehåller många tjänster för att automatisera konfigurationsåtgärder. Medan Runbooks hanterar process automatisering kan konfiguration och uppdaterings hantering hjälpa till att hantera konfigurationen.

## <a name="govern"></a>Styrning

Styrning tillhandahåller mekanismer och processer för att behålla kontrollen över dina program och resurser i Azure. Det omfattar att planera initiativ och att fatta beslut om strategiska prioriteringar.
Styrning i Azure implementeras främst genom två tjänster. Med [Azure policy](./policy/overview.md) kan du skapa, tilldela och hantera princip definitioner för att tillämpa regler för dina resurser.
Den här funktionen håller resurserna i överensstämmelse med företagets standarder.
Med [Azure Cost Management](../cost-management-billing/cost-management-billing-overview.md) kan du spåra moln användning och utgifter för dina Azure-resurser och andra moln leverantörer.

## <a name="secure"></a>Skydda

Hantera säkerheten för dina resurser och data. Ett säkerhets program omfattar att utvärdera hot, samla in och analysera data och efterlevnad av dina program och resurser. Säkerhets övervakning och hot analys tillhandahålls av [Azure Security Center](../security-center/security-center-intro.md), som innehåller enhetlig säkerhets hantering och Avancerat skydd för arbets belastningar i hybrid moln. Mer information och vägledning om hur du skyddar Azure-resurser finns i [Introduktion till Azure-säkerhet](../security/fundamentals/overview.md) .

## <a name="protect"></a>Skydda

Skydd syftar till att hålla dina program och data tillgängliga, även med avbrott som ligger utanför din kontroll. Skydd i Azure tillhandahålls av två tjänster. [Azure Backup](../backup/backup-introduction-to-azure-backup.md) tillhandahåller säkerhetskopiering och återställning av data, antingen i molnet eller lokalt. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) ger affärs kontinuitet och omedelbar återställning under en katastrof.

## <a name="migrate"></a>Migrera

Migrering refererar till att överföra arbetsbelastningar som körs lokalt till Azure-molnet.
[Azure Migrate](../migrate/migrate-overview.md) är en tjänst som hjälper dig att utvärdera migreringens lämplighet för lokala virtuella datorer till Azure. Azure Site Recovery migrerar virtuella datorer [från lokala platser](../site-recovery/migrate-tutorial-on-premises-azure.md) eller [från Amazon Web Services](../site-recovery/migrate-tutorial-aws-azure.md). [Azure Database migration](../dms/dms-overview.md) hjälper dig att migrera databas källor till Azure Data Platforms.
