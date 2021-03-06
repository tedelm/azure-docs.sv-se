---
title: 'Snabbstart: Skapa kunskapsbas – REST, Go – QnA Maker'
description: I den här Go REST-baserade snabbstarten går vi igenom hur du skapar ett exempel på QnA Maker-kunskapsbas, programmatiskt, som visas i Azure-instrumentpanelen för ditt Cognitive Services-API-konto.
ms.date: 12/16/2019
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 221220345f4f3b7aff2a32c956d921f677ca0627
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/29/2020
ms.locfileid: "78851920"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-go"></a>Snabbstart: Skapa en kunskapsbas i QnA Maker med hjälp av Go

Den här snabbstarten går igenom hur du programmatiskt skapar ett exempel på QnA Maker-kunskapsbas. QnA Maker extraherar automatiskt frågor och svar för delvis strukturerat innehåll, som vanliga frågor från [datakällor](../Concepts/knowledge-base.md). Modellen för kunskapsbasen har definierats i JSON som skickas i brödtexten i API-begäran.

Den här snabbstarten anropar API:er för QnA Maker:
* [Skapa kunskapsbas](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)
* [Få åtgärdsinformation](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails)

[Reference documentation](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase) | [Exempel](https://github.com/Azure-Samples/cognitive-services-qnamaker-go/blob/master/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go) på referens dokumentation

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Krav

* [Go 1.10.1](https://golang.org/dl/)
* Du måste ha en [QnA Maker-tjänst](../How-To/set-up-qnamaker-service-azure.md). Om du vill hämta din nyckel och slut punkt (som innehåller resurs namnet) väljer du **snabb start** för resursen i Azure Portal.

## <a name="create-a-knowledge-base-go-file"></a>Skapa en Go-fil för kunskapsbas

Skapa en fil som heter `create-new-knowledge-base.go`.

## <a name="add-the-required-dependencies"></a>Lägga till nödvändiga beroenden

Längst upp i `create-new-knowledge-base.go` lägger du till följande rader för att lägga till nödvändiga beroenden i projektet:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=1-11 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Lägga till nödvändiga konstanter
När du har lagt till nödvändiga beroenden lägger du till de konstanter som krävs för åtkomst till QnA Maker.

Ställ in följande värden:

* `<your-qna-maker-subscription-key>`– **Nyckeln** är en 32 tecken sträng och är tillgänglig i Azure Portal på den QNA Maker resursen på snabb starts sidan. Detta är inte samma som för förutsägelse slut punkts nyckel.
* `{your-resource-name}`– Ditt **resurs namn** används för att skapa slut punkts-URL: en för redigering, i formatet `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`. Detta är inte samma URL som används för att fråga efter förutsägelse slut punkten.

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=13-20 "Add the required constants")]

## <a name="add-the-kb-model-definition"></a>Lägga till KB-modelldefinitionen
Efter konstanterna lägger du till följande KB-modelldefinition. Modellen konverteras till en sträng efter definitionen.

[!code-go[Add the KB model definition](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=22-44 "Add the KB model definition")]

## <a name="add-supporting-structures-and-functions"></a>Lägga till strukturer och funktioner

Sedan lägger du till följande stödfunktioner.

1. Lägg till strukturen för en HTTP-förfrågan:

    [!code-go[Add the structure for an HTTP request](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=46-49 "Add the structure for an HTTP request")]

2. Lägg till följande metod för att hantera en POST för QnA Maker-API: er. I den här snabbstarten används POST för att skicka KB-definition till QnA Maker.

    [!code-go[Add the POST method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=51-66 "Add the POST method")]

3. Lägg till följande metod för att hantera en GET för QnA Maker-API: er. I den här snabbstarten används GET för att kontrollera statusen för skapandeåtgärden.

    [!code-go[Add the GET method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=68-83 "Add the GET method")]

## <a name="add-function-to-create-kb"></a>Lägga till funktioner för att skapa KB

Lägg till följande funktioner för att göra en HTTP POST-begäran för att skapa kunskapsbasen. _Skapande_ **åtgärds-ID:t** returneras i **platsen** för POST-svarets huvudfält och används sedan som en del av vägen i GET-begäran. `Ocp-Apim-Subscription-Key` är QnA Maker-tjänstenyckeln, som används för autentisering.

[!code-go[Add the create_kb method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=85-97 "Add the create_kb method")]

Detta API-anrop anropar ett JSON-svar som innehåller åtgärds-ID. Använd åtgärds-ID:t för att fastställa om KB har skapats.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-function-to-get-status"></a>Lägga till funktion för att hämta status

Lägg till följande funktion för att göra en HTTP GET-begäran för att kontrollera åtgärdsstatusen. `Ocp-Apim-Subscription-Key` är QnA Maker-tjänstenyckeln, som används för autentisering.

[!code-go[Add the check_status method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=99-108 "Add the check_status method")]

Upprepa anropet tills det lyckas eller misslyckas:

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```
## <a name="add-main-function"></a>Lägga till Main-funktion

Följande funktion är huvudfunktionen och skapar KB och upprepar kontroller av statusen. Eftersom KB-skapandet kan ta lite tid måste du upprepa anropen för att kontrollera statusen tills statusen lyckas eller misslyckas.

[!code-go[Add the main method](~/samples-qnamaker-go/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base.go?range=110-140 "Add the main method")]


## <a name="compile-the-program"></a>Kompilera programmet
Ange följande kommando för att kompilera filen. Kommandotolken returnerar inte någon information för ett lyckat bygge.

```bash
go build create-new-knowledge-base.go
```

## <a name="run-the-program"></a>Köra programmet

Ange följande kommando på en kommandorad för att köra programmet. Begäran skickas till API:et för QnA Maker för att skapa KB, och sedan söker den efter resultat var 30:e sekund. Varje svar skrivs ut till konsolfönstret.

```bash
go run create-new-knowledge-base
```

När kunskapsbasen har skapats kan du visa den i QnA Maker-portalen, på sidan [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (Mina kunskapsbaser).

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Referens för QnA Maker (V4) REST API](https://go.microsoft.com/fwlink/?linkid=2092179)