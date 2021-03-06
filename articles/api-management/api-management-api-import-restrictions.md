---
title: Begränsningar och information om stöd för API-format
titleSuffix: Azure API Management
description: Information om kända problem och begränsningar för stöd för Open API, WSDL och WADL-format i Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/02/2020
ms.author: apimpm
ms.openlocfilehash: 61d43addfdf9008cb7aa8a073dcf3bb702cb55f1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/28/2020
ms.locfileid: "76513379"
---
# <a name="api-import-restrictions-and-known-issues"></a>API-importbegränsningar och kända problem

## <a name="about-this-list"></a>Om den här listan

När du importerar ett API kan du komma över vissa begränsningar eller identifiera problem som måste åtgärdas innan du kan utföra importen. I den här artikeln dokumenteras dessa begränsningar, sorterade efter import formatet för API: et. Det beskriver också hur OpenAPI-export fungerar.

## <a name="openapiswagger-import-limitations"></a><a name="open-api"> </a>Import begränsningar för openapi/Swagger

Om du får fel när du importerar OpenAPI-dokumentet ser du till att du har validerat det i förväg. Du kan göra detta antingen med hjälp av designern i Azure Portal (design-frontend-OpenAPI specifikation) eller med ett verktyg från tredje part, till exempel <a href="https://editor.swagger.io">Swagger Editor</a>.

### <a name="general"></a><a name="open-api-general"> </a>Allmänt

-   Obligatoriska parametrar i både sökväg och fråga måste ha unika namn. (I OpenAPI måste parameter namnet bara vara unikt inom en plats, till exempel sökväg, fråga, rubrik. Men i API Management tillåter vi att åtgärder diskrimineras av både sökväg och frågeparametrar (som OpenAPI inte stöder). Därför kräver vi att parameter namn är unika inom hela URL-mallen.)
-   `\$ref`pekare kan inte referera till externa filer.
-   `x-ms-paths`och `x-servers` är de enda tillägg som stöds.
-   Anpassade tillägg ignoreras vid import och sparas eller bevaras inte för export.
-   `Recursion`-API Management har inte stöd för definierade definitioner rekursivt (t. ex. scheman som refererar till sig själva).
-   Käll filens URL (om tillgänglig) används för relativa server-URL: er.
-   Säkerhets definitioner ignoreras.
-   Infogade schema definitioner för API-åtgärder stöds inte. Schema definitioner definieras i API-omfånget och kan refereras till i API-åtgärder för begäran eller svars omfattningar.
-   En definierad URL-parameter måste vara en del av URL-mallen.
-   `Produces`nyckelord, som beskriver MIME-typer som returneras av ett API, stöds inte. 

### <a name="openapi-version-2"></a><a name="open-api-v2"> </a>Openapi version 2

-   Endast JSON-format stöds.

### <a name="openapi-version-3"></a><a name="open-api-v3"> </a>Openapi version 3

-   Om många `servers` anges försöker API Management välja den första HTTPS-URL: en. Om det inte finns några HTTPs-URL: er – den första HTTP-URL: en. Om det inte finns några HTTP-URL: er måste serverns URL vara tom.
-   `Examples`stöds inte, men `example` är.

## <a name="openapi-import-update-and-export-mechanisms"></a>OpenAPI-funktioner för import, uppdatering och export

### <a name="add-new-api-via-openapi-import"></a>Lägg till nytt API via OpenAPI-import

För varje åtgärd som hittas i OpenAPI-dokumentet skapas en ny åtgärd med Azures resurs namn och visnings namn som har angetts `operationId` till `summary` respektive. `operationId`värdet normaliseras enligt reglerna som beskrivs nedan. `summary`värdet importeras som det är och längden är begränsad till 300 tecken.

Om `operationId` inte har angetts (det vill säga, eller `null`är tomt), genereras värdet för Azure Resource Name genom att kombinera http-metod och Sök vägs mal len, till `get-foo`exempel.

Om `summary` inte anges (det vill säga, inte finns `null`eller är tomt), `display name` anges värdet till `operationId`. Om `operationId` inte anges genereras värdet visnings namn genom att en mall för http-metod och sökväg kombineras, till exempel `Get - /foo`.

### <a name="update-an-existing-api-via-openapi-import"></a>Uppdatera ett befintligt API via OpenAPI-import

Under Importera befintligt API ändras till matchnings-API som beskrivs i OpenAPI-dokumentet. Varje åtgärd i OpenAPI-dokumentet matchas med en `operationId` befintlig åtgärd genom att jämföra värdet med Azures resurs namn för den befintliga åtgärden.

Om en matchning hittas kommer befintliga åtgärds egenskaper att uppdateras "på plats".

Om ingen matchning hittas skapas en ny åtgärd med hjälp av reglerna som beskrivs i avsnittet ovan. För varje ny åtgärd kommer importen att försöka kopiera principer från en befintlig åtgärd med samma HTTP-metod och sökmall.

Alla befintliga omatchade åtgärder tas bort.

Följ dessa rikt linjer om du vill göra importen mer förutsägbar.

- Se till att ange `operationId` egenskapen för varje åtgärd.
- Avstå från att `operationId` ändra efter den inledande importen.
- Ändra `operationId` aldrig och http-metod eller Sök vägs mal len samtidigt.

### <a name="export-api-as-openapi"></a>Exportera API som OpenAPI

För varje åtgärd exporteras dess Azure-resurs namn som ett `operationId`, och visnings namnet exporteras som en. `summary`
Normaliserings regler för operationId

- Konvertera till gemener.
- Ersätt varje sekvens av icke-alfanumeriska tecken med ett enda streck, till exempel `GET-/foo/{bar}?buzz={quix}` , omvandlas till `get-foo-bar-buzz-quix-`.
- Trimma streck på båda sidor, till exempel, `get-foo-bar-buzz-quix-` blir`get-foo-bar-buzz-quix`
- Trunkera för att passa 76 tecken, fyra tecken mindre än den maximala gränsen för ett resurs namn.
- Använd återstående fyra tecken för dedupliceringens suffix, om det behövs, i form av `-1, -2, ..., -999`.


## <a name="wsdl"></a><a name="wsdl"> </a>WSDL

WSDL-filer används för att skapa SOAP-vidarekoppling och SOAP-till-REST-API: er.

-   **SOAP-bindningar** – endast SOAP-bindningar av formatet "Document" och "literal" stöds. Det finns inget stöd för "RPC"-format eller SOAP-kodning.
-   **WSDL: import** -detta attribut stöds inte. Kunderna bör slå samman importen till ett dokument.
-   **Meddelanden med flera delar** – dessa typer av meddelanden stöds inte.
-   **WCF-wsHttpBinding** – SOAP-tjänster som skapats med Windows Communication Foundation ska använda BasicHttpBinding-wsHttpBinding stöds inte.
-   **MTOM** -tjänster som använder MTOM <em>kan</em> fungera. Statsstöd erbjuds inte för tillfället.
-   **Rekursion** – typer som definieras rekursivt (till exempel för att referera till en matris) stöds inte av APIM.
-   **Flera namn rymder** – flera namn områden kan användas i ett schema, men endast mål namn området kan användas för att definiera meddelande delar. Andra namn områden än målet, som används för att definiera andra indata-eller utdata-element, bevaras inte. Även om ett sådant WSDL-dokument kan importeras, kommer att ha mål namn området för WSDL i Exportera alla meddelande delar.
-   **Matriser** – SOAP-till-rest-omvandling stöder bara omslutna matriser som visas i exemplet nedan:

```xml
    <complexType name="arrayTypeName">
        <sequence>
            <element name="arrayElementValue" type="arrayElementType" minOccurs="0" maxOccurs="unbounded"/>
        </sequence>
    </complexType>
    <complexType name="typeName">
        <sequence>
            <element name="element1" type="someTypeName" minOccurs="1" maxOccurs="1"/>
            <element name="element2" type="someOtherTypeName" minOccurs="0" maxOccurs="1" nillable="true"/>
            <element name="arrayElement" type="arrayTypeName" minOccurs="1" maxOccurs="1"/>
        </sequence>
    </complexType>
```

## <a name="wadl"></a><a name="wadl"> </a>WADL

Det finns för närvarande inga kända WADL import problem.
