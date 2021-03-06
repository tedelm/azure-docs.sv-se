---
title: Felsöka Azure Stream Analytics utdata
description: Den här artikeln beskriver tekniker för att felsöka dina utgående anslutningar i Azure Stream Analytics jobb.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/31/2020
ms.custom: seodec18
ms.openlocfilehash: fb01bf613ef007be6eafa98e12f4c5f4b5c607ea
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/25/2020
ms.locfileid: "83831184"
---
# <a name="troubleshoot-azure-stream-analytics-outputs"></a>Felsöka Azure Stream Analytics utdata

I den här artikeln beskrivs vanliga problem med Azure Stream Analytics utgående anslutningar och hur du felsöker problem med utmatningen. Många fel söknings steg kräver att resursen och andra diagnostikloggar aktive ras för ditt Stream Analytics-jobb. Om du inte har aktiverat resurs loggar kan du läsa [felsöka Azure Stream Analytics med hjälp av resurs loggar](stream-analytics-job-diagnostic-logs.md).

## <a name="the-job-doesnt-produce-output"></a>Jobbet genererar inte utdata

1. Kontrol lera anslutningen till utdata med knappen **Testa anslutning** för varje utdata.
1. Titta på [övervaknings mått](stream-analytics-monitoring.md) på fliken **övervaka** . Eftersom värdena aggregeras fördröjs måtten med några minuter.

   * Om värdet för **indata-händelser** är större än noll kan jobbet läsa indata. Om värdet för **inloggning** inte är större än noll är det ett problem med jobbets indatatyper. Mer information finns i [Felsöka indata-anslutningar](stream-analytics-troubleshoot-input.md) .
   * Om värdet för **data konverterings fel** är större än noll och stiger, se [Azure Stream Analytics data fel](data-errors.md) för detaljerad information om data konverterings fel.
   * Om värdet för **körnings fel** är större än noll, tar jobbet emot data, men genererar fel när frågan bearbetas. Om du vill hitta felen går du till [gransknings loggarna](../azure-resource-manager/management/view-activity-logs.md)och filtrerar sedan på statusen **misslyckades** .
   * Om värdet för **indata-händelser** är större än noll och värdet för **utgående händelser** är lika med noll, är en av följande påståenden sant:
      * Bearbetningen av frågan resulterade i noll utdata-händelser.
      * Händelser eller fält kan vara felaktigt utformade, vilket resulterar i noll utdata efter bearbetningen av frågan.
      * Jobbet kunde inte skicka data till mottagar mottagare för anslutning eller autentiserings skäl.

   I åtgärds logg meddelanden förklaras ytterligare information, inklusive vad som händer, förutom i de fall där frågesträngen filtrerar alla händelser. Om bearbetningen av flera händelser genererar fel sammanställs felen var tionde minut.

## <a name="the-first-output-is-delayed"></a>Den första utdatan är fördröjd

När ett Stream Analytics jobb startar läses inloggade händelser in. Men det kan finnas en fördröjning i resultatet, i vissa fall.

Stora tids värden i temporala frågeuttryck kan bidra till utdata-fördröjningen. För att skapa rätt utdata över lång tid i Windows, läser strömmande jobb data från den senaste tiden för att fylla tids perioden. Data kan vara upp till sju dagar tidigare. Inga utdata skapas förrän de utestående indata-händelserna har lästs. Det här problemet kan vara en yta när systemet uppgraderar strömnings jobben. När en uppgradering äger rum startas jobbet om. Sådana uppgraderingar inträffar vanligt vis en gång i taget av några månader.

Var försiktig när du utformar din Stream Analytics fråga. Om du använder ett stort tidsfönster för temporala element i jobbets frågesyntax kan det leda till en fördröjning i de första utdata när jobbet startar eller startar om. Mer än flera timmar, upp till sju dagar, betraktas som en lång tids period.

En minskning av den här typen av första fördröjning är att använda Query parallellisering-tekniker som partitionering av data. Du kan också lägga till fler enheter för strömning för att förbättra data flödet tills jobbet har fångats upp.  Mer information finns i [överväganden när du skapar Stream Analytics-jobb](stream-analytics-concepts-checkpoint-replay.md).

Dessa faktorer påverkar de första utdatanas tids linjer:

* Användningen av fönster mängder, till exempel en GROUP BY-sats för rullande, hoppande och glidande fönster:

  * För rullande-eller hoppande-fönster agg regeringar genereras resultatet i slutet av fönster tidsram.
  * I ett glidande fönster genererar resultatet när en händelse går in eller avslutar glidnings fönstret.
  * Om du planerar att använda en stor fönster storlek, till exempel mer än en timme, är det bäst att välja ett hoppande eller en glidande fönster. Med dessa fönster typer kan du se utdata oftare.

* Användning av temporala kopplingar, till exempel JOIN med DATEDIFF:
  * Matchar generera så fort båda sidorna av de matchade händelserna kommer in.
  * Data som saknar matchning, t. ex. yttre koppling, genereras i slutet av DATEDIFF-fönstret, för varje händelse på vänster sida.

* Användning av temporala analys funktioner, till exempel ISFIRST, LAST och fördröjning med gräns varaktighet:
  * För analys funktioner genereras utdata för varje händelse. Det finns ingen fördröjning.

## <a name="the-output-falls-behind"></a>Utdata hamnar bakom

Under normal drift av ett jobb kan utdata ha längre och längre tids perioder. Om resultatet hamnar efter detta kan du hitta rotor saken genom att undersöka följande faktorer:

* Anger om den underordnade sinken är begränsad
* Anger om den överordnade källan är begränsad
* Om bearbetnings logiken i frågan är beräknings intensiv

Om du vill visa information om utdata väljer du direkt uppspelnings jobbet i Azure Portal och väljer sedan **jobb diagram**. För varje Indatatyp finns det ett händelse mått per partition. Om måttet håller på att öka är det en indikator att system resurserna är begränsade. Ökningen kan bero på begränsning av utgående mottagare eller hög CPU-användning. Mer information finns i [data driven fel sökning med hjälp av jobb diagrammet](stream-analytics-job-diagram-with-metrics.md).

## <a name="key-violation-warning-with-azure-sql-database-output"></a>Varning om nyckel fel med Azure SQL Database utdata

När du konfigurerar en Azure SQL-databas som utdata till ett Stream Analytics jobb, infogar den Mass poster i mål tabellen. I allmänhet Azure Stream Analytics garantier [på minst en gång för leverans](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) till utgående mottagare. Du kan fortfarande [få en exakt leverans]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) till en SQL-utdata när en SQL-tabell har en unik restriktion definierad.

När du ställer in unika nyckel begränsningar i SQL-tabellen tar Azure Stream Analytics bort dubbla poster. Den delar in data i batchar och infogar batcharna rekursivt tills en enskild dubblettpost hittas. Processen dela och infoga ignorerar dubbletterna en i taget. För ett strömmande jobb som har många dubblettrader-rader är processen ineffektiv och tids krävande. Om du ser varnings meddelanden om flera nyckel överträdelser i din aktivitets logg under den föregående timmen är det troligt att SQL-utdata är långsammare för hela jobbet.

Lös problemet genom att [Konfigurera indexet]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) som orsakar nyckel överträdelsen genom att aktivera alternativet IGNORE_DUP_KEY. Med det här alternativet kan SQL ignorera dubblettvärden under Mass infogningar. Azure SQL Database skapar bara ett varnings meddelande i stället för ett fel. Det innebär att Azure Stream Analytics inte längre genererar fel av fel vid primär nyckel.

Observera följande observationer när du konfigurerar IGNORE_DUP_KEY för flera typer av index:

* Du kan inte ange IGNORE_DUP_KEY på en primär nyckel eller en unik begränsning som använder ALTER INDEX. Du måste släppa indexet och återskapa det.  
* Du kan ange IGNORE_DUP_KEY med hjälp av ALTER INDEX för ett unikt index. Den här instansen skiljer sig från en primär nyckel/unik begränsning och skapas med hjälp av ett CREATE-INDEX eller en INDEX definition.  
* Alternativet IGNORE_DUP_KEY gäller inte för kolumn lagrings index eftersom du inte kan genomdriva unika objekt.  

## <a name="column-names-are-lowercase-in-azure-stream-analytics-10"></a>Kolumn namn är gemener i Azure Stream Analytics (1,0)

När du använder den ursprungliga kompatibilitetsnivån (1,0) ändrar Azure Stream Analytics kolumn namn till gemener. Detta beteende har åtgärd ATS i senare kompatibilitetsnivå. Om du vill behålla fallet går du till kompatibilitetsnivån 1,1 eller senare. Mer information finns i [kompatibilitetsnivån för Stream Analytics jobb](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level).

## <a name="get-help"></a>Få hjälp

Om du behöver ytterligare hjälp kan du prova vår [Microsoft Q&en fråge sida för Azure Stream Analytics](https://docs.microsoft.com/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Nästa steg

* [Introduktion till Azure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Språk referens för Azure Stream Analytics-fråga](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referens för Azure Stream Analytics hanterings REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
