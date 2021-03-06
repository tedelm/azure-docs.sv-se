---
title: Publicera som en Azure Event Grid-partner
description: Publicera som en Azure Event Grid partner ämnes typ. Lär dig mer om resurs modellen och publicerings flödet för partner ämnen.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/18/2020
ms.author: babanisa
ms.openlocfilehash: 2a7e2b9f731dbf05dfeb2ac01f1ae258c5250827
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170009"
---
# <a name="onboard-as-an-azure-event-grid-partner"></a>Publicera som en Azure Event Grid-partner

Den här artikeln beskriver hur du privat använder Azure Event Grid partner resurser och hur du blir en offentligt tillgänglig partner ämnes typ.

Du behöver inte särskild behörighet för att börja använda Event Grid resurs typer som är kopplade till publicerings händelser som en Event Grid-partner. I själva verket kan du använda dem idag för att publicera händelser privat till dina egna Azure-prenumerationer och testa resurs modellen om du överväger att bli en partner.

## <a name="become-an-event-grid-partner"></a>Bli en Event Grid-partner

Om du är intresse rad av att bli en offentlig Event Grid-partner börjar du med att fylla i [det här formuläret](https://aka.ms/gridpartnerform). Kontakta sedan Event Grid-teamet på [GridPartner@microsoft.com](mailto:gridpartner@microsoft.com) .

## <a name="how-partner-topics-work"></a>Så här fungerar partner ämnen
Partner ämnen tar den befintliga arkitekturen som Event Grid redan använder för att publicera händelser från Azure-resurser, till exempel Azure Storage och Azure-IoT Hub, och gör dessa verktyg offentligt tillgängliga för alla som ska använda. Att använda dessa verktyg är som standard privat enbart för Azure-prenumerationen. Om du vill göra dina händelser offentligt tillgängliga, fyller du i formuläret och [kontaktar Event Grids teamet](mailto:gridpartner@microsoft.com).

Med partner ämnen kan du publicera händelser till Azure Event Grid för användning av flera innehavare.

### <a name="onboarding-and-event-publishing-overview"></a>Översikt över registrering och händelse publicering

#### <a name="partner-flow"></a>Partner flöde

1. Skapa en Azure-klient om du inte redan har en.
1. Använd Azure CLI för att skapa en ny Event Grid `partnerRegistration` . Den här resursen innehåller information, till exempel visnings namn, beskrivning, installations-URI och så vidare.

    ![Avsnittet Skapa en partner](./media/partner-onboarding-how-to/create-partner-registration.png)

1. Skapa en eller flera partner namn rymder i varje region där du vill publicera händelser. Tjänsten Event Grid tillhandahåller en publicerings slut punkt (till exempel `https://contoso.westus-1.eventgrid.azure.net/api/events` ) och åtkomst nycklar.

    ![Skapa ett namn område för partner](./media/partner-onboarding-how-to/create-partner-namespace.png)

1. Gör det möjligt för kunder att registrera sig i systemet som de vill ha ett partner ämne.
1. Kontakta Event Grid-teamet och meddela att du vill att din partner ämnes typ ska bli offentlig.

#### <a name="customer-flow"></a>Kund flöde

1. Kunden besöker Azure Portal och noterar det Azure-prenumerations-ID och resurs grupp som de vill att partner avsnittet ska skapas i.
1. Kunden begär ett partner ämne via systemet. Som svar skapar du en händelse tunnel till ditt partner namn område.
1. Event Grid skapar ett **väntande** partner ämne i kundens Azure-prenumeration och resurs grupp.

    ![Skapa en händelse kanal](./media/partner-onboarding-how-to/create-event-tunnel-partner-topic.png)

1. Kunden aktiverar partner ämnet via Azure Portal. Händelser kan nu flöda från din tjänst till kundens Azure-prenumeration.

    ![Aktivera ett partner ämne](./media/partner-onboarding-how-to/activate-partner-topic.png)

## <a name="resource-model"></a>Resursmodell


Följande resurs modell är för partner ämnen.

### <a name="partner-registrations"></a>Partner registreringar
* Klusterresursen`partnerRegistrations`
* Används av: partners
* Beskrivning: fångar de globala metadata för SaaS-partner (program vara som en tjänst) (till exempel namn, visnings namn, beskrivning, installations-URI).
    
    Att skapa eller uppdatera en partner registrering är en fristående åtgärd för partnern. Den här självbetjänings funktionen gör det möjligt för partner att bygga och testa hela flödet från slut punkt till slut punkt.
    
    Endast Microsofts godkända partner registreringar kan identifieras av kunder.
* Omfång: skapas i partnerns Azure-prenumeration. Metadata är synliga för kunder när de har offentliggjorts.

### <a name="partner-namespaces"></a>Partner namn rymder
* Resurs: partnerNamespaces
* Används av: partners
* Beskrivning: tillhandahåller en regional resurs för publicering av kund händelser till. Varje partner namn område har en publicerings slut punkt och auth-nycklar. Namn området är också hur partnern begär ett partner ämne för en bestämd kund och listar aktiva kunder.
* Omfattning: bor i partnerns prenumeration.

### <a name="event-channel"></a>Händelse kanal
* Klusterresursen`partnerNamespaces/eventChannels`
* Används av: partners
* Beskrivning: händelse tunnlarna är en spegling av kundens partner ämne. Genom att skapa en händelse tunnel och ange kundens Azure-prenumeration och resurs grupp i metadata signalerar du till Event Grid för att skapa ett partner ämne för kunden. Event Grid utfärdar ett ARM-anrop för att skapa en motsvarande partnerTopic i kundens prenumeration. Partner ämnet skapas i ett väntande tillstånd. Det finns en 1-till-en-länk mellan varje händelse tunnel och ett partner ämne.
* Omfattning: bor i partnerns prenumeration.

### <a name="partner-topics"></a>Partnerämnen
* Klusterresursen`partnerTopics`
* Används av: kunder
* Beskrivning: samarbets ämnen liknar anpassade ämnen och system ämnen i Event Grid. Varje partner ämne är associerat med en speciell källa (till exempel `Contoso:myaccount` ) och en speciell typ av partner ämne (till exempel contoso). Kunder skapar händelse prenumerationer i partner avsnittet för att dirigera händelser till olika händelse hanterare.

    Kunder kan inte skapa den här resursen direkt. Det enda sättet att skapa ett partner ämne är via en partner åtgärd som skapar en händelse tunnel.
* Omfattning: bor i kundens prenumeration.

### <a name="partner-topic-types"></a>Partner ämnes typer
* Klusterresursen`partnerTopicTypes`
* Används av: kunder
* Beskrivning: partner ämnes typer är tenantwide resurs typer som gör det möjligt för kunder att identifiera listan över godkända partner ämnes typer. URL: en ser ut så härhttps://management.azure.com/providers/Microsoft.EventGrid/partnerTopicTypes)
* Omfattning: global

## <a name="publish-events-to-event-grid"></a>Publicera händelser till Event Grid
När du skapar ett namn område för en partner i en Azure-region får du en regional slut punkt och motsvarande auth-nycklar. Publicera batchar av händelser till den här slut punkten för alla kund händelse tunnlar i det namn området. Baserat på fältet källa i händelsen, Azure Event Grid mappar varje händelse med motsvarande partner ämnen.

### <a name="event-schema-cloudevents-v10"></a>Händelse schema: CloudEvents v 1.0
Publicera händelser till Azure Event Grid med CloudEvents 1,0-schemat. Event Grid stöder både strukturerat läge och batch-läge. CloudEvents 1,0 är det enda händelse schema som stöds för partner namn områden.

### <a name="example-flow"></a>Exempel flöde

1.  Publicerings tjänsten gör ett HTTP-inlägg `https://contoso.westus2-1.eventgrid.azure.net/api/events?api-version=2018-01-01` .
1.  I begäran inkluderar du ett huvud värde med namnet AEG-SAS-Key som innehåller en nyckel för autentisering. Den här nyckeln tillhandahålls när namn området för partner skapas. Till exempel är ett giltigt huvud värde AEG-SAS-Key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg = =.
1.  Ange rubrik för innehålls typ till "Application/cloudevents-batch + JSON; charset = UTF-8a ".
1.  Utför ett HTTP-inlägg i publicerings-URL: en med en batch med händelser som motsvarar den regionen. Ett exempel:

``` json
[
{
    "specversion" : "1.0-rc1",
    "type" : "com.contoso.ticketcreated",
    "source" : " com.contoso.account1",
    "subject" : "tickets/123",
    "id" : "A234-1234-1234",
    "time" : "2019-04-05T17:31:00Z",
    "comexampleextension1" : "value",
    "comexampleothervalue" : 5,
    "datacontenttype" : "application/json",
    "data" : {
          object-unique-to-each-publisher
    }
},
{
    "specversion" : "1.0-rc1",
    "type" : "com.contoso.ticketclosed",
    "source" : "https://contoso.com/account2",
    "subject" : "tickets/456",
    "id" : "A234-1234-1234",
    "time" : "2019-04-05T17:31:00Z",
    "comexampleextension1" : "value",
    "comexampleothervalue" : 5,
    "datacontenttype" : "application/json",
    "data" : {
          object-unique-to-each-publisher
    }
}
]
```

När du har bokfört till partnerNamespace-slutpunkten får du ett svar. Svaret är en standard-HTTP-svarskod. Några vanliga svar är:

| Resultat                             | Svar              |
|------------------------------------|-----------------------|
| Klart                            | 200 OK                |
| Felaktigt format för händelse data    | 400 Felaktig begäran       |
| Ogiltig åtkomst nyckel                 | 401 obehörig      |
| Felaktig slut punkt                 | 404 – Hittades inte         |
| Matris eller händelse överskrider storleks gränser | 413-nyttolasten är för stor |

## <a name="references"></a>Referenser

  * [Swagger](https://github.com/ahamad-MS/azure-rest-api-specs/blob/master/specification/eventgrid/resource-manager/Microsoft.EventGrid/preview/2020-04-01-preview/EventGrid.json)
  * [ARM-mall](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/allversions)
  * [Schema för ARM-mall](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2020-04-01-preview/Microsoft.EventGrid.json)
  * [REST API:er](https://docs.microsoft.com/rest/api/eventgrid/version2020-04-01-preview/partnernamespaces)
  * [CLI-tillägg](https://docs.microsoft.com/cli/azure/ext/eventgrid/?view=azure-cli-latest)

### <a name="sdks"></a>SDK:er
  * [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid/5.3.1-preview)
  * [Python](https://pypi.org/project/azure-mgmt-eventgrid/3.0.0rc6/)
  * [Java](https://search.maven.org/artifact/com.microsoft.azure.eventgrid.v2020_04_01_preview/azure-mgmt-eventgrid/1.0.0-beta-3/jar)
  * [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid/versions/0.19.0)
  * [JS](https://www.npmjs.com/package/@azure/arm-eventgrid/v/7.0.0)
  * [Kör](https://github.com/Azure/azure-sdk-for-go)


## <a name="next-steps"></a>Nästa steg
- [Översikt över partner ämnen](partner-topics-overview.md)
- [Formulär för partner ämnen onboarding](https://aka.ms/gridpartnerform)
- [Auth0-partner ämne](auth0-overview.md)
- [Så här använder du Auth0-partner ämnet](auth0-how-to.md)
