---
title: Frågor om identifiering, utvärdering och beroende analys i Azure Migrate
description: Få svar på vanliga frågor om identifiering, utvärdering och beroende analys i Azure Migrate.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 9374330044bcd0c0c5f2be44688c2b35760d4418
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/09/2020
ms.locfileid: "82996741"
---
# <a name="discovery-assessment-and-dependency-analysis---common-questions"></a>Identifiering, utvärdering och beroende analys – vanliga frågor

I den här artikeln besvaras vanliga frågor om identifiering, utvärdering och beroende analys i Azure Migrate. Om du har andra frågor kontrollerar du följande resurser:

- [Allmänna frågor](resources-faq.md) om Azure Migrate
- Frågor om [Azure Migrate-enheten](common-questions-appliance.md)
- Frågor om [Server migrering](common-questions-server-migration.md)
- Få svar på frågor i [Azure Migrate-forumet](https://aka.ms/AzureMigrateForum)


## <a name="what-geographies-are-supported-for-discovery-and-assessment-with-azure-migrate"></a>Vilka geografiska områden stöds för identifiering och utvärdering med Azure Migrate?

Granska de geografiska områden som stöds för [offentliga moln](migrate-support-matrix.md#supported-geographies-public-cloud) och [myndighetsmoln](migrate-support-matrix.md#supported-geographies-azure-government).


## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>Hur många virtuella datorer kan jag identifiera med en apparat?

Du kan identifiera upp till 10 000 virtuella VMware-datorer, upp till 5 000 virtuella Hyper-V-datorer och upp till 250 fysiska servrar med hjälp av en enda apparat. Om du har fler datorer läser du om [skalning av en Hyper-V-utvärdering](scale-hyper-v-assessment.md), [skalning av en VMware-utvärdering](scale-vmware-assessment.md)eller [skalning av en fysisk server-utvärdering](scale-physical-assessment.md).

## <a name="i-cant-see-some-vm-types-in-azure-government"></a>Jag kan inte se vissa typer av virtuella datorer i Azure Government

VM-typer som stöds för utvärdering och migrering är beroende av tillgängligheten på Azure Government plats. Du kan [Granska och jämföra](https://azure.microsoft.com/global-infrastructure/services/?regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia&products=virtual-machines) VM-typer i Azure Government.


## <a name="the-size-of-my-vm-changed-can-i-run-an-assessment-again"></a>Storleken på den virtuella datorn ändrades. Kan jag köra en utvärdering igen?

Azure Migrates enheten samlar kontinuerligt in information om den lokala miljön.  En utvärdering är en tidpunkts ögonblicks bild av lokala virtuella datorer. Om du ändrar inställningarna för en virtuell dator som du vill utvärdera använder du alternativet beräkna om du vill uppdatera utvärderingen med de senaste ändringarna.

## <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>Hur gör jag för att identifiera virtuella datorer i en miljö med flera organisationer?

- **VMware**: om en miljö delas mellan klienter och du inte vill identifiera en innehavares virtuella datorer i en annan klient prenumeration, skapar du VMware vCenter Server autentiseringsuppgifter som bara kan komma åt de virtuella datorer som du vill identifiera. Använd sedan autentiseringsuppgifterna när du startar identifiering i Azure Migrate-installationen.
- **Hyper-v**: identifiering använder autentiseringsuppgifter för Hyper-v-värden. Om virtuella datorer delar samma Hyper-V-värd finns det för närvarande inget sätt att separera identifieringen.  

## <a name="do-i-need-vcenter-server"></a>Behöver jag vCenter Server?

Ja, Azure Migrate kräver vCenter Server i en VMware-miljö för att utföra identifiering. Azure Migrate stöder inte identifiering av ESXi-värdar som inte hanteras av vCenter Server.

## <a name="what-are-the-sizing-options"></a>Vad är storleks alternativen?

Vid samma storlek som lokal storlek kan Azure Migrate inte beakta prestanda data för virtuella datorer för utvärdering. Azure Migrate bedömer VM-storlekar baserat på den lokala konfigurationen. Med prestanda-baserad storleks ändring baseras storleken på användnings data.

Till exempel om en lokal virtuell dator har fyra kärnor och 8 GB minne med 50% processor användning och 50% minnes användning:
- Som lokal storlek rekommenderar vi en Azure VM-SKU som har fyra kärnor och 8 GB minne.
- Prestandabaserade storleks ändringar kommer att rekommendera en VM-SKU som har två kärnor och 4 GB minne, eftersom användnings procenten beaktas.

På samma sätt beror disk storleken på storleks kriterierna och lagrings typen:
- Om storleks kriteriet är prestanda beroende och lagrings typen är automatisk, använder Azure Migrate IOPS-och data flödes värden för disken i kontot när den identifierar mål disk typ (standard eller Premium).
- Om storleks kriteriet är prestanda beroende och lagrings typen är Premium rekommenderar Azure Migrate en Premium-disk-SKU som baseras på den lokala diskens storlek. Samma logik används för disk storlek när storleken är lokalt och lagrings typen är standard eller Premium.

## <a name="does-performance-history-and-utilization-affect-sizing"></a>Påverkar prestanda historiken och användningen storleken?

Ja, prestanda historiken och användningen påverkar storlek i Azure Migrate.

### <a name="performance-history"></a>Prestandahistorik

För prestandabaserade storleks ändringar samlar Azure Migrate prestanda historiken för lokala datorer och använder den för att rekommendera den virtuella datorns storlek och disk typ i Azure:

1. Enheten registrerar kontinuerligt den lokala miljön för att samla in användnings data i real tid var 20: e sekund.
1. Enheten samlar in de insamlade 20-sekunderna exemplen och använder dem för att skapa en enda data punkt var 15: e minut.
1. För att skapa data punkten väljer installationen det högsta värdet från alla 20-sekunders exempel.
1. Enheten skickar data punkten till Azure.

### <a name="utilization"></a>Användning

När du skapar en utvärdering i Azure, beroende på prestanda varaktighet och percentilvärdet för prestanda historik som anges, beräknar Azure Migrate det effektiva användning svärdet och använder det sedan för storleks ändring.

Om du till exempel ställer in varaktigheten på en dag och percentilvärdet till 95 percentil, sorterar Azure Migrate de 15 minuters exempel punkter som skickats av insamlaren för den senaste dagen i stigande ordning. Den plockar 95 percentilvärdet som effektiv användning.

Genom att använda 95 percentil-värdet ser du till att avvikande värden ignoreras. Mät värden kan inkluderas om din Azure Migrate använder 99 percentilen. Om du vill välja högsta användnings nivå för perioden utan saknade avvikande värden, ställer du in Azure Migrate att använda 99-percentilen.

## <a name="how-are-import-based-assessments-different-from-assessments-with-discovery-source-as-appliance"></a>Hur skiljer sig de importbaserade utvärderingarna från utvärderingar med identifierings källa som apparat?

Import-baserade utvärderingar är skapade med datorer som importeras till Azure Migrate med hjälp av en CSV-fil. Endast fyra fält är obligatoriska för import: Server namn, kärnor, minne och operativ system. Här är några saker att tänka på: 
 - Beredskaps kriterierna är mindre strikta i import-baserade utvärderingar i Start typs parametern. Om start typen inte anges förutsätts att datorn har BIOS-starttyp och att datorn inte är markerad som **villkorligt klar**. I utvärderingar med identifierings källa som apparat markeras beredskapen som **villkorligt redo** om start typen saknas. Den här skillnaden i beredskaps beräkningen är att användare kanske inte har all information på datorerna i det tidiga skedet vid planering av migrering när importbaserade utvärderingar görs. 
 - De prestandabaserade import utvärderingarna använder det användnings värde som användaren har angett för beräkningar med rätt storlek. Eftersom användning svärdet tillhandahålls av användaren inaktive ras alternativen **prestanda historik** och **percentil** i utvärderings egenskaperna. I utvärderingar med identifierings källa som apparat plockas det valda percentilvärdet från de prestanda data som samlas in av produkten.

## <a name="what-is-dependency-visualization"></a>Vad är beroende visualisering?

Beroende visualisering kan hjälpa dig att utvärdera grupper av virtuella datorer för att migrera med större tillförlitlighet. Beroende visualisering mellan kontroller av dator beroenden innan du kör en utvärdering. Det hjälper till att se till att inget är kvar bakom och hjälper till att undvika oväntade avbrott när du migrerar till Azure. Azure Migrate använder Tjänstkarta-lösningen i Azure Monitor för att aktivera beroende visualisering. [Läs mer](concepts-dependency-visualization.md).

> [!NOTE]
> Agent-baserad beroende analys är inte tillgänglig i Azure Government. Du kan använda agentens beroende analys

## <a name="whats-the-difference-between-agent-based-and-agentless"></a>Vad är skillnaden mellan agent-och agenten?

Skillnaderna mellan agent utan visualisering och agentbaserade visualiseringar sammanfattas i tabellen.

**Krav** | **Utan agent** | **Agent-baserad**
--- | --- | ---
Support | Det här alternativet är för närvarande en för hands version och är bara tillgängligt för virtuella VMware-datorer. [Granska](migrate-support-matrix-vmware.md#agentless-dependency-analysis-requirements) operativ system som stöds. | Allmän tillgänglighet (GA).
Agent | Du behöver inte installera agenter på datorer som du vill kryssa för. | Agenter som ska installeras på varje lokal dator som du vill analysera: [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)och [beroende agenten](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
Krav | [Granska](concepts-dependency-visualization.md#agentless-analysis) kraven och distributions kraven. | [Granska](concepts-dependency-visualization.md#agent-based-analysis) kraven och distributions kraven.
Log Analytics | Krävs inte. | Azure Migrate använder [tjänstkarta](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) -lösningen i [Azure Monitor loggar](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) för beroende visualisering. [Läs mer](concepts-dependency-visualization.md#agent-based-analysis).
Så här fungerar det | Fångar upp TCP-anslutningsfel på datorer aktiverade för beroende visualisering. Efter identifieringen samlar den in data i intervall om fem minuter. | Tjänstkarta agenter som installerats på en dator samla in data om TCP-processer och inkommande/utgående anslutningar för varje process.
Data | Käll datorns Server namn, process, program namn.<br/><br/> Mål datorns Server namn, process, program namn och port. | Käll datorns Server namn, process, program namn.<br/><br/> Mål datorns Server namn, process, program namn och port.<br/><br/> Antalet anslutningar, svars tid och data överförings information samlas in och är tillgängliga för Log Analytics frågor. 
Visualisering | Beroende karta för enskild server kan visas över en varaktighet på en timme till 30 dagar. | Beroende karta för en enskild server.<br/><br/> Kartan kan endast visas över en timme.<br/><br/> Beroende karta för en grupp med servrar.<br/><br/> Lägga till och ta bort servrar i en grupp från MAP-vyn.
Dataexport | Kan för närvarande inte hämtas i tabell format. | Data kan frågas med Log Analytics.


## <a name="do-i-need-to-deploy-the-appliance-for-agentless-dependency-analysis"></a>Behöver jag distribuera enheten för en agent lös beroende analys?

Ja, [Azure Migrate](migrate-appliance.md) -installationen måste distribueras.

## <a name="do-i-pay-for-dependency-visualization"></a>Betalar jag för beroende visualisering?

Nej. Läs mer om [Azure Migrate prissättning](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="what-do-i-install-for-agent-based-dependency-visualization"></a>Vad installerar jag för en agent-baserad beroende visualisering?

Om du vill använda agentbaserade beroende visualisering, laddar du ned och installerar agenter på varje lokal dator som du vill utvärdera:

- [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)
- [Beroendeagent](../azure-monitor/platform/agents-overview.md#dependency-agent)
- Om du har datorer som inte har Internet anslutning kan du hämta och installera Log Analytics gateway på dem.

Du behöver bara dessa agenter om du använder en agent-baserad beroende visualisering.

## <a name="can-i-use-an-existing-workspace"></a>Kan jag använda en befintlig arbets yta?

Ja, för en agent-baserad beroende visualisering kan du koppla en befintlig arbets yta till migreringsjobbet och använda den för beroende visualisering. 

## <a name="can-i-export-the-dependency-visualization-report"></a>Kan jag exportera beroendevisualiseringsrapporten?

Nej, det går inte att exportera den beroende visualiserings rapporten i agent-baserad visualisering. Azure Migrate använder dock Tjänstkarta och du kan använda [tjänstkarta-REST API](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) för att hämta beroenden i JSON-format.

## <a name="can-i-automate-agent-installation"></a>Kan jag automatisera Agent installationen?

För agent-baserad beroende visualisering:

- Använd ett [skript för att installera beroende agenten](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples).
- För MMA [använder du kommando raden eller Automation](../azure-monitor/platform/log-analytics-agent.md#installation-and-configuration)eller använder ett [skript](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).
- Förutom skript kan du använda distributions verktyg som Microsoft Endpoint Configuration Manager och [Intigua](https://www.intigua.com/intigua-for-azure-migration) för att distribuera agenterna.

## <a name="what-operating-systems-does-mma-support"></a>Vilka operativ system stöder MMA?

- Visa listan över [Windows-operativsystem som stöds av MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
- Visa listan över [Linux-operativsystem som stöds av MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

## <a name="can-i-visualize-dependencies-for-more-than-one-hour"></a>Kan jag visualisera beroenden i mer än en timme?

För en agent-baserad visualisering kan du visualisera beroenden i upp till en timme. Du kan gå tillbaka så långt som en månad till ett visst datum i historiken, men den maximala varaktigheten för visualiseringen är en timme. Du kan till exempel använda tids längden i beroende kartan för att Visa beroenden för igår, men du kan bara Visa beroenden för ett entimmes fönster. Du kan dock använda Azure Monitor loggar för att [fråga beroende data](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) under en längre tid.

För övervakning utan agent kan du Visa beroende kartan för en enskild server från en varaktighet på mellan en timme och 30 dagar.

## <a name="can-i-visualize-dependencies-for-groups-of-more-than-10-vms"></a>Kan jag visualisera beroenden för grupper med fler än 10 virtuella datorer?

Du kan [visualisera beroenden](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) för grupper som har upp till 10 virtuella datorer. Om du har en grupp som har fler än 10 virtuella datorer rekommenderar vi att du delar upp gruppen i mindre grupper och sedan visualiserar beroendena.

## <a name="next-steps"></a>Nästa steg

Läs [Azure Migrate översikt](migrate-services-overview.md).
