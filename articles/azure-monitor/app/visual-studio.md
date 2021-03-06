---
title: Felsöka i Visual Studio med Azure Application insikter
description: Prestandaanalys och diagnostik för webbappar vid felsökning och i produktion.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 07/07/2017
ms.reviewer: mbullwin
ms.custom: vs-azure
ms.openlocfilehash: 8905222214d58eeba24ecf50da768ffa1d65c39d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "77670890"
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a>Felsöka dina program med Azure Application insikter i Visual Studio
I Visual Studio (2015 och senare) kan du analysera prestanda och diagnostisera problem i din ASP.NET-webbapp både när du felsöker och i produktion med hjälp av telemetri från [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md).

Om du har skapat din ASP.NET-webbapp med Visual Studio 2017 eller senare har den redan Application Insights SDK. I annat fall [lägger du till Application Insights i din app](../../azure-monitor/app/asp-net.md), om du inte redan gjort det.

När appen är live i produktion visar du normalt Application Insights-telemetrin på [Azure Portal](https://portal.azure.com). Där kan du också ställa in aviseringar och använda kraftfulla övervakningsverktyg. Men när det gäller felsökning kan du också söka efter och analysera telemetri i Visual Studio. Du kan använda Visual Studio för att analysera telemetri både från din produktions plats och från fel söknings körningar på din utvecklings dator. I det senare fallet kan du analysera felsökningskörningar även om du inte har konfigurerat SDK för att skicka telemetri till Azure Portal ännu. 

## <a name="debug-your-project"></a><a name="run"></a> Felsöka ditt projekt
Kör din webbapp i lokalt felsökningsläge genom att välja F5. Öppna olika sidor för att skapa viss telemetri.

I Visual Studio visas ett antal händelser som har loggats av Application Insights-modulen i projektet.

![Knappen Application Insights visas under felsökningar i Visual Studio.](./media/visual-studio/appinsights-09eventcount.png)

Klicka på den här knappen om du vill söka i telemetri. 

## <a name="application-insights-search"></a>Söka med Application Insights
I sökfönstret för Application Insights visas händelser som har loggats. (Om du har loggat in på Azure när du konfigurerar Application Insights kan du söka i samma händelser i Azure Portal.)

![Högerklicka på projektet och välj Application Insights, Sök](./media/visual-studio/34.png)

> [!NOTE] 
> När du har markerat eller avmarkerat filter klickar du på knappen Sök i slutet av sökfältet.
>

Fritextsökningen fungerar för alla fält i händelserna. Du kan till exempel söka efter en del av URL-adressen för en sida, efter värdet för en egenskap, t.ex. klientens stad, eller efter specifika ord i en spårningslogg.

Klicka på en händelse om du vill visa händelsens detaljerade egenskaper.

För begäranden till webbappen kan du klicka vidare till koden.

![Under Information om begäran klickar du vidare till koden.](./media/visual-studio/31.png)

Du kan också öppna relaterade objekt för att diagnostisera misslyckade begäranden eller undantag.

![Rulla ned till relaterade artiklar under Information om begäran](./media/visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a>Visa undantag och misslyckade förfrågningar
Undantagsrapporter visas i sökfönstret. (I vissa äldre versioner av ASP.NET måste du [konfigurera undantagsövervakning](../../azure-monitor/app/asp-net-exceptions.md) för att se undantagen som hanteras av ramverket.)

Klicka på ett undantag för att visa en stackspårning. Om koden för appen är öppen i Visual Studio kan du klicka dig igenom från stackspårningen till relevant kodrad.

![Undantag för stackspårning](./media/visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-the-code"></a>Visa sammanfattningar av förfrågningar och undantag i koden
I kod lins linjen ovanför varje hanterare-metod ser du antalet begär Anden och undantag som loggats av Application Insights under de senaste 24 h.

![Undantag för stackspårning](./media/visual-studio/21.png)

> [!NOTE] 
> CodeLens visar Application Insights-data endast om du har [konfigurerat appen för att skicka telemetri till Application Insights-portalen](../../azure-monitor/app/asp-net.md).
>

[Läs mer om Application Insights i CodeLens](../../azure-monitor/app/visual-studio-codelens.md)

## <a name="trends"></a>Trends
Trends är ett verktyg för visualisering av hur din app ska bete sig över tiden. 

Välj **Utforska telemetritrender** med verktygsfältsknappen eller sökfönstret för Application Insights. Välj en av fem vanliga frågor för att komma igång. Du kan analysera olika datauppsättningar baserat på telemetrityper, tidsintervall och andra egenskaper. 

För att hitta avvikelser i dina data väljer du ett av avvikelsealternativen i listrutan ”Vytyp”. Filtreringsalternativen längst ned i fönstret gör det lättare att justera efter specifika delmängder av dina telemetri.

![Trends](./media/visual-studio/51.png)

[Mer om Trends](../../azure-monitor/app/visual-studio-trends.md).

## <a name="local-monitoring"></a>Lokal övervakning
(Från Visual Studio 2015 uppdatering 2) Om du inte har konfigurerat SDK för att skicka telemetri till Application Insights portal (så att det inte finns någon Instrumentation-nyckel i ApplicationInsights. config) visas telemetri från din senaste felsökningssession i fönstret diagnostik. 

Detta är lämpligt om du redan har publicerat en tidigare version av din app. Du vill undvika att telemetri från dina felsökningssessioner blandas med telemetrin på Application Insights-portalen från den publicerade appen.

Det är också användbart om du har [anpassad telemetri](../../azure-monitor/app/api-custom-events-metrics.md) som du vill felsöka innan du skickar telemetri till portalen.

* *Först konfigurerade jag Application Insights att skicka telemetri till portalen. Men nu vill jag bara se Telemetrin i Visual Studio.*
  
  * I sökfönstrets inställningar finns det ett alternativ för att söka i lokal diagnostik även om din app skickar telemetri till portalen.
  * Om du vill stoppa telemetri som skickas till portalen kommenterar du ut `<instrumentationkey>...` raden från ApplicationInsights. config. När du är redo att skicka telemetri till portalen igen, tar du bort kommentaren.


## <a name="next-steps"></a>Nästa steg
|  |  |
| --- | --- |
| **[Lägga till mer information](../../azure-monitor/app/asp-net-more.md)**<br/>Övervaka användning, tillgänglighet, beroenden och undantag. Integrera spårningar från loggningsramverk. Skriv anpassad telemetri. |![Visual Studio](./media/visual-studio/64.png) |
| **[Arbeta med Application Insights-portalen](../../azure-monitor/app/overview-dashboard.md)**<br/>Visa instrument paneler, kraftfulla diagnostik-och analys verktyg, aviseringar, en Live-beroende karta över ditt program och exportera telemetridata. |![Visual Studio](./media/visual-studio/62.png) |

