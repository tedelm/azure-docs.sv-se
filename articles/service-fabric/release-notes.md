---
title: Azure Service Fabric-versioner
description: Viktig information för Azure Service Fabric. Innehåller information om de senaste funktionerna och förbättringarna i Service Fabric.
ms.date: 06/10/2019
ms.topic: conceptual
hide_comments: true
hideEdit: true
ms.openlocfilehash: 28870a197af07e964a50a06ffeef08f3b71451f4
ms.sourcegitcommit: b396c674aa8f66597fa2dd6d6ed200dd7f409915
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/07/2020
ms.locfileid: "82891715"
---
# <a name="service-fabric-releases"></a>Service Fabric versioner

| <a href="https://github.com/Azure/Service-Fabric-Troubleshooting-Guides" target="blank">Fel söknings guider</a> 
| <a href="https://github.com/Azure/service-fabric-issues" target="blank">problem</a> 
| med<a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-versions" target="blank">versioner</a> 
| <a href="https://docs.microsoft.com/azure/service-fabric/service-fabric-support" target="blank">som</a> 
| stöds<a href="https://azure.microsoft.com/resources/samples/?service=service-fabric&sort=0" target="blank">kod exempel</a>

Den här artikeln innehåller mer information om de senaste versionerna och uppdateringarna av Service Fabric Runtime och SDK: er.

## <a name="whats-new-in-service-fabric"></a>Vad är nytt i Service Fabric

### <a name="service-fabric-71"></a>Service Fabric 7,1
På grund av den aktuella COVID-tjänstekrisen och med tanke på de utmaningar som våra kunder möter, gör vi 7,1 tillgängliga, men uppgraderar inte automatiskt kluster som är inställda på att ta emot automatiska uppgraderingar. Vi pausar automatiska uppgraderingar tills vidare se till att kunderna kan installera uppgraderingar när de är mest lämpliga för dem, för att undvika oväntade avbrott.

Du kommer att kunna uppdatera till 7,1 via [Azure Portal](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-version-azure#upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal) eller via en [Azure Resource Manager distribution](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-version-azure#set-the-upgrade-mode-using-a-resource-manager-template).

Service Fabric kluster med aktiverade automatiska uppgraderingar börjar ta emot 7,1-uppdateringen automatiskt när vi fortsätter med standard distributions proceduren. Vi kommer att tillhandahålla ett annat meddelande innan standard distributionen börjar på den [Service Fabric tekniska Community-webbplatsen](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric).
Vi har också publicerat uppdateringar till slutet av support datumet för större versioner från 6,5 till 7,1 [här](https://docs.microsoft.com/azure/service-fabric/service-fabric-versions#supported-versions). 

## <a name="what-is-new-in-service-fabric-71"></a>Vad är nytt i Service Fabric 7,1?
Vi är glada över att kunna presentera nästa version av Service Fabric. Den här versionen har lästs in med viktiga funktioner och förbättringar. Några av huvud funktionerna är markerade nedan:
## <a name="key-announcements"></a>Viktiga meddelanden
- **Allmän tillgänglighet** för [ **Service Fabric hanterade identiteter för Service Fabric program**](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity)
- [**Stöd för Ubuntu 18,04**](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-create-vnet-and-linux-cluster)
 - För [**hands version: stöd för virtuell dator med den virtuella datorns skalnings uppsättning**](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-azure-deployment-preparation#use-ephemeral-os-disks-for-virtual-machine-scale-sets)* *: de tillfälliga OS-diskarna skapas på den lokala virtuella datorn och sparas inte på fjärrAzure Storage. De rekommenderas för alla Service Fabric Node-typer (primär och sekundär), på grund av traditionella beständiga OS-diskar, tillfälliga OS-diskar:
      -  Minska svars tiden för Läs/skriv till OS-disk
      -  Aktivera snabbare återställnings-och avbildnings hanterings åtgärder
      -  Minska totalkostnaden (diskarna är kostnads fria och debiteras inga ytterligare lagrings kostnader)
- Stöd för deklaration av [**tjänst slut punkts certifikat för Service Fabric program efter eget ämnes namn**](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-manifest-resources).
- [**Stöd för hälso avsökningar för containerbaserade tjänster**](https://docs.microsoft.com/azure/service-fabric/probes-codepackage): stöd för direktmigreringens avsöknings funktion för program i behållare. Med hjälp av direktmigreringens avsökning kan du meddela om liveheten för det behållar programmet och när de inte svarar inom rimlig tid, vilket leder till en omstart. 
- [**Stöd för initierare kod paket**](https://docs.microsoft.com/azure/service-fabric/initializer-codepackages) för [behållare](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-containers-overview) och [körbara gäst](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-guest-executables-introduction) program. Detta gör det möjligt att köra kod paket (t. ex. behållare) i en angiven ordning för att utföra service paket initiering.
- **FabricObserver och ClusterObserver** är tillstånds lösa program som fångar Service Fabric telemetri som är relaterade till olika aspekter av ett SF-kluster. Båda programmen är klara att distribueras till Windows-produktionsanläggningar för att samla in omfattande telemetri med implementerat stöd för ApplicationInsights, EventSource och LogAnalytics.
    - [**FabricObserver (fo) 2,0**](https://github.com/microsoft/service-fabric-observer)-körs på alla noder, genererar hälso händelser och genererar telemetri när de angivna tröskelvärdena för resursanvändning nås. Den här versionen innehåller flera förbättringar i övervakning, data hantering, hälso information, strukturerad telemetri.
     - [**ClusterObserver (co) 1,1**](https://github.com/microsoft/service-fabric-observer/tree/master/ClusterObserver) -körs på en nod, fångar upp hälso telemetri på kluster nivå. I den här versionen övervakar ClusterObserver också nodens status och genererar telemetri när noden är nere/inaktive rad under längre tid än den användardefinierade tids perioden.

### <a name="improve-application-life-cycle-experience"></a>Förbättra programmets livs cykel upplevelse

- För **[hands version: begär dränering](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-advanced#avoid-connection-drops-during-planned-downtime-of-stateless-services)**: under planerat underhåll av tjänster, till exempel tjänst uppgraderingar eller Node-inaktive ring, vill du tillåta tjänsterna att tömma anslutningarna på ett smidigt sätt. Den här funktionen lägger till en instans stängnings fördröjning i tjänst konfigurationen. Under de planerade åtgärderna tar SF bort tjänstens adress från identifieringen och väntar sedan den här varaktigheten innan tjänsten stängs av.
- **[Automatisk identifiering och balansering av under kluster](https://docs.microsoft.com/azure/service-fabric/cluster-resource-manager-subclustering )**: under kluster sker när tjänster med olika placerings begränsningar har ett gemensamt [belastnings mått](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-metrics). Om belastningen på de olika uppsättningarna med noder skiljer sig avsevärt, anser Service Fabric Cluster Resource Manager att klustret är obalanserat, även om det har det bästa möjliga saldot på grund av placerings begränsningarna. Därför försöker det att balansera om klustret, vilket kan orsaka onödiga tjänst transporter (eftersom "obalans" inte kan förbättras avsevärt). Från och med den här versionen försöker kluster resurs hanteraren nu att automatiskt identifiera de här sorteringen av konfigurationer och förstå när obalansen kan åtgärdas genom flyttningen, och i stället bör den lämna saker som är ensamma eftersom ingen avsevärd förbättring kan göras.  
- [**Olika flytt kostnader för sekundära repliker**](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-movement-cost): vi har lanserat ett nytt flytt kostnads värde VeryHigh som ger ytterligare flexibilitet i vissa scenarier för att definiera om en separat flytt kostnad ska användas för sekundära repliker.
- Aktive rad [**direktmigreringens avsöknings**](https://docs.microsoft.com/azure/service-fabric/probes-codepackage ) funktion för program i behållare. Med hjälp av direktmigreringens avsökning kan du meddela om liveheten för det behållar programmet och när de inte svarar inom rimlig tid, vilket leder till en omstart.
- [**Kör till slut för ande/en gång för tjänster**](https://docs.microsoft.com/azure/service-fabric/run-to-completion)**

### <a name="image-store-improvements"></a>Avbildningsarkiv förbättringar
 - Service Fabric 7,1 använder **anpassad transport för att skydda fil överföring mellan noder som standard**. Beroendet av SMB-filresursen tas bort från version 7,1. De skyddade SMB-filresurserna är fortfarande befintliga på noder som innehåller Avbildningsarkiv tjänst replik för kundens val att välja från standard och för att uppgradera och nedgradera till gammal version.
       
 ### <a name="reliable-collections-improvements"></a>Förbättringar av pålitliga samlingar

- [**I minnet lagrar bara stöd för tillstånds känsliga tjänster med Reliable Collections**](https://docs.microsoft.com/azure/service-fabric/service-fabric-work-with-reliable-collections#volatile-reliable-collections): volatile Reliable Collections gör att data kan sparas till disk för hållbarhet mot storskaliga avbrott, kan användas för arbets belastningar som replikerad cache, till exempel där tillfällig data förlust kan tolereras. Baserat på [begränsningar och begränsningar för flyktiga pålitliga samlingar](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-reliable-collections-guidelines#volatile-reliable-collections)rekommenderar vi detta för arbets belastningar som inte behöver beständighet, för tjänster som hanterar sällsynta tillfällen för att förlora kvorum.
- För [**hands version: Service Fabric backup Explorer**](https://github.com/microsoft/service-fabric-backup-explorer): för att under lätta hanteringen av pålitliga säkerhets kopieringar för Service Fabric tillstånds känsliga program kan Service Fabric backup Explorer göra det möjligt för användare att
    - Granska och granska innehållet i de pålitliga samlingarna,
    - Uppdatera aktuellt tillstånd till en konsekvent vy
    - Skapa en säkerhets kopia av den aktuella ögonblicks bilden av de pålitliga samlingarna
    - Åtgärda skadade data
                 
### <a name="service-fabric-71-releases"></a>Service Fabric 7,1-versioner
| Utgivningsdatum | Frisläpp | Mer information |
|---|---|---|
| 20 april 2020 | [Azure Service Fabric 7,1](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-1-release/ba-p/1311373)  | [Viktig information](https://github.com/microsoft/service-fabric/tree/master/release_notes/Service-Fabric-71-releasenotes.md)|


### <a name="service-fabric-70"></a>Service Fabric 7,0

Azure Service Fabric 7,0 är nu tillgängligt! Du kommer att kunna uppdatera till 7,0 via Azure Portal eller via en Azure Resource Manager distribution. På grund av kundernas feedback om versioner under den helgdags perioden börjar vi inte att automatiskt uppdatera kluster som är inställda för att ta emot automatiska uppgraderingar förrän januari.
I januari kommer vi att återuppta standard proceduren för distributionen och kluster med aktiverade automatiska uppgraderingar börjar ta emot 7,0-uppdateringen automatiskt. Vi kommer att tillhandahålla ett annat meddelande innan distributionen påbörjas.
Vi uppdaterar även våra planerade versions datum för att ange att den här principen ska beaktas. Här hittar du uppdateringar för våra framtida [versions scheman](https://github.com/Microsoft/service-fabric/#service-fabric-release-schedule).
 
Det här är den senaste versionen av Service Fabric och har lästs in med viktiga funktioner och förbättringar.

### <a name="key-announcements"></a>Viktiga meddelanden
 - [**KeyVaultReference-stöd för program hemligheter (för hands version)**](https://docs.microsoft.com/azure/service-fabric/service-fabric-keyvault-references): Service Fabric program som har aktiverat [hanterade identiteter](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity) kan nu direkt referera till en Key Vault hemlig URL som en miljö variabel, program parameter eller autentiseringsuppgifter för containerns lagrings plats. Service Fabric kommer automatiskt att lösa hemligheten med hjälp av programmets hanterade identitet. 
     
- **Förbättrad uppgraderings säkerhet för tillstånds lösa tjänster**: för att garantera tillgängligheten under en program uppgradering har vi introducerat nya konfigurationer för att definiera det [minsta antalet instanser för tillstånds lösa tjänster](https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription?view=azure-dotnet) som ska anses tillgängliga. Tidigare var det här värdet 1 för alla tjänster och kunde inte ändras. Med den nya säkerhets kontrollen per tjänst kan du se till att tjänsterna behåller ett minsta antal instanser under program uppgraderingar, kluster uppgraderingar och annat underhåll som förlitar sig på Service Fabric hälso-och säkerhets kontroller.
  
- [**Resurs gränser för användar tjänster**](https://docs.microsoft.com/azure/service-fabric/service-fabric-resource-governance#enforcing-the-resource-limits-for-user-services): användare kan konfigurera resurs gränser för användar tjänsterna på en nod för att förhindra scenarier som resurs utbelastning av Service Fabric system tjänster. 
  
- [**Mycket hög service flytt kostnad**](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-movement-cost) för en replik typ. Repliker med mycket hög flytt kostnad kommer bara att flyttas om det finns en begränsnings överträdelse i klustret som inte kan åtgärdas på något annat sätt. I det länkade dokumentet finns mer information om när användningen av en "mycket hög" flytt kostnad är rimlig och ytterligare överväganden.
  
-  **Ytterligare säkerhets kontroller för klustret**: i den här versionen har vi infört en konfigurerbar säkerhets kontroll för Dirigerings-nods kvorum. På så sätt kan du anpassa hur många startnoder som måste vara tillgängliga under kluster livs cykel-och hanterings scenarier. Åtgärder som tar klustret lägre än det konfigurerade värdet blockeras. I dag är standardvärdet alltid ett kvorum för startnoderna, till exempel om du har sju startnoder, kommer en åtgärd som tar dig under 5 startnoder att blockeras som standard. Med den här ändringen kan du göra det lägsta säkra värdet 6, vilket innebär att endast en Seed-nod kan vara nere i taget.
   
- Stöd har lagts till för [**att hantera säkerhets kopierings-och återställnings tjänsten i Service Fabric Explorer**](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-quickstart-azurecluster). Detta gör följande aktiviteter möjligt direkt från SFX: identifiera säkerhets kopierings-och återställnings tjänsten, skapa en säkerhets kopierings princip, aktivera automatisk säkerhets kopiering, använda adhoc-säkerhetskopiering, utlösa återställnings åtgärder och söka efter befintliga säkerhets kopior.

- Vi presenterar tillgängligheten för [**ReliableCollectionsMissingTypesTool**](https://github.com/hiadusum/ReliableCollectionsMissingTypesTool): med det här verktyget kan du validera att de typer som används i pålitliga samlingar är framåtriktade och bakåtkompatibla under en rullande program uppgradering. Detta bidrar till att förhindra uppgraderings fel eller data förlust och skadade data på grund av saknade eller inkompatibla typer.

- [**Aktivera stabil läsning på sekundära repliker**](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-configuration#configuration-names-1): stabila läsningar begränsar sekundära repliker till att returnera värden som har varit bekräftat.

Dessutom innehåller den här versionen andra nya funktioner, fel korrigeringar och förbättringar av support, tillförlitlighet och prestanda. En fullständig lista över ändringar finns i [viktig information](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_70.md).

### <a name="service-fabric-70-releases"></a>Service Fabric 7,0-versioner

| Utgivningsdatum | Frisläpp | Mer information |
|---|---|---|
| Den 18 november 2019 | [Azure Service Fabric 7,0](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Service-Fabric-7-0-Release/ba-p/1015482)  | [Viktig information](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_70.md)|
| 30 januari 2020 | [Uppdaterings version för Azure Service Fabric 7,0](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-second-refresh-release/ba-p/1137690)  | [Viktig information](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service-Fabric-70CU2-releasenotes.md)|
| 6 februari 2020 | [Uppdaterings version för Azure Service Fabric 7,0](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-third-refresh-release/ba-p/1156508)  | [Viktig information](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service-Fabric-70CU3-releasenotes.md)|
| 2 mars 2020 | [Uppdaterings version för Azure Service Fabric 7,0](https://techcommunity.microsoft.com/t5/azure-service-fabric/azure-service-fabric-7-0-fourth-refresh-release/ba-p/1205414)  | [Viktig information](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service-Fabric-70CU4-releasenotes.md)

### <a name="service-fabric-65"></a>Service Fabric 6,5

Den här versionen innehåller förbättringar av support, tillförlitlighet och prestanda, nya funktioner, fel korrigeringar och förbättringar för att förenkla hanteringen av kluster och program livs cykeln.

> [!IMPORTANT]
> Service Fabric 6,5 är den slutliga versionen med stöd för Service Fabric-verktyg i Visual Studio 2015. Kunderna uppmanas att gå vidare till [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) .

Här är what's New i Service Fabric 6,5:

- Service Fabric Explorer innehåller ett [avbildningsarkiv visnings](service-fabric-visualizing-your-cluster.md#image-store-viewer) program för att inspektera program som du har överfört till avbildnings lagret.

- [POA-1.4.0 (patch Orchestration Application)](service-fabric-patch-orchestration-application.md) innehåller många själv diagnostiska förbättringar. [1.4.0](https://github.com/microsoft/Service-Fabric-POA/releases/tag/v1.4.0) Kunder med POA rekommenderas att flytta till den här versionen.

- [EventStore-tjänsten är aktive rad som standard](service-fabric-visualizing-your-cluster.md#event-store) i Service Fabric 6,5-kluster om du inte har valt ut.

- Har lagt till [replik livs cykel händelser](service-fabric-diagnostics-event-generation-operational.md#replica-events) för tillstånds känsliga tjänster.

- [Bättre insyn i status för Dirigerings nod](service-fabric-understand-and-troubleshoot-with-system-health-reports.md#seed-node-status), inklusive varningar på kluster nivå om en*Seed-nod är ohälsosam (av*, *borttagen* eller *okänd*).

- Med [Service Fabric-verktyget för haveri beredskap för program](https://github.com/Microsoft/Service-Fabric-AppDRTool) kan Service Fabric tillstånds känsliga tjänster återställas snabbt när det primära klustret påträffar en katastrof. Data från det primära klustret synkroniseras kontinuerligt i det sekundära standby-programmet med hjälp av periodisk säkerhets kopiering och återställning.

- Visual Studio-stöd för [att publicera .net Core-appar till Linux-baserade kluster](service-fabric-how-to-publish-linux-app-vs.md).

- [Azure Service Fabric CLI (SFCTL)](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) installeras automatiskt för Service Fabric 6,5 (och senare versioner) när du uppgraderar eller skapar ett nytt Linux-kluster i Azure.

- [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) installeras som standard på MacOS/Linux Onebox behållaravbildningen-kluster.

Mer information finns i [versions anmärkningar för Service Fabric 6,5](https://github.com/Azure/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf).

### <a name="service-fabric-65-releases"></a>Service Fabric 6,5-versioner

| Utgivningsdatum | Frisläpp | Mer information |
|---|---|---|
| 11 juni 2019 | [Azure Service Fabric 6,5](https://blogs.msdn.microsoft.com/azureservicefabric/2019/06/11/azure-service-fabric-6-5-release/)  | [Viktig information](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65.pdf)|
| 2 juli 2019 | [Uppdaterings version för Azure Service Fabric 6,5](https://blogs.msdn.microsoft.com/azureservicefabric/2019/07/04/azure-service-fabric-6-5-refresh-release/)  | [Viktig information](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU1.pdf)  |
| 29 juli 2019 | [Uppdaterings version för Azure Service Fabric 6,5](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Second-Refresh-Release/ba-p/800523)  | [Viktig information](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU2.pdf)  |
| Aug 23, 2019 | [Uppdaterings version för Azure Service Fabric 6,5](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Third-Refresh-Release/ba-p/818599)  | [Viktig information](https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU3.pdf)  |
| 14 oktober 2019 | [Uppdaterings version för Azure Service Fabric 6,5](https://techcommunity.microsoft.com/t5/Azure-Service-Fabric/Azure-Service-Fabric-6-5-Fifth-Refresh-Release/ba-p/913296)  | [Viktig information] (https://github.com/microsoft/service-fabric/blob/master/release_notes/Service_Fabric_ReleaseNotes_65CU5.md  |


## <a name="previous-versions"></a>Tidigare versioner

### <a name="service-fabric-64-releases"></a>Service Fabric 6,4-versioner

| Utgivningsdatum | Frisläpp | Mer information |
|---|---|---|
| 30 november 2018 | [Azure Service Fabric 6,4](https://blogs.msdn.microsoft.com/azureservicefabric/2018/11/30/azure-service-fabric-6-4-release/)  | [Viktig information](https://msdnshared.blob.core.windows.net/media/2018/12/Service-Fabric-6.4-Release.pdf)|
| 12 december 2018 | [Azure Service Fabric 6,4 uppdaterings version för Windows-kluster](https://blogs.msdn.microsoft.com/azureservicefabric/2018/12/12/azure-service-fabric-6-4-refresh-for-windows-clusters/)  | [Viktig information](https://msdnshared.blob.core.windows.net/media/2018/12/Links.pdf)  |
| 4 februari 2019 | [Uppdaterings version för Azure Service Fabric 6,4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/02/04/azure-service-fabric-6-4-refresh-release/) | [Viktig information](https://msdnshared.blob.core.windows.net/media/2019/02/Service-Fabric-6.4CU3-Release-Notes.pdf) |
| 4 mars 2019 | [Uppdaterings version för Azure Service Fabric 6,4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/03/12/azure-service-fabric-6-4-refresh-release-2/) | [Viktig information](https://msdnshared.blob.core.windows.net/media/2019/03/Service-Fabric-6.4CU4-Release-Notes.pdf)
| 8 april 2019 | [Uppdaterings version för Azure Service Fabric 6,4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/04/08/azure-service-fabric-6-4-refresh-release-5/) | [Viktig information](https://msdnshared.blob.core.windows.net/media/2019/04/Service-Fabric-6.4CU5-ReleaseNotes3.pdf)
| 2 maj 2019 | [Uppdaterings version för Azure Service Fabric 6,4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/02/azure-service-fabric-6-4-refresh-release-3/) | [Viktig information](https://msdnshared.blob.core.windows.net/media/2019/05/Service-Fabric-64CU6-Release-Notes-V2.pdf)
| 28 maj, 2019 | [Uppdaterings version för Azure Service Fabric 6,4](https://blogs.msdn.microsoft.com/azureservicefabric/2019/05/28/azure-service-fabric-6-4-refresh-release-4/) | [Viktig information](https://msdnshared.blob.core.windows.net/media/2019/05/Service_Fabric_64CU7_Release_Notes1.pdf)
