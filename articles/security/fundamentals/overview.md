---
title: Introduktion till Azure-säkerhet | Microsoft Docs
description: Lär dig om Azure-säkerhet, dess tjänster och hur det fungerar.
services: security
documentationcenter: na
author: UnifyCloud
manager: rkarlin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2019
ms.author: TomSh
ms.openlocfilehash: b39eb24b5611e36d14b151aaec96941ff52f3dfa
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/03/2020
ms.locfileid: "82731711"
---
# <a name="introduction-to-azure-security"></a>Introduktion till Azure-säkerhet
## <a name="overview"></a>Översikt
Vi vet att säkerheten är ett jobb i molnet och hur viktigt det är att du hittar korrekt och tidsödande information om Azure-säkerhet. Ett av de bästa orsakerna till att använda Azure för dina program och tjänster är att dra nytta av dess breda uppsättning säkerhets verktyg och funktioner. Med dessa verktyg och funktioner kan du skapa säkra lösningar på den säkra Azure-plattformen. Microsoft Azure ger konfidentialitet, integritet och tillgänglighet för kund information, samtidigt som du aktiverar transparent ansvars rätt.

Den här artikeln innehåller en omfattande översikt över säkerheten i Azure.

### <a name="azure-platform"></a>Azure-plattformen
Azure är en offentlig moln tjänst plattform som har stöd för ett brett urval av operativ system, programmeringsspråk, ramverk, verktyg, databaser och enheter. Den kan köra Linux-behållare med Docker-integrering, Bygg appar med Java Script, python, .NET, PHP, Java och Node. js. Bygg Server delar för iOS-, Android-och Windows-enheter.

Azures offentliga moln tjänster har stöd för samma teknik miljon tals utvecklare och IT-proffs är redan förlitande på och litar på. När du bygger på, eller migrerar IT-tillgångar till, förlitar sig en offentlig moln tjänst leverantör på organisationens möjligheter att skydda dina program och data med tjänsterna och de kontroller som de tillhandahåller för att hantera säkerheten för dina molnbaserade till gångar.

Azures infrastruktur har utformats från anläggningar till program för att vara värd för miljon tals kunder samtidigt, och det ger en säker grund där företag kan uppfylla sina säkerhets krav.

Dessutom ger Azure dig en mängd olika konfigurerbara säkerhets alternativ och möjligheten att kontrol lera dem så att du kan anpassa säkerheten för att uppfylla de unika kraven i din organisations distributioner. Det här dokumentet hjälper dig att förstå hur Azures säkerhetsfunktioner kan hjälpa dig att uppfylla dessa krav.

> [!Note]
> Det primära fokuset i det här dokumentet är av kundbaserade kontroller som du kan använda för att anpassa och öka säkerheten för dina program och tjänster.
>
> Information om hur Microsoft skyddar själva Azure-plattformen finns i säkerhet för [Azure-infrastrukturen](infrastructure.md).

## <a name="summary-of-azure-security-capabilities"></a>Översikt över funktioner i Azure

### <a name="features-to-secure-the-azure-platform"></a>Funktioner för att skydda Azure-plattformen
Följande funktioner är funktioner som du kan granska för att garantera att Azure-plattformen hanteras på ett säkert sätt. Det finns länkar till mer information om hur Microsoft hanterar frågor om kund förtroende på fyra områden: säker plattform, sekretess & kontroller, efterlevnad och transparens.

| [Säker plattform](https://www.microsoft.com/trustcenter/Security/default.aspx)  | [Sekretess & kontroller](https://www.microsoft.com/trustcenter/Privacy/default.aspx)  |[Efterlevnad](https://www.microsoft.com/trustcenter/Compliance/default.aspx)   | [Oh](https://www.microsoft.com/trustcenter/Transparency/default.aspx) |
| :-- | :-- | :-- | :-- |
| [Säkerhets utvecklings cykel](https://www.microsoft.com/sdl/), interna granskningar | [Hantera dina data hela tiden](https://www.microsoft.com/trustcenter/Privacy/You-own-your-data) | [Säkerhetscenter](https://www.microsoft.com/trustcenter/default.aspx) |[Hur Microsoft skyddar kund information i Azure-tjänster](https://www.microsoft.com/trustcenter/Transparency/default.aspx) |
| [Obligatorisk säkerhetsutbildning, bakgrunds kontroller](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx) |  [Kontroll på data plats](https://www.microsoft.com/trustcenter/Privacy/Where-your-data-is-located) |  [Hubb för vanliga kontroller](https://www.microsoft.com/trustcenter/Common-Controls-Hub) |[Hur Microsoft hanterar data platser i Azure-tjänster](https://azuredatacentermap.azurewebsites.net/)|
| [Testning av inträngande](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx), [intrångs identifiering, DDoS](https://www.microsoft.com/trustcenter/Security/ThreatManagement), [granskningar & loggning](https://www.microsoft.com/trustcenter/Security/AuditingAndLogging) | [Ge data åtkomst på dina villkor](https://www.microsoft.com/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms) |  [Check lista för Cloud Services förfallna noggrannhet](https://www.microsoft.com/trustcenter/Compliance/Due-Diligence-Checklist) |[Vem i Microsoft kan komma åt dina data på vilka villkor](https://www.microsoft.com/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)|
| [Tillstånd för konst Data Center](https://www.microsoft.com/cloud-platform/global-datacenters), fysisk säkerhet, [säkert nätverk](network-overview.md) | [Svara på juridisk tillämpning](https://www.microsoft.com/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data) |  [Efterlevnad per tjänst, plats & bransch](https://www.microsoft.com/trustcenter/Compliance/default.aspx) |[Hur Microsoft skyddar kund information i Azure-tjänster](https://www.microsoft.com/trustcenter/Transparency/default.aspx)|
|  [Svar på säkerhets incidenter](https://aka.ms/SecurityResponsepaper), [delat ansvar](https://aka.ms/sharedresponsibility) |[Stränga sekretess standarder](https://www.microsoft.com/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards) |  | [Granska certifiering för Azure-tjänster, OH-hubb](https://www.microsoft.com/trustcenter/Compliance/default.aspx)|

### <a name="features-to-secure-data-and-application"></a>Funktioner för att skydda data och program
Beroende på moln tjänst modellen finns det varierande ansvar för vem som ansvarar för att hantera säkerheten för programmet eller tjänsten. Det finns funktioner som är tillgängliga i Azure-plattformen för att hjälpa dig att uppfylla dessa ansvars områden via inbyggda funktioner och genom partner lösningar som kan distribueras till en Azure-prenumeration.

De inbyggda funktionerna är ordnade i sex funktionella områden: åtgärder, program, lagring, nätverk, beräkning och identitet. Ytterligare information om de funktioner och funktioner som är tillgängliga i Azure-plattformen i de här sex områdena finns i sammanfattnings information.

## <a name="operations"></a>Åtgärder
Det här avsnittet innehåller ytterligare information om viktiga funktioner i säkerhets åtgärder och sammanfattnings information om dessa funktioner.

### <a name="security-and-audit-dashboard"></a>Säkerhet och granskning instrument panel
[Säkerhet och granskning-lösningen](../../security-center/security-center-intro.md) ger en omfattande översikt över din ORGANISATIONs IT-position med [inbyggda Sök frågor](https://blogs.technet.microsoft.com/msoms/2016/01/21/easy-microsoft-operations-management-suite-search-queries/) för viktiga problem som kräver din uppmärksamhet. [Säkerhet och granskning](https://technet.microsoft.com/library/mt484091.aspx) -instrumentpanelen är Start skärmen för allt som är relaterat till säkerhet i Azure Monitor loggar. Den ger bättre inblick i säkerhets läget för dina datorer. Här kan du också visa alla händelser från de senaste 24 timmarna, sju dagarna eller ett annat anpassat tidsintervall.

Dessutom kan du konfigurera säkerhet & efterlevnad för att [automatiskt utföra vissa åtgärder](https://blogs.technet.microsoft.com/robdavies/2016/04/20/simple-look-at-oms-alert-remediation-with-runbooks-part-1/) när en enskild händelse identifieras.

### <a name="azure-resource-manager"></a>Azure Resource Manager
Med [Azure Resource Manager](../../azure-resource-manager/management/deployment-models.md) kan du arbeta med resurserna i din lösning som en grupp. Du kan distribuera, uppdatera eller ta bort alla resurser i lösningen i en enda, samordnad åtgärd. Du använder en [Azure Resource Manager mall](https://blogs.technet.microsoft.com/canitpro/2015/06/29/devops-basics-infrastructure-as-code-arm-templates/) för distribution och mallen kan fungera för olika miljöer, till exempel testning, mellanlagring och produktion. Resource Manager tillhandahåller säkerhets-, gransknings- och taggningsfunktioner som hjälper dig att hantera dina resurser efter distributionen.

Azure Resource Manager mallbaserade distributioner hjälper till att förbättra säkerheten för lösningar som distribueras i Azure, eftersom standardinställningarna för säkerhets kontroll och kan integreras i standardiserade mallbaserade distributioner. Detta minskar risken för säkerhets konfigurations fel som kan äga rum under manuella distributioner.

### <a name="application-insights"></a>Application Insights
[Application Insights](https://docs.microsoft.com/azure/application-insights/) är en utöknings bar APM-tjänst (Application Performance Management) för webbutvecklare. Med Application Insights kan du övervaka dina Live-webbprogram och automatiskt identifiera prestanda avvikelser. Den innehåller kraftfulla analys verktyg som hjälper dig att diagnostisera problem och förstå vad användare faktiskt gör med dina appar. Den övervakar ditt program hela tiden den körs, både under testningen och när du har publicerat eller distribuerat det.

Application Insights skapar diagram och tabeller som visar dig, till exempel vilka tider på dagen du får de flesta användare, hur appen är och hur väl den betjänas av externa tjänster som den är beroende av.

Om det uppstår krascher, fel eller prestanda problem kan du söka igenom telemetridata i detalj för att diagnosticera orsaken. Tjänsten skickar dig e-postmeddelanden om det finns några ändringar i appens tillgänglighet och prestanda. Program insikter blir därmed ett värdefullt säkerhets verktyg eftersom det hjälper till med tillgänglighet i säkerhets Triad sekretess, integritet och tillgänglighet.

### <a name="azure-monitor"></a>Azure Monitor
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) erbjuder visualisering, frågor, routning, avisering, automatisk skalning och automatisering av data både från Azure-infrastrukturen ([aktivitets loggen](../../azure-monitor/platform/platform-logs-overview.md)) och varje enskild Azure-resurs ([diagnostikloggar](../../azure-monitor/platform/platform-logs-overview.md)). Du kan använda Azure Monitor för att varna dig om säkerhetsrelaterade händelser som genereras i Azure-loggar.

### <a name="azure-monitor-logs"></a>Azure Monitor-loggar
[Azure Monitor loggar](https://azure.microsoft.com/documentation/services/log-analytics/) – tillhandahåller en IT-hanterings lösning för både lokal infrastruktur baserad på plats och från tredje part (till exempel AWS), utöver Azure-resurser. Data från Azure Monitor kan dirigeras direkt till Azure Monitor loggar så att du kan se mått och loggar för hela miljön på ett och samma ställe.

Azure Monitor-loggar kan vara ett användbart verktyg i kriminal tekniska och andra säkerhets analyser, eftersom verktyget gör det möjligt att snabbt söka igenom stora mängder säkerhetsrelaterade poster med en flexibel fråge metod. Dessutom [kan lokala brand väggar och proxy-loggar exporteras till Azure och göras tillgängliga för analys med hjälp av Azure Monitor loggar.](../../log-analytics/log-analytics-agent-windows.md)

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](../../advisor/index.yml) är en anpassad moln konsult som hjälper dig att optimera dina Azure-distributioner. Den analyserar din resurskonfiguration och användningstelemetri. Det rekommenderar sedan lösningar för att förbättra [prestanda](../../advisor/advisor-performance-recommendations.md), [säkerhet](../../advisor/advisor-security-recommendations.md)och [hög tillgänglighet](../../advisor/advisor-high-availability-recommendations.md) för dina resurser samtidigt som du letar efter möjligheter att [minska dina totala Azure-utgifter](../../advisor/advisor-cost-recommendations.md). Azure Advisor ger säkerhets rekommendationer, vilket avsevärt kan förbättra den övergripande säkerhets position för lösningar som du distribuerar i Azure. Dessa rekommendationer hämtas från säkerhets analys som utförs av [Azure Security Center.](../../security-center/security-center-intro.md)

### <a name="azure-security-center"></a>Azure Security Center
[Security Center](../../security-center/security-center-intro.md) hjälper dig att förhindra, identifiera och svara på hot med ökad insyn i och kontroll över säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Dessutom kan Security Center hjälpa till med säkerhets åtgärder genom att tillhandahålla en enda instrument panel som innehåller aviseringar och rekommendationer som kan utföras direkt. Ofta kan du åtgärda problem med ett enda klick i Security Center-konsolen.
## <a name="applications"></a>Program
Avsnittet innehåller ytterligare information om viktiga funktioner i program säkerhet och sammanfattnings information om dessa funktioner.

### <a name="web-application-vulnerability-scanning"></a>Genomsökning av webb program sårbarhet
Ett av de enklaste sätten att komma igång med att testa om säkerhets risker i [App Service-appen](../../app-service/overview.md) är att använda [integrering med TINFOIL-säkerhet](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) för att utföra en sårbarhets sökning i ett enda klick i din app. Du kan visa test resultaten i en lättanvänd rapport och lära dig hur du åtgärdar varje sårbarhet med stegvisa instruktioner.

### <a name="penetration-testing"></a>Testning av inträngning
Om du föredrar att utföra dina egna inläsnings test eller om du vill använda ett annat skanner paket eller en annan leverantör måste du följa [godkännande processen för Azure-inträngning](https://docs.microsoft.com/azure/security/fundamentals/pen-testing ) och få ett tidigare godkännande för att kunna utföra önskade inläsnings test.

### <a name="web-application-firewall"></a>Brand vägg för webbaserade program
Brand väggen för webbaserade program (WAF) i [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) skyddar webb program från vanliga webbaserade attacker som SQL-inmatning, Cross-Site skript attacker och kapning av sessioner. Den är förkonfigurerad med skydd mot hot som identifieras av [OWASP (Open Web Application Security Project) som de viktigaste 10 vanligaste säkerhets problemen](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

### <a name="authentication-and-authorization-in-azure-app-service"></a>Autentisering och auktorisering i Azure App Service
[App Service autentisering/auktorisering](../../app-service/overview-authentication-authorization.md) är en funktion som gör att ditt program kan logga in användare så att du inte behöver ändra kod på appens Server del. Det är ett enkelt sätt att skydda ditt program och arbeta med data per användare.

### <a name="layered-security-architecture"></a>Skiktad säkerhets arkitektur
Eftersom [App Service miljöer](../../app-service/environment/app-service-app-service-environment-intro.md) tillhandahåller en isolerad körnings miljö som distribueras till en [Azure-Virtual Network](../../virtual-network/virtual-networks-overview.md)kan utvecklare skapa en lager säkerhets arkitektur som ger olika nivåer av nätverks åtkomst för varje program nivå. En vanlig önskan är att dölja API-backend-ändar från allmän Internet åtkomst och bara tillåta att API: er anropas av överordnade webb program. [Nätverks säkerhets grupper (NSG: er)](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/) kan användas på Azure Virtual Network-undernät som innehåller App Service miljöer för att begränsa offentlig åtkomst till API-program.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Diagnostik för webb server och Programdiagnostik
App Service Web Apps innehåller diagnostiska funktioner för loggning av information från både webb servern och webb programmet. De är logiskt åtskilda i [Web Server Diagnostics](../../app-service/troubleshoot-diagnostic-logs.md) och [Application Diagnostics](https://technet.microsoft.com/library/hh530058(v=sc.12).aspx). Webb servern innehåller två viktiga framsteg vid diagnostisering och fel sökning av webbplatser och program.

Den första nya funktionen är information i real tid om programpooler, arbets processer, platser, program domäner och körnings begär Anden. De andra nya fördelarna är de detaljerade spårnings händelser som spårar en begäran under hela processen för begäran och svar.

Om du vill aktivera insamling av dessa spårnings händelser kan IIS 7 konfigureras för att automatiskt fånga fullständiga spårnings loggar i XML-format, för alla specifika begär Anden baserat på förfluten tid eller fel svars koder.

#### <a name="web-server-diagnostics"></a>Diagnostik för webb server
Du kan aktivera eller inaktivera följande typer av loggar:

-   Detaljerad fel loggning-detaljerad fel information för HTTP-statuskod som indikerar ett fel (status kod 400 eller senare). Detta kan innehålla information som kan hjälpa dig att avgöra varför servern returnerade felkoden.

-   Spårning av misslyckade begär Anden-detaljerad information om misslyckade förfrågningar, inklusive spårning av IIS-komponenter som används för att bearbeta begäran och den tid som tagits i varje komponent. Detta kan vara användbart om du försöker öka plats prestandan eller om du vill isolera vad som orsakar att ett särskilt HTTP-fel returneras.

-   Webb Server loggning – information om HTTP-transaktioner med utökat logg fils format för W3C. Detta är användbart när du fastställer allmänna plats mått, till exempel antalet begär Anden som hanteras eller hur många begär Anden som kommer från en speciell IP-adress.

#### <a name="application-diagnostics"></a>Programdiagnostik
Med [Application Diagnostics](../../app-service/troubleshoot-diagnostic-logs.md) kan du samla in information som produceras av ett webb program. ASP.NET-program kan använda klassen [system. Diagnostics. trace](https://msdn.microsoft.com/library/system.diagnostics.trace) för att logga information i Application Diagnostics-loggen. I Programdiagnostik finns det två huvudtyper för händelser: de som är relaterade till programprestanda och de som är relaterade till programfel. Felen kan delas upp ytterligare i underavdelningar som anslutning, säkerhet och fel. Underavdelningen för fel är ofta relaterad till problem med programkoden.

I Programdiagnostik kan du gruppera händelser på följande vis:

-   Alla (visar alla händelser)
-   Programfel (visar undantagshändelser)
-   Prestanda (visar prestandahändelser)

## <a name="storage"></a>Storage
Avsnittet innehåller ytterligare information om viktiga funktioner i Azure Storage Security och sammanfattnings information om dessa funktioner.

### <a name="role-based-access-control-rbac"></a>Rollbaserad åtkomstkontroll (RBAC)
Du kan skydda ditt lagrings konto med hjälp av rollbaserad Access Control (RBAC). Att begränsa åtkomsten baserat på [behovet av att känna till](https://en.wikipedia.org/wiki/Need_to_know) och [minsta behörighets](https://en.wikipedia.org/wiki/Principle_of_least_privilege) säkerhets principer är att det är absolut nödvändigt för organisationer som vill tillämpa säkerhets principer för data åtkomst. Dessa behörigheter beviljas genom att tilldela lämplig RBAC-roll till grupper och program i ett visst omfång. Du kan använda [Inbyggda RBAC-roller](../../role-based-access-control/built-in-roles.md), till exempel lagrings konto deltagare, för att tilldela behörigheter till användare. Åtkomst till lagrings nycklarna för ett lagrings konto med hjälp av [Azure Resource Manager](../../storage/blobs/security-recommendations.md) modellen kan styras via rollbaserad Access Control (RBAC).

### <a name="shared-access-signature"></a>Signatur för delad åtkomst
En [signatur för delad åtkomst (Shared Access Signature, SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md) ger delegerad åtkomst till resurser på ditt lagringskonto. SAS innebär att du kan bevilja en klient begränsad behörighet till objekt i ditt lagrings konto under en angiven period och med en angiven uppsättning behörigheter. Du kan bevilja dessa begränsade behörigheter utan att behöva dela dina konto åtkomst nycklar.

### <a name="encryption-in-transit"></a>Kryptering vid överföring
Kryptering under överföring är en mekanism för att skydda data när de överförs mellan nätverk. Med Azure Storage kan du skydda data med:
-   [Kryptering på transport nivå](../../storage/blobs/security-recommendations.md), till exempel https när du överför data till eller från Azure Storage.

-   [Kabel kryptering](../../storage/blobs/security-recommendations.md), till exempel [SMB 3,0-kryptering](../../storage/blobs/security-recommendations.md) för [Azure-filresurser](../../storage/files/storage-dotnet-how-to-use-files.md).

-   Kryptering på klient sidan, för att kryptera data innan de överförs till lagrings utrymmet och för att dekryptera data när de har överförts från lagrings utrymmet.

### <a name="encryption-at-rest"></a>Vilande kryptering
För många organisationer är data kryptering i vila ett obligatoriskt steg mot data sekretess, efterlevnad och data suveränitet. Det finns tre funktioner för Azure Storage-säkerhet som ger kryptering av data som är "i vila":

-   Med [kryptering för lagringstjänst](../../storage/common/storage-service-encryption.md) kan du begära att lagrings tjänsten automatiskt krypterar data när de skrivs till Azure Storage.

-   [Kryptering på klient sidan](../../storage/common/storage-client-side-encryption.md) tillhandahåller även funktionen för kryptering i vila.

-   Med [Azure Disk Encryption](../azure-security-disk-encryption-overview.md) kan du kryptera de OS-diskar och data diskar som används av en virtuell IaaS-dator.

### <a name="storage-analytics"></a>Lagringsanalys
[Azure-lagringsanalys](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) utför loggning och tillhandahåller mått data för ett lagrings konto. Du kan använda dessa data för att spåra begär Anden, analysera användnings trender och diagnostisera problem med ditt lagrings konto. Lagringsanalys loggar detaljerad information om lyckade och misslyckade begäranden till en lagringstjänst. Den här informationen kan användas för att övervaka enskilda begäranden och för att diagnostisera problem med en lagringstjänst. Begär Anden loggas med bästa möjliga ansträngning. Följande typer av autentiserade begäranden loggas:
-   Lyckade förfrågningar.

-   Misslyckade förfrågningar, inklusive timeout, begränsning, nätverk, auktorisering och andra fel.

-   Begär Anden som använder en signatur för delad åtkomst (SAS), inklusive misslyckade och lyckade förfrågningar.

-   Begär Anden för analys av data.

### <a name="enabling-browser-based-clients-using-cors"></a>Aktivera webbläsarbaserade klienter med CORS
[Resurs delning mellan ursprung (CORS)](https://docs.microsoft.com/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) är en mekanism som gör det möjligt för domäner att ge varandra behörigheter för åtkomst till var and ras resurser. Användar agenten skickar extra huvuden för att säkerställa att JavaScript-koden som läses in från en viss domän får åtkomst till resurser som finns i en annan domän. Den senare domänen svarar sedan med extra huvuden som tillåter eller nekar den ursprungliga domänen åtkomst till resurserna.

Azure Storage-tjänster stöder nu CORS så att när du har angett CORS-reglerna för tjänsten utvärderas en korrekt autentiserad begäran mot tjänsten från en annan domän för att avgöra om den är tillåten enligt de regler som du har angett.

## <a name="networking"></a>Nätverk
Avsnittet innehåller ytterligare information om viktiga funktioner i Azure nätverks säkerhet och sammanfattnings information om dessa funktioner.

### <a name="network-layer-controls"></a>Kontroller för nätverks lager
Nätverks åtkomst kontroll är en begränsning av anslutningen till och från vissa enheter eller undernät och utgör kärnan i nätverks säkerheten. Målet med nätverks åtkomst kontroll är att se till att dina virtuella datorer och tjänster är tillgängliga enbart för användare och enheter som du vill att de ska vara tillgängliga för.

#### <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper
En [nätverks säkerhets grupp (NSG)](../../virtual-network/virtual-network-vnet-plan-design-arm.md) är en grundläggande tillstånds känslig brand vägg för paket filtrering och du kan styra åtkomsten baserat på en [5-tupel](https://www.techopedia.com/definition/28190/5-tuple). NSG: er tillhandahåller inte kontroll av program lager eller autentiserade åtkomst kontroller. De kan användas för att styra trafiken mellan undernät i en Azure-Virtual Network och trafik mellan en Azure-Virtual Network och Internet.

#### <a name="route-control-and-forced-tunneling"></a>Flödes kontroll och Tvingad tunnel trafik
Möjligheten att styra routnings beteendet på dina virtuella Azure-nätverk är en viktig funktion för nätverks säkerhet och åtkomst kontroll. Om du till exempel vill se till att all trafik till och från din Azure-Virtual Network går igenom den virtuella säkerhetsenheten måste du kunna styra och anpassa Dirigerings beteendet. Det kan du göra genom att konfigurera användardefinierade vägar i Azure.

Med [användardefinierade vägar](../../virtual-network/virtual-networks-udr-overview.md) kan du anpassa inkommande och utgående vägar för trafik som går in i och ut ur enskilda virtuella datorer eller undernät för att se till att den mest säkra vägen är möjlig. [Tvingad tunnel trafik](https://www.petri.com/azure-forced-tunneling) är en mekanism som du kan använda för att se till att dina tjänster inte har tillåtelse att initiera en anslutning till enheter på Internet.

Detta skiljer sig från att kunna acceptera inkommande anslutningar och sedan svara på dem. Frontend-webbservrar måste svara på begär Anden från Internet värdar, så att Internet-källan tillåts inkommande till dessa webb servrar och webb servrarna kan svara.

Tvingad tunnel trafik används ofta för att tvinga utgående trafik till Internet att gå igenom lokala säkerhetsproxyservrar och brand väggar.

#### <a name="virtual-network-security-appliances"></a>Virtual Network säkerhets anordningar
Även om nätverks säkerhets grupper, användardefinierade vägar och Tvingad tunnel trafik tillhandahåller en säkerhets nivå för nätverks-och transport skikt i [OSI-modellen](https://en.wikipedia.org/wiki/OSI_model), kan det finnas tillfällen när du vill aktivera säkerhet på högre nivåer i stacken. Du kan komma åt dessa utökade nätverks säkerhetsfunktioner med hjälp av en Azure-partner nätverks säkerhetsenhets lösning. Du hittar de mest aktuella Azure Partner Network Security-lösningarna genom att gå till [Azure Marketplace](https://azure.microsoft.com/marketplace/) och söka efter "säkerhet" och "nätverks säkerhet".

### <a name="azure-virtual-network"></a>Azure Virtual Network
Ett Azure-virtuellt nätverk (VNet) är en representation av ditt eget nätverk i molnet. Det är en logisk isolering av Azure Network Fabric som är dedikerad för din prenumeration. Du kan helt styra IP-adressblocken, DNS-inställningarna, säkerhetsprinciperna och routingtabellerna inom det här nätverket. Du kan segmentera ditt VNet i undernät och placera virtuella Azure IaaS-datorer (VM) och/eller [moln tjänster (PaaS roll instanser)](../../cloud-services/cloud-services-choose-me.md) i virtuella Azure-nätverk.

Du kan dessutom ansluta det virtuella nätverket till ditt lokala nätverk med ett av [anslutningsalternativen](../../vpn-gateway/index.yml) som finns i Azure. I princip kan du expandera ditt nätverk till Azure, med fullständig kontroll över IP-adressblock och de fördelar som Azure på företagsnivå erbjuder.

Azure-nätverk stöder olika scenarier för säker fjärråtkomst. Några av dessa är:

-   [Ansluta enskilda arbets stationer till en Azure-Virtual Network](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

-   [Ansluta ett lokalt nätverk till ett Azure-Virtual Network med en VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

-   [Ansluta ett lokalt nätverk till ett Azure-Virtual Network med en särskild WAN-länk](../../expressroute/expressroute-introduction.md)

-   [Ansluta virtuella Azure-nätverk till varandra](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

### <a name="vpn-gateway"></a>VPN Gateway
Om du vill skicka nätverks trafik mellan Azure-Virtual Network och din lokala plats måste du skapa en VPN-gateway för Azure-Virtual Network. En [VPN-gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md) är en typ av virtuell nätverksgateway som skickar krypterad trafik över en offentlig anslutning. Du kan också använda VPN-gatewayer för att skicka trafik mellan virtuella Azure-nätverk via Azures nätverks infrastruktur resurs.

### <a name="express-route"></a>Express Route
Microsoft Azure [ExpressRoute](../../expressroute/expressroute-introduction.md) är en särskild WAN-länk som du kan använda för att utöka dina lokala nätverk till Microsoft-molnet via en dedikerad privat anslutning som under lättas av en anslutnings leverantör.

![Express Route](./media/overview/azure-security-fig1.png)

Med ExpressRoute kan du upprätta anslutningar till Microsofts molntjänster, till exempel Microsoft Azure, Office 365 och CRM Online. Anslutningen kan vara från ett ”any-to-any”-nätverk (IP VPN), ett ”point-to-point”-nätverk med Ethernet eller en virtuell korsanslutning via en anslutningsleverantör på en samlokaliseringsanläggning.

ExpressRoute-anslutningar går inte via det offentliga Internet och kan därför anses vara säkrare än VPN-baserade lösningar. Det innebär att ExpressRoute-anslutningar är tillförlitligare, snabbare, har kortare svarstider och högre säkerhet än vanliga anslutningar över Internet.


### <a name="application-gateway"></a>Application Gateway
Microsoft [Azure Application Gateway](../../application-gateway/overview.md) tillhandahåller en [program leverans kontroll (ADC)](https://en.wikipedia.org/wiki/Application_delivery_controller) som en tjänst som erbjuder olika belastnings Utjämnings funktioner i Layer 7 för ditt program.

![Application Gateway](./media/overview/azure-security-fig2.png)

Det gör att du kan optimera webb server gruppens produktivitet genom att avlasta CPU-intensiva TLS-avslutning till Application Gateway (kallas även "TLS-avläsning" eller "TLS bryggning"). Det ger även andra funktioner för Layer 7-routning, inklusive resursallokerings distribution av inkommande trafik, cookie-baserad sessionsgräns, URL-sökväg baserad Routning och möjligheten att vara värd för flera webbplatser bakom en enda Application Gateway. Azure Application Gateway är en Layer 7-lastbalanserare.

Den tillhandahåller redundans och prestandabaserad routning av HTTP-begäranden mellan olika servrar, oavsett om de finns i molnet eller lokalt.

Programmet innehåller många ADC-funktioner (Application Delivery Controller), inklusive HTTP-belastnings utjämning, mappning mellan cookie-baserade sessioner, [TLS-avlastning](../../application-gateway/tutorial-restrict-web-traffic-powershell.md), anpassade hälso avsökningar, stöd för flera platser och många andra.

### <a name="web-application-firewall"></a>Brandvägg för webbaserade program
Brand väggen för webbaserade program är en funktion i [Azure Application Gateway](../../application-gateway/overview.md) som ger skydd till webb program som använder Application Gateway för standard funktioner för program leverans kontroll (ADC). Brandvägg för webbaserade program gör detta genom att skydda dem mot de flesta av de 10 vanligaste OWASP-säkerhetsproblemen.

![Brandvägg för webbaserade program](./media/overview/azure-security-fig1.png)

-   Skydd mot SQL-inmatning

-   Skydd mot vanliga webbattacker, som kommandoinmatning, dold HTTP-begäran, delning av HTTP-svar och attack genom införande av fjärrfil

-   Skydd mot åtgärder som inte följer HTTP-protokollet

-   Skydd mot avvikelser i HTTP-protokollet som att användaragent för värden och accept-huvud saknas

-   Skydd mot robotar, crawlers och skannrar

-   Identifiering av vanliga program konfigurationer (dvs. Apache, IIS osv.)

En centraliserad brandvägg mot webbattacker underlättar säkerhetshanteringen och håller programmet bättre skyddat mot intrång. En brandväggslösning för webbaserade program kan även reagera snabbare på ett säkerhetshot genom att åtgärda en känd svaghet på en central plats jämfört med om korrigeringar ska utföras i varje enskilt webbprogram. Befintliga programgatewayer kan enkelt konverteras till en programgateway med brandvägg för webbprogram.

### <a name="traffic-manager"></a>Traffic Manager
Med Microsoft [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) kan du styra distributionen av användar trafik för tjänst slut punkter i olika data Center. Tjänst slut punkter som stöds av Traffic Manager inkluderar virtuella Azure-datorer, Web Apps och moln tjänster. Du kan även använda Traffic Manager med externa slutpunkter som inte är Azure-relaterade. Traffic Manager använder Domain Name System (DNS) för att dirigera klient begär anden till den lämpligaste slut punkten baserat på en [Traffic-routningsmetod](../../traffic-manager/traffic-manager-routing-methods.md) och tillståndet för slut punkterna.

Traffic Manager tillhandahåller en mängd metoder för trafik cirkulation som passar olika program behov, [övervakning](../../traffic-manager/traffic-manager-monitoring.md)av slut punkts hälsa och automatisk redundans. Traffic Manager har bra återhämtningsförmåga i händelse av fel, inklusive fel som påverkar en hel Azure-region.

### <a name="azure-load-balancer"></a>Azure Load Balancer
[Azure Load Balancer](../../load-balancer/load-balancer-overview.md) levererar hög tillgänglighet och nätverksprestanda till dina program. Det är en belastningsutjämnare för Layer 4 (TCP, UDP) som distribuerar inkommande trafik mellan felfria instanser av tjänster som definierats i en belastningsutjämnad uppsättning. Azure Load Balancer kan konfigureras för att:

-   Belastningsutjämna inkommande Internet trafik till virtuella datorer. Den här konfigurationen kallas för [offentlig belastnings utjämning](../../load-balancer/components.md#frontend-ip-configurations).

-   Belastnings Utjämnings trafik mellan virtuella datorer i ett virtuellt nätverk, mellan virtuella datorer i moln tjänster eller mellan lokala datorer och virtuella datorer i ett virtuellt nätverk mellan olika platser. Den här konfigurationen kallas [intern belastnings utjämning](../../load-balancer/components.md#frontend-ip-configurations).

- Vidarebefordra extern trafik till en enskild virtuell dator

### <a name="internal-dns"></a>Intern DNS
Du kan hantera listan över DNS-servrar som används i ett VNet i Hanteringsportal eller i nätverks konfigurations filen. Kunden kan lägga till upp till 12 DNS-servrar för varje VNet. När du anger DNS-servrar är det viktigt att kontrol lera att du visar kundens DNS-servrar i rätt ordning för kundens miljö. DNS-serverns listor fungerar inte med Round-Robin. De används i den ordning som de anges. Om den första DNS-servern i listan kan nås använder klienten den DNS-servern oavsett om DNS-servern fungerar som den ska eller inte. Om du vill ändra DNS-serverns ordning för kundens virtuella nätverk tar du bort DNS-servrarna från listan och lägger till dem igen i den ordning som kunden vill. DNS stöder tillgänglighets aspekten för säkerhets Triad "CIA".

### <a name="azure-dns"></a>Azure DNS
[Domain Name System](https://technet.microsoft.com/library/bb629410.aspx)eller DNS ansvarar för översättning (eller matchning) av en webbplats eller ett tjänst namn till dess IP-adress. [Azure DNS](../../dns/dns-overview.md) är en värdtjänst för DNS-domäner som ger namnmatchning med hjälp av Microsoft Azure-infrastrukturen. Genom att använda Azure som värd för dina domäner kan du hantera dina DNS-poster med samma autentiseringsuppgifter, API:er, verktyg och fakturering som för dina andra Azure-tjänster. DNS stöder tillgänglighets aspekten för säkerhets Triad "CIA".

### <a name="azure-monitor-logs-nsgs"></a>Azure Monitor loggar NSG: er
Du kan aktivera följande diagnostiska logg kategorier för NSG: er:

-   Händelse: innehåller poster för vilka NSG-regler tillämpas på virtuella datorer och instans roller baserade på MAC-adress. Status för dessa regler samlas in var 60: e sekund.

-   Regel räknare: innehåller poster för hur många gånger varje NSG-regel används för att neka eller tillåta trafik.

### <a name="security-center"></a>Security Center
[Azure Security Center](../../security-center/security-center-intro.md) fort löp ande analyserar säkerhets läget för dina Azure-resurser för metod tips för nätverks säkerhet. När Security Center identifierar potentiella säkerhets risker skapar den [rekommendationer](../../security-center/security-center-recommendations.md) som vägleder dig genom processen att konfigurera de nödvändiga kontrollerna för att skärp och skydda dina resurser.

## <a name="compute"></a>Compute
Avsnittet innehåller ytterligare information om viktiga funktioner i det här området och sammanfattnings information om dessa funktioner.

### <a name="antimalware--antivirus"></a>Antivirus program & Antivirus
Med Azure IaaS kan du använda program mot skadlig kod från säkerhets leverantörer som Microsoft, Symantec, Trend Micro, McAfee och Kasper Sky för att skydda dina virtuella datorer från skadliga filer, annons program och andra hot. [Microsoft Antimalware](antimalware.md) för Azure Cloud Services och Virtual Machines är en skydds funktion som hjälper dig att identifiera och ta bort virus, spionprogram och annan skadlig program vara. Microsoft Antimalware tillhandahåller konfigurerbara aviseringar när kända skadlig eller oönskad program vara försöker installera sig själv eller köra på dina Azure-system. Microsoft Antimalware kan också distribueras med hjälp av Azure Security Center

### <a name="hardware-security-module"></a>Modul för maskin varu säkerhet
Kryptering och autentisering förbättrar inte säkerheten om inte själva nycklarna är skyddade. Du kan förenkla hanteringen och säkerheten för viktiga hemligheter och nycklar genom att lagra dem i [Azure Key Vault](../../key-vault/general/overview.md). Key Vault ger dig möjlighet att lagra dina nycklar i HSM: er (Hardware Security modules) som är certifierade enligt standarden FIPS 140-2 på nivå 2. Dina SQL Server krypterings nycklar för säkerhets kopiering eller [transparent data kryptering](https://msdn.microsoft.com/library/bb934049.aspx) kan lagras i Key Vault med alla nycklar och hemligheter från dina program. Behörigheter och åtkomst till dessa skyddade objekt hanteras via [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

### <a name="virtual-machine-backup"></a>Säkerhets kopiering av virtuell dator
[Azure Backup](../../backup/backup-overview.md) är en lösning som skyddar dina program data med noll kapital investeringar och minimala drifts kostnader. Program fel kan skada dina data och mänskliga fel kan leda till buggar i dina program som kan leda till säkerhets problem. Med Azure Backup skyddas dina virtuella datorer som kör Windows och Linux.

### <a name="azure-site-recovery"></a>Azure Site Recovery
En viktig del av organisationens strategi för [affärs kontinuitet/haveri beredskap (BCDR)](../../best-practices-availability-paired-regions.md) är att ta reda på hur du håller företags arbets belastningar och appar igång när de planerade och oplanerade avbrott uppstår. [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) hjälper till att dirigera replikering, redundans och återställning av arbets belastningar och appar så att de är tillgängliga från en annan plats om din primära plats slutar fungera.

### <a name="sql-vm-tde"></a>VIRTUELL SQL-TDE
[Transparent data kryptering (TDE)](../../virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault.md) och CLE (Column Level Encryption) är krypterings funktioner i SQL Server. Den här typen av kryptering kräver att kunderna hanterar och lagrar de kryptografiska nycklar som du använder för kryptering.

Tjänsten Azure Key Vault (AKV) är utformad för att förbättra säkerheten och hanteringen av dessa nycklar på en säker plats med hög tillgänglighet. Med SQL Server-anslutning kan SQL Server använda nycklarna från Azure Key Vault.

Om du kör SQL Server med lokala datorer finns det steg du kan följa för att få åtkomst till Azure Key Vault från din lokala SQL Server dator. Men för SQL Server i virtuella Azure-datorer kan du spara tid genom att använda funktionen för Azure Key Vault-integrering. Med några Azure PowerShell-cmdletar för att aktivera den här funktionen, kan du automatisera konfigurationen som krävs för en virtuell SQL-dator för att få åtkomst till ditt nyckel valv.

### <a name="vm-disk-encryption"></a>VM Disk Encryption
[Azure Disk Encryption](../azure-security-disk-encryption-overview.md) är en ny funktion som hjälper dig att kryptera dina Windows-och Linux-IaaS virtuella dator diskar. Den tillämpar standarden för BitLocker-funktionen i Windows och funktionen DM-crypt i Linux för att tillhandahålla volym kryptering för operativ systemet och data diskarna. Lösningen är integrerad med Azure Key Vault som hjälper dig att styra och hantera disk krypterings nycklar och hemligheter i din Key Vault prenumeration. Lösningen ser också till att alla data på de virtuella dator diskarna krypteras i vila i Azure Storage.

### <a name="virtual-networking"></a>Virtuellt nätverk
Virtuella datorer behöver nätverks anslutning. För att stödja detta krav kräver Azure att virtuella datorer är anslutna till en Azure-Virtual Network. En Azure-Virtual Network är en logisk konstruktion som byggts ovanpå den fysiska Azures nätverks infrastruktur resurs. Varje logisk [Azure-Virtual Network](../../virtual-network/virtual-networks-overview.md) isoleras från alla andra virtuella Azure-nätverk. Den här isoleringen hjälper till att säkerställa att nätverks trafiken i dina distributioner inte är tillgänglig för andra Microsoft Azure kunder.

### <a name="patch-updates"></a>Uppdaterings uppdateringar
Uppdaterings uppdateringar ger grunden för att hitta och åtgärda potentiella problem och förenkla hanteringen av program uppdaterings processen, både genom att minska antalet program uppdateringar som du måste distribuera i företaget och öka din möjlighet att övervaka efterlevnad.

### <a name="security-policy-management-and-reporting"></a>Hantering och rapportering av säkerhets principer
[Security Center](../../security-center/security-center-intro.md) hjälper dig att förhindra, identifiera och svara på hot, och ger dig ökad insyn i och kontroll över säkerheten för dina Azure-resurser. Den ger integrerad säkerhetsövervakning och princip hantering över dina Azure-prenumerationer, hjälper till att upptäcka hot som annars kan gå vidare och fungerar med ett brett eko system med säkerhetslösningar.

## <a name="identity-and-access-management"></a>Identitets- och åtkomsthantering
Att skydda system, program och data börjar med identitetsbaserade åtkomst kontroller. Funktionerna för identitets-och åtkomst hantering som är inbyggda i Microsofts affärs produkter och tjänster hjälper till att skydda din organisations och personliga information från obehörig åtkomst samtidigt som du gör den tillgänglig för legitima användare närhelst och var de behöver dem.

### <a name="secure-identity"></a>Skydda identitet
Microsoft använder flera säkerhets metoder och tekniker för sina produkter och tjänster för att hantera identitet och åtkomst.

-   [Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) kräver att användare använder flera metoder för åtkomst, lokalt och i molnet. Den ger stark autentisering med en rad enkla verifierings alternativ, samtidigt som användarna får en enkel inloggnings process.

-   [Microsoft Authenticator](https://aka.ms/authenticator) ger en användarvänlig Multi-Factor Authentication upplevelse som fungerar med både Microsoft Azure Active Directory-och Microsoft-konton och har stöd för wearables-och finger avtrycks-baserade godkännanden.

-   [Tvång av lösen ords principer](https://azure.microsoft.com/documentation/articles/active-directory-passwords-policy/) ökar säkerheten för traditionella lösen ord genom att krav på längd och komplexitet, framtvingad regelbunden rotation och konto utelåsning efter misslyckade autentiseringsförsök gjordes.

-   [Tokenbaserad autentisering](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/) möjliggör autentisering via Azure Active Directory.

-   Med [rollbaserad åtkomst kontroll (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/) kan du bevilja åtkomst baserat på användarens tilldelade roll, vilket gör det enkelt att ge användare bara den mängd åtkomst som de behöver för att utföra sina jobb uppgifter. Du kan anpassa RBAC enligt organisationens affärs modell och risk tolerans.

-   Med [integrerad identitets hantering (hybrid identitet)](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/) kan du behålla kontrollen över användarnas åtkomst till interna Data Center och moln plattformar, vilket skapar en enda användar identitet för autentisering och auktorisering till alla resurser.

### <a name="secure-apps-and-data"></a>Säkra appar och data
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)en omfattande moln lösning för identitets-och åtkomst hantering, hjälper till att skydda åtkomsten till data i program på plats och i molnet och fören klar hanteringen av användare och grupper. Den kombinerar kärn katalog tjänster, avancerad identitets styrning, säkerhet och program åtkomst hantering, och gör det enkelt för utvecklare att bygga principbaserad identitets hantering i sina appar. För att förbättra ditt Azure Active Directory kan du lägga till betalfunktioner med hjälp av Azure Active Directory Basic-, Premium P1- och Premium P2-versionerna.

| Kostnads fria/vanliga funktioner     | Grundläggande funktioner    |Premium P1-funktioner |Premium P2-funktioner | Azure Active Directory koppling – endast Windows 10-relaterade funktioner|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Katalog objekt](../../active-directory/active-directory-whatis.md), [hantering av användare/grupper (Lägg till/uppdatera/ta bort)/användarbaserad etablering, enhets registrering](../../active-directory/active-directory-whatis.md), [enkel inloggning (SSO)](../../active-directory/active-directory-whatis.md), [lösen ords ändring via självbetjäning för moln användare](../../active-directory/active-directory-whatis.md), [Anslut (synkronisering motor som utökar lokala kataloger till Azure Active Directory)](../../active-directory/active-directory-whatis.md), [säkerhets-/användnings rapporter](../../active-directory/active-directory-whatis.md)       |   [Gruppbaserad åtkomst hantering/-etablering](../../active-directory/active-directory-whatis.md), [självbetjäning för återställning av lösen ord för moln användare](../../active-directory/active-directory-whatis.md), [företags anpassning (inloggnings sidor/åtkomst panel anpassning)](../../active-directory/active-directory-whatis.md), [programproxy](../../active-directory/active-directory-whatis.md), [SLA 99,9%](../../active-directory/active-directory-whatis.md) |  [Självbetjänings grupp och program hantering/självbetjäning för program tillägg/dynamiska grupper](../../active-directory/active-directory-whatis.md), [lösen ords återställning via självbetjäning/ändring/upplåsning med lokal skriv](../../active-directory/active-directory-whatis.md)åtgärd, [Multi-Factor Authentication (molnet och lokalt (MFA Server))](../../active-directory/active-directory-whatis.md), [MIM Cal + MIM-Server](../../active-directory/active-directory-whatis.md), [Cloud App Discovery](../../active-directory/active-directory-whatis.md), [Connect Health](../../active-directory/active-directory-whatis.md), [Automatisk lösen ords förnyelse för grupp konton](../../active-directory/active-directory-whatis.md)|    [Identitets skydd](../../active-directory/identity-protection/overview.md), [Privileged Identity Management](../../active-directory/privileged-identity-management/pim-configure.md)|   [Anslut en enhet till Azure AD, Desktop SSO, Microsoft Passport för Azure AD, Administrator BitLocker-återställning](../../active-directory/active-directory-whatis.md), [MDM-automatisk registrering, självbetjäning BitLocker-återställning, ytterligare lokala administratörer till Windows 10-enheter via Azure AD Join](../../active-directory/active-directory-whatis.md)|


- [Cloud App Discovery](../../active-directory/cloudappdiscovery-get-started.md) är en Premium-funktion i Azure Active Directory som gör att du kan identifiera moln program som används av de anställda i din organisation.

- [Azure Active Directory Identity Protection](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) är en säkerhets tjänst som använder Azure Active Directory avvikelse identifierings funktioner för att tillhandahålla en sammanslagen vy över risk identifieringar och potentiella sårbarheter som kan påverka organisationens identiteter.

- Med [Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kan du ansluta virtuella Azure-datorer till en domän utan att behöva distribuera domänkontrollanter. Användarna loggar in på de virtuella datorerna med sina företags Active Directory autentiseringsuppgifter och kan få sömlös åtkomst till resurser.

- [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) är en hög tillgänglig, global identitets hanterings tjänst för konsumentbaserade appar som kan skalas till hundratals miljoner identiteter och integreras mellan mobila och webbplattformar. Dina kunder kan logga in på alla dina appar via anpassningsbara upplevelser som använder befintliga konton för sociala medier, eller så kan du skapa nya fristående autentiseringsuppgifter.

- [Azure Active Directory B2B-samarbete](https://aka.ms/aad-b2b-collaboration) är en säker partner integrerings lösning som stöder relationer mellan företag genom att göra det möjligt för partner att komma åt företagets program och data selektivt genom att använda sina egna hanterade identiteter.

- Med [Azure Active Directory Join](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/) kan du utöka moln funktioner till Windows 10-enheter för centraliserad hantering. Det gör det möjligt för användarna att ansluta till företagets eller organisationens moln genom Azure Active Directory och fören klar åtkomsten till appar och resurser.

- [Azure Active Directory-programproxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/) ger enkel inloggning och säker fjärråtkomst för webb program som finns lokalt.

## <a name="next-steps"></a>Nästa steg

- Förstå ditt [delade ansvar i molnet](shared-responsibility.md).

- Lär dig hur [Azure Security Center](https://azure.microsoft.com/services/security-center/) kan hjälpa dig att förhindra, identifiera och svara på hot med ökad insyn och kontroll över säkerheten för dina Azure-resurser.
