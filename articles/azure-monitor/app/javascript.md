---
title: Azure Application insikter om JavaScript-webbappar
description: Hämta sid visning och antal sessioner, webb klient data, enstaka sid program (SPA) och spåra användnings mönster. Identifiera undantag och prestandaproblem på JavaScript-baserade webbsidor.
ms.topic: conceptual
author: Dawgfan
ms.author: mmcc
ms.date: 09/20/2019
ms.openlocfilehash: 50ce0d57ec7395c69bf65e41b67f0cb005a43cb8
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82854975"
---
# <a name="application-insights-for-web-pages"></a>Application Insights för webbsidor

Visa prestanda och användning för webbsidor eller appar. Om du lägger till [Application Insights](app-insights-overview.md) till sid skriptet får du tids inställningar för sid inläsningar och AJAX-anrop, antal och information om webb läsar undantag och AJAX-fel, samt antal användare och sessioner. Allt detta kan visas efter sida, klientoperativsystem- och webbläsarversion, geografisk plats och andra dimensioner. Du kan ställa in varningar för antal fel eller långsam sidinläsning. Och genom att infoga spårning av anrop i JavaScript-kod kan du spåra hur olika funktioner i ditt webbsideprogram används.

Application Insights kan användas med alla webbsidor – du lägger bara till ett stycke JavaScript-kod. Om din webb tjänst är [Java](java-get-started.md) eller [ASP.net](asp-net.md)kan du använda SDK: er för Server sidan tillsammans med SDK SDK för klient sidan för att få en heltäckande förståelse för appens prestanda.

## <a name="adding-the-javascript-sdk"></a>Lägga till JavaScript SDK

1. Först behöver du en Application Insights-resurs. Om du inte redan har en resurs-och Instrumentation-nyckel följer du anvisningarna för att [skapa en ny resurs](create-new-resource.md).
2. Kopiera Instrumentation-nyckeln från den resurs där du vill att din JavaScript-telemetri ska skickas.
3. Lägg till Application Insights JavaScript SDK till din webb sida eller app via något av följande två alternativ:
    * [NPM-installation](#npm-based-setup)
    * [JavaScript-kodfragment](#snippet-based-setup)

> [!IMPORTANT]
> Använd bara en metod för att lägga till Java Script SDK i ditt program. Om du använder installations programmet för NPM ska du inte använda kodfragmentet och vice versa.

> [!NOTE]
> NPM-installationen installerar JavaScript SDK som ett beroende av ditt projekt, vilket aktiverar IntelliSense, medan kodfragmentet hämtar SDK vid körning. Båda har stöd för samma funktioner. Men utvecklare som vill ha mer anpassade händelser och konfiguration brukar välja NPM-installation, medan användare söker efter snabb aktivering av den färdiga Web Analytics-valet för kodfragmentet.

### <a name="npm-based-setup"></a>NPM-baserad installation

```js
import { ApplicationInsights } from '@microsoft/applicationinsights-web'

const appInsights = new ApplicationInsights({ config: {
  instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE'
  /* ...Other Configuration Options... */
} });
appInsights.loadAppInsights();
appInsights.trackPageView(); // Manually call trackPageView to establish the current user/session/pageview
```

### <a name="snippet-based-setup"></a>Kodfragment-baserad installation

Om din app inte använder NPM kan du direkt Instrumenta dina webb sidor med Application Insights genom att klistra in det här kodfragmentet överst på varje sida. Helst bör det vara det första skriptet i `<head>` avsnittet så att det kan övervaka eventuella eventuella problem med alla dina beroenden. Om du använder en program vara från en blixt Server lägger du till kodfragmentet överst i filen `_Host.cshtml` i `<head>` avsnittet.

```html
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(n){var o={config:n,initialize:!0},t=document,e=window,i="script";setTimeout(function(){var e=t.createElement(i);e.src=n.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",t.getElementsByTagName(i)[0].parentNode.appendChild(e)});try{o.cookie=t.cookie}catch(e){}function a(n){o[n]=function(){var e=arguments;o.queue.push(function(){o[n].apply(o,e)})}}o.queue=[],o.version=2;for(var s=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];s.length;)a("track"+s.pop());var r="Track",c=r+"Page";a("start"+c),a("stop"+c);var u=r+"Event";if(a("start"+u),a("stop"+u),a("addTelemetryInitializer"),a("setAuthenticatedUserContext"),a("clearAuthenticatedUserContext"),a("flush"),o.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===n.disableExceptionTracking||n.extensionConfig&&n.extensionConfig.ApplicationInsightsAnalytics&&!0===n.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){a("_"+(s="onerror"));var p=e[s];e[s]=function(e,n,t,i,a){var r=p&&p(e,n,t,i,a);return!0!==r&&o["_"+s]({message:e,url:n,lineNumber:t,columnNumber:i,error:a}),r},n.autoExceptionInstrumented=!0}return o}(
{
  instrumentationKey:"INSTRUMENTATION_KEY"
}
);(window[aiName]=aisdk).queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

### <a name="sending-telemetry-to-the-azure-portal"></a>Skicka telemetri till Azure Portal

Som standard samlar Application Insights JavaScript SDK automatiskt in ett antal telemetridata som är användbara för att fastställa hälso tillståndet för ditt program och den underliggande användar upplevelsen. Dessa omfattar:

- Ej **fångade undantag** i appen, inklusive information om
    - Stack spårning
    - Undantags information och meddelande som medföljer felet
    - Rad & kolumn antal fel
    - URL där ett fel genererades
- Begär Anden om **nätverks beroenden** som görs av appen **XHR** och **hämtningen** (hämtnings samlingen är inaktive rad som standard) begär Anden, inkludera information om
    - URL för beroende källa
    - Kommando & metod som används för att begära beroendet
    - Varaktighet för begäran
    - Resultat kod och lyckad status för begäran
    - ID (om det finns) för användaren som gör begäran
    - Korrelations kontext (om det finns någon) där begäran görs
- **Användar information** (till exempel plats, nätverk, IP)
- **Enhets information** (till exempel webbläsare, OS, version, språk, modell)
- **Sessionsinformation**

### <a name="telemetry-initializers"></a>Telemetri-initierare
Telemetri initierare används för att ändra innehållet i insamlad telemetri innan det skickas från användarens webbläsare. De kan också användas för att förhindra att vissa telemetri skickas, genom att returnera `false`. Du kan lägga till flera telemetri-initierare i Application Insights-instansen och de körs när du lägger till dem.

Indataargumentet till `addTelemetryInitializer` är ett motanrop som tar ett [`ITelemetryItem`](https://github.com/microsoft/ApplicationInsights-JS/blob/master/API-reference.md#addTelemetryInitializer) argument och returnerar ett `boolean` eller. `void` Om det `false`returneras, skickas inte objektet telemetri, annars går det vidare till nästa telemetri-initierare, om sådana finns, eller skickas till slut punkten för telemetri-samlingen.

Ett exempel på att använda telemetri-initierare:
```ts
var telemetryInitializer = (envelope) => {
  envelope.data.someField = 'This item passed through my telemetry initializer';
};
appInsights.addTelemetryInitializer(telemetryInitializer);
appInsights.trackTrace({message: 'This message will use a telemetry initializer'});

appInsights.addTelemetryInitializer(() => false); // Nothing is sent after this is executed
appInsights.trackTrace({message: 'this message will not be sent'}); // Not sent
```

## <a name="configuration"></a>Konfiguration
De flesta konfigurations fälten får ett namn som är förfalskade som standard. Alla fält är valfria förutom för `instrumentationKey`.

| Name | Standardvärde | Beskrivning |
|------|---------|-------------|
| instrumentationKey | null | **Obligatoriskt**<br>Instrumentation-nyckel som du fick från Azure Portal. |
| accountId | null | Ett valfritt konto-ID, om din app grupperar användare till konton. Inga blank steg, kommatecken, semikolon, likheter eller lodräta staplar |
| sessionRenewalMs | 1800000 | En session loggas om användaren är inaktiv under den här tiden i millisekunder. Standardvärdet är 30 minuter |
| sessionExpirationMs | 86400000 | En session loggas om den fortsätter under den här tiden i millisekunder. Standardvärdet är 24 timmar |
| maxBatchSizeInBytes | 10000 | Max storlek för telemetri batch. Om en batch överskrider den här gränsen skickas den omedelbart och en ny batch startas |
| maxBatchInterval | 15 000 | Hur lång tid det tar att gruppera telemetri innan det skickas (millisekunder) |
| disableExceptionTracking | falskt | Om det här värdet är sant är undantag inte autosamlade. Standardvärdet är false. |
| disableTelemetry | falskt | Om det här värdet är sant samlas ingen telemetri in eller skickas. Standardvärdet är false. |
| enableDebug | falskt | Om det här värdet är sant genereras **interna** fel söknings data som ett undantag **i stället** för att loggas, oavsett inställningarna för SDK-loggning. Standardvärdet är false. <br>***Obs:*** Om du aktiverar den här inställningen tas all telemetri bort när ett internt fel inträffar. Detta kan vara användbart för att snabbt identifiera problem med konfigurationen eller användningen av SDK. Om du inte vill förlora telemetri vid fel sökning kan du överväga att använda `consoleLoggingLevel` eller `telemetryLoggingLevel` i stället för `enableDebug`. |
| loggingLevelConsole | 0 | Loggar **interna** Application Insights fel i konsolen. <br>0: av, <br>1: endast kritiska fel, <br>2: allt (fel & varningar) |
| loggingLevelTelemetry | 1 | Skickar **interna** Application Insights fel som telemetri. <br>0: av, <br>1: endast kritiska fel, <br>2: allt (fel & varningar) |
| diagnosticLogInterval | 10000 | inhemska Avsöknings intervall (i MS) för intern loggnings kön |
| samplingPercentage | 100 | Procent andel av händelser som ska skickas. Standardvärdet är 100, vilket innebär att alla händelser skickas. Ange detta om du vill bevara din data Kap för storskaliga program. |
| autoTrackPageVisitTime | falskt | Om värdet är true, på en sid visningar, spåras och skickas den föregående instrumenterade sidans visnings tid och skickas som telemetri och en ny timer startas för den aktuella sid visningar. Standardvärdet är false. |
| disableAjaxTracking | falskt | Om värdet är true samlas inga AJAX-anrop in. Standardvärdet är false. |
| disableFetchTracking | true | Om det här värdet är sant samlas inga hämtnings förfrågningar in. Standardvärdet är true |
| overridePageViewDuration | falskt | Om värdet är true ändras standard beteendet för trackPageView till post end för sid visningens varaktighets intervall när trackPageView anropas. Om värdet är false och ingen anpassad varaktighet anges för trackPageView, beräknas sid visningens prestanda med hjälp av API: t för navigering. Standardvärdet är false. |
| maxAjaxCallsPerView | 500 | Standard 500-styr hur många AJAX-anrop som ska övervakas per sid visning. Ange till-1 om du vill övervaka alla (obegränsat) AJAX-anrop på sidan. |
| disableDataLossAnalysis | true | Om det här värdet är falskt kontrol leras den interna telemetri-buffertarna vid start för objekt som ännu inte har skickats. |
| disableCorrelationHeaders | falskt | Om värdet är False kommer SDK att lägga till två huvuden ("Request-ID" och "Request-context") till alla beroende begär Anden för att korrelera dem med motsvarande begär Anden på Server sidan. Standardvärdet är false. |
| correlationHeaderExcludedDomains |  | Inaktivera korrelations rubriker för vissa domäner |
| correlationHeaderDomains |  | Aktivera korrelations rubriker för vissa domäner |
| disableFlushOnBeforeUnload | falskt | Standard falskt. Om värdet är true anropas inte Flush-metoden när onBeforeUnload event triggers |
| enableSessionStorageBuffer | true | Default True. Om värdet är true lagras bufferten med all telemetri som inte har skickats i session Storage. Bufferten återställs vid sid inläsning |
| isCookieUseDisabled | falskt | Standard falskt. Om värdet är true kommer SDK inte att lagra eller läsa data från cookies.|
| cookieDomain | null | Anpassad cookie-domän. Detta är användbart om du vill dela Application Insights cookies över under domäner. |
| isRetryDisabled | falskt | Standard falskt. Om det här värdet är falskt försöker du igen på 206 (delvis utfört), 408 (timeout), 429 (för många begär Anden), 500 (internt Server fel), 503 (tjänsten är inte tillgänglig) och 0 (offline, endast om det har identifierats) |
| isStorageUseDisabled | falskt | Om värdet är true kommer SDK inte att lagra eller läsa data från lokal lagring och sessionstoken. Standardvärdet är false. |
| isBeaconApiDisabled | true | Om det här värdet är falskt skickar SDK all telemetri med hjälp av [Beacon-API: et](https://www.w3.org/TR/beacon) |
| onunloadDisableBeacon | falskt | Standard falskt. När fliken är stängd skickar SDK all återstående telemetri med hjälp av Beacon- [API: et](https://www.w3.org/TR/beacon) |
| sdkExtension | null | Anger namnet på SDK-tillägget. Endast alfabetiska tecken tillåts. Tilläggs namnet läggs till som ett prefix till taggen AI. Internal. sdkVersion (till exempel ext_javascript: 2.0.0). Standardvärdet är null. |
| isBrowserLinkTrackingEnabled | falskt | Standardvärdet är false. Om värdet är true, kommer SDK att spåra alla förfrågningar om [webb läsar länkar](https://docs.microsoft.com/aspnet/core/client-side/using-browserlink) . |
| appId | null | AppId används för korrelationen mellan AJAX-beroenden på klient sidan med begär Anden på Server sidan. När Beacon-API är aktiverat kan det inte användas automatiskt, men det kan ställas in manuellt i konfigurationen. Standardvärdet är null |
| enableCorsCorrelation | falskt | Om värdet är true, kommer SDK att lägga till två huvuden ("Request-ID" och "Request-context") till alla CORS-begäranden för att korrelera utgående AJAX-beroenden med motsvarande begär Anden på Server sidan. Standardvärdet är false |
| namePrefix | Odefinierad | Ett valfritt värde som ska användas som namn postfix för localStorage och cookie-namn.
| enableAutoRouteTracking | falskt | Spåra automatiskt väg ändringar i en enskild sida (SPA). Om värdet är true skickar varje väg ändring en ny sid visningar till Application Insights. Ändringar av hash-`example.com/foo#bar`vägar () registreras också som nya sid visningar.
| enableRequestHeaderTracking | falskt | Om värdet är true, kommer AJAX-& Hämta begärandehuvuden att spåras, standardvärdet är false.
| enableResponseHeaderTracking | falskt | Om värdet är true spåras svars rubriker för AJAX-& Hämta. standard är falskt.
| distributedTracingMode | `DistributedTracingModes.AI` | Ställer in läget för distribuerad spårning. Om AI_AND_W3C läge eller W3C-läge är inställt, genereras W3C trace context-rubriker (traceparent/tracestate) och tas med i alla utgående begär Anden. AI_AND_W3C tillhandahålls för bakåtkompatibilitet med alla äldre Application Insights instrumenterade tjänster.

## <a name="single-page-applications"></a>Program med en sida

Som standard hanterar **inte** denna SDK tillstånds väg ändringar som inträffar i program med en enda sida. Om du vill aktivera automatisk väg ändrings spårning för ditt program på en sida `enableAutoRouteTracking: true` kan du lägga till i konfigurations konfigurationen.

För närvarande erbjuder vi ett separat [reagerar-plugin-program](#react-extensions)som du kan initiera med det här SDK: t. Den utför även spårning av ändringar i flödet, samt att samla in [andra reagera på en speciell telemetri](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md).

> [!NOTE]
> Använd `enableAutoRouteTracking: true` endast om du **inte** använder den reagera plugin-programmet. Båda kan skicka nya PageViews när vägen ändras. Om båda är aktiverade kan duplicerade PageViews skickas.

## <a name="configuration-autotrackpagevisittime"></a>Konfiguration: autoTrackPageVisitTime

Genom att `autoTrackPageVisitTime: true`ställa in den tid som en användare lägger på varje sida spåras. På varje ny sid visningar skickas varaktigheten som användaren har använt på *föregående* sida som ett [anpassat mått](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-custom-overview) med namnet `PageVisitTime`. Det här anpassade måttet visas i [Metrics Explorer](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started) som ett "log-baserat mått".

## <a name="react-extensions"></a>Reagera på tillägg

| Tillägg |
|---------------|
| [Reagera](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-js/README.md)|
| [Reagera inbyggd](https://github.com/microsoft/ApplicationInsights-JS/blob/17ef50442f73fd02a758fbd74134933d92607ecf/extensions/applicationinsights-react-native/README.md)|

## <a name="explore-browserclient-side-data"></a>Utforska data från webbläsare/klient Sidan

Du kan visa data från webbläsare/klient sidan genom att gå till **mått** och lägga till enskilda mått som du är intresse rad av:

![](./media/javascript/page-view-load-time.png)

Du kan också visa dina data från Java Script SDK via webbläsarens upplevelse i portalen.

Välj **webbläsare** och välj sedan **haverier** eller **prestanda**.

![](./media/javascript/browser.png)

### <a name="performance"></a>Prestanda

![](./media/javascript/performance-operations.png)

### <a name="dependencies"></a>Beroenden

![](./media/javascript/performance-dependencies.png)

### <a name="analytics"></a>Analytics

Om du vill fråga din telemetri som samlas in av JavaScript SDK väljer du knappen **Visa i loggar (analys)** . Genom att lägga `where` till en `client_Type == "Browser"`-sats i visas endast data från Java Script SDK och all telemetri på Server sidan som samlas in av andra SDK: er.
 
```kusto
// average pageView duration by name
let timeGrain=5m;
let dataset=pageViews
// additional filters can be applied here
| where timestamp > ago(1d)
| where client_Type == "Browser" ;
// calculate average pageView duration for all pageViews
dataset
| summarize avg(duration) by bin(timestamp, timeGrain)
| extend pageView='Overall'
// render result in a chart
| render timechart
```

### <a name="source-map-support"></a>Stöd för käll kartor

Minified-callstacken för din undantags telemetri kan vara unminified i Azure Portal. Alla befintliga integreringar på panelen undantags information fungerar med den nya unminified-callstacken.

#### <a name="link-to-blob-storage-account"></a>Länk till Blob Storage-konto

Du kan länka din Application Insights-resurs till din egen Azure Blob Storage-behållare för att automatiskt unminify anrops stackar. För att komma igång, se [stöd för automatiska käll kartor](./source-map-support.md).

### <a name="drag-and-drop"></a>Dra och släpp

1. Välj ett objekt för telemetri av undantag i Azure Portal om du vill visa dess "transaktions information från slut punkt till slut punkt"
2. Identifiera vilka käll mappningar som motsvarar den här anrops stacken. Käll kartan måste matcha en stack Rams käll fil, men med suffixet`.map`
3. Dra och släpp käll kartorna till anrops stacken i Azure Portal![](https://i.imgur.com/Efue9nU.gif)

### <a name="application-insights-web-basic"></a>Application Insights Web Basic

För en låg upplevelse kan du i stället installera den grundläggande versionen av Application Insights
```
npm i --save @microsoft/applicationinsights-web-basic
```
Den här versionen har minimalt antal funktioner och funktioner och förlitar sig på att du ska kunna skapa dem på det sätt som passar dig bäst. Den utför till exempel ingen autoinsamling (ej fångade undantag, AJAX osv.). API: erna för att skicka vissa typer av `trackTrace`telemetri `trackException`, som, osv., ingår inte i den här versionen, så du måste ange ett eget gränssnitt. Den enda API som är tillgänglig är `track`. Ett [exempel](https://github.com/Azure-Samples/applicationinsights-web-sample1/blob/master/testlightsku.html) finns här.

## <a name="examples"></a>Exempel

Körbara-exempel finns i [Application Insights JavaScript SDK-exempel](https://github.com/topics/applicationinsights-js-demo)

## <a name="upgrading-from-the-old-version-of-application-insights"></a>Uppgradera från den gamla versionen av Application Insights

Bryta ändringar i SDK v2-versionen:
- Vissa API-anrop, till exempel trackPageView och trackException, har uppdaterats för att möjliggöra bättre API-signaturer. Det finns inte stöd för att köra i Internet Explorer 8 och tidigare versioner av webbläsaren.
- Telemetri-kuvertet har fält namn och struktur ändringar på grund av data schema uppdateringar.
- Flyttad `context.operation` till `context.telemetryTrace`. Vissa fält ändrades också (`operation.id` --> `telemetryTrace.traceID`).
  - Använd `appInsights.properties.context.telemetryTrace.traceID = Util.generateW3CId()`om du vill uppdatera aktuellt sid visningar-ID manuellt (till exempel i Spa-appar).
    > [!NOTE]
    > Om du vill behålla spårnings-ID: t unikt `Util.newId()`, där du `Util.generateW3CId()`tidigare använde, använder du nu. Båda i slut ändan är åtgärds-ID.

Om du använder den aktuella Application Insights PRODUCTion SDK (1.0.20) och vill se om den nya SDK: n fungerar i körnings miljön uppdaterar du URL: en beroende på ditt aktuella SDK-inläsnings scenario.

- Ladda ned via CDN-scenario: uppdatera kodfragmentet som du använder för att peka på följande URL:
   ```
   "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
   ```

- NPM-scenario: `downloadAndSetup` anropa för att hämta det fullständiga ApplicationInsights-skriptet från CDN och initiera det med instrumentande nyckel:

   ```ts
   appInsights.downloadAndSetup({
     instrumentationKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx",
     url: "https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js"
     });
   ```

Testa i intern miljö för att kontrol lera att övervakningsfunktionerna fungerar som förväntat. Om alla fungerar uppdaterar du API-signaturerna på lämpligt sätt till SDK v2-versionen och distribuerar i produktions miljöerna.

## <a name="sdk-performanceoverhead"></a>SDK-prestanda/-kostnad

Vid bara 25 KB-GZipped, och endast ~ 15 MS att initiera, lägger Application Insights till en försumbar mängd loadtime till din webbplats. Genom att använda kodfragmentet läses minimala komponenter i biblioteket in snabbt. Under tiden laddas det fullständiga skriptet ned i bakgrunden.

Medan skriptet hämtas från CDN, är all spårning av sidan i kö. När det nedladdade skriptet har slutförts asynkront initieras spåras alla händelser som placerats i kö. Det innebär att du inte förlorar någon telemetri under hela sidans livs cykel. Den här installations processen ger din sida ett sömlöst analys system, osynligt för dina användare.

> Sammanfattning:
> - **25 KB** GZipped
> - **15 MS** allmän initierings tid
> - **Noll** spårning saknas under sidans livs cykel

## <a name="browser-support"></a>Stöd för webbläsare

![Chrome](https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![IE](https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![Opera](https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png) | ![Safari](https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png)
--- | --- | --- | --- | --- |
Chrome senaste ✔ |  Senaste ✔ för Firefox | IE 9 + & Edge-✔ | Senaste ✔ för Opera | Senaste ✔ för Safari |

## <a name="open-source-sdk"></a>SDK för öppen källkod

Application Insights JavaScript SDK är öppen källkod för att Visa käll koden eller för att bidra till projektet besök den [officiella GitHub-lagringsplatsen](https://github.com/Microsoft/ApplicationInsights-JS).

## <a name="next-steps"></a><a name="next"></a>Nästa steg
* [Spåra användning](usage-overview.md)
* [Anpassade händelser och mätvärden](api-custom-events-metrics.md)
* [Skapa – mät – lär](usage-overview.md)
