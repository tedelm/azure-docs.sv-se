---
title: Så här utlöser du komplexa åtgärder med Azure Monitor aviseringar
description: Lär dig hur du skapar en logisk app-åtgärd för att bearbeta Azure Monitor aviseringar.
author: dkamstra
ms.author: dukek
ms.topic: conceptual
ms.date: 07/18/2018
ms.subservice: alerts
ms.openlocfilehash: 655a3acc44a1418778b37fbef85e5df75d042317
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "78206244"
---
# <a name="how-to-trigger-complex-actions-with-azure-monitor-alerts"></a>Så här utlöser du komplexa åtgärder med Azure Monitor aviseringar

Den här artikeln visar hur du konfigurerar och utlöser en Logic app för att skapa en konversation i Microsoft Teams när en avisering utlöses.

## <a name="overview"></a>Översikt

När en Azure Monitor-avisering utlöses anropar den en [Åtgärds grupp](../../azure-monitor/platform/action-groups.md). Med åtgärds grupper kan du utlösa en eller flera åtgärder för att meddela andra om en avisering och även åtgärda det.

Den allmänna processen är:

-   Skapa Logic-appen för respektive aviserings typ.

-   Importera en exempel nytto last för respektive aviserings typ till Logic-appen.

-   Definiera beteendet för Logic app.

-   Kopiera HTTP-slutpunkten för Logic-appen till en Azure-åtgärds grupp.

Processen påminner om du vill att Logic app ska utföra en annan åtgärd.

## <a name="create-an-activity-log-alert-administrative"></a>Skapa en aktivitets logg avisering: administrativ

1.  I Azure Portal väljer du **skapa en resurs** i det övre vänstra hörnet.

2.  Sök efter och välj **Logic app**och välj sedan **skapa**.

3.  **Namnge din Logic app, Välj**en **resurs grupp**och så vidare.

    ![Skapa en Logic app](media/action-groups-logic-app/create-logic-app-dialog.png "Skapa en logikapp")

4.  Välj **skapa** för att skapa Logic-appen. Ett popup-meddelande indikerar att Logic-appen har skapats. Välj **Starta resurs** för att öppna **Logic Apps designer**.

5.  Välj utlösaren: **när en HTTP-begäran tas emot**.

    ![Utlösare för logikappar](media/action-groups-logic-app/logic-app-triggers.png "Utlösare för logikappar")

6.  Välj **Redigera** för att ändra utlösaren för http-begäran.

    ![Utlösare för HTTP-begäran](media/action-groups-logic-app/http-request-trigger-shape.png "Utlösare för HTTP-begäran")

7.  Välj **Generera schemat genom att använda en exempelnyttolast**.

    ![Använda en exempel nytto Last](media/action-groups-logic-app/use-sample-payload-button.png "Använda en exempel nytto Last")

8.  Kopiera och klistra in följande exempel nytto Last i dialog rutan:

    ```json
        {
            "schemaId": "Microsoft.Insights/activityLogs",
            "data": {
                "status": "Activated",
                "context": {
                "activityLog": {
                    "authorization": {
                    "action": "microsoft.insights/activityLogAlerts/write",
                    "scope": "/subscriptions/…"
                    },
                    "channels": "Operation",
                    "claims": "…",
                    "caller": "logicappdemo@contoso.com",
                    "correlationId": "91ad2bac-1afa-4932-a2ce-2f8efd6765a3",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-04-03T22:33:11.762469+00:00",
                    "eventDataId": "ec74c4a2-d7ae-48c3-a4d0-2684a1611ca0",
                    "level": "Informational",
                    "operationName": "microsoft.insights/activityLogAlerts/write",
                    "operationId": "61f59fc8-1442-4c74-9f5f-937392a9723c",
                    "resourceId": "/subscriptions/…",
                    "resourceGroupName": "LOGICAPP-DEMO",
                    "resourceProviderName": "microsoft.insights",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:33:36.1068742+00:00",
                    "resourceType": "microsoft.insights/activityLogAlerts"
                }
                },
                "properties": {}
            }
        }
    ```

9. **Logic App Designer** visar ett popup-fönster för att påminna dig om att begäran som skickas till Logic-appen måste ange **Content-Type-** huvudet till **Application/JSON**. Stäng popup-fönstret. Azure Monitors aviseringen anger sidhuvudet.

    ![Ange rubrik för innehålls typ](media/action-groups-logic-app/content-type-header.png "Ange rubrik för innehålls typ")

10. Välj **+** **nytt steg** och välj sedan **Lägg till en åtgärd**.

    ![Lägga till en åtgärd](media/action-groups-logic-app/add-action.png "Lägga till en åtgärd")

11. Sök efter och välj Microsoft Teams-anslutningsprogrammet. Välj åtgärden **Microsoft Teams-publicera meddelande** .

    ![Microsoft Teams-åtgärder](media/action-groups-logic-app/microsoft-teams-actions.png "Microsoft Teams-åtgärder")

12. Konfigurera Microsoft Teams-åtgärden. **Logic Apps designer** ber dig att autentisera till ditt Office 365-konto. Välj det **Team-ID** och **kanal-ID** som meddelandet ska skickas till.

13. Konfigurera meddelandet med hjälp av en kombination av statisk text och referenser till \<fälten\> i det dynamiska innehållet. Kopiera och klistra in följande text i **meddelande** fältet:

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    Sök sedan efter och Ersätt \<fälten\> med dynamiska innehålls etiketter med samma namn.

    > [!NOTE]
    > Det finns två dynamiska fält med namnet **status**. Lägg till båda fälten i meddelandet. Använd fältet i **activityLog** egenskaps påse och ta bort det andra fältet. Håll markören över fältet **status** för att se den fullständigt kvalificerade fält referensen, som visas på följande skärm bild:

    ![Microsoft Teams-åtgärd: publicera ett meddelande](media/action-groups-logic-app/teams-action-post-message.png "Microsoft Teams-åtgärd: publicera ett meddelande")

14. Överst i **Logic Apps designer**väljer du **Spara** för att spara din Logic app.

15. Öppna den befintliga åtgärds gruppen och Lägg till en åtgärd som refererar till Logic app. Om du inte har en befintlig åtgärds grupp kan du läsa [skapa och hantera åtgärds grupper i Azure Portal](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) för att skapa en. Glöm inte att spara ändringarna.

    ![Uppdatera åtgärds gruppen](media/action-groups-logic-app/update-action-group.png "Uppdatera åtgärds gruppen")

Nästa gången en avisering anropar din åtgärds grupp, anropas din Logic Apps.

## <a name="create-a-service-health-alert"></a>Skapa en avisering om tjänstens hälsa

Azure Service Health poster ingår i aktivitets loggen. Processen för att skapa aviseringen liknar att [skapa en aktivitets logg avisering](#create-an-activity-log-alert-administrative), men med några få ändringar:

- Steg 1 till 7 är desamma.
- I steg 8 använder du följande exempel nytto last för utlösaren för HTTP-begäran:

    ```json
    {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "channels": "Admin",
                    "correlationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "description": "…",
                    "eventSource": "ServiceHealth",
                    "eventTimestamp": "2018-04-03T22:44:43.7467716+00:00",
                    "eventDataId": "9ce152f5-d435-ee31-2dce-104228486a6d",
                    "level": "Informational",
                    "operationName": "Microsoft.ServiceHealth/incident/action",
                    "operationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "properties": {
                        "title": "...",
                        "service": "...",
                        "region": "Global",
                        "communication": "...",
                        "incidentType": "Incident",
                        "trackingId": "...",
                        "impactStartTime": "2018-03-22T21:40:00.0000000Z",
                        "impactMitigationTime": "2018-03-22T21:41:00.0000000Z",
                        "impactedServices": "[{"ImpactedRegions"}]",
                        "defaultLanguageTitle": "...",
                        "defaultLanguageContent": "...",
                        "stage": "Active",
                        "communicationId": "11000001466525",
                        "version": "0.1.1"
                    },
                    "status": "Active",
                    "subscriptionId": "...",
                    "submissionTimestamp": "2018-04-03T22:44:50.8013523+00:00"
                }
            },
            "properties": {}
        }
    }
    ```

-  Steg 9 och 10 är desamma.
-  I steg 11 till 14 använder du följande process:

   1. Välj **+** **nytt steg** och välj sedan **Lägg till ett villkor**. Ange följande villkor så att Logic app bara körs när indatan matchar värdena nedan.  När du anger version svärdet i text rutan sätter du citat tecken runt dem ("0.1.1") för att se till att den utvärderas som en sträng och inte en numerisk typ.  Systemet visar inte citat tecknen om du återgår till sidan, men den underliggande koden behåller fortfarande sträng typen.   
       - `schemaId == Microsoft.Insights/activityLogs`
       - `eventSource == ServiceHealth`
       - `version == "0.1.1"`

      !["Villkor för Service Health nytto Last"](media/action-groups-logic-app/service-health-payload-condition.png "Villkor för Service Health nytto Last")

   1. I villkoret **om sant** följer du anvisningarna i steg 11 till 13 i [skapa en aktivitets logg avisering](#create-an-activity-log-alert-administrative) för att lägga till åtgärden Microsoft Teams.

   1. Definiera meddelandet med hjälp av en kombination av HTML och dynamiskt innehåll. Kopiera och klistra in följande innehåll i **meddelande** fältet. Ersätt fälten `[incidentType]`, `[trackingID]`, `[title]`och `[communication]` med dynamiska innehålls etiketter med samma namn:

       ```html
       <p>
       <b>Alert Type:</b>&nbsp;<strong>[incidentType]</strong>&nbsp;
       <strong>Tracking ID:</strong>&nbsp;[trackingId]&nbsp;
       <strong>Title:</strong>&nbsp;[title]</p>
       <p>
       <a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.</a>
       </p>
       <p>[communication]</p>
       ```

       !["Service Health True condition post Action"](media/action-groups-logic-app/service-health-true-condition-post-action.png "Service Health True condition post-åtgärd")

   1. För villkoret **om falskt** anger du ett användbart meddelande:

       ```html
       <p><strong>Service Health Alert</strong></p>
       <p><b>Unrecognized alert schema</b></p>
       <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.\</a></p>
       ```

       !["Service Health falsk villkor post åtgärd"](media/action-groups-logic-app/service-health-false-condition-post-action.png "Service Health falsk villkors post åtgärd")

- Steg 15 är detsamma. Följ anvisningarna för att spara din Logic app och uppdatera åtgärds gruppen.

## <a name="create-a-metric-alert"></a>Skapa en måtta avisering

Processen för att skapa en måtta aviseringar liknar att [skapa en aktivitets logg avisering](#create-an-activity-log-alert-administrative), men med några få ändringar:

- Steg 1 till 7 är desamma.
- I steg 8 använder du följande exempel nytto last för utlösaren för HTTP-begäran:

    ```json
    {
    "schemaId": "AzureMonitorMetricAlert",
    "data": {
        "version": "2.0",
        "status": "Activated",
        "context": {
        "timestamp": "2018-04-09T19:00:07.7461615Z",
        "id": "...",
        "name": "TEST-VM CPU Utilization",
        "description": "",
        "conditionType": "SingleResourceMultipleMetricCriteria",
        "condition": {
            "windowSize": "PT15M",
            "allOf": [
                {
                    "metricName": "Percentage CPU",
                    "dimensions": [
                    {
                        "name": "ResourceId",
                        "value": "d92fc5cb-06cf-4309-8c9a-538eea6a17a6"
                    }
                ],
                "operator": "GreaterThan",
                "threshold": "5",
                "timeAggregation": "PT15M",
                "metricValue": 1.0
            }
            ]
        },
        "subscriptionId": "...",
        "resourceGroupName": "TEST",
        "resourceName": "test-vm",
        "resourceType": "Microsoft.Compute/virtualMachines",
        "resourceId": "...",
        "portalLink": "..."
        },
        "properties": {}
    }
    }
    ```

- Steg 9 och 10 är desamma.
- I steg 11 till 14 använder du följande process:

  1. Välj **+** **nytt steg** och välj sedan **Lägg till ett villkor**. Ange följande villkor så att Logic app bara körs när indatan matchar dessa värden nedan. När du anger version svärdet i text rutan sätter du citat tecken runt dem ("2,0") för att se till att den utvärderas som en sträng och inte en numerisk typ.  Systemet visar inte citat tecknen om du återgår till sidan, men den underliggande koden behåller fortfarande sträng typen. 
     - `schemaId == AzureMonitorMetricAlert`
     - `version == "2.0"`
       
       !["Villkor för nytto lastens nytto Last"](media/action-groups-logic-app/metric-alert-payload-condition.png "Villkor för nytto last för mått")

  1. I villkoret **om sant** lägger du till ett **för varje** slinga och åtgärden Microsoft Teams. Definiera meddelandet med hjälp av en kombination av HTML och dynamiskt innehåll.

      !["Mått avisering True condition post åtgärd"](media/action-groups-logic-app/metric-alert-true-condition-post-action.png "Mått avisering, True condition post-åtgärd")

  1. I villkoret **om falskt** definierar du en Microsoft Teams-åtgärd för att kommunicera att mått aviseringen inte matchar förväntningarna i Logic app. Ta med JSON-nyttolasten. Observera hur du refererar till `triggerBody` det dynamiska innehållet i `json()` uttrycket.

      !["Mått varning falsk villkor post åtgärd"](media/action-groups-logic-app/metric-alert-false-condition-post-action.png "Mått avisering, felaktig villkors post åtgärd")

- Steg 15 är detsamma. Följ anvisningarna för att spara din Logic app och uppdatera åtgärds gruppen.

## <a name="calling-other-applications-besides-microsoft-teams"></a>Anropa andra program förutom Microsoft Teams
Logic Apps har flera olika anslutningar som gör att du kan utlösa åtgärder i en mängd olika program och databaser. Slack, SQL Server, Oracle, Salesforce, är bara några exempel. Mer information om anslutningar finns i [Logic app-kopplingar](../../connectors/apis-list.md).  

## <a name="next-steps"></a>Nästa steg
* Få en [Översikt över Azure aktivitets logg aviseringar](../../azure-monitor/platform/alerts-overview.md) och lär dig hur du tar emot aviseringar.  
* Lär dig hur du [konfigurerar aviseringar när ett Azure Service Health-meddelande publiceras](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).
* Läs mer om [Åtgärds grupper](../../azure-monitor/platform/action-groups.md).

