---
title: Långsamma Query-loggar – Azure Database for MySQL
description: Beskriver långsamma frågemeddelanden som är tillgängliga i Azure Database for MySQL och de tillgängliga parametrarna för att aktivera olika loggnings nivåer.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/13/2020
ms.openlocfilehash: f834ba3355d362e59e2e44f37eca0560b9bf4d7a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81271989"
---
# <a name="slow-query-logs-in-azure-database-for-mysql"></a>Långsamma Query-loggar i Azure Database for MySQL
I Azure Database for MySQL är den långsamma fråge loggen tillgänglig för användare. Åtkomst till transaktions loggen stöds inte. Den långsamma frågans logg kan användas för att identifiera Flask halsar i prestanda för fel sökning.

Mer information om den långsamma loggen MySQL finns i [avsnittet om långsam fråga](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)i MySQL-referensen.

## <a name="configure-slow-query-logging"></a>Konfigurera loggning av långsam fråga 
Som standard är den långsamma frågans logg inaktive rad. Om du vill aktivera det `slow_query_log` anger du till på. Detta kan aktive ras med Azure Portal eller Azure CLI. 

Andra parametrar som du kan justera är:

- **long_query_time**: om en fråga tar längre tid än long_query_time (i sekunder) som frågan loggas. Standardvärdet är 10 sekunder.
- **log_slow_admin_statements**: om on innehåller administrativa uttryck som ALTER_TABLE och ANALYZE_TABLE i de instruktioner som skrivs till slow_query_log.
- **log_queries_not_using_indexes**: bestämmer om frågor som inte använder index ska loggas i slow_query_log
- **log_throttle_queries_not_using_indexes**: den här parametern begränsar antalet icke-indexfrågor som kan skrivas till den långsamma fråge loggen. Den här parametern börjar gälla när log_queries_not_using_indexes är inställt på på.
- **log_output**: om "File", tillåter att den långsamma fråge loggen skrivs till både den lokala serverns lagrings plats och för att Azure Monitor diagnostikloggar. Om du använder "ingen" skrivs den långsamma fråge loggen bara till Azure Monitor-diagnostikloggar. 

> [!IMPORTANT]
> Om dina tabeller inte är indexerade kan inställning av `log_queries_not_using_indexes` parametrarna `log_throttle_queries_not_using_indexes` och på on påverka MySQL-prestanda eftersom alla frågor som körs mot dessa icke-indexerade tabeller skrivs till den långsamma frågans logg.<br><br>
> Om du planerar att logga långsamma förfrågningar under en längre tid, rekommenderar vi att du anger `log_output` till "ingen". Om detta är inställt på "File" skrivs dessa loggar till den lokala serverns lagring och kan påverka MySQL-prestanda. 

Se [logg dokumentationen för en långsam fråga](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) i MySQL för att få en fullständig beskrivning av logg parametrarna för långsamma frågor.

## <a name="access-slow-query-logs"></a>Komma åt långsamma Query-loggar
Det finns två alternativ för att komma åt långsamma Query-loggar i Azure Database for MySQL: lokala server lagrings-eller Azure Monitor diagnostikloggar. Detta anges med hjälp av `log_output` parametern.

För lokal server lagring kan du Visa och ladda ned långsamma frågemeddelanden med hjälp av Azure Portal eller Azure CLI. I Azure Portal navigerar du till servern i Azure Portal. Under **övervaknings** rubriken väljer du sidan **Server loggar** . Mer information om Azure CLI finns i [Konfigurera och komma åt långsamma Query-loggar med Azure CLI](howto-configure-server-logs-in-cli.md). 

Med Azure Monitor diagnostikloggar kan du skicka en långsam loggfil till Azure Monitor loggar (Log Analytics), Azure Storage eller Event Hubs. Se [nedan](concepts-server-logs.md#diagnostic-logs) för mer information.

## <a name="local-server-storage-log-retention"></a>Kvarhållning av lagrings logg för lokal server
När du loggar till serverns lokala lagring är loggarna tillgängliga i upp till sju dagar från skapandet. Om den totala storleken på de tillgängliga loggarna överstiger 7 GB, tas de äldsta filerna bort tills utrymmet är tillgängligt.

Loggarna roteras var 24: e timme eller 7 GB, beroende på vilket som kommer först.

> [!Note]
> Logg kvarhållning ovan gäller inte för loggar som är skickas med Azure Monitor diagnostikloggar. Du kan ändra kvarhållningsperioden för data mottagare som skickas till (t. ex. Azure Storage).

## <a name="diagnostic-logs"></a>Diagnostikloggar
Azure Database for MySQL är integrerat med Azure Monitor diagnostikloggar. När du har aktiverat långsamma Query-loggar på MySQL-servern kan du välja att de ska skickas till Azure Monitor loggar, Event Hubs eller Azure Storage. Om du vill veta mer om hur du aktiverar diagnostikloggar kan du läsa avsnittet så här i [dokumentationen för diagnostikloggar](../azure-monitor/platform/platform-logs-overview.md).

I följande tabell beskrivs vad som finns i varje logg. Beroende på utmatnings metoden kan de fält som ingår och i vilken ordning de visas variera.

| **Egenskap** | **Beskrivning** |
|---|---|
| `TenantId` | Ditt klient-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated`UTC | Tidstämpel när loggen registrerades i UTC |
| `Type` | Loggens typ. Alltid`AzureDiagnostics` |
| `SubscriptionId` | GUID för den prenumeration som servern tillhör |
| `ResourceGroup` | Namnet på den resurs grupp som servern tillhör |
| `ResourceProvider` | Namnet på resurs leverantören. Alltid`MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Resurs-URI |
| `Resource` | Namnet på servern |
| `Category` | `MySqlSlowLogs` |
| `OperationName` | `LogEvent` |
| `Logical_server_name_s` | Namnet på servern |
| `start_time_t`UTC | Tiden då frågan började |
| `query_time_s` | Total tid i sekunder som frågan tog för att köras |
| `lock_time_s` | Total tid i sekunder som frågan låstes |
| `user_host_s` | Användarnamn |
| `rows_sent_s` | Antal rader som skickats |
| `rows_examined_s` | Antal rader som granskas |
| `last_insert_id_s` | [last_insert_id](https://dev.mysql.com/doc/refman/8.0/en/information-functions.html#function_last-insert-id) |
| `insert_id_s` | Infoga ID |
| `sql_text_s` | Fullständig fråga |
| `server_id_s` | Serverns ID |
| `thread_id_s` | Tråd-ID |
| `\_ResourceId` | Resurs-URI |

> [!Note]
> För `sql_text`kommer loggen att trunkeras om den överskrider 2048 tecken.

## <a name="analyze-logs-in-azure-monitor-logs"></a>Analysera loggar i Azure Monitor loggar

När dina långsamma fråge loggar är skickas för att Azure Monitor loggar via diagnostikloggar kan du utföra ytterligare analyser av dina långsamma frågor. Nedan visas några exempel frågor som hjälper dig att komma igång. Se till att uppdatera följande med Server namnet.

- Frågor som är längre än 10 sekunder på en viss server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```

- Lista de 5 vanligaste frågorna på en viss server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | order by query_time_d desc
    | take 5
    ```

- Sammanfatta långsamma frågor efter minsta, högsta, genomsnittliga och standard avvikelse på en viss server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count(), min(query_time_d), max(query_time_d), avg(query_time_d), stdev(query_time_d), percentile(query_time_d, 95) by LogicalServerName_s
    ```

- Grafa den långsamma frågans distribution på en viss server

    ```Kusto
    AzureDiagnostics
    | where LogicalServerName_s == '<your server name>'
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | summarize count() by LogicalServerName_s, bin(TimeGenerated, 5m)
    | render timechart
    ```

- Visa frågor som är längre än 10 sekunder på alla MySQL-servrar med diagnostiska loggar aktiverade

    ```Kusto
    AzureDiagnostics
    | where Category == 'MySqlSlowLogs'
    | project TimeGenerated, LogicalServerName_s, event_class_s, start_time_t , query_time_d, sql_text_s 
    | where query_time_d > 10
    ```    
    
## <a name="next-steps"></a>Nästa steg
- [Så här konfigurerar du långsamma Query-loggar från Azure Portal](howto-configure-server-logs-in-portal.md)
- [Konfigurera långsamma Query-loggar från Azure CLI](howto-configure-server-logs-in-cli.md).
