---
title: Övervaka Azure Storage | Microsoft Docs
description: Lär dig hur du övervakar prestanda och tillgänglighet för Azure Storage.
author: normesta
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring
ms.openlocfilehash: c8125001b5960a0bf770e8e015ad757a277629ea
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684899"
---
# <a name="monitor-azure-storage"></a>Övervaka Azure Storage

När du har viktiga program och affärs processer som förlitar sig på Azure-resurser, vill du övervaka resurserna för deras tillgänglighet, prestanda och drift. I den här artikeln beskrivs övervaknings data som genereras av Azure Storage och hur du kan använda funktionerna i Azure Monitor för att analysera aviseringar för dessa data.

> [!NOTE]
> Azure Storage loggar i Azure Monitor finns i offentlig för hands version och är tillgänglig för för hands testning i alla offentliga moln regioner. För att registrera dig i för hands versionen, se [den här sidan](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Den här för hands versionen aktiverar loggar för blobbar (som innehåller Azure Data Lake Storage Gen2), filer, köer, tabeller, Premium Storage-konton i generell användning v1 och lagrings konton för generell användning v2. Klassiska lagrings konton stöds inte.

## <a name="monitor-overview"></a>Övervaka översikt

**Översikts** sidan i Azure Portal för varje lagrings resurs innehåller en kort översikt över resursanvändningen, till exempel begär Anden och fakturering per timme. Den här informationen är användbar, men bara en liten mängd av övervaknings data är tillgänglig. En del av dessa data samlas in automatiskt och är tillgängliga för analys så fort du skapar lagrings resursen. Du kan aktivera ytterligare typer av data insamling med en del konfiguration.

## <a name="what-is-azure-monitor"></a>Vad är Azure Monitor?
Azure Storage skapar övervaknings data med hjälp av [Azure Monitor](../../azure-monitor/overview.md), som är en fullständig stack övervaknings tjänst i Azure. Azure Monitor innehåller en fullständig uppsättning funktioner för att övervaka dina Azure-resurser och resurser i andra moln och lokalt. 

Mer information om Azure Monitor finns i [övervaka Azure-resurser med Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md). I artikeln beskrivs följande ämnen:

- Vad är Azure Monitor?
- Kostnader för övervakning
- Övervaknings data som samlas in i Azure
- Konfigurerar data insamling
- Standard verktyg i Azure för analys och avisering om övervaknings data

Följande avsnitt bygger på den här artikeln genom att beskriva de specifika data som samlats in från Azure Storage. Exempel visar hur du konfigurerar data insamling och analyserar data med Azure-verktyg.

## <a name="monitor-data-from-azure-storage"></a>Övervaka data från Azure Storage

Azure Storage samlar in samma typer av övervaknings data som andra Azure-resurser, som beskrivs i [övervaka data från Azure-resurser](../../azure-monitor/insights/monitor-azure-resource.md#monitoring-data). Mer information om loggar och mått som skapats av Azure Storage finns i [Azure Storage övervaknings data referens](monitor-storage-reference.md).

Mått och loggar i Azure Monitor endast stöd för Azure Resource Manager lagrings konton. Azure Monitor stöder inte klassiska lagrings konton. Om du vill använda mått eller loggar på ett klassiskt lagrings konto måste du migrera till ett Azure Resource Manager lagrings konto. Se [migrera till Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

Du kan fortsätta använda klassiska mått och loggar om du vill. Faktum är att klassiska mått och loggar är tillgängliga parallellt med mått och loggar i Azure Monitor. Stödet finns kvar tills Azure Storage avslutar tjänsten med äldre mått och loggar.

### <a name="logs-in-azure-monitor-preview"></a>Loggar i Azure Monitor (för hands version)

Logg poster skapas endast om det finns begär Anden som görs mot tjänst slut punkten. Om ett lagrings konto till exempel har aktivitet i dess BLOB-slutpunkt men inte i dess tabell-eller Queue-slutpunkter, skapas bara loggar som hör till Blob-tjänsten. Azure Storage loggar innehåller detaljerad information om lyckade och misslyckade förfrågningar till en lagrings tjänst. Den här informationen kan användas för att övervaka enskilda begäranden och för att diagnostisera problem med en lagringstjänst. Begär Anden loggas med bästa möjliga ansträngning.

#### <a name="log-authenticated-requests"></a>Logga autentiserade begär Anden

 Följande typer av autentiserade begäranden loggas:

- Lyckade begäranden
- Misslyckade begäranden, inklusive timeout, begränsning, nätverk, auktorisering och andra fel
- Begär Anden som använder en signatur för delad åtkomst (SAS) eller OAuth, inklusive misslyckade och lyckade förfrågningar
- Begär Anden om analys av data (klassiska loggdata i **$logs** container-och klass mått data i **$Metric** tabeller)

Begär Anden som görs av själva lagrings tjänsten, till exempel när loggen skapas eller tas bort, loggas inte. En fullständig lista över de loggade data som loggas finns i [lagrings loggade åtgärder och status meddelanden](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) och [logg format för lagring](monitor-storage-reference.md).

#### <a name="log-anonymous-requests"></a>Logga anonyma begär Anden

 Följande typer av anonyma begär Anden loggas:

- Lyckade begäranden
- Serverfel
- Timeout-fel för både klienten och servern
- Misslyckade GET-begäranden med felkoden 304 (inte ändrad)

Alla andra misslyckade anonyma begär Anden loggas inte. En fullständig lista över de loggade data som loggas finns i [lagrings loggade åtgärder och status meddelanden](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) och [logg format för lagring](monitor-storage-reference.md).

## <a name="configuration"></a>Konfiguration

Plattforms mått och aktivitets loggen samlas in automatiskt, men du måste skapa en diagnostisk inställning för att samla in resurs loggar eller vidarebefordra dem utanför Azure Monitor. För att processen ska kunna skapa en diagnostisk inställning med hjälp av Azure Portal, Azure CLI eller PowerShell, se [skapa diagnostisk inställning för att samla in plattforms loggar och mått i Azure](../../azure-monitor/platform/diagnostic-settings.md).

När du skapar en diagnostisk inställning väljer du den typ av lagring som du vill aktivera loggar för, till exempel en BLOB, kö, tabell eller fil. Om du skapar en diagnostisk inställning i Azure Portal kan du välja resursen från en lista. Om du använder PowerShell eller Azure CLI måste du använda resurs-ID för lagrings typen. Du hittar resurs-ID: t i Azure Portal genom att öppna sidan **Egenskaper** för ditt lagrings konto.

Du måste också ange de kategorier av åtgärder som du vill samla in loggar för. Kategorierna för Azure Storage visas i den här tabellen.

| Kategori | Description |
|:---|:---|
| StorageRead | Läs åtgärder på blobbar. |
| StorageWrite | Skriv åtgärder på blobbar. |
| StorageDelete | Ta bort åtgärder på blobbar. |

## <a name="analyze-metric-data"></a>Analysera mått data

Du kan analysera mått för Azure Storage med mått från andra Azure-tjänster med hjälp av Metrics Explorer. Öppna Metrics Explorer genom att välja **mått** på **Azure Monitor** -menyn. Mer information om hur du använder det här verktyget finns i [komma igång med Azure Metrics Explorer](../../azure-monitor/platform/metrics-getting-started.md). 

Det här exemplet visar hur du visar **transaktioner** på konto nivå.

![Skärm bild av åtkomst till mått i Azure Portal](./media/monitor-storage/access-metrics-portal.png)

För mått som stöder dimensioner kan du filtrera måttet med det önskade dimension svärdet. Det här exemplet visar hur du visar **transaktioner** på konto nivå för en speciell åtgärd genom att välja värden för dimensionen **API-namn** .

![Skärm bild av åtkomst till mått med dimension i Azure Portal](./media/monitor-storage/access-metrics-portal-with-dimension.png)

En fullständig lista över de dimensioner som Azure Storage stöder finns i [mått dimensioner](monitor-storage-reference.md#metrics-dimensions).

Alla mät värden för Azure Storage finns i följande namnrymder:

- Microsoft. Storage/storageAccounts
- Microsoft. Storage/storageAccounts/blobServices
- Microsoft. Storage/storageAccounts/fileServices
- Microsoft. Storage/storageAccounts/queueServices
- Microsoft. Storage/storageAccounts/tableServices

För en lista över alla Azure Monitor-support-mått, som innehåller Azure Storage, se [Azure Monitor mått som stöds](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).


### <a name="access-metrics"></a>Åtkomst mått

> [!TIP]
> Om du vill visa Azure CLI-eller .NET-exempel väljer du de motsvarande flikarna som visas här.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>Lista mått definitionen

Du kan visa mått definitionen för ditt lagrings konto eller den enskilda lagrings tjänsten, till exempel BLOB, fil, tabell eller Queue Service. Använd cmdleten [Get-AzMetricDefinition](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetricdefinition?view=azps-3.3.0) .

I det här exemplet ersätter du `<resource-ID>` plats hållaren med resurs-ID för hela lagrings kontot eller resurs-ID: t för en enskild lagrings tjänst, t. ex. blob, fil, tabell eller Queue Service. Du hittar dessa resurs-ID: n på **egenskaps** sidorna för ditt lagrings konto i Azure Portal.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="read-metric-values"></a>Läs mått värden

Du kan läsa mått värden på konto nivå för ditt lagrings konto eller den enskilda lagrings tjänsten, till exempel BLOB, fil, tabell eller Queue Service. Använd cmdleten [Get-AzMetric](https://docs.microsoft.com/powershell/module/Az.Monitor/Get-AzMetric?view=azps-3.3.0) .

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>Lista mått definitionen på konto nivå

Du kan visa mått definitionen för ditt lagrings konto eller den enskilda lagrings tjänsten, till exempel BLOB, fil, tabell eller Queue Service. Använd kommandot [AZ Monitor Metric List-definitions](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list-definitions) .
 
I det här exemplet ersätter du `<resource-ID>` plats hållaren med resurs-ID för hela lagrings kontot eller resurs-ID: t för en enskild lagrings tjänst, t. ex. blob, fil, tabell eller Queue Service. Du hittar dessa resurs-ID: n på **egenskaps** sidorna för ditt lagrings konto i Azure Portal.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Läs mått värden på konto nivå

Du kan läsa mått värden för ditt lagrings konto eller den enskilda lagrings tjänsten, t. ex. blob, fil, tabell eller Queue Service. Använd kommandot [AZ Monitor Metric List](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list) .

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

### <a name="net"></a>[.NET](#tab/dotnet)

Azure Monitor tillhandahåller [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) för att läsa mått definition och-värden. [Exempel koden](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) visar hur du använder SDK med olika parametrar. Du måste använda `0.18.0-preview` eller en senare version för lagrings mått.
 
I de här exemplen ersätter du `<resource-ID>` plats hållaren med resurs-ID för hela lagrings kontot eller resurs-ID: t för en enskild lagrings tjänst, t. ex. blob, fil, tabell eller Queue Service. Du hittar dessa resurs-ID: n på **egenskaps** sidorna för ditt lagrings konto i Azure Portal.

Ersätt `<subscription-ID>` variabeln med ID: t för din prenumeration. Anvisningar för hur du hämtar värden för `<tenant-ID>` , `<application-ID>` och `<AccessKey>` finns i [använda portalen för att skapa ett Azure AD-program och tjänstens huvud namn som kan komma åt resurser](https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/). 

#### <a name="list-the-account-level-metric-definition"></a>Lista mått definitionen på konto nivå

I följande exempel visas hur du listar en mått definition på konto nivån:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";


        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            // Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

#### <a name="read-account-level-metric-values"></a>Läs mått värden på konto nivå

I följande exempel visas hur du läser `UsedCapacity` data på konto nivå:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            // Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

#### <a name="read-multidimensional-metric-values"></a>Läsa flerdimensionella mått värden

För flerdimensionella mått måste du definiera metadata-filter om du vill läsa mått data för vissa dimensions värden.

I följande exempel visas hur du läser mått data för måttet som stöder multidimension:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for blob storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default";
        var subscriptionId = "<subscription-ID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

---

## <a name="analyze-log-data"></a>Analysera loggdata

Du kan komma åt resurs loggar antingen som en BLOB i ett lagrings konto, som händelse data eller genom att logga analys frågor.

En detaljerad referens för de fält som visas i dessa loggar finns i [Azure Storage övervaknings data referens](monitor-storage-reference.md).

> [!NOTE]
> Azure Storage loggar i Azure Monitor finns i offentlig för hands version och är tillgänglig för för hands testning i alla offentliga moln regioner. För att registrera dig i för hands versionen, se [den här sidan](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u). Den här för hands versionen aktiverar loggar för blobbar (som innehåller Azure Data Lake Storage Gen2), filer, köer, tabeller, Premium Storage-konton i generell användning v1 och lagrings konton för generell användning v2. Klassiska lagrings konton stöds inte.

### <a name="access-logs-in-a-storage-account"></a>Åtkomst till loggar i ett lagrings konto

Loggar visas som blobbar som lagras i en behållare på mål lagrings kontot. Data samlas in och lagras i en enda blob som en linje avgränsad JSON-nyttolast. Namnet på blobben följer denna namngivnings konvention:

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/blobServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

Här är ett exempel:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/blobServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="access-logs-in-an-event-hub"></a>Åtkomst loggar i en Event Hub

Loggar som skickas till en Event Hub lagras inte som en fil, men du kan kontrol lera att händelsehubben har fått logg informationen. I Azure Portal går du till händelsehubben och kontrollerar att antalet **inkommande meddelanden** är större än noll. 

![Granskningsloggar](media/monitor-storage/event-hub-log.png)

Du kan komma åt och läsa loggdata som skickas till händelsehubben med hjälp av säkerhets information och verktyg för händelse hantering och övervakning. Mer information finns i [vad kan jag göra med övervaknings data som skickas till min Event Hub?](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub).

### <a name="access-logs-in-a-log-analytics-workspace"></a>Åtkomst loggar i en Log Analytics-arbetsyta

Du kan komma åt loggar som skickas till en Log Analytics arbets yta genom att använda Azure Monitor logg frågor.

Mer information finns i [Kom igång med Log Analytics i Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).

Data lagras i dessa tabeller.

| Tabell | Description |
|:---|:---|
|StorageBlobLogs | Loggar som beskriver aktivitet i Blob Storage. |
|StorageFileLogs | Loggar som beskriver aktivitet i fil resurser. |
|StorageQueueLogs | Loggar som beskriver aktivitet i köer.|
|StorageTableLogs| Loggar som beskriver aktivitet i tabeller.|

### <a name="azure-storage-log-analytics-queries-in-azure-monitor"></a>Azure Storage Log Analytics frågor i Azure Monitor

Här följer några frågor som du kan ange i **logg Sök** fältet för att hjälpa dig att övervaka dina Azure Storage-konton. Dessa frågor fungerar med det [nya språket](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

Använd de här frågorna för att hjälpa dig att övervaka dina Azure Storage-konton:

* För att visa en lista över de 10 vanligaste felen under de senaste tre dagarna.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by StatusText
    | top 10 by count_ desc
    ```
* För att visa de 10 främsta åtgärder som orsakade flest fel under de senaste tre dagarna.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by OperationName
    | top 10 by count_ desc
    ```
* För att visa en lista över de 10 främsta åtgärderna med längsta svars tid från slut punkt till slut punkt under de senaste tre dagarna.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d)
    | top 10 by DurationMs desc
    | project TimeGenerated, OperationName, DurationMs, ServerLatencyMs, ClientLatencyMs = DurationMs - ServerLatencyMs
    ```
* För att visa en lista över alla åtgärder som orsakade begränsnings fel på Server sidan under de senaste tre dagarna.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText contains "ServerBusy"
    | project TimeGenerated, OperationName, StatusCode, StatusText
    ```
* För att visa en lista över alla begär Anden med anonym åtkomst under de senaste tre dagarna.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and AuthenticationType == "Anonymous"
    | project TimeGenerated, OperationName, AuthenticationType, Uri
    ```
* För att skapa ett cirkel diagram med åtgärder som använts under de senaste tre dagarna.
    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d)
    | summarize count() by OperationName
    | sort by count_ desc 
    | render piechart
    ```
## <a name="faq"></a>Vanliga frågor

**Stöder Azure Storage mått för Managed Disks eller ohanterade diskar?**

Nej. Azure Compute stöder mått på diskar. Mer information finns i [per disk mått för hanterade och ohanterade diskar](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/).

## <a name="next-steps"></a>Nästa steg

- En referens för loggar och mått som skapats av Azure Storage finns i [Azure Storage övervaknings data referens](monitor-storage-reference.md).
- Mer information om övervakning av Azure-resurser finns i [övervaka Azure-resurser med Azure Monitor](../../azure-monitor/insights/monitor-azure-resource.md).
- Mer information om hur du migrerar mått finns i [Azure Storage metriska migreringar](./storage-metrics-migration.md).

